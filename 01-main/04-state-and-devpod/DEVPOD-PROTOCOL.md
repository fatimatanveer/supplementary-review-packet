# Matched DevPod path experiment — 5 October 2026

Freeze before collection. DevPod 0.6.15 on this macOS arm64 host, isolated DevPod
home and Docker provider, no IDE, SSH_ADD_PRIVATE_KEYS=false. Reuse one disposable
workspace across three fresh source fixtures and fresh command sessions; regenerate
outcome identities and destination keys, but do not claim three independent hosts
or workspaces. Source binaries and image match the corrected-agent study.

Compare host OpenSSH direct, host OpenSSH forwarding through DevPod's stdio SSH
server, and native `devpod ssh --agent-forwarding` to the same workspace. Retrieve
the workspace's RSA host key during isolated setup; freeze it as HOP and require
strict host-key checking on the native route. The local stdio server's `none`
transport authentication is recorded; it is not outcome authentication. Outcome
SSH uses public-only identities, fixed Ed25519 destination host keys, no fallback
authentication and no multiplexing. No personal agent or private keys are mounted.

For each of direct DEST_A policy and HOP plus HOP>DEST_A policy, first qualify
native direct/forwarded A at both destinations and unconstrained B on both routes
(eight gates). Stop if a gate fails. Then measure native DevPod A/B at both
destinations without prescribing their policy outcomes. Capture complete agent
frames with one upstream connection per downstream connection. Decode and verify
accepted host signatures, binding flags and signing context; host keys may be RSA
on HOP while outcome keys remain Ed25519. Report absence of HOP only from captured
successful signing context, never from a failed route.

After both phases qualify, restore direct policy and compare original, stock,
repaired and dedicated sources: A at both destinations, direct known-B signing,
and native B health. This supplies a second application integration, not field
usability evidence. Inspect workspace mounts; the credential source has a separate
container and no source state or original socket is mounted into the workspace.

Three fixtures; stop on setup/qualification failure and retain records. No outcome
is forced to match VS Code or native forwarding. Preserve unexpected results and
amend instruments only with a new recorded attempt. All runs are author-operated, no independent external reproduction is asserted.
