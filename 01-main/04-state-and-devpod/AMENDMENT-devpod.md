# DevPod setup amendment — 5 October 2026

Attempt devpod-v1 stopped before native qualification or product observations.
The runner expected a workspace-id/local-folder label that this provider does not
emit. Docker inspection identifies the task-created workspace by its exact unique
host-workspace mount label. Attempt devpod-v2 uses that label and still requires
exactly one match and inspects its mounts. No policy expectation or endpoint is
changed; the failed attempt and frozen runner remain retained.

Setup observations: the image lacks Git and DevPod reports its Git credential
setup unavailable. SSH remains separately qualified. The local stdio SSH server
accepts transport authentication method `none`; the isolated DevPod/Docker access
path is trusted infrastructure. Outcome authentication must still complete using
the declared public key. The RSA HOP key is captured before collection and is
strictly checked on the native OpenSSH path. This differs from the historical
Kali/Ubuntu DevPod fixture and is reported as a separate configuration.

Attempt devpod-v2 qualifies direct native endpoints but cannot qualify the HOP
comparison: the four forwarded connections present four different RSA host-key
fingerprints, each differing from the setup key. Strict host-key verification
therefore fails before outcome authentication. These are ineligible topology
observations, not A-policy denials. No product outcome was collected in that
attempt. 

A separate endpoint-only series (devpod-direct-v1) is frozen after this finding.
It qualifies four native direct A/B endpoints, measures four DevPod A/B endpoints,
then compares original/stock/repaired/dedicated sources with permitted/forbidden A,
known-B requests and source-health controls: 24 records per fresh source fixture.
Three fixtures reuse the workspace and create fresh command sessions. Destination
keys remain fixed and strictly checked within each fixture. The series tests
destination/identity composition and observed signing context only; it does not
resolve the failed HOP policy comparison or replace its records.
