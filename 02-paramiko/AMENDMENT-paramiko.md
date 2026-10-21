# Prospective target extension: Paramiko 4.0.0

Recorded after the three-fixture sequence pilot and before any Paramiko live
target measurement. The initial pilot is method development, not held-out
validation: both seeded defect classes were detected by the fixed suite.

Choose Paramiko 4.0.0's documented AgentRequestHandler over a real authenticated
SSH transport to a disposable OpenSSH HOP. Pin that version rather than claiming
the newest release. This target has not previously been measured in this study.
Its documented forwarding API motivates a specific prediction: final-destination
constraints will survive byte forwarding, while an OpenSSH-compatible HOP
binding may be absent. This is a test of predictive transfer, not a claim that
binding requirements or their omission are newly discovered.

Use three fresh fixtures, pinned offline runtime, separate transport key, strict
known_hosts verification, no alternate authentication, and two matched final
destinations. Use the whole source A+B; Paramiko is not an identity-selection
filter and B denial is not an expected capability. Never infer identity exclusion.

For origin policy DEST_A, native direct A→A allows, A→B denies; native forwarded
A→A and A→B deny. For HOP policy HOP plus HOP>DEST_A, native direct A→A denies,
native forwarded A→A allows and A→B denies. Qualify both native relations and
source B health before each target session. A failed gate stops the fixture.

Run real SSH A/B to both destinations from the Paramiko-forwarded socket under
each policy, and preserve public upstream frames. Decode all session bindings,
their forwarding flags, and the supplied host keys. Measure the socket's two
logical connections with a short policy-revocation/restoration sequence, comparing
with matched source histories (B is expected to remain usable). Same-device,
author-operated evidence only. No independently recruited operator.

At most two qualified transport sessions per fixture (one per policy), plus
qualification and a ten-step raw sequence per session. Retain setup failures,
all source/target requests, exact package hashes, and output; do not disable
strict host verification to obtain a result. Package retrieval is separate from
the offline experiment. A socket lifetime limitation is reported as such, not
as destination-policy refusal. Final result claims stay limited to this version
and this explicit forwarding API configuration.
