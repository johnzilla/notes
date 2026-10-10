# What Can You Check About an embit Release?

*The release note's questions, put to one library*

This note applies [What Can You Check About a Software Release?](what-can-you-check-about-a-release.md) to one library. It records what the public artifacts show. It does not assess the library's code.

| Document | embit-release-records.md |
| :---- | :---- |
| **Version** | 0.3 — peer review draft |
| **Audience** | People who install embit or depend on a project that does: read [Start here](#start-here). Technical reviewers: read the full note. |
| **Method** | The method of the release note, version 0.3. Separate three checks — public, signed, rebuilt — and tie each claim to a public artifact. Record a figure with the listing it came from and the date that listing was read. |
| **Changes** | See the [changelog](#changelog). |
| **Scope** | The embit library as published: the file on the Python Package Index (PyPI), the release tags in the `diybitcoinhardware/embit` repository, and the project's written release process. This is not a review of the library's cryptography, a finding about any person, or a review of any wallet or device that uses the library. Signing-device firmware is out of scope. |

## Contents

- [Start here](#start-here)
- [1. What the note establishes](#1-what-the-note-establishes)
  - [1.1 Evidence](#11-evidence)
  - [1.2 Terms](#12-terms)
- [2. A library between two release processes](#2-a-library-between-two-release-processes)
- [3. Public records](#3-public-records)
  - [3.1 The 0.8.0 file on PyPI](#31-the-080-file-on-pypi)
  - [3.2 The 0.8.1 and 0.8.2 tags](#32-the-081-and-082-tags)
  - [3.3 The written release process](#33-the-written-release-process)
  - [3.4 The release run for 0.8.2](#34-the-release-run-for-082)
- [4. The three checks, by object](#4-the-three-checks-by-object)
- [5. Beside the four processes](#5-beside-the-four-processes)
- [6. Downstream copies](#6-downstream-copies)
- [7. Questions that make a claim reviewable](#7-questions-that-make-a-claim-reviewable)
- [8. Limits and source use](#8-limits-and-source-use)
- [9. References](#9-references)
- [Changelog](#changelog)

## Start here

This section is for anyone who installs embit, or who uses a wallet that includes it. You do not need to read code to use it. The rest of the note is the record behind each point.

**What embit is.** embit is a Bitcoin library written in Python. Other projects build on it, including software for signing devices. A person can install embit from PyPI, the public index of Python packages. A project can also copy embit straight from its source repository.

**Three things the public record shows.** Each one was read on 9 October 2026.

1. **The file on PyPI is version 0.8.0, published in May 2024.** PyPI has no newer version. See [§3.1](#31-the-080-file-on-pypi).
2. **Versions 0.8.1 and 0.8.2 exist as signed tags in the source repository.** A tag is a named point in the source history. Neither version is a file on PyPI. See [§3.2](#32-the-081-and-082-tags).
3. **The project wrote a new release process in version 0.8.1.** The process builds and publishes from an automated system, with checksums and build records. No run of that process has completed publishing yet. See [§3.3](#33-the-written-release-process) and [§3.4](#34-the-release-run-for-082).

The library is between two ways of publishing. The file on PyPI comes from the earlier way. The written rules describe the later way. Most of this note follows from that one fact.

**What you can check today.** The answer depends on how you get the library.

- **If you install from PyPI.** Compare the hash of the file you downloaded with the hash that PyPI lists. A hash is a short fingerprint of a file. A match shows that you received the file PyPI holds. It does not show who built the file.
- **If you copy from the source repository.** Verify the signature on the tag. A signature shows which key vouched for the tag. First decide why you believe that key belongs to the project.

**How to judge an answer.** A good answer shows you something you can open. A weak answer repeats the claim in different words. A weak answer does not prove a problem. It shows that the claim is not proved yet.

**What this note does not do.** It does not say whether embit is safe to use, and it does not compare embit with other libraries.

## 1. What the note establishes

A release claim needs a defined object. For embit on 9 October 2026 there were three: a file on PyPI, two later tags in the source repository, and a written process that describes future files. They are different objects, and a statement about one does not carry to another.

This note uses public pages, public repository contents, and the public record of one automated run. No account setting, private configuration, or private conversation was read. The note reports what those sources show on the date read. The project is changing its release process, so a later reading can differ.

### 1.1 Evidence

Each record in §3 has: **claim; object and date; public source; what was read; result; unresolved.** The fixed entries "Not applicable" and "Not read for this note" have the meanings in the [release note §1.1](what-can-you-check-about-a-release.md#11-evidence).

### 1.2 Terms

Terms from the [release note §1.2](what-can-you-check-about-a-release.md#12-terms) keep their meaning. This note adds the following.

| Term | Meaning here |
| :---- | :---- |
| **Package index** | A public service that stores release files for a programming language. PyPI is the package index for Python. |
| **Source distribution (sdist)** | A release file that holds a project's source and packaging data. |
| **Wheel** | A release file in a ready-to-install format. |
| **Lightweight tag** | A name for a commit. It has no signature of its own. The commit it names can still carry a signature. |
| **Annotated tag** | A tag stored as its own object, with a tagger and a date. It can carry a signature. |
| **Trusted Publisher** | A PyPI feature. An automated build system proves its identity to PyPI and receives a short-lived upload token, in place of a stored password or token. |
| **PyPI attestation** | A signed statement that binds a release file to a digest of its contents. PyPI accepts attestations from Trusted Publisher identities. |
| **Vendoring** | Copying a library's source into another project, in place of installing it from a package index. |

[PyPI Trusted Publishers](https://docs.pypi.org/trusted-publishers/), [PyPI attestations](https://docs.pypi.org/attestations/)

## 2. A library between two release processes

| Date | Event | Source |
| :---- | :---- | :---- |
| 30 May 2024 | Version 0.8.0 is tagged and uploaded to PyPI as one source distribution. The archive includes seven prebuilt `libsecp256k1` libraries. | §3.1 |
| 28 April 2026 | A commit titled "secp256k1: stop shipping prebuilt native binaries" removes those libraries from the source tree. | §3.2 |
| 2 June 2026 | Version 0.8.1 is tagged with a signed annotated tag. The same version adds a release document, a release workflow, a security policy, and a package content policy. | §3.2, §3.3 |
| 8 August 2026 | Version 0.8.2 is tagged with a signed annotated tag. The release workflow starts and is cancelled. | §3.2, §3.4 |
| 9 October 2026 | PyPI lists 0.8.0 as the latest version. | §3.1 |

Two groups of people get different objects.

- **People who install from PyPI** get the 0.8.0 file. That file predates the written process and the removal of the prebuilt libraries.
- **Projects that vendor from the repository** get a commit or a tag. One downstream project in §6 pins the commit behind tag `v0.8.2`.

The project's security policy says: "Security fixes are provided for the latest PyPI release line." On the date read, the changes in 0.8.1 and 0.8.2 were in the repository and not on PyPI.

## 3. Public records

### 3.1 The 0.8.0 file on PyPI

**Claim.** PyPI holds one file for embit 0.8.0, a source distribution. The file has a published hash. PyPI shows no attestation for it. The archive holds Python source and seven prebuilt libraries.

**Object and date.** `embit-0.8.0.tar.gz`, read 9 October 2026 through the PyPI project page, JSON API, simple index, and integrity API. The archive was downloaded and hashed the same day. Its regular files were compared with the generated source archive for commit `84cce66fb831fa6d625fb73f28e03605f3c04e28`, the target of tag `v0.8.0`.

**Public source.** `pypi.org/project/embit/`. `pypi.org/pypi/embit/json`. `pypi.org/simple/embit/`. The PyPI integrity API for the file. The `diybitcoinhardware/embit` repository.

**What was read.** The version list. The file record for 0.8.0. The integrity response. The archive member names. The SHA-256 of the downloaded file. The tag and commit for `v0.8.0`. The regular-file contents of the PyPI archive and the generated source archive for that commit, compared after removing each archive's top-level directory name.

**Result.**

- **What PyPI publishes.** One file for 0.8.0: a source distribution of 763,120 bytes, uploaded 2024-05-30T10:56:48Z. PyPI holds no wheel for this version. PyPI lists 0.8.0 as the latest version, and it lists no 0.8.1 or 0.8.2.
- **Hash.** The downloaded file hashed to `8bf4b10073c67400370ce523fb16f035fe759f6fdd987c579bdcc268d75ed770`. That value equals the SHA-256 digest in the JSON record and on the simple index. A downstream project's pin file records the same value (§6).
- **Signature.** The file record has no PGP signature. PyPI stopped accepting PGP signatures in May 2023: "PyPI has removed support for uploading PGP signatures with new releases." An upload in 2024 could not carry one. The absence is a property of the index at that date, not a choice made for this file. [PyPI: removing PGP](https://blog.pypi.org/posts/2023-05-23-removing-pgp/)
- **Attestation.** The integrity API returned 404, "No provenance available for embit-0.8.0.tar.gz". The project page answers "Uploaded using Trusted Publishing?" with "No".
- **Source history.** The git ref `v0.8.0` is a lightweight tag that names commit `84cce66`. The tag has no signature of its own. The commit carries a signature, and the hosting service marks it verified.
- **What is inside.** The archive has 76 members. Fifty are `.py` files. Seven are prebuilt `libsecp256k1` libraries under `src/embit/util/prebuilt/`, for macOS, Linux, and Windows on several processor types. A pure-Python secp256k1 module is also present.
- **Comparison with the tagged tree.** All 60 regular files shared by the PyPI archive and the generated source archive for commit `84cce66` match byte for byte. These include all seven prebuilt libraries. Seven regular files occur only in the PyPI archive: `PKG-INFO`, `setup.cfg`, and five files under `src/embit.egg-info/`. They are packaging metadata. The comparison covers file contents, not archive headers, file modes, timestamps, or a rebuild of the archive.
- **The prebuilt libraries are in the public tree.** The seven libraries match the public tree as finished files. That match does not identify the source or build inputs that produced them. No build record tying these files to a `libsecp256k1` commit was found in the repository evidence read.

A hash on an index page is a fingerprint. A matching hash shows that a downloader received the file the index holds. It does not identify who built the file.

**Unresolved.** The seven additional packaging files were not regenerated from the tagged source. The prebuilt libraries were not rebuilt, and they were not tied to a public `libsecp256k1` commit. The signature on commit `84cce66` was not verified outside the hosting service. No second builder's report was found for this file.

### 3.2 The 0.8.1 and 0.8.2 tags

**Claim.** Tags `v0.8.1` and `v0.8.2` are annotated and signed. Each names a commit in the public history. Neither tag has a release file on PyPI.

**Object and date.** The two tag objects and the commits they name, read at repository commit `2b375a3` on 9 October 2026. The release page for `v0.8.2`, read the same day.

**Public source.** The `diybitcoinhardware/embit` repository. The hosting service's tag verification record and release page.

**What was read.** The tag objects. The signature header of each named commit. The source tree at each tag. The submodule pin. The verification record. The release page.

**Result.**

- **Tags.** `v0.8.1` is an annotated tag dated 2026-06-02 that names commit `b5d694a`. `v0.8.2` is an annotated tag dated 2026-08-08T15:20:39Z that names commit `eb6104f`. One tagger made both tags. The signed tag-object IDs are `c6cb52b5a20868dfca132844e97d214e1a853a27` for `v0.8.1` and `a29a00c297e9a66d41bd3c2da06bd450aae7db1c` for `v0.8.2`. These identify the tag objects, separately from their target commits.
- **Signatures.** Both tag objects carry a PGP signature. The hosting service reports each signature as valid. The signature on `v0.8.2` names key fingerprint `F2DB C4C6 14C1 13E2 B15F 879A DD5C 1264 EBD6 45BE`.
- **Commits.** Commit `b5d694a` carries a signature. Commit `eb6104f` does not. For 0.8.0 the commit is signed and the tag is not. For 0.8.2 the tag is signed and the commit is not. In each case one signed object names the release point.
- **Public history.** Commit `eb6104f` is an ancestor of the default branch at commit `2b375a3`. This note checked that ancestry in a clone.
- **Prebuilt libraries.** The source trees at `v0.8.1` and `v0.8.2` hold no `.so`, `.dll`, or `.dylib` file. Commit `92e016b`, dated 2026-04-28, removed them. The 0.8.1 changelog entry reads: "Remove bundled native `libsecp256k1` binaries from package artifacts."
- **Submodule.** The repository pins `ElementsProject/secp256k1-zkp` at commit `d9560e0` as a submodule. The pin is the same at `v0.8.0` and at `v0.8.2`.
- **Release page.** The release page for `v0.8.2` lists two assets. Both are the source archives that the hosting service generates for every tag. The project attached no file of its own: no built package, no hash file, and no attestation.
- **Key retrieval and authentication.** This note read the key fingerprint from the signature and from the hosting service's verification record. The repository files read for this note do not publish a key fingerprint. The fingerprint was recorded; the public key was not retrieved or authenticated, and no local signature verification was performed.

A tag signature vouches for the tag and, through it, for a commit. It does not vouch for a file built from that commit. No project-built source distribution or wheel is published for these two tags in the records read. The generated source archives are separate release files; this note did not compare their contents with the signed tags.

**Unresolved.** No public key was retrieved, and no signature was verified locally. The note did not establish how a reader would tie the key to the project through a second channel. The automatically generated source archives were not compared with the tag.

### 3.3 The written release process

**Claim.** From version 0.8.1 the repository holds a written release process. The process calls for publishing only from an automated workflow, through a PyPI Trusted Publisher. The workflow generates checksums, a software bill of materials, and build attestations; their destinations differ. The process requires release files without bundled native libraries.

**Object and date.** `RELEASING.md`, `SECURITY.md`, `docs/package-content-policy.md`, and `.github/workflows/release.yml`, at repository commit `2b375a3`, read 9 October 2026.

**Public source.** The `diybitcoinhardware/embit` repository and the pinned publishing action in `pypa/gh-action-pypi-publish`.

**What was read.** The release document. The security policy. The package content policy. The release workflow file. The `attestations` input in `pypa/gh-action-pypi-publish/action.yml` at the workflow's pinned commit `cef221092ed1bacb1cc03d23a2d87d1d172e277b`, read 9 October 2026.

**Result.**

- **Where publishing happens.** The release document says: "This project publishes from GitHub Actions only. Do not run `twine upload` from local machines."
- **Stated rules.** The document lists four. Release tags "must reference commits already merged into the protected default branch." Artifacts "must be built once in the unprivileged build job, then reused in publish." Publishing "must use PyPI Trusted Publisher through the protected `pypi` environment." Release artifacts "must remain pure Python (no bundled native libraries)."
- **What the workflow implements.** A build job has read-only permission. It checks that the tag's commit is merged into the default branch, runs the tests, builds a source distribution and a wheel, verifies the package contents, and writes a hash file and a software bill of materials. A separate publish job runs in the `pypi` environment. It checks the hashes, creates a build provenance attestation for each file, and uploads through a publish action that is pinned to a commit.
- **Destinations.** The build job stores the distributions, checksums, software bill of materials, and inspection report together as an Actions artifact. The publish job uploads the distributions to PyPI. The workflow has no step that uploads files to the release page, although the release document expects checks against files there.
- **Two attestation paths.** The publish job calls `actions/attest-build-provenance` for the source distribution and wheel. Separately, the pinned PyPI publish action defaults its `attestations` input to `true`, and this workflow does not override it. PyPI attestations are therefore enabled for Trusted Publishing; they are distinct from the build-provenance steps. The cancelled run reached neither path.
- **Approval.** The document describes a checklist "Before Approving `pypi`". That wording describes a person who approves the publish job after reviewing the build output.
- **After publishing.** The document lists checks that compare the files on PyPI and on the release page with the hashes from the build.
- **Second builder.** The process has one builder, the automated workflow. It does not call for a second party to rebuild the files. In the terms of the release note, the process addresses the signed check and provenance. It does not address the rebuilt check.
- **Relation to the 0.8.0 file.** The rules date from version 0.8.1. The 0.8.0 file predates them. The rules describe files that the process will publish. They are not a description of the file that PyPI holds today.

A written process is evidence of what the project intends. The record of a completed run would be evidence of what was done. §3.4 covers the one run so far.

**Unresolved.** Three settings that the process relies on are not visible in the repository files: the protection rules on the default branch, the required reviewers on the `pypi` environment, and the Trusted Publisher configuration on PyPI. The note did not establish how release-page files would be published to satisfy the post-publish checklist.

### 3.4 The release run for 0.8.2

**Claim.** The release workflow has run once, for tag `v0.8.2`. A maintainer account cancelled the run before the build job finished. The run published no file.

**Object and date.** Release workflow run `31264239479`, attempt 1, for commit `eb6104fd85d3becabba628756cd5e1b75619f3a1`, read 9 October 2026 through its public run page and job records.

**Public source.** The workflow run list and the run page in the `diybitcoinhardware/embit` repository.

**What was read.** The run's trigger, conclusion, annotations, job results, and artifact list.

**Result.**

- **Trigger.** The push of tag `v0.8.2` started the run on 8 August 2026.
- **Conclusion.** The run's conclusion is "cancelled". The run page carries the annotation that a maintainer account cancelled it. The run lasted under one minute.
- **How far the run went.** The build job completed the checkout, the ancestry check, the tests, the build, and the package content check. The run was cancelled during the next step, “Smoke install from local artifacts.” The later steps did not run: the software bill of materials, the hash file, and the upload of the build output. The publish job did not start.
- **Output.** The run stored no artifact. No attestation was created. No file was uploaded to PyPI.

A cancelled run is a different fact from a failed check. The record shows that someone with access stopped the run. It does not show why.

**Unresolved.** The reason for the cancellation was not read. The note did not read whether a later release is planned through this workflow.

## 4. The three checks, by object

| Check | The 0.8.0 file on PyPI | Tags `v0.8.1` and `v0.8.2` |
| :---- | :---- | :---- |
| **Public** | The source history is public, and tag `v0.8.0` names a commit in it. The 60 shared regular files match the tagged tree, including seven prebuilt libraries; seven additional files are packaging metadata. The native libraries remain public as finished files without an established link to their source and build inputs. | The source history is public. Each tag names a commit that a reader can open. |
| **Signed** | No signature on the file; PyPI did not accept PGP signatures at the upload date. No PyPI attestation. The commit behind the tag carries a signature. | Each annotated tag carries a signature that the hosting service reports as valid. The fingerprint was recorded. Public-key retrieval, authentication, and local signature verification were not performed. |
| **Rebuilt** | No second builder's report was found. | Not applicable to the tag objects. No project-built source distribution or wheel was found. Generated source archives are separate files whose correspondence with the tags was not checked. |
| **What the downloader checks** | The file's hash against the hash that PyPI lists. That step uses no key. | The tag signature, after the reader has a reason to tie the key to the project. |

The public check shows that a change can be read. It does not show that anyone read it.

## 5. Beside the four processes

The rows below are the rows of the [release note §4.1](what-can-you-check-about-a-release.md#41-comparison-by-step). To compare, read each row here beside the same row there. A library that other projects copy is a different kind of object from a program that people download and run. Several rows are "Not applicable" for that reason.

| Step | embit, as read 9 October 2026 |
| :---- | :---- |
| **What the project publishes** | One source distribution on PyPI (0.8.0). Signed tags for 0.8.1 and 0.8.2, with generated source archives on the 0.8.2 release page. The written process describes a source distribution and a wheel. |
| **Where the release ties to source history** | Public tags. `v0.8.0` is a lightweight tag on a signed commit. `v0.8.1` and `v0.8.2` are signed annotated tags. |
| **Object that is signed** | For 0.8.0, the commit. For 0.8.1 and 0.8.2, the tag. No signature on a release file. |
| **Who signs** | The key named by the commit or tag signature. The note relies on the hosting service's verification record and does not authenticate the key holder. |
| **Where a reader retrieves keys** | Not read for this note. Only a fingerprint was recorded; no public key was retrieved. |
| **What the documents offer to authenticate a key** | No key fingerprint in the repository files read. |
| **Rebuild in the written process** | Not in the written process. The workflow is the one builder. |
| **What a failed rebuild blocks** | Not applicable. The written process has an approval step before publishing, and no rebuild step. |
| **How a builder is added** | Not applicable. |
| **Inputs that builders share** | Not applicable. |
| **What the downloader checks** | From PyPI: the file's hash against the index. From the repository: the tag signature. |
| **Other public records** | A release document, a security policy, and a package content policy. The workflow names a software bill of materials and build attestations; no completed run has produced them. |

In one respect embit's tags resemble the kernel record in the release note: a developer's signature names a point in the source history, and whoever compiles or copies that source later makes a new object with its own record.

## 6. Downstream copies

These entries show which object a downstream project names. They are not reviews of those projects.

- **A pin to the PyPI file.** SeedSigner's `requirements.txt` on the `dev` branch pins `embit==0.8.0`. The `seedsigner-os` build files record the SHA-256 `8bf4b100…5ed770` for `embit-0.8.0.tar.gz`, the same value as in §3.1. The same directory holds `verify-secp256k1-binary.sh`. The script checks that the expected ARM library filenames and binding modules are present, and that the pure-Python fallback and unexpected prebuilt entries are absent. The script does not hash, rebuild, or execute the native libraries. Its checks do not establish their build provenance.
- **A pin to a commit.** Krux includes embit as a git submodule at commit `eb6104f`, the commit behind tag `v0.8.2`. Its build files compile the `libsecp256k1` that embit's submodule pins and place the result where embit looks for it.

A pin to a hash shows which file a project named. A pin to a commit shows which source a project named. Neither pin signs anything, and neither covers the image that the downstream project builds afterwards. That image is a downstream build with its own record.

## 7. Questions that make a claim reviewable

The claims are from the [release note §6](what-can-you-check-about-a-release.md#6-questions-that-make-a-claim-reviewable).

| If the claim is | What the record shows | Artifact that would complete it |
| :---- | :---- | :---- |
| **The source is open** | A public repository. All 60 regular files shared by the 0.8.0 archive and tagged tree match, including seven native libraries; seven additional files are packaging metadata. | Evidence linking the bundled native libraries to public source and the build inputs that produced them. Matching finished binaries does not supply that link. |
| **It is signed** | A signed commit for 0.8.0. Signed tags for 0.8.1 and 0.8.2. | A signature or attestation on a release file, and a published way to authenticate the key |
| **Anyone can reproduce it** | A workflow that builds once | A second builder's signed report of the same hash for a published file |
| **We publish provenance** | Separate build-provenance steps and PyPI attestations enabled in the workflow. The cancelled run produced neither. | The attestation for a published file, and who issued it |
| **Releases follow the written process** | The process, and one cancelled run. The workflow has no release-page upload step. | A completed publishing run and evidence that the release-page files and post-publish checks satisfy the written process |
| **The artifacts are pure Python** | No `.so`, `.dll`, or `.dylib` files were found in the source trees at `v0.8.1` and `v0.8.2`. The 0.8.0 file on PyPI predates the rule and holds seven prebuilt libraries. | A published file built under the rule |
| **You can verify it yourself** | A hash comparison for the PyPI file. A tag signature for the repository. | A verification step for a release file that names a key |

A missing artifact means only that this note does not show the claim.

## 8. Limits and source use

- This note records release evidence. It does not assess the library's code, its cryptography, or its fitness for any use.
- No person is assessed. The note names roles: the tagger, a maintainer account, the commit author.
- The note reads one library on one date. The project changed its release process within the year before that date, and the record can change again. Read the sources for the date you need.
- A library is a different object from an application. The release note's comparison was written for programs that people download and run. Rows that do not fit a library are marked "Not applicable".
- Figures from PyPI are as read on 9 October 2026. For version 0.3, the JSON record was reread, and the archive was downloaded, hashed, and recounted. Shared regular-file contents were compared with the generated source archive for commit `84cce66`. This was a file comparison, not a reproducible build or local verification of the commit signature.
- Quotes from the repository are the text at commit `2b375a3`.
- Signing-device firmware and wallet images are out of scope. §6 names two downstream projects only to show which object each one pins.
- A missing artifact means only that this note does not show the claim.

## 9. References

Each source is listed once with the version or date consulted and the sections that cite it.

**embit**, at [diybitcoinhardware/embit commit 2b375a3](https://github.com/diybitcoinhardware/embit/tree/2b375a33bd8926caec7e53d7cfd41b165d196566).

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [RELEASING.md](https://github.com/diybitcoinhardware/embit/blob/2b375a33bd8926caec7e53d7cfd41b165d196566/RELEASING.md) | The written release process | §3.3, §7 |
| [SECURITY.md](https://github.com/diybitcoinhardware/embit/blob/2b375a33bd8926caec7e53d7cfd41b165d196566/SECURITY.md) | Supported versions and release integrity notes | §2, §3.3 |
| [docs/package-content-policy.md](https://github.com/diybitcoinhardware/embit/blob/2b375a33bd8926caec7e53d7cfd41b165d196566/docs/package-content-policy.md) | What a published file may contain | §3.3 |
| [.github/workflows/release.yml](https://github.com/diybitcoinhardware/embit/blob/2b375a33bd8926caec7e53d7cfd41b165d196566/.github/workflows/release.yml) | The release workflow | §3.3, §3.4 |
| [CHANGELOG.md](https://github.com/diybitcoinhardware/embit/blob/2b375a33bd8926caec7e53d7cfd41b165d196566/CHANGELOG.md) | Changes in 0.8.1 and 0.8.2 | §2, §3.2 |
| [Source tree for v0.8.0](https://github.com/diybitcoinhardware/embit/tree/84cce66fb831fa6d625fb73f28e03605f3c04e28) | Commit `84cce66` and its source tree | §3.1 |
| [Source tree for v0.8.1](https://github.com/diybitcoinhardware/embit/tree/b5d694a79790c502f7725781332aa33d26a5486d) | Commit `b5d694a` and its source tree | §3.2 |
| [Source tree for v0.8.2](https://github.com/diybitcoinhardware/embit/tree/eb6104fd85d3becabba628756cd5e1b75619f3a1) | Commit `eb6104f` and its source tree | §3.2, §6 |
| [Signed tag object for v0.8.1](https://api.github.com/repos/diybitcoinhardware/embit/git/tags/c6cb52b5a20868dfca132844e97d214e1a853a27) | Tag object, target commit, signature, and hosting-service verification | §3.2 |
| [Signed tag object for v0.8.2](https://api.github.com/repos/diybitcoinhardware/embit/git/tags/a29a00c297e9a66d41bd3c2da06bd450aae7db1c) | Tag object, target commit, signature, and hosting-service verification | §3.2 |
| [Generated source archive, commit 84cce66](https://codeload.github.com/diybitcoinhardware/embit/tar.gz/84cce66fb831fa6d625fb73f28e03605f3c04e28) | Tagged-tree regular files used in the archive comparison | §3.1 |
| [Commit 92e016b](https://github.com/diybitcoinhardware/embit/commit/92e016b4d5a6ec329d2fc23dfdebfea7b8061258) | Removal of the prebuilt libraries | §2, §3.2 |
| [Release page for v0.8.2](https://github.com/diybitcoinhardware/embit/releases/tag/v0.8.2) | Assets and tag verification, read 9 October 2026 | §3.2 |
| [Release workflow runs](https://github.com/diybitcoinhardware/embit/actions/workflows/release.yml) | The one run, read 9 October 2026 | §3.4 |
| [Release run 31264239479, attempt 1](https://github.com/diybitcoinhardware/embit/actions/runs/31264239479/attempts/1) | The cancelled run for commit `eb6104f`, read 9 October 2026 | §3.4 |
| [Release run job records](https://api.github.com/repos/diybitcoinhardware/embit/actions/runs/31264239479/attempts/1/jobs) | Step conclusions for attempt 1, read 9 October 2026 | §3.4 |

**Publishing action**, at commit `cef221092ed1bacb1cc03d23a2d87d1d172e277b`, read 9 October 2026.

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [pypa/gh-action-pypi-publish/action.yml](https://github.com/pypa/gh-action-pypi-publish/blob/cef221092ed1bacb1cc03d23a2d87d1d172e277b/action.yml) | The enabled-by-default PyPI attestation input | §3.3 |

**PyPI**, read 9 October 2026. The project listings can change; the archive and integrity endpoint below name version 0.8.0.

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [embit project page](https://pypi.org/project/embit/) | Versions, upload date, and the Trusted Publishing answer | §3.1 |
| [embit JSON record](https://pypi.org/pypi/embit/json) | The file record and digest | §3.1 |
| [embit 0.8.0 source distribution](https://files.pythonhosted.org/packages/83/88/b054b00ade6d2a41749e15976cdcec4b7ec4656ac1cb917ce3de395528d1/embit-0.8.0.tar.gz) | The archive hashed, counted, and compared | §3.1 |
| [embit 0.8.0 integrity record](https://pypi.org/integrity/embit/0.8.0/embit-0.8.0.tar.gz/provenance) | The response reporting no provenance for this file | §3.1 |
| [embit simple index](https://pypi.org/simple/embit/) | The file link and digest | §3.1 |
| [Removing PGP from PyPI, 23 May 2023](https://blog.pypi.org/posts/2023-05-23-removing-pgp/) | The end of PGP signature uploads | §3.1 |
| [Trusted Publishers](https://docs.pypi.org/trusted-publishers/) | The Trusted Publisher feature | §1.2 |
| [Attestations](https://docs.pypi.org/attestations/) | PyPI attestations | §1.2 |

**Downstream projects**

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [SeedSigner requirements.txt, commit b2199c6](https://github.com/SeedSigner/seedsigner/blob/b2199c68b475eab0035b863b156f149757a1b4a6/requirements.txt) | The pin to embit 0.8.0 | §6 |
| [seedsigner-os python-embit, commit c4cb767](https://github.com/SeedSigner/seedsigner-os/tree/c4cb767a00ace8edf5e9414450c2849e15958ad7/opt/external-packages/python-embit) | The hash pin and the library check script | §3.1, §6 |
| [verify-secp256k1-binary.sh, commit c4cb767](https://github.com/SeedSigner/seedsigner-os/blob/c4cb767a00ace8edf5e9414450c2849e15958ad7/opt/external-packages/python-embit/verify-secp256k1-binary.sh) | Filename, module-presence, and fallback-absence checks | §6 |
| [Krux, commit 9b3a431](https://github.com/selfcustody/krux/tree/9b3a4314a205767606b1ad9fbd1da06ad908f38e) | The submodule pin and the library build task | §6 |

## Changelog

Newest first. Versioning follows [STYLE.md](STYLE.md). This note uses the tag prefix `embit-v` because the repository's unprefixed version tags already identify the custody note.

| Version | Change |
| :---- | :---- |
| 0.3 — 9 October 2026 | [Content commit](https://github.com/johnzilla/notes/commit/f37d48000029f72cc52c76d0098ff3a9729e85b1). Records the byte-for-byte match of all 60 regular files shared by the 0.8.0 PyPI archive and tagged tree, including the seven native libraries, and identifies seven additional packaging files (§3.1). Keeps native-library build provenance and regeneration of packaging metadata unresolved. Corrects fingerprint retrieval versus public-key retrieval and local verification (§3.2, §4, §5). Records both signed tag-object IDs and distinguishes generated source archives from project-built distributions. Separates workflow output destinations, records the missing release-page upload step, and confirms that the pinned publishing action enables PyPI attestations separately from the build-provenance steps (§3.3). Pins the cancelled run and attempt, names its cancelled smoke-install step (§3.4), and narrows the downstream script's checks to what it performs (§6). Updates the claims table, source-use limits, references, and README. Version tag: `embit-v0.3`. |
| 0.2 — 9 October 2026 | Restructured to match the other notes: Start here, contents, numbered records, references pinned to commits. Reframed around the change between two release processes (§2). Removes the names of people. Adds that the commit behind tag `v0.8.0` is signed; that PyPI did not accept PGP signatures at the upload date; that the prebuilt libraries are in the public source tree at `v0.8.0` and were removed by a recorded commit before 0.8.1; that the release page lists two generated source archives; and that a maintainer account cancelled the release run. Closes three unresolved items: the ancestry of the 0.8.2 commit, the submodule commit, and whether 0.8.0 was uploaded through a Trusted Publisher. Adds the security policy and the package content policy as sources. Records that one downstream project pins the 0.8.2 commit and that another checks the library in its built image. Removes the table of counts. |
| 0.1 — 9 October 2026 | First draft, read against release note 0.3. Records for the PyPI file and the later tag, the six questions, a table of counts, a comparison column, downstream pointers, and a claims table. |
