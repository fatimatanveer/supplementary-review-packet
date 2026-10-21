# Results and attempt accounting — 5 October 2026


## Consolidated current-source state study

Three state-v1 fixtures qualified and completed using the retained stock/repaired
ssh-agent-filter 0.5.3 binaries, corrected OpenSSH 10.5p1 agent and cached runtime.
Each preserves the earlier 72 primary direct/topology cells, restart event and
42 baseline state records, then adds all 23 state-case labels in both phases on
native/repaired endpoints. Each records 349 authentication/direct observations and
201 extended protocol records: 92 semantic case groups plus five management probes.

The permitted/forbidden endpoint matrices agree with the preceding qualified
study. Repaired B is excluded without enumeration. Constrained missing, invalid,
cross-connection and mismatched-context requests fail. Two live clients bind,
synchronize and can use their own tested session but not the other's. Unconstrained
A continues signing raw/SSHSIG-shaped data. The repair adds identity selection,
not a standalone signed-data grammar. Selected management operations refuse and
the A/B health controls remain usable. Retained connections close after restart;
fresh bound signing and real authentication recover.

The [amended audit](receipts/state-v1-amended.json) checks 23 baseline plus 62
extended signatures per fixture. The initial receipt (receipts/state-v1.json)
fails six assertions because the checker incorrectly required native disconnect
on the malformed frame. Native replies with code 5; the repaired filter closes.
[The amendment](AMENDMENT-audit.md) documents the retrospective interpretation
correction. Raw records and frozen collection inputs are unchanged. No malformed
request is treated as evidence of a valid authorization decision.

## DevPod follow-up

The installed macOS DevPod CLI is 0.6.15. It uses an isolated DevPod home, Docker
provider, no IDE, SSH_ADD_PRIVATE_KEYS=false and one disposable workspace. The
workspace contains no original-agent socket, source state or outcome private keys.
The source fixture runs separately and is reached through the instrumented
credential channel. Source keys regenerate between fixtures; the workspace is
reused. This differs from the historical Kali/Ubuntu configuration.

| Attempt | Recorded endpoints | Disposition |
| --- | --- | --- |
| devpod-v1 | 0 | Workspace lookup used unavailable labels; prequalification setup failure |
| devpod-v2 | 8 native gates | Four direct gates pass; four forwarded probes fail strict host-key verification before outcome authentication |
| devpod-direct-v1 | 3 × 24 | Separate direct-policy protocol amendment; all fixtures qualify and complete |

The stdio SSH server presented four distinct RSA fingerprints in the four
forwarded attempts, differing from the setup key. A policy using a fixed HOP
identity therefore could not qualify in that schedule. This is an unresolved
topology comparison; it is not four policy denials or a product security flaw.
The [scope amendment](AMENDMENT-devpod.md) preserves the failed route and defines
the narrower direct-policy study before its collection.

In all three completed direct-policy fixtures:

| Exposed source | A at DEST_A | A at DEST_B | Direct known B |
| --- | --- | --- | --- |
| Original A+B | Allow | Deny | Allow |
| Stock selected-A filter | Deny | Deny | Deny |
| Repaired selected-A filter | Allow | Deny | Deny |
| Dedicated constrained-A agent | Allow | Deny | Deny |

Accepted successful product signing context contains the destination with flag 0,
without a HOP binding. The [DevPod audit](receipts/devpod-final.json) verifies 14
signatures per fixture, exact source affinity, schedules, public identity and
destination attribution, and accepted binding/signing context. This result extends
direct-policy integration to another shipping application implementation; it does
not fill the native HOP comparison or establish arbitrary DevPod forwarding-path
preservation.

## Replay and scientific limits

[The aggregate replay](receipts/replay-v1.json) passes current state, completed
DevPod, preceding VS Code and preceding Git checks in separate processes. The
runner does not supply the checker's expectations. Nevertheless, both remain
products of the same author-operated research process. These checks validate
saved records, not an independent specification, human reports or field reliability.

The final archive includes the current and historical evidence. Its packaging
receipt identifies structural checks and clean-extraction replay separately from
scientific collection.
