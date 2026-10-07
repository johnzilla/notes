# notes

Notes and working documents, shared for collaboration.

## [What A Bitcoin Custody Pitch Has To Show](what-a-custody-pitch-has-to-show.md)

**Version 0.15, peer review draft.** Comments on this version can cite the [fixed v0.15 text](https://github.com/johnzilla/notes/blob/v0.15/what-a-custody-pitch-has-to-show.md); see the [changelog](what-a-custody-pitch-has-to-show.md#changelog) for revisions.

A threat-model and evidence-review framework for Bitcoin custody arrangements, written for security researchers and technical reviewers. It separates what a custody claim ("only you can spend," "no single party can move it," "recovery is guaranteed") says from what the protocol enforces and what a deployment would have to prove. It works through a single-signature baseline and nine configurations: institutional, collaborative, and holder-controlled 2-of-3; multiple timed spending paths; joint signing with delayed exit; split backups of a single-signature secret; threshold and aggregate signing; accounts with external spending authority; and escrow. A further section covers payment channels. For each, it states the protocol consequences and the evidence a review should require. It then walks the full custody lifecycle, including signing-device supply chain, signer key leakage through nonces and the transfer channel, fee and deadline feasibility, and chain reorganizations, and covers quorum size, software that reaches a quorum of keys, shared failures, privacy, and coercion. It ends with questions that make a claim reviewable, a references section with each specification pinned to the version consulted, and a changelog. Configurations, not providers: no organization is named or assessed.
