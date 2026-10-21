# Shipping V2 context and development workflow — frozen before execution

## Questions

Does the installed VS Code Remote SSH → Dev Containers channel represent the SSH
backend host in the context delivered to the source? Can the repaired filter supply
the three endpoint requirements through an actual development container?

Use installed VS Code and exact extension versions recorded by the runner. A
disposable SSH backend (HOP) uses a separate transport identity. Its ordinary
transport has ForwardAgent no; the development container is opened through the
shipping Dev Containers extension. Sources, HOP, destinations and identities are
fresh. Neither the user's personal agent nor existing sessions are touched.

## Topology and trust boundary

The original constrained agent and existing stock/repaired Linux binaries run in
an isolated source container with only destination sshd ports published to host
loopback. Host Unix sockets connect to its agent sockets via one docker-exec stdio
relay per connection. These instrumentation relays preserve complete frames and
connection lifetime and record public request/reply bytes. No agent TCP endpoint
is exposed to the development container. A host-only source selector closes old
connections when switching arms; each new connection records its selected source.

The SSH backend runs the Docker CLI to start the development container. It is a
trusted orchestration host with Docker-daemon access; the tested adversary is code
inside the final development container, which must have no Docker socket, outcome
private keys, source-state mount or alternate agent endpoint. This is not an
isolation claim against a compromised Docker-controlling backend. The origin host,
Docker engine, native agent and instrumentation remain trusted.

Docker's Linux arm64 CLI 28.5.2 is acquired from the official static distribution,
hashed and mounted into the disposable backend only. No project dependencies or
user application settings are changed. VS Code uses a new user-data directory,
disabled updates/telemetry, and only the necessary remote extensions enabled.

## Schedule and qualification

Plan three fresh sessions. Before product probes qualify native direct A/DEST_A
allow, A/DEST_B deny and B/both allow. With direct DEST_A policy qualify native
forwarded A/DEST_A deny, and B forwarding health. With both HOP and HOP>DEST_A
edges qualify forwarded A/DEST_A allow, forwarded A/DEST_B deny and direct
A/DEST_A deny. HOP permits transport-key login only. Failure of any native gate
makes the associated product policy phase ineligible.

For the product channel under direct and forwarded policies, test A at both
destinations, B route health, and direct known-key requests without enumeration.
These product outcomes are observations, not prescribed native equivalence.
Decode accepted context for successful authentication. Classify the path as
carrying HOP context, carrying only destination context, losing usable context,
or unresolved. Do not infer signer location or a policy bypass from a matching
fingerprint or from a failed route.

For the workflow restore direct DEST_A policy and select original, stock filter,
repaired filter and dedicated constrained-A source in turn. Exercise real SSH
authentication and direct B requests from the same shipping development container.
Expected endpoint pattern: original allows permitted A and B; stock excludes B but
loses permitted A; repaired and dedicated sources allow permitted A, deny forbidden
A, and refuse direct B. Verify positive responses cryptographically and require
healthy destination controls. If the product does not transport direct-policy
authentication, report the workflow unavailable rather than force this expectation.

## Evidence and stopping rules

Retain source freezes, versions, startup logs, configuration, public identities,
commands, client output, per-connection public traces, container mount inspection,
qualification and attempt status. Destroy disposable private keys during cleanup.
Each product startup has a bounded timeout; preserve failures. No UI screenshot or
visual iteration is required. Instrument/configuration defects permit a documented
amendment and fresh attempt. New scientific expectations must never be changed to
make an unexpected product observation pass.

