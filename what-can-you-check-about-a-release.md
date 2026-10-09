# What Can You Check About a Software Release?

*Questions to ask about a published file*

It describes kinds of release records, not projects. It does not rank them.

| Document | what-can-you-check-about-a-release.md |
| --- | --- |
| **Version** | 0.1 — peer review draft |
| **Audience** | People deciding what a download page is evidence of: read [Start here](#start-here). Technical reviewers: read the full note. |
| **Method** | Separate three checks — reviewed, signed, rebuilt — and tie each claim to a public artifact. A number is recorded with the listing or rule it came from, and with the date that listing was read. |
| **Changes** | See the [changelog](#changelog). |
| **Scope** | Public release records for source archives and distributed binaries. This is not a finding about any organization, a review-hour comparison, or an assessment of every package a distributor ships. |

## Contents

- [Start here](#start-here)
- [1. What the note establishes](#1-what-the-note-establishes)
  - [1.1 Evidence](#11-evidence)
  - [1.2 Terms](#12-terms)
- [2. Three checks](#2-three-checks)
- [3. Public records](#3-public-records)
  - [3.1 Bitcoin Core release binaries](#31-bitcoin-core-release-binaries)
  - [3.2 Linux kernel source archive](#32-linux-kernel-source-archive)
  - [3.3 Tor Browser packages](#33-tor-browser-packages)
  - [3.4 Debian archive packages](#34-debian-archive-packages)
  - [3.5 A maintainer-signed release](#35-a-maintainer-signed-release)
- [4. What a number is evidence of](#4-what-a-number-is-evidence-of)
- [5. Questions that make a claim reviewable](#5-questions-that-make-a-claim-reviewable)
- [6. Limits and source use](#6-limits-and-source-use)
- [7. References](#7-references)
- [Changelog](#changelog)

## Start here

This section is for anyone looking at a download page. You do not need to build the software to use it. The rest of the note is the record behind each question.

**What a release is.** A license can say the source is available. A release is a file someone published: a source archive, a binary, or both. The file can be honest about what it is and still leave a different question open. "Open source" does not answer who read the change, who signed the file, or who else produced the same hash.

**The questions that matter.** Ask these about a release you did not build yourself. Ask them about a release you did build, if other people are being asked to run your binary.

1. **Was the change public?** Can someone other than the author read the diff that went into this file? A public repository, a mailed patch, or a signed tag is evidence you can check. A private branch that appears only as a binary is not that evidence.
2. **Who signed the file you downloaded?** A detached signature and a published key tell you who attested that file. They do not, by themselves, tell you that a second person built it.
3. **Did someone else rebuild it?** A second signature over the same hash, from a different builder, is evidence that someone else started from the stated source and produced that output. One signature is not that evidence.
4. **What object was signed?** A signature on a source tarball attests the archive. A signature on a binary attests the binary. A distributor who compiles source later produces a new record.
5. **What is the number counting?** A rule can require a count before upload. A folder listing can show who attested a tag. A dashboard can show a share of packages. Those are different artifacts. Recount the one you mean.

**How to judge an answer.** A good answer shows you something you can open: a document, a signature file, a folder of attestations, a dashboard. A weak answer repeats the claim in different words. A weak answer does not prove a problem. It shows that the claim is not proved yet.

**What this note does not do.** It does not rank projects. It does not say that more signatures are a better project. It does not compare review-hours. The right weight for each check depends on what you need the file to be evidence of.

## 1. What the note establishes

A release claim needs a defined object: a tag, a tarball, a binary, a hash file, or a package in an archive. "Open source," "signed," and "reproducible" describe different properties. None follows from a license name or from a download button.

This note uses public documents and public listings. It does not inspect a private build machine, a signing key ceremony, or a distributor's unpublished configuration. Consequences below are tied to the artifact named in the same paragraph.

### 1.1 Evidence

Each record in this note has: **claim; object and date; public source; what was read; result; what is unresolved.** Dates are the date the listing was read, not a claim that the listing is stable.

### 1.2 Terms

- **Reviewed.** The change is in a place someone other than the author can read. This note does not grade the review.
- **Signed.** A published key attests a named file. This authenticates a publisher. It does not establish that the file matches source unless a separate check says so.
- **Rebuilt.** Someone other than the publisher started from the stated source and produced the same hash. Matching hashes are evidence about the build. They are not evidence about who reviewed the commits.
- **Publish gate.** A written step that must be done before the file is uploaded. A document that calls a practice desirable is not a gate.
- **Downstream build.** A later compilation by a distributor, an image builder, or a container build. It is its own record.

## 2. Three checks

| Check | Question | Artifact that answers it | What it does not answer |
| --- | --- | --- | --- |
| Reviewed | Can someone other than the author read the diff? | A public pull request, a mailed patch, a signed tag | Whether the binary matches that diff |
| Signed | Who attested the file you downloaded? | A detached signature and a published key | Whether anyone else built it |
| Rebuilt | Did someone else produce the same hash from the stated source? | A second signature over that hash, from a different builder | Whether that builder reviewed the commits |

A release can pass one check and not the others. Record which object you checked.

## 3. Public records

### 3.1 Bitcoin Core release binaries

**Claim.** Release binaries are uploaded after independent Guix builds match, and those matches are published as signed hash files.

**Object and date.** The release process document on the master branch, read 9 October 2026. The `31.0` attestation folder, listed 9 October 2026.

**Public source.** `doc/release-process.md` in the Bitcoin Core repository. Attestations in `bitcoin-core/guix.sigs`.

**What was read.** The publish step and the codesign step. The signer directories under `guix.sigs/31.0`.

**Result.**

- The build has two stages. The first stage compiles from the tag. The second stage attaches Windows and macOS code signatures to those outputs.
- The publish heading is: "After 6 or more people have guix-built and their results match." The following step combines `all.SHA256SUMS.asc` from the signers and uploads the binaries with that combined signature file.
- The codesign step says: "Once the Windows and macOS builds each have 3 matching signatures, they will be signed with their respective release keys."
- Attestations are filed as `guix.sigs/<version>/<signer>/`. A builder key is added by pull request. Commit access to the source repository is not the documented requirement for adding a key.
- On 9 October 2026 the `31.0` folder listed 21 signer directories. That is a listing of who attested that tag. It is not a count of people who reviewed the commits in the tag. The written upload rule is 6 or more matching builds, not 21.

**Unresolved.** This note did not rerun a Guix build. It did not check that every directory contains a matching `all.SHA256SUMS`. A later release will have a different folder.

### 3.2 Linux kernel source archive

**Claim.** A kernel.org release tarball is attested by one developer signature on the archive. The project ships source. The binary a machine boots is a later build.

**Object and date.** The signature instructions and the reproducible-builds note, read 9 October 2026.

**Public source.** `kernel.org/signature.html`. `Documentation/kbuild/reproducible-builds.rst` in the kernel tree.

**What was read.** The verification instructions, the published key list, and the reproducible-builds document.

**Result.**

- Release tarballs carry an OpenPGP signature. The published check is to verify that signature against a key listed for the developer who issued the release.
- That signature attests the archive the named key signed. It is not a set of independent rebuilds of a binary.
- The kernel's reproducible-builds document describes how people who ship binaries can avoid unreproducible output. It is not written as the step before a kernel.org tarball is published.
- A distributor compiles a kernel after the source release. That compilation is a second record. A signature on the upstream tarball does not attest the distributor's binary.

**Unresolved.** This note did not verify a particular tarball. It did not survey distributor kernels.

### 3.3 Tor Browser packages

**Claim.** The design document requires matching builds from at least two release engineers, on different machines and different networks, for official stable and alpha releases. Packages also carry a project signature. Those are different checks.

**Object and date.** Tor Browser design document, master copy, read 9 October 2026. No current release signature directory was listed for this draft.

**Public source.** `Design-Documents/Tor-Browser-Design-Doc.md` in the Tor Project applications wiki.

**Result.** The document says official releases require matching builds from at least two release engineers using different build environments, defined there as different physical computers on different networks owned and administered by different entities. It also says the project signs and publishes the hash file. A project signature answers who published the package. Matching builds answer whether those builders produced the same hashes. This note does not treat the design text as a count of builders on a current release.

**Unresolved.** No current Tor Browser hash directory was listed for this draft.

### 3.4 Debian archive packages

**Claim.** Debian's testing migration can refuse a package that is not reproducible. The public dashboard reports a share of source packages, not a count of named builders on one file.

**Object and date.** The Reproducible Builds report for May 2026, and the Debian reproducibility dashboard, read 9 October 2026.

**Public source.** reproducible-builds.org monthly report, May 2026. `reproducible.debian.net` (forky/amd64).

**Result.**

- The May 2026 report says the Debian release team enabled migration software to block migration of new packages that cannot be reproduced, and of existing testing packages that regress.
- On the dashboard reading used here, forky/amd64 showed 37,807 of 39,496 source packages reproducible, reported as 95.7%. That is a package metric for that suite and architecture. It is not a signer count for one release file.
- An archive rule and a multi-signer hash file are different artifacts. Both can be checked. Neither is the other.

**Unresolved.** Dashboard percentages move. Recount the suite you mean. This note did not rebuild a Debian package.

### 3.5 A maintainer-signed release

**Claim.** A release signed by its maintainer is evidence that the signer published that file.

**Result.** The evidence is the signature and the published key. It is not evidence that someone else built the file, and it is not evidence about who read the diff, unless those artifacts are also published. A single-maintainer project can have a public diff and a signature. Those two checks still leave the rebuild check open.

## 4. What a number is evidence of

Figures below are a written rule or a listing read on 9 October 2026. None of them is a score.

| Figure | What was counted | Source | What it is not |
| --- | --- | --- | --- |
| 6 or more | Matching Guix builds named as the step before upload | `doc/release-process.md` | A count of reviewers |
| 3 | Matching signatures named before Windows and macOS release keys are applied | `doc/release-process.md` | The upload rule for every file |
| 21 | Signer directories under `guix.sigs/31.0` on 9 October 2026 | `bitcoin-core/guix.sigs` | The publish rule, and not a review count |
| 1 | OpenPGP signature on a kernel.org release tarball | `kernel.org/signature.html` | An independent rebuild of the binary you boot |
| 95.7% | 37,807 of 39,496 forky/amd64 source packages on the dashboard reading used here | `reproducible.debian.net` | A count of named builders on one file |

A larger number is a different artifact. Recount the folder or the dashboard before treating the figure as current.

## 5. Questions that make a claim reviewable

| Claim you may hear | Artifact to ask for | If it is missing |
| --- | --- | --- |
| "The source is open." | The tag or commit the file says it was built from | The license does not identify the file you downloaded |
| "It is signed." | The detached signature and the published key | A download button is not a signature |
| "Anyone can reproduce it." | A second builder's signature over the same hash, or a public rebuild log for that file | A build document is not a second build |
| "Several people signed the release." | The directory or signature file, and whether they signed the same hash | A contributor list is not a hash attestation |
| "The operating system is reproducible." | The record for the binary you boot, not only the upstream tarball | An upstream signature does not cover a later compilation |
| "This percentage is reproducible." | The suite, architecture, date, and whether the figure is packages or builders | A dashboard for one suite is not a release attestation |

## 6. Limits and source use

- Review volume is out of scope. The kernel tree is larger and older than the records in §3.1. This note does not compare those histories.
- A Guix signer did not necessarily review the commits in the tag. A reviewer did not necessarily rebuild the release.
- Signer directories include whoever filed an attestation for that version. Count the folder for the version you mean. Release candidates are separate folders.
- The binary a machine boots, a phone image, and a container build are downstream records.
- Quotes from `doc/release-process.md` are the sentences in that file on the master branch as read for this draft. If the file changes, the quote changes with it.
- No organization is assessed. A missing artifact means the claim is not shown by the record in this note.

## 7. References

- Bitcoin Core release process, `doc/release-process.md`, master branch. Publish step: "After 6 or more people have guix-built and their results match." Codesign step: "Once the Windows and macOS builds each have 3 matching signatures, they will be signed with their respective release keys." https://github.com/bitcoin/bitcoin/blob/master/doc/release-process.md
- Bitcoin Core Guix attestations. https://github.com/bitcoin-core/guix.sigs — `31.0` signer directories listed 9 October 2026.
- Linux kernel release signatures. https://kernel.org/signature.html
- Linux kernel reproducible builds. https://www.kernel.org/doc/html/latest/kbuild/reproducible-builds.html
- Reproducible Builds project, May 2026 report, Debian migration note. https://reproducible-builds.org/reports/2026-05/
- Debian reproducibility dashboard, forky/amd64, read 9 October 2026. https://reproducible.debian.net/
- Tor Browser design document. Official releases require matching builds from at least two release engineers on different machines and networks. https://gitlab.torproject.org/tpo/applications/wiki/-/blob/master/Design-Documents/Tor-Browser-Design-Doc.md

## Changelog

- **0.1 — 9 October 2026.** Peer review draft. Three checks. Public records for Bitcoin Core release binaries, kernel.org source archives, Tor Browser design notes, the Debian forky migration rule, and a maintainer-signed release. Figures dated to the listing that was read.
