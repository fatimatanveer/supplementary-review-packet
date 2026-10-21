# Schedule amendment, 2026-10-05, before any Paramiko target measurement

This amendment freezes the operation list, gates, and predictions for the
experiment named in `AMENDMENT-paramiko.md`. It does not change the pilot
corpus or the pilot mutant. No target request has been sent under this
schedule.

## Harness adaptation, from Paramiko 4.0.0 source

`AgentRequestHandler.__init__` calls `Channel.request_forward_agent`. That
method sends `auth-agent-req@openssh.com` with the reply flag clear, then
registers a handler. On `auth-agent@openssh.com`, `AgentClientProxy` connects
to `SSH_AUTH_SOCK` and copies bytes in both directions in 512-byte reads. The
class does not parse agent messages and does not construct
`session-bind@openssh.com`. The installed sources are not modified.

The harness keeps that session alive with a remote `printf` of `SSH_AUTH_SOCK`
and `exec sleep`. Real OpenSSH clients on the same disposable host then use
that socket. This is the sshd-created forwarded endpoint. The harness does not
use `AgentServerProxy` and does not insert a synthetic HOP binding into the
Paramiko path.

Paramiko connects to `127.0.0.1` port 22343 with `RejectPolicy`, a dedicated
known_hosts entry for `[127.0.0.1]:22343`, the transport key only,
`allow_agent=False`, `look_for_keys=False`, and no password, passphrase, GSSAPI,
or keyboard-interactive credential. The transport key is not an outcome
credential.

## Sessions

Three fresh fixtures. Each opens three transport sessions, in order:
unconstrained qualification, origin policy `DEST_A`, and HOP policy `HOP` plus
`HOP>DEST_A`. The two policy sessions are the declared policy measurements.
The qualification session is the additional liveness session. Each of the three
sessions runs the same ten operations below. Both logical connections are
opened before step 1 and closed after step 10. Policy edits happen while those
connections stay open. Removing A uses `ssh-add -d` on A's public file only.

## Ten operations

1. Connection 0 binds DEST_A, forwarding flag clear, valid host signature.
2. Connection 0 signs a hostbound A authentication for that same session id.
3. Connection 1 signs a raw B challenge.
4. Administrative removal of A.
5. Connection 0 repeats the step 2 bytes.
6. Connection 1 repeats the step 3 bytes.
7. Administrative restoration of the same public A under the session policy.
8. Connection 0 repeats the step 2 bytes on the retained connection.
9. Connection 1 binds DEST_B, forwarding flag clear, valid host signature.
10. Connection 1 signs a hostbound A authentication for DEST_B.

## Gates, which stop or exclude the fixture

Native direct and native OpenSSH-forwarded results must match
`schedule_paramiko.json` `native_gates`. Unconstrained Paramiko real SSH for A
and B to both destinations must allow. B through Paramiko must allow in every
session; otherwise an A result from that path is inconclusive and the fixture
is excluded. The unconstrained native forward trace must contain a
cryptographically valid session-bind naming the HOP host key. A missing socket,
authentication timeout, or EOF is inconclusive, not a destination denial.

## Predictions, which may fail without invalidating the audit

These were fixed before measurement. A miss is a result.

- Final-destination constraint: Paramiko-forwarded A to DEST_B is deny under
  both origin and HOP policies.
- HOP context: Paramiko supplies no HOP binding. Under origin policy,
  Paramiko-forwarded A to DEST_A allows, while native OpenSSH forwarding
  denies. Under HOP policy, Paramiko-forwarded A to DEST_A denies, while
  native OpenSSH forwarding allows.
- Raw step 2 allows under unconstrained and origin policies, and denies under
  HOP policy, because the raw request carries only a DEST_A binding.
- Step 5 denies and step 6 allows in every policy.
- Step 8 matches step 2. Step 10 denies under origin and HOP, and allows when
  the key is unconstrained.
- Bind replies are success. Temporal integrity requires the source replay to
  match the forwarded bytes and decisions. That match does not prove a
  physical hop was present.

Source-policy edits are local administration. They are not attacker actions.
B denial is not an expected Paramiko capability.
