# Checker amendment v1.1, after the v1 audit

The live collection remains harness `paramiko-shim-3fix-v1` in
`evidence/paramiko-shim-3fix-v1/`. This amendment does not rerun it.
`receipt-paramiko-shim-3fix-v1.json` is retained. It reported 5 errors, 126
verified signatures, and matching A→DEST_A transcripts on every causal cell.
Those 5 errors are checker defects, not a second experiment.

## Search text in checker source

The v1 private-key scan looked for the OpenSSH armor line in every saved
file. That line is also the search literal in the snapshotted
`verify_shim.py` and `verify_shim_3fix.py`. The v1 audit reported those two
source files as private-key material. v1.1 still scans every saved file
except the `verify_shim*.py` sources, and it builds the search text from two
adjacent parts so this checker does not contain the armor as one literal.

## Destination connection

On each HOP-policy native forward of A to DEST_B, the v1 window contained two
agent connections. One bound only the HOP host key with flag 0 and listed A
and B. The other bound the HOP host key with flag 1 and then the DEST_B host
key with flag 0, listed only B, and carried no A signature. The SSH transcript
was `Permission denied (publickey)`. v1 pooled the connections, treated the
HOP-only listing of A as part of the DEST_B attempt, and called the denial
unexplained.

v1.1 classifies a destination attempt from the connection that binds that
destination's host key. The separate HOP-only identities answer remains in
the trace and is not the DEST_B decision. A denial still counts as policy
enforcement only when that destination connection is an identities refusal or
a signing refusal, the transcript is a public-key denial, and the same-phase
B control allows. An absent destination binding stays unexplained.
