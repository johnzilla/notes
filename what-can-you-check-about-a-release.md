# What Can You Check About a Software Release?

*Questions to ask about a published file*

This note compares release processes. It does not rank them.

| Document | what-can-you-check-about-a-release.md |
| :---- | :---- |
| **Version** | 0.3 — peer review draft |
| **Audience** | People deciding what a download page is evidence of: read [Start here](#start-here). Technical reviewers: read the full note. |
| **Method** | Separate three checks — public, signed, rebuilt — and tie each claim to a public artifact. Put the same questions to each process. Record a number with the listing or rule it came from, and with the date that listing was read. |
| **Changes** | See the [changelog](#changelog). |
| **Scope** | Public release records for software: source archives and distributed binaries. The Bitcoin project covered is Bitcoin Core. Three other well-known processes are read beside it: the Linux kernel, Tor Browser, and the Debian archive. This is not a finding about any organization, a threat model for any project, a review-hour comparison, or a survey of Bitcoin software. Signing-device firmware is out of scope. |

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
  - [3.5 Debian test dashboard](#35-debian-test-dashboard)
- [4. The processes side by side](#4-the-processes-side-by-side)
  - [4.1 Comparison by step](#41-comparison-by-step)
  - [4.2 What a number is evidence of](#42-what-a-number-is-evidence-of)
- [5. Provenance records](#5-provenance-records)
  - [5.1 Kinds of record](#51-kinds-of-record)
  - [5.2 How the records map to the checks](#52-how-the-records-map-to-the-checks)
- [6. Questions that make a claim reviewable](#6-questions-that-make-a-claim-reviewable)
- [7. Limits and source use](#7-limits-and-source-use)
- [8. References](#8-references)
- [Changelog](#changelog)

## Start here

This section is for anyone looking at a download page. You do not need to build the software to use it. The rest of the note is the record behind each question.

**What a release is.** A release is a file that someone published. The file can be source code, a program ready to run, or both. A license can say that the source is open. The license does not tell you who read a change, who signed the file, or who else built the same file. Those are separate questions, and each one has its own evidence.

**Four words you will see.** A *hash* is a short fingerprint of a file. If one byte of the file changes, the hash changes. A *signature* is a mark that only the holder of a secret key can make. A signature shows that the key holder vouches for a file. A *diff* is the list of lines that a change adds and removes. To *rebuild* is to make the program again from its source and compare the hash of the result.

**The questions that matter.** Ask these questions about a release that you did not build. Ask them about a release that you did build, if you ask other people to run it.

1. **Was the change public?** Find out if someone other than the author can read the diff that went into the file. A public repository or a mailed patch is evidence you can open. A change that appears only as a finished program is not that evidence. See [§2](#2-three-checks).
2. **Who signed the file, and why do you believe the key is theirs?** A signature tells you which key vouched for the file. A key is only a file until you tie it to a person or a project. Ask what ties the key to the signer you expect. A fingerprint that you confirmed through a second channel is one answer. See [§2](#2-three-checks) and [§4.1](#41-comparison-by-step).
3. **Did someone else rebuild it?** A second person can build from the same source and sign a report of the hash they got. A matching report is evidence about the build. The report is that person's statement, so check the signature on it. One signature on the file does not give you that evidence. See [§3](#3-public-records).
4. **What object was signed?** A signature on a source archive vouches for the archive. A signature on a program vouches for the program. A company that builds the source later makes a new file with its own record. See [§4.1](#41-comparison-by-step).
5. **What is the number counting?** A written rule can require a count before a file is published. A folder can show who vouched for one version. A dashboard can show a share of packages. Those numbers count different things. Count again the one you mean. See [§4.2](#42-what-a-number-is-evidence-of).
6. **What do you check yourself?** Each process gives the person who downloads a different step to do. Ask what that step is and which keys it relies on. See [§4.1](#41-comparison-by-step).

**How to judge an answer.** A good answer shows you something you can open: a document, a signature file, a folder of signed hash files, a dashboard. A weak answer repeats the claim in different words. A weak answer does not prove a problem. It shows that the claim is not proved yet. [§6](#6-questions-that-make-a-claim-reviewable) lists more claims and the artifact to ask for.

**Why the processes differ.** The projects publish different things. One publishes programs for several systems. One publishes source only. One publishes tens of thousands of packages. A step that fits one of them can have no meaning for another. A blank in the comparison is often a difference in what is published, not a missing step.

**What this note does not do.** It does not rank projects, and it does not compare how much review each project does. The right weight for each check depends on what you need the file to be evidence of.

## 1. What the note establishes

A release claim needs a defined object: a tag, a source archive, a binary, a hash file, or a package in an archive. "Open source," "signed," and "reproducible" describe different properties. None follows from a license name or from a download button.

This note uses public documents and public listings. No private build machine, key ceremony, or unpublished configuration was inspected. Each consequence below is tied to the artifact named in the same paragraph.

The note puts the same questions to four processes. The comparison shows where the processes make different decisions. The four projects publish different objects, serve different readers, and work at different scales.

### 1.1 Evidence

Each record in §3 has: **claim; object and date; public source; what was read; result; unresolved.** A date is the date the listing was read, not a claim that the listing is stable.

The comparison in §4.1 uses two fixed entries beside the facts:

| Entry | Meaning |
| :---- | :---- |
| **Not applicable** | The step does not concern what this project publishes. Example: a rebuild of a binary, where the project publishes source only. |
| **Not read for this note** | The step may exist. No public document for it was read for this version. The entry is a limit of the note, not a statement about the project. |

### 1.2 Terms

| Term | Meaning here |
| :---- | :---- |
| **Release file** | The file a person downloads: a source archive, a binary, or a package. |
| **Public** | The change is in a place where someone other than the author can read it. This check does not establish that anyone did read it, and the note does not grade review. |
| **Signed** | A published key vouches for a named file. A signature identifies the key that signed. It does not identify the key's holder; that needs key authentication (see below). It does not establish that the file matches the source unless a separate check says so. |
| **Rebuilt** | A builder other than the publisher started from the stated source and produced the same hash. The public evidence is the builder's signed report of that result. A verified signature authenticates the report. It does not show, by itself, that a separate build took place. Matching reports are evidence about the build. They are not evidence about who read the commits. |
| **Key retrieval and key authentication** | Retrieval is where a reader gets a key. Authentication is the reason a reader ties that key to the expected signer. They are separate steps. |
| **Publisher** | The party that uploads the release file. |
| **Builder** | A person or machine that compiles the stated source and records the hashes of the output. |
| **Attestation** | A signed statement about a file. In §3.1 an attestation is a hash file with a builder's signature. |
| **Hash file** | A file that lists release files with the hash of each one. A signature on a hash file covers every file listed in it. |
| **Publish gate** | A written step that must be done before the file is uploaded. A document that calls a practice desirable is not a gate. A written gate is evidence of the stated process, not proof that each release followed it. |
| **Distributor** | A party that compiles another project's source and publishes the result. That later compilation is a downstream build, and it has its own record. |
| **Provenance record** | A statement about how a file was produced: the source, the builder, and the inputs. See [§5](#5-provenance-records). |
| **Specification** | A document that says how software should behave. A specification is not a release file. See [§7](#7-limits-and-source-use). |

## 2. Three checks

| Check | Question | Artifact that answers it | What it does not answer |
| :---- | :---- | :---- | :---- |
| **Public** | Can someone other than the author read the diff? | A public repository history, a public pull request, or a mailed patch, tied to the tag or commit that the release names | Whether anyone read the diff, and whether the binary matches it |
| **Signed** | Which key vouched for the file you downloaded? | A signature, the signed object, and a reason to tie the key to the expected signer | Whether anyone else built the file |
| **Rebuilt** | Did another builder produce the same hash from the stated source? | A second builder's signed report of the same hash, with the signature verified against that builder's key | Whether a separate build took place beyond the builder's report, whether that builder read the commits, and whether the builders shared inputs |

A release can pass one check and not the others. Record which object you checked.

Five points apply to every record in §3.

- **A signed tag is a signature, not a review.** A signed tag shows which key named a commit as the release. The tag belongs to the signed check. The public check needs the history behind the tag.
- **One signature can still be a complete record.** A release with a public history and one maintainer signature has two of the three checks. The rebuilt check stays open until a second builder publishes a matching hash. For a project that publishes source only, the rebuilt check is not applicable to the release file.
- **A matching signature is a report.** A builder's signed hash file says: this key reports these hashes for this version. Verifying the signature authenticates the report. It does not show how the builder got the hashes. This note gives a provenance record the same treatment in [§5](#5-provenance-records): the record is its issuer's claim. Several reports from builders with separately authenticated keys are stronger evidence than one. They remain reports.
- **Getting a key is not the same as authenticating it.** A key from a second website is not independent if the same party controls both sites. A key from the download page is useful if the reader confirmed its fingerprint elsewhere first. The question is why the reader ties the key to the expected signer. Cross-signatures from known keys, a fingerprint confirmed in person or through a second channel, and a key carried over from an earlier verified release are possible answers.
- **Matching builders can share inputs.** Two builders who use the same prebuilt compiler packages have less independence than two builders who each build the compiler. A count of builders does not record that difference. Ask which inputs the builders had in common.

The custody note in this repository lists four levels of build evidence: published source; a build reproducible from that source; independent parties who reproduced and attested the released binary; and evidence that a device runs that binary. See [Who Can Move Your Bitcoin? §3](who-can-move-your-bitcoin.md#3-review-the-complete-custody-lifecycle). The three checks here cover the first three levels. The fourth level, what a machine actually runs, is outside this note.

## 3. Public records

### 3.1 Bitcoin Core release binaries

**Claim.** The release process calls for upload of the release binaries after independent Guix builds match. The matching results are published as signed hash files.

**Object and date.** The release process document, the Guix build document, and the binary verification document, at repository commit `4bacf21`, read 9 October 2026. The `31.0` attestation folder at `guix.sigs` commit `f6c609b`, listed 9 October 2026.

**Public source.** `doc/release-process.md`, `contrib/guix/README.md`, and `contrib/verify-binaries/README.md` in the Bitcoin Core repository. Attestations and builder keys in `bitcoin-core/guix.sigs`.

**What was read.** The tagging step, the build and attest steps, the codesign step, and the publish step. The builder directories under `guix.sigs/31.0`, and each hash file in them. The key directory. The verification instructions.

**Result.**

- **Tag.** The tagging script performs consistency checks and then creates a signed tag. Builders fetch and check out that tag.
- **Two build stages.** The first stage (`noncodesigned`) compiles from the tag. Windows and macOS code signatures are then made from those outputs. In the second stage (`all`), builders attach the code signatures. The `all` hash file "covers all the binaries uploaded to the website, and is what to check release binaries against."
- **Code signing.** The codesign step says: "Once the Windows and macOS builds each have 3 matching signatures, they will be signed with their respective release keys." A platform code signature is a separate signature from a builder's attestation. A holder of a release key makes it.
- **Publish gate.** The publish heading is: "After 6 or more people have guix-built and their results match." The next step combines the `all.SHA256SUMS.asc` file from every builder into one `SHA256SUMS.asc`. The publisher uploads the binaries, the `SHA256SUMS` file, and that combined signature file.
- **Builders.** The repository files attestations as `guix.sigs/<version>/<signer>/`. A builder adds a key by pull request to `builder-keys`. The documents read do not make commit access to the source repository a requirement for adding a key. On 9 October 2026 the key directory held 39 key files.
- **Shared inputs.** The Guix document says builders "can decide whether or not to use **substitutes** (pre-built packages)." An attestation does not record which choice a builder made.
- **The listing for 31.0.** On 9 October 2026 the `31.0` folder held 21 builder directories. All 21 held a `noncodesigned.SHA256SUMS` file, and the 21 files were identical. 19 of the 21 held an `all.SHA256SUMS` file, and the 19 files were identical. Each `all` file listed 28 release files.
- **What the downloader checks.** The verification document says: "you (the end user) decide which of these public keys you trust." The downloader checks the signatures on the hash file against the chosen keys, then compares the hash of the download with the hash file. A script, `verify.py`, does both steps. The downloader can set the trusted keys and a minimum number of good signatures.
- **Timestamp.** The release process says the server "will automatically create an OpenTimestamps file and torrent of the directory." See [§5](#5-provenance-records).

The 21 and the 19 are listings of who filed an attestation for one tag. Identical files show the same reported hashes. This note did not verify the signatures, so it does not show which keys made the reports. Neither number is a count of people who read the commits in the tag. The written gate is 6 or more matching builds.

**Unresolved.** No Guix build was rerun for this note. No PGP signature in the folder was verified; the hash files were compared as files. The note did not establish which builders used substitutes. A later release will have a different folder.

### 3.2 Linux kernel source archive

**Claim.** The developer who makes a kernel release signs the source archive. The project publishes source. The binary a machine boots is a later build by someone else.

**Object and date.** The signature instructions, the maintainer PGP guide, and the reproducible-builds document, read 9 October 2026.

**Public source.** `kernel.org/signature.html`. `Documentation/process/maintainer-pgp-guide.rst` and `Documentation/kbuild/reproducible-builds.rst` in the kernel tree, as published at `kernel.org/doc/html/latest`.

**What was read.** The verification instructions, the key instructions, the checksum-file description, the guidance on signed tags, and the reproducible-builds document.

**Result.**

- **Developer signature.** The page says: "Every kernel release comes with a cryptographic signature from the person making the release." It also says: "the signature is made against the uncompressed version of the archive." One signature therefore covers each compressed form of the same archive.
- **Checksum file.** A separate system produces a `sha256sums.asc` file and signs it "with a PGP key generated for this purpose." The page says: "These checksums are NOT intended to replace developer signatures." It describes the file as a quick way to check content on a mirror.
- **Tags.** The maintainer guide says: "git repositories provide PGP signatures on all tags." A reader can therefore check the signed tag as well as the signed archive.
- **Keys.** kernel.org publishes developer keys through a Web Key Directory and through a git repository of keys. Other developers cross-sign the keys. The signature page lists key fingerprints for four developers who commonly make releases.
- **Stated trust model.** The maintainer guide says trust "must always be placed with developers and never with the code hosting infrastructure."
- **Rebuilds.** The release file is source. A rebuild of a binary is not applicable to that file. The kernel's reproducible-builds document tells people who build kernel binaries which settings they must control, such as the build timestamp, user, and host. It is not written as a step before a source release.
- **Downstream builds.** A distributor compiles a kernel after the source release. That compilation is a separate record. A signature on the upstream archive does not cover the distributor's binary.
- **What the downloader checks.** The downloader gets the developer's key, uncompresses the archive, and verifies the signature against the uncompressed file. The page links a script that automates those steps.

**Unresolved.** No particular archive or tag was verified for this note. No distributor kernel was surveyed.

### 3.3 Tor Browser packages

**Claim.** The project's key signs each package. The release directory also holds hash files for the build, with further signatures on them. A project signature and matching builds are different checks.

**Object and date.** The signature verification page, read 9 October 2026. The `15.0.24` release directory, listed 9 October 2026. The Tor Browser design document, as read for version 0.1 of this note.

**Public source.** `support.torproject.org/tbb/how-to-verify-signature/`. `dist.torproject.org/torbrowser/15.0.24/`. `Design-Documents/Tor-Browser-Design-Doc.md` in the Tor Project applications wiki.

**What was read.** The verification instructions. The file names in the release directory. For the design document, see Unresolved.

**Result.**

- **Package signature.** The verification page says: "The Tor Browser team signs Tor Browser releases." It says each download comes with a signature file "with the same name as the package." The signed object is the package file.
- **Keys.** The page fetches the project key through a Web Key Directory. It says the key "is also available on keys.openpgp.org." It publishes the key fingerprint.
- **Hash files.** The `15.0.24` directory holds two hash files: `sha256sums-unsigned-build.txt` and `sha256sums-signed-build.txt`. Each has an `.asc` signature file.
- **Further signatures.** The unsigned-build hash file has two more signature files. Each of those file names ends in a different builder name. The listing therefore shows three signature files on that one hash file.
- **Design text.** As read for version 0.1, the design document says official releases require matching builds from at least two release engineers. It defines the build environments as different physical computers, on different networks, owned and administered by different entities.
- **What the downloader checks.** The downloader fetches the project key and verifies the signature file against the package.

A project signature answers who published the package. Signatures from builders on one hash file answer whether those builders report the same hashes. The design text states a requirement. The directory listing shows files for one release.

**Unresolved.** The note did not read where the project ties a release to its source history. No signature in the directory was verified for this note. The note did not establish which key made each signature, or whether the two named builders meet the design document's definition of different environments. The design document link returned a sign-in page on 9 October 2026, so its text was not reread for this version. The documents read do not say how a builder is added or which inputs builders share.

### 3.4 Debian archive packages

**Claim.** Debian's migration software can block a package from the *testing* suite when a rebuild service cannot reproduce the package. That service rebuilds the binary packages that Debian distributes.

**Object and date.** The Reproducible Builds report for May 2026, the rebuild service's front page, and the `apt-secure` manual page, read 9 October 2026.

**Public source.** `reproducible-builds.org/reports/2026-05/`. `reproduce.debian.net`. `apt-secure(8)` in Debian trixie.

**What was read.** The report's passage on the release team's announcement. The rebuild service's description of its method. The manual page's description of archive signatures.

**Result.**

- **Rule.** The report quotes the Debian Release Team: "we've decided it's time to say that **Debian must ship reproducible packages**." It continues: "we have enabled our migration software to block migration of new packages that can't be reproduced." The same rule covers "existing packages in *testing* that regress in reproducibility." The report's inserted note names `reproduce.debian.net` as the place where reproduction is tested.
- **What the service rebuilds.** The service says it "attempts to bit-for-bit identically rebuild each Debian binary package found in the distribution archive." The object is the binary that Debian distributes, not a test build.
- **Timing.** The rebuild follows publication to the archive. The rule acts on movement between suites. Compare §3.1, where matching builds come before upload.
- **Shared inputs.** The service uses the `.buildinfo` file from the original build. It says: "The goal is to replicate the same build process that is used by Debian during package publication." The rebuild therefore copies the original inputs on purpose. The question it answers is whether the same inputs give the same binary.
- **Builders.** The service asks for "independent rebuilders" in "a *diverse* variety of setups and settings." The front page lists the hosts that do the rebuilds. It does not name an operator for each host.
- **Signatures.** The manual page describes a chain. Package checksums go into a `Packages` file. Checksums of the `Packages` files go into a `Release` file. "The Release file is then signed by the archive key." The page also says: "apt-secure does not review signatures at a package level." The signed object is the archive index, not each package.
- **What the downloader checks.** The package tool checks the signature on the `Release` file and follows the checksum chain to the package.

**Unresolved.** The note did not read where the archive ties a binary package to its source history. The announcement itself was read only as the report quotes it. The front page of the rebuild service did not show figures in the form read, so this record has no count. The note did not read which architectures the migration rule covers, what exceptions exist, or where archive keys are published. No Debian package was rebuilt.

### 3.5 Debian test dashboard

**Claim.** A public dashboard reports the share of Debian packages that build to the same result twice in a test system. That figure is a package metric for one suite and architecture.

**Object and date.** The forky/amd64 statistics page and the variations page, read 9 October 2026.

**Public source.** `tests.reproducible-builds.org/debian/`.

**What was read.** The summary line for forky/amd64. The table of what differs between the two builds.

**Result.**

- **Figure.** On 9 October 2026 the page said: "37827 packages (95.8%) successfully built reproducibly in forky/amd64."
- **Method.** The test system builds each package twice and compares the two results. It changes the environment between the builds on purpose. The variations include the hostname, the user, the time zone, the locale, and the shell.
- **What the figure counts.** The figure counts packages for which two test builds matched. Both builds come from one test system. The figure is not a rebuild of the binary in the archive, and it is not a count of builders on one file.
- **Relation to §3.4.** The dashboard and the rebuild service are different systems. The report in §3.4 ties the migration rule to the rebuild service, not to this dashboard.

**Unresolved.** Dashboard figures move. Count again the suite you mean. The note did not read how often each package is retested.

## 4. The processes side by side

### 4.1 Comparison by step

Each row puts one question to all four processes. Read down a column to see one process. Read across a row to see a decision made four ways. The entries come from §3. "Not applicable" and "Not read for this note" have the meanings in [§1.1](#11-evidence). A cell is a summary. The Unresolved line of each record in §3 states what the note did not check, including every signature named in this table.

| Step | Bitcoin Core | Linux kernel | Tor Browser | Debian archive |
| :---- | :---- | :---- | :---- | :---- |
| **What the project publishes** | Binaries for several systems, built from a signed tag | A source archive | Binary packages for several systems | Binary packages in a distribution archive |
| **Where the release ties to source history** | A signed tag, `v<version>`, in the public repository. Builders check out that tag. | Signed tags in the public git repositories | Not read for this note | Not read for this note |
| **Object that is signed** | A hash file that lists every release file | The uncompressed source archive. Separately, a checksum file. Tags are also signed. | Each package file. Hash files for the build are also signed. | The archive's `Release` file, which leads by checksums to each package |
| **Who signs** | Each builder who filed an attestation. Release-key holders add platform code signatures for Windows and macOS. | The developer who makes the release. A dedicated key on a separate system signs the checksum file. | The project's team key. Two further signature files on a hash file carry builder names. | The archive key |
| **Where a reader retrieves keys** | The `builder-keys` directory in the attestation repository | A Web Key Directory and a git repository of keys, with cross-signatures between developers | A Web Key Directory and a public key server | Not read for this note |
| **What the documents offer to authenticate a key** | The key directory in the attestation repository. The downloader chooses which builder keys to trust. | Cross-signatures between developers, and fingerprints on the signature page | A fingerprint on the verification page | Not read for this note |
| **Rebuild in the written process** | Yes, before upload. The gate is 6 or more matching builds. | Not applicable. The release file is source. | Yes, in the design text as read for version 0.1: matching builds from at least two release engineers. The text was not reread for this version. | Yes, after publication. A rebuild service rebuilds the distributed binary. |
| **What a failed rebuild blocks** | Upload, under the written gate | Not applicable | Not read for this note | Migration of the package to *testing*, as quoted in a report. The rule's exceptions and architecture scope were not read. |
| **How a builder is added** | A pull request that adds a key | Not applicable | Not read for this note | The rebuild service asks for independent rebuilders |
| **Inputs that builders share** | The Guix build definition. Each builder chooses prebuilt packages or a build from source. | Not applicable | Not read for this note | The original build's inputs, copied on purpose from the `.buildinfo` file |
| **What the downloader checks** | Signatures on the hash file against chosen keys, then the hash of the download | The developer signature against the uncompressed archive | The team signature against the package | The `Release` signature and the checksum chain, done by the package tool |
| **Other public records** | A timestamp file for the release directory | Not read for this note | Not read for this note | A `.buildinfo` file for each distributed binary package |

Four differences explain most of the table.

- **Source or binary.** The kernel release is source. No binary exists at that point for a second builder to match. The rebuild question moves to whoever compiles the source later.
- **One program or an archive.** Bitcoin Core and Tor Browser each publish one program for several systems. A small set of builders can rebuild all of it for each release. Debian publishes tens of thousands of packages, so its rebuild is a service and its figure is a share of packages.
- **Before or after.** Bitcoin Core's written gate puts the rebuild before upload. Debian's rule acts after the first publication, at the move between suites.
- **Many keys or one.** Bitcoin Core publishes many builder signatures and leaves the choice of keys to the downloader. The kernel and Tor Browser publish a signature from a developer key or a team key. Debian signs an index with an archive key.

Each difference follows from what the project publishes and for whom.

### 4.2 What a number is evidence of

Each figure below is a written rule or a listing read on 9 October 2026. The rows count different things, so no row is comparable with another row.

| Figure | What was counted | Source | What it is not |
| :---- | :---- | :---- | :---- |
| 6 or more | Matching Guix builds named in the heading of the publish step | Bitcoin Core `doc/release-process.md` | A count of reviewers, or a count for any one release |
| 3 | Matching signatures named before the Windows and macOS release keys sign | Bitcoin Core `doc/release-process.md` | The publish gate |
| 21 | Builder directories under `guix.sigs/31.0`, each with a first-stage hash file | `bitcoin-core/guix.sigs` | The publish gate, or a review count |
| 19 | Builder directories under `guix.sigs/31.0` with an `all.SHA256SUMS` file, the file that release binaries are checked against | `bitcoin-core/guix.sigs` | A count of builders who avoided shared prebuilt inputs |
| 39 | Key files in `builder-keys` | `bitcoin-core/guix.sigs` | A count of builders for any one release |
| 1 | Developer signature on a kernel.org release archive. A signed checksum file exists beside it. | `kernel.org/signature.html` | A rebuild of the binary a machine boots |
| At least 2 | Release engineers whose builds must match, in the Tor Browser design text | Tor Browser design document | A count of builders on a given release |
| 3 | Signature files on the unsigned-build hash file in the `15.0.24` directory | `dist.torproject.org` | A verified count of independent builders |
| 95.8% | 37,827 packages whose two test builds matched, forky/amd64 | `tests.reproducible-builds.org` | A rebuild of the distributed binary, or a count of builders on one file |

A larger figure in one row does not mean more of what another row counts. Count the folder or the dashboard again before you treat a figure as current.

## 5. Provenance records

A signature answers who vouches for a file. A rebuild answers whether another builder got the same file. A provenance record answers a third question: how was the file produced? A provenance record names some of the source, the builder, the build steps, and the inputs.

A provenance record is a claim by whoever issued it. The record is as reliable as its issuer and the system that made it. A provenance record is not a rebuild. Two parties who rebuild do not need a provenance record to compare hashes. A build system that issues a provenance record has not shown that anyone else can produce the same file.

### 5.1 Kinds of record

**Signed hash file.** A builder lists the hashes of the build output and signs the list. Bitcoin Core's attestations take this form (§3.1), and the Tor Browser directory holds signed hash files (§3.3). The record says: this key reports these hashes for this version. The record carries little description of the build. The version in the folder name ties the record to a tag. When several builders sign the same hashes, the set of records is several reports of the same result. Each report is its builder's claim, in the same way that a provenance record is its issuer's claim. A reader who verifies the signatures has authenticated reports of a rebuild. That is the most direct public evidence for the rebuilt check in this note. It is not proof that separate builds took place.

**Build-information file.** A file that records the conditions of a build so that another party can repeat it. Debian's rebuild service uses the `.buildinfo` file from the original build (§3.4). The record says: this build used these inputs. On its own, the file is the original builder's statement. The file becomes evidence about the build when a rebuilder uses it and gets the same binary.

**SLSA provenance.** SLSA is a published framework for supply-chain records. It describes provenance as "the verifiable information about software artifacts describing where, when, and how something was produced." The record has a build definition, with the build type, the parameters, and the resolved dependencies. It also has run details, with an identifier for the builder. The framework's build levels describe how hard the record is to forge. At Build L1, the record "is trivial to bypass or forge." At Build L3, forging the record "requires exploiting a vulnerability." From Build L2, the build platform must "generate and sign the provenance itself," and the consumer validates that the provenance is authentic. At Build L1, provenance "may be incomplete and/or unsigned." Signed provenance therefore ties a platform's statement to a file. No level requires that a second party rebuild the file. A higher level is evidence about the build platform that issued the record. [SLSA provenance, version 1.2](https://slsa.dev/spec/v1.2/provenance), [SLSA build levels, version 1.2](https://slsa.dev/spec/v1.2/build-track-basics)

**in-toto attestation.** in-toto defines a general container for statements of this kind. It defines an attestation as "authenticated metadata about one or more software artifacts." A statement "binds the attestation to a particular subject" and names the type of the statement's content. An envelope carries the signature. SLSA provenance is one content type that the container can carry. The container says who signed a statement about which files. It does not say that the statement is true. [in-toto attestation framework, v1](https://github.com/in-toto/attestation/blob/main/spec/README.md)

**Transparency log.** A public log to which signing events are appended. The Sigstore project describes its log, Rekor, as "an immutable, tamper-resistant ledger of metadata generated within a software project's supply chain." A log entry is evidence that a signed statement was recorded and that other people can find it. A log entry is not evidence that the signed file is correct. A log helps a reader detect a signature that the key holder did not expect. [Sigstore log overview](https://docs.sigstore.dev/logging/overview/)

**Timestamp.** A proof that some data existed before a point in time. OpenTimestamps states: "A timestamp proves that some data existed prior to some point in time." The Bitcoin Core release server creates an OpenTimestamps file for the release directory (§3.1). A timestamp is evidence about when. It is not evidence about who built the data or from what source. [OpenTimestamps](https://opentimestamps.org/)

### 5.2 How the records map to the checks

| Record | Who issues it | What it states | Check it helps answer | What it does not answer | Seen in §3 |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Signature on a release file** | The publisher | This key vouches for this file | Signed | How the file was built, and whether anyone else built it | §3.2, §3.3 |
| **Signed hash file from a builder** | Each builder | This key reports these hashes for this version | Signed. Rebuilt, as authenticated reports, when several builders sign the same hashes and the signatures are verified. | Whether separate builds took place, and which inputs the builders shared | §3.1, §3.3 |
| **Signed archive index** | The archive | These package checksums belong to this archive state | Signed | Whether a package matches its source | §3.4 |
| **Build-information file** | The original builder | This build used these inputs | Rebuilt, when a rebuilder uses the file and matches the binary | Whether different inputs give the same binary | §3.4 |
| **SLSA provenance** | A build platform | This platform ran this build definition on these inputs | Signed, from Build L2, where the build platform signs its own statement about the file. Not rebuilt. The record mainly answers how the file was produced. | Whether a second party can produce the same file | Not found in the documents read |
| **in-toto attestation** | Any key holder | A signed statement of a named type about named files | Depends on the statement it carries | Whether the statement is true | Not found in the documents read |
| **Transparency log entry** | A log operator | This signed statement was recorded at this position in the log | Signed, by making signatures public | Whether the signed file is correct | Not found in the documents read |
| **Timestamp** | A timestamp service | This data existed before this time | None of the three. It dates a record. | Who built the data, and from what | §3.1 |
| **Dashboard figure** | A test system | This share of packages built the same twice in the test system | None of the three for one release file | Who rebuilt a given file | §3.5 |

"Not found in the documents read" is a statement about the documents listed in §8. It is not a statement that a project does not publish such a record.

A process can give evidence for the rebuilt check without any provenance format. Verified signed hash files from several builders do that. A process can also publish signed provenance and leave the rebuilt check open. The two kinds of evidence do not replace each other. Both are statements by the party that signed them.

## 6. Questions that make a claim reviewable

| If the claim is | Request | If the artifact is missing |
| :---- | :---- | :---- |
| **The source is open** | The tag or commit that the release file says it was built from, and the public history behind that tag | The license does not identify the file you downloaded |
| **The change was reviewed** | The public place where the change was discussed, tied to the tag. The public check in §2 shows only that the change could be read. | A public repository is not a record of who read a change |
| **It is signed** | The signature, the object it covers, and the reason to tie the key to the expected signer | A download button is not a signature, and a key is not yet an identity |
| **Anyone can reproduce it** | A second builder's signed report of the same hash, with a key you can authenticate, or a public rebuild result for that file | A build document is not a second build |
| **Several people signed the release** | The directory or signature file, and whether each signature covers the same hash | A contributor list is not a set of attestations |
| **The builders are independent** | Which machines, networks, and prebuilt inputs the builders had in common | A count of builders does not record what they shared |
| **Releases wait for matching builds** | The written step, and the listing for the release you mean | A written gate is not a record of one release |
| **The operating system is reproducible** | The record for the binary you run, not only for the upstream source archive | An upstream signature does not cover a later compilation |
| **This percentage is reproducible** | The suite, the architecture, the date, and what the figure counts: test builds, distributed binaries, or builders | A dashboard for one suite is not an attestation for one file |
| **We publish provenance** | The record, who issued it, and whether anyone other than the issuer rebuilt the file | A provenance record is the issuer's statement about its own build |
| **It follows the specification** | The changes behind the release tag that implement the specification, and the tests | A published specification is not a release record (§7) |
| **You can verify it yourself** | The exact step for the person who downloads, the keys that step relies on, and how the reader authenticates those keys | A verification step is as strong as the reader's reason to trust the key |

## 7. Limits and source use

- This note compares written processes and public listings. It is not a threat model, and it does not assess the security of any project.
- Review is out of scope. The public check shows that a change can be read. No record here shows how much review a change received, and the note does not compare review between projects.
- The four projects publish different objects. An entry of "Not applicable" in §4.1 follows from what a project publishes. An entry of "Not read for this note" is a limit of this note.
- A builder who attests a tag did not necessarily read the commits in the tag. A person who read the commits did not necessarily rebuild the release.
- A builder directory holds an attestation from whoever filed one for that version. Count the folder for the version you mean. Release candidates have separate folders.
- A written gate describes the process. The listing for one release shows what was filed for that release. Read both.
- The binary a machine boots, a phone image, and a container build are downstream builds with their own records. Signing-device firmware is out of scope and will be treated separately.
- A quote from a pinned source is the text at the commit given in §8. A quote from an unpinned page is the text as read on the date given. If the page changes, the quote can change with it.
- A specification is not a release. A Bitcoin Improvement Proposal (BIP) is a specification document. The BIP repository says that a published BIP "does not indicate that it is a good idea, has community consensus, or that it is about to be adopted." A published BIP is evidence that a proposal was written down in public. It is not evidence that a release implements the proposal. The BIP process has no release file, hash, or rebuild, so §3 has no record for it. [BIP repository README](https://github.com/bitcoin/bips/blob/927b6de9915c9262615a6399de51b200f81e5aa4/README.mediawiki), [BIP process (BIP 3)](https://github.com/bitcoin/bips/blob/927b6de9915c9262615a6399de51b200f81e5aa4/bip-0003.md)
- A missing artifact means only that this note does not show the claim.

## 8. References

Each source is listed once with the version or date consulted and the sections that cite it. A source establishes what a document says. It does not establish that every release followed the document (§7).

**Bitcoin Core**, at [bitcoin/bitcoin commit 4bacf21](https://github.com/bitcoin/bitcoin/tree/4bacf21a13c2ed25ef9362ca38f26bf0a67d22c9), and [bitcoin-core/guix.sigs commit f6c609b](https://github.com/bitcoin-core/guix.sigs/tree/f6c609b0dd7012066f0d1ee9914ef30e6e5d8b6c).

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [doc/release-process.md](https://github.com/bitcoin/bitcoin/blob/4bacf21a13c2ed25ef9362ca38f26bf0a67d22c9/doc/release-process.md) | Tagging, building, attesting, code signing, and upload steps | §3.1, §4, §5.1 |
| [contrib/guix/README.md](https://github.com/bitcoin/bitcoin/blob/4bacf21a13c2ed25ef9362ca38f26bf0a67d22c9/contrib/guix/README.md) | The Guix build, including the choice to use substitutes | §3.1, §4.1 |
| [contrib/verify-binaries/README.md](https://github.com/bitcoin/bitcoin/blob/4bacf21a13c2ed25ef9362ca38f26bf0a67d22c9/contrib/verify-binaries/README.md) | The downloader's verification step and key choice | §3.1, §4.1 |
| [guix.sigs README.md](https://github.com/bitcoin-core/guix.sigs/blob/f6c609b0dd7012066f0d1ee9914ef30e6e5d8b6c/README.md) | Build stages, directory structure, and how a builder adds a key | §3.1, §4.1 |
| [guix.sigs 31.0](https://github.com/bitcoin-core/guix.sigs/tree/f6c609b0dd7012066f0d1ee9914ef30e6e5d8b6c/31.0) | Attestations filed for tag `v31.0` | §3.1, §4.2 |
| [guix.sigs builder-keys](https://github.com/bitcoin-core/guix.sigs/tree/f6c609b0dd7012066f0d1ee9914ef30e6e5d8b6c/builder-keys) | Builder keys | §3.1, §4.2 |

**Bitcoin Improvement Proposals**, at [bitcoin/bips commit 927b6de](https://github.com/bitcoin/bips/tree/927b6de9915c9262615a6399de51b200f81e5aa4).

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [README.mediawiki](https://github.com/bitcoin/bips/blob/927b6de9915c9262615a6399de51b200f81e5aa4/README.mediawiki) | What publication of a BIP indicates | §7 |
| [BIP 3](https://github.com/bitcoin/bips/blob/927b6de9915c9262615a6399de51b200f81e5aa4/bip-0003.md) | Updated BIP Process | §7 |

**Other sources**, read 9 October 2026 unless a version is given. These pages are not pinned to a commit.

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [Linux kernel release signatures](https://kernel.org/signature.html) | Developer signatures, the checksum file, and key retrieval | §3.2, §4 |
| [Kernel maintainer PGP guide](https://www.kernel.org/doc/html/latest/process/maintainer-pgp-guide.html) | Signed tags, key distribution, and the stated trust model | §3.2, §4.1 |
| [Kernel reproducible builds](https://www.kernel.org/doc/html/latest/kbuild/reproducible-builds.html) | Settings that affect whether a kernel build is reproducible | §3.2 |
| [Tor Browser signature verification](https://support.torproject.org/tbb/how-to-verify-signature/) | The package signature, the project key, and the downloader's step | §3.3, §4.1 |
| [Tor Browser 15.0.24 release directory](https://dist.torproject.org/torbrowser/15.0.24/) | Files published for one release | §3.3, §4.2 |
| [Tor Browser design document](https://gitlab.torproject.org/tpo/applications/wiki/-/blob/master/Design-Documents/Tor-Browser-Design-Doc.md) | The stated requirement for matching builds. Read for version 0.1; not reread for version 0.2. | §3.3, §4 |
| [Reproducible Builds report, May 2026](https://reproducible-builds.org/reports/2026-05/) | The report's account of the Debian migration rule | §3.4 |
| [Debian Release Team announcement, May 2026](https://lists.debian.org/debian-devel-announce/2026/05/msg00001.html) | The migration rule. Read only as quoted in the report above. | §3.4 |
| [reproduce.debian.net](https://reproduce.debian.net/) | The rebuild service for distributed Debian binary packages | §3.4, §4.1, §5.1 |
| [apt-secure(8), Debian trixie](https://manpages.debian.org/trixie/apt/apt-secure.8.en.html) | The archive signature and checksum chain | §3.4, §4.1 |
| [Debian test dashboard, forky/amd64](https://tests.reproducible-builds.org/debian/forky/index_suite_amd64_stats.html) | The share of packages whose two test builds match | §3.5, §4.2 |
| [Debian test variations](https://tests.reproducible-builds.org/debian/index_variations.html) | What differs between the two test builds | §3.5 |
| [SLSA provenance, version 1.2](https://slsa.dev/spec/v1.2/provenance) | The provenance record and its fields | §5.1 |
| [SLSA build levels, version 1.2](https://slsa.dev/spec/v1.2/build-track-basics) | Build levels L1 to L3, and which levels require signed provenance | §5.1, §5.2 |
| [in-toto attestation framework, v1](https://github.com/in-toto/attestation/blob/main/spec/README.md) | Statement, predicate, envelope, and bundle | §5.1 |
| [Sigstore log overview](https://docs.sigstore.dev/logging/overview/) | The Rekor transparency log | §5.1 |
| [OpenTimestamps](https://opentimestamps.org/) | What a timestamp proves | §5.1 |

## Changelog

Newest first. Versioning follows [STYLE.md](STYLE.md): the minor number changes when a claim, consequence, citation, or reader-facing text changes. Version 0.1 was not tagged, so the section numbers changed in 0.2 without a major version. Section numbers stay fixed from 0.2.

| Version | Change |
| :---- | :---- |
| 0.3 — 9 October 2026 | Review fixes. Treats a builder's signed hash file as the builder's report, in the same way as a provenance record: §1.2, §2, §3.1, §5.1, §5.2, and §6 no longer say that matching signatures answer the rebuilt check by themselves. Separates key retrieval from key authentication in §1.2, §2, §4.1, and §6. Corrects the SLSA row in §5.2: provenance is signed by the build platform from Build L2. Adds a §4.1 row for where each release ties to its source history. Carries the Tor Browser and Debian limits into the §4.1 cells. Moves the BIP paragraph from §1.2 to §7 and shortens it. Removes repeated statements that the note does not rank. |
| 0.2 — 9 October 2026 | Reframed as a comparison of release processes. Renames the "reviewed" check to "public" and states that the note does not establish review. Adds a step-by-step comparison of the four processes (§4.1), with "Not applicable" and "Not read for this note" as fixed entries. Adds a provenance section with a mapping table (§5). Adds the downloader's step, key publication, and shared build inputs to each record. Adds a paragraph that separates a specification, such as a BIP, from a release. Splits the Debian record: the migration rule and its rebuild service (§3.4), and the test dashboard (§3.5). Corrects the dashboard link and reads the figure as 37,827 packages, 95.8%. Splits the Bitcoin Core listing into 21 first-stage and 19 second-stage attestations and records that each set matches. Records that the kernel signature covers the uncompressed archive and that a signed checksum file exists. Adds a release directory listing to the Tor Browser record. Moves the maintainer-signed release from §3.5 into §2. Pins Bitcoin Core sources to commits and adds "Cited in" to the references. Renumbers: Questions is §6, Limits is §7, References is §8. |
| 0.1 — 9 October 2026 | Peer review draft. Three checks. Public records for Bitcoin Core release binaries, kernel.org source archives, Tor Browser design notes, the Debian forky migration rule, and a maintainer-signed release. Figures dated to the listing that was read. |
