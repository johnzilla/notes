# notes

Notes and working documents, shared for collaboration.

## [What A Bitcoin Custody Pitch Has To Show](what-a-custody-pitch-has-to-show.md)

A threat-model and evidence-review framework for Bitcoin custody arrangements, written for security researchers and technical reviewers. It separates what a custody claim ("only you can spend," "no single party can move it," "recovery is guaranteed") says from what a protocol actually enforces. It works through a single-signature baseline and nine configurations: institutional and collaborative multisig, timed recovery paths, split backups, threshold signing, accounts, escrow, and payment channels. For each, it states the protocol consequences and the evidence a deployment would need to supply. It then walks the full custody lifecycle, covers quorum size and shared failures, privacy, and coercion, and ends with questions that make a claim reviewable. Configurations, not providers: no organization is named or assessed.
