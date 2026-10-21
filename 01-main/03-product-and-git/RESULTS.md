# Final validation results — 4 October 2026

## Current-source filter with corrected upstream agent

Canonical live evidence: evidence/fixed-agent-v1. Three fresh fixtures completed with native, stock 0.5.3 and repaired 0.5.3 paths. Each contains 72 primary endpoints, 145 authentication/direct records including controls, and 42 state/protocol records. The retained OpenSSH 10.5p1 agent was combined with the previously built filter binaries; client/server versions were retained. Binary and source hashes are frozen with the run.

| Operation | Native | Stock | Repaired |
| --- | --- | --- | --- |
| Direct-policy A at DEST_A / DEST_B | allow / deny | deny / deny | allow / deny |
| Direct B without enumeration | allow | deny | deny |
| Direct-policy A through HOP to DEST_A | deny | deny | deny |
| HOP-policy A directly at DEST_A | deny | deny | deny |
| HOP-policy A through HOP to DEST_A / DEST_B | allow / deny | deny / deny | allow / deny |

Selected invalid/missing/cross-connection and concurrent-session requests are refused. Malformed signing closes the repaired path and remains inconclusive for authorization. Held paths close after upstream restart; fresh unbound requests refuse and fresh correctly bound requests sign. The new process-event record checks that restart occurs between pre-restart signing and retained-path failure, and records distinct old/new PIDs. No transparent recovery is claimed.

## Additive evidence audit

Canonical receipt: receipts/semantic-v4/summary.json. The baseline verifier still checks request semantics, signatures, schedules and permitted topology traces. The new layer additionally checks binding responses, per-client partial order, available connection IDs, temporal controls and client-log destination/key attribution.

| Corpus | Fixtures | Verified signatures | Contradictions | Coverage |
| --- | --- | --- | --- | --- |
| Historical factorial/state | 3 historical | 441 | 0 | Client connection IDs and restart events absent; management-state semantics outside scope |
| Earlier current-source | 3 historical | 69 | 0 | Restart event absent |
| Fresh corrected-agent combination | 3 fresh | 69 | 0 | All fields required by the added checks present |

There are 453 historical and six earlier-current-source coverage-gap entries across their fixtures. These are per-case labels, not distinct flaws, failed endpoints or statistical trials. Zero fresh gaps applies only to the declared checks.

Eight historical and twelve per current-format corpus corrupted-evidence checks were rejected for their intended reasons (32 total). Each family also accepted a valid reversed serialization order, preserving the distinction between record ordering and causal partial order. Mutations leave canonical evidence unchanged. This establishes selected fault sensitivity, not oracle independence or exhaustive correctness.

## Retained audit corrections

semantic-v1 reached the fresh series before its closing manifest was written. Initial checks also misclassified supplementary B path-health labels as primary A topology labels. After exact-label correction, semantic-v3 detected a response-code assumption: the old agent used 5 for rejected invalid binding, while 10.5p1 used extension failure 28. RFC 9987 §5.8 and the retained upstream source explain that difference. semantic-v4 accepts explicit rejection codes and still requires 6 for accepted bindings. Subsequent constrained signing refuses in both versions. This is an explained oracle correction; no raw result or scientific endpoint expectation was changed.

## Shipping-product context and integration

Canonical live evidence: evidence/product-v6; audit: receipts/product-v6-audit-v1.json. Three fresh sessions each completed 52 records with the same product matrix. VS Code 1.140.0, Remote SSH 0.128.0 and Dev Containers 0.469.0 ran on macOS arm64 with disposable Linux arm64 backends/containers. This is the nested Remote SSH → Dev Containers path, with lower ForwardAgent no and no lower-shell agent. The Docker-controlling backend is trusted; the adversary is code in the final development container. Mount inspection retains only its public workspace and VS Code volume.

| Origin-agent policy | Native direct A: DEST_A / DEST_B | Native through HOP A: DEST_A / DEST_B | Product A: DEST_A / DEST_B |
| --- | --- | --- | --- |
| DEST_A | allow / deny | deny / deny | allow / deny |
| HOP plus HOP>DEST_A | deny / deny | allow / deny | deny / deny |

For permitted product signing, the accepted same-connection chain contains only DEST_A with forwarding flag 0. Native forwarded signing contains HOP with flag 1 followed by the destination with flag 0. Product B remains healthy at both destinations in both policy phases. Raw known-A signing refuses and raw known-B signing succeeds. These observations establish final-destination enforcement and absence of HOP in the tested product signing context; they do not establish full forwarding-path preservation or an undocumented vulnerability.

| Source exposed through the same product channel | A: DEST_A / DEST_B | Direct known B |
| --- | --- | --- |
| Original multi-identity agent | allow / deny | allow |
| Stock 0.5.3 filter | deny / deny | deny |
| Repaired 0.5.3 filter | allow / deny | deny |
| Dedicated constrained-A agent | allow / deny | deny |

The source selector closes prior connections and the audit verifies the selected source on each captured operation. Product authentication is real SSH execution from the development container. The product audit verifies 90 authentication/direct signatures across 156 records, native gates, source association, destination identity, accepted binding chains and exact direct requests. All four targeted mutations are rejected for their intended reasons; a valid reversed serialization passes. The product oracle is author-operated and was written after collection; observed product outcomes are reported rather than forced to match native forwarding.

### Product attempt accounting

| Attempt | Retained observations | Disposition |
| --- | --- | --- |
| product-v1 | 8 native; 0 product | Host restart; no closing status/manifest; recovery record retained |
| product-v2 | 0 | Stock socket on Docker host mount unsupported; moved to container-local storage |
| product-v3 | 8 native; 0 product | Startup download timeout; cleanup race retained |
| product-v4 | 8 native; 0 product | CLI downloaded, SFTP transfer unavailable in fixture; bounded timeout |
| product-v5 | 0 | Docker API unavailable before fixture creation |
| product-v6 | 3 × 52 complete | Qualified canonical product series; source and version freezes retained |

Early attempts used an older installed VS Code version. V6's freeze and actual remote CLI log identify 1.140.0. These attempts do not estimate product reliability; setup corrections, application-version change and interruption prevent such an inference. See AMENDMENT-product-restart.md. No product failure is counted as a destination-policy denial.

## Concrete Git workflow

A separately frozen fresh fixture (evidence/git-v1; GIT-PROTOCOL.md) performs real Git clones from the development container. Git is installed only in disposable source/development containers; package versions and commands are retained. Both destinations expose the same one-commit disposable repository and authorize A+B. Native controls are repeated after package installation. There are 32 records: 16 qualification/health and 16 clone/direct/control observations.

Original, repaired and dedicated sources clone the expected commit and checked-out README hash from DEST_A. Stock refuses that clone; every source refuses cloning from DEST_B with an explicit SSH public-key denial. Native direct B succeeds, while stock/repaired/dedicated B requests refuse. Thus the original source supplies excess identity access, stock loses permitted work, and repaired/dedicated sources meet the tested operational requirements. The repository contains no private keys.

receipts/git-v1-audit-v1.json verifies 18 signatures, the eight clone outcomes, captured commit/content hashes, source identity, native gates and accepted destination-only chains. Two deliberate corruptions (wrong cloned content and false outcome) are detected. This is one operational demonstration, not a second three-session replication, a Git security study or a usability claim.

