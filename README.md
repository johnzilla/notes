# notes

Notes and working documents, shared for collaboration.

## [Who Can Move Your Bitcoin?](who-can-move-your-bitcoin.md)

*Questions to ask about any way of holding it*

**Version 0.18, peer review draft.** Comments on this version can cite the [fixed v0.18 text](https://github.com/johnzilla/notes/blob/v0.18/who-can-move-your-bitcoin.md); see the [changelog](who-can-move-your-bitcoin.md#changelog) for revisions.

A threat-model and evidence-review framework for Bitcoin custody arrangements, for people choosing how to hold bitcoin and for technical reviewers. It opens with a plain-language Start here section. It separates what a custody claim ("only you can spend," "no single party can move it," "recovery is guaranteed") says from what the protocol enforces and what a deployment would have to prove. It works through a single-signature baseline and nine configurations: institutional, collaborative, and holder-controlled 2-of-3; multiple timed spending paths; joint signing with delayed exit; split backups of a single-signature secret; threshold and aggregate signing; accounts with external spending authority; and escrow. A further section covers payment channels. For each, it states the protocol consequences and the evidence a review should require. It then walks the full custody lifecycle, including signing-device supply chain, signer key leakage through nonces and the transfer channel, fee and deadline feasibility, and chain reorganizations, and covers quorum size, software that reaches a quorum of keys, shared failures, privacy, and coercion. It ends with questions that make a claim reviewable, a references section with each specification pinned to the version consulted, and a changelog. It describes kinds of arrangements, not companies: no organization is named or assessed.

## [What Can You Check About a Software Release?](what-can-you-check-about-a-release.md)

*Questions to ask about a published file*

**Version 0.4, peer review draft.** See the [changelog](what-can-you-check-about-a-release.md#changelog) for revisions.

A comparison of software release processes, for people deciding what a download page is evidence of and for technical reviewers. It opens with a plain-language Start here section. It separates three checks: whether a change was public, who signed the release file, and whether another builder rebuilt it. It reads Bitcoin Core's release process beside three other well-known processes: the Linux kernel, Tor Browser, and the Debian archive. For each, it records the signed object, where keys are published, where a rebuild sits in the process, and what the person who downloads checks. A side-by-side table puts the same questions to all four. A further section covers provenance records, such as signed hash files, build-information files, SLSA provenance, transparency logs, and timestamps, and maps each to the checks. Every figure is tied to the rule or listing it came from and the date it was read. The note compares processes and does not rank them: no organization is assessed.

## [What Can You Check About an embit Release?](embit-release-records.md)

*The release note's questions, put to one library*

**Version 0.6, peer review draft.** See the [changelog](embit-release-records.md#changelog) for revisions.

A worked example of the release note's method, applied to embit, a Bitcoin library for Python that other projects build on. It opens with a plain-language Start here section. It records three objects: the 0.8.0 file on the Python Package Index, the signed tags for 0.8.1 and 0.8.2 in the source repository, and the written release process that the project added in 0.8.1. For each, it states what the public, signed, and rebuilt checks show, what the person who downloads can check, and what stays unresolved. It records the byte-for-byte match of 60 files shared by the PyPI archive and tagged tree, while leaving the native libraries' build provenance unresolved. It distinguishes workflow output destinations and the two attestation paths, and states what one downstream check script actually checks. It notes which object two downstream projects pin. Every reading is tied to a pinned commit or to the date a page was read. The note records release evidence only: it does not assess the library's code, and no person is assessed.

## [Which Code Actually Runs?](which-code-actually-runs.md)

*Questions to ask when software has more than one way to do a job*

**Version 0.3, peer review draft.** See the [changelog](which-code-actually-runs.md#changelog) for revisions.

A note on fallbacks in software that makes or uses keys, for people who rely on such software and for technical reviewers. It opens with a plain-language Start here section. It defines a fallback, the selection between two ways to do a job, and the difference between code that is present and code that is reached. It describes four kinds of fallback and what a reader could inspect for each. It reads two public records: the random source for seeds in Coldcard firmware, from the maker's account and one independent analysis, and the secp256k1 implementation in the embit library. A table sets out what separates the two records, so that no conclusion carries from one to the other. It ends with the controls a project can show, each paired with the artifact that lets a reader check it, and with questions that make a claim reviewable. It makes no claim about how common the pattern is, and no organization is assessed.
