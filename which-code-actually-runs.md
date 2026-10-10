# Which Code Actually Runs?

*Questions to ask when software has more than one way to do a job*

This note describes kinds of fallback. It reads two public records as examples.

| Document | which-code-actually-runs.md |
| :---- | :---- |
| **Version** | 0.1 — peer review draft |
| **Audience** | People who rely on a wallet, a signing device, or a library to make or use keys: read [Start here](#start-here). Technical reviewers: read the full note. |
| **Method** | Identify each place where software chooses between two ways to do the same job. For each, record what decides the choice, when, and what a reader could inspect to learn which way ran. Tie each statement to a public source. |
| **Changes** | See the [changelog](#changelog). |
| **Scope** | Fallbacks in software that makes or uses keys: random sources, cryptographic implementations, and the code that selects between them. Two public records are read. This is not an incident survey, a finding about any organization or person, or an assessment of the security of any product. |

## Contents

- [Start here](#start-here)
- [1. What the note establishes](#1-what-the-note-establishes)
  - [1.1 Evidence](#11-evidence)
  - [1.2 Terms](#12-terms)
- [2. Kinds of fallback](#2-kinds-of-fallback)
- [3. Public records](#3-public-records)
  - [3.1 Coldcard firmware: the random source for seeds](#31-coldcard-firmware-the-random-source-for-seeds)
  - [3.2 embit: the secp256k1 implementation](#32-embit-the-secp256k1-implementation)
- [4. What separates the two records](#4-what-separates-the-two-records)
- [5. What a project can show](#5-what-a-project-can-show)
- [6. Questions that make a claim reviewable](#6-questions-that-make-a-claim-reviewable)
- [7. Limits and source use](#7-limits-and-source-use)
- [8. References](#8-references)
- [Changelog](#changelog)

## Start here

This section is for anyone who relies on software to make or protect keys. You do not need to read code to use it. The rest of the note is the record behind each question.

**What a fallback is.** Software often has two ways to do one job. The first way is the one its makers intend. The second way is a backup, called a fallback. The fallback takes over when the first way is not available. A fallback usually exists for a good reason. It lets the software work on an older machine, or on a device that lacks a part.

**Why it matters.** The two ways can differ in strength. If the backup takes over and nothing tells you, the software still works. It looks the same. You have no sign that the weaker way is in use.

**The questions that matter.** Ask these about any software that makes or uses keys.

1. **Does the software have more than one way to do this job?** Ask for the list. Random numbers and signing are the two jobs to ask about first. See [§2](#2-kinds-of-fallback).
2. **What decides which way runs?** A setting chosen when the software was built can decide it. A check made when the software starts can decide it. Ask which. See [§2](#2-kinds-of-fallback).
3. **Would anyone know if the backup took over?** Ask whether the software stops, warns, or carries on. See [§3](#3-public-records).
4. **How does the backup differ?** A backup can be slower and equally strong. It can also be weaker. Ask what the makers say about it. See [§4](#4-what-separates-the-two-records).
5. **What shows which way ran in the copy you have?** A document that describes the intended way is not that evidence. See [§5](#5-what-a-project-can-show).

**How to judge an answer.** A good answer shows you something you can check. Examples are a test that fails when the intended part is missing, or a check that the backup is absent from the finished product. A weak answer repeats the claim in different words. A weak answer does not prove a problem. It shows that the claim is not proved yet.

**What this note does not do.** It does not say that any product is safe or unsafe. It does not say that fallbacks are wrong. It helps you ask which code runs.

## 1. What the note establishes

A claim about a fallback needs a defined object: a function, a build of a program, or an installed copy. "It uses the hardware random source" and "it uses the native library" are claims about what runs. Neither follows from the presence of that code in a source tree or in a binary.

The other notes in this repository stop one step earlier. The release note asks what a published file is evidence of. The custody note lists four levels of build evidence and ends with "evidence that the device runs that binary." This note asks a further question inside one binary or one package: of the alternatives it contains, which one executes. See [What Can You Check About a Software Release? §2](what-can-you-check-about-a-release.md#2-three-checks) and [Who Can Move Your Bitcoin? §3](who-can-move-your-bitcoin.md#3-review-the-complete-custody-lifecycle).

This note uses public statements and public source. No device was tested, and no build was reproduced.

### 1.1 Evidence

Each record in §3 has: **claim; object and date; public source; what was read; result; unresolved.** A statement by a maker about its own product is recorded as the maker's statement. An analysis by another party is recorded as that party's statement.

### 1.2 Terms

| Term | Meaning here |
| :---- | :---- |
| **Fallback** | A second way to do a job, used when the intended way is not available. |
| **Intended path** | The way the makers mean the software to use. |
| **Selection** | The step that decides which way runs. |
| **Build-time selection** | Selection made when the software is compiled or linked. Every copy of that build then uses the same way. |
| **Run-time selection** | Selection made when the software starts or when a function is called. Two copies of one file can use different ways. |
| **Silent** | The software gives no error, warning, or record when the fallback is chosen. A fallback can be described in a document and still be silent when it runs. |
| **Fail closed** | The software stops when the intended way is not available. |
| **Presence and reachability** | Presence: the intended code is in the file. Reachability: the job in question actually calls it. Presence does not establish reachability. |
| **Parity** | Two ways return the same results for the same inputs. Parity is about results. It does not cover timing behavior or the quality of random values. |

## 2. Kinds of fallback

| Kind | When selection happens | What decides | What a reader could inspect |
| :---- | :---- | :---- | :---- |
| **A random source chosen at build time** | Compile or link | A configuration value, and which function a name resolves to at link time | The build configuration, the linked symbols, and a build check that fails on the wrong binding |
| **A cryptographic implementation chosen at import** | Program start | Whether a native library loads | The selection code, and whether a failed load raises an error |
| **A per-function mix** | Program start, per function | Whether the loaded library exports each function | The table of optional functions, and a way to list which implementation serves each |
| **A substitute written for one platform** | Compile | The build target | The source for that target, and the tests that run on it |

Three properties apply to every kind.

- **A fallback has a reason.** It lets the software run where the intended way is missing. Record the reason. Whether that reason still holds is a separate question.
- **Working software is not evidence of selection.** A build that compiles, and a program that returns results, look the same with either way in use.
- **A description of the intended path is not evidence of selection.** The evidence is a check that would fail if the other way were in use.

## 3. Public records

### 3.1 Coldcard firmware: the random source for seeds

**Claim.** In affected firmware releases, seed generation drew on a software generator in place of the device's hardware random source. The cause was a build-time selection that took the fallback while the build succeeded.

**Object and date.** The maker's security advisory and technical account, both first published 30 July 2026, and an independent analysis published the same day. All three were read on 9 October 2026.

**Public source.** The maker's advisory and technical account on its blog. An analysis on an engineering blog by another company's Bitcoin engineering and security team.

**What was read.** The affected versions. The stated cause. The stated effect on seed strength. The stated fix. The maker's statement on why earlier review did not find the fault.

**Result.**

- **The intended path.** The firmware has its own wrapper for the microcontroller's hardware random source. The production board configuration sets the macro `MICROPY_HW_ENABLE_RNG` to zero. The analysis quotes the comment beside it: "We have our own version of this code."
- **The fallback.** MicroPython includes a software generator for boards without a hardware source. The analysis identifies it as the Yasmarang generator.
- **Selection.** A cryptographic library in the firmware guarded its use of the random function with `#ifndef`. The analysis states: "`#ifndef` verifies only that the macro exists. It does not reject a macro whose value is zero." The macro was defined as zero, so the guard passed. The library's random function then resolved, at link time, to MicroPython's function, because the firmware's own wrapper did not export that name. The maker's account says: "A build and link integration error meant that setting did not have the intended effect."
- **When the job reached the fallback.** Both accounts date the change to March 2021, when seed generation moved from the firmware's own random function to the library's.
- **No signal.** The build succeeded. Neither account describes an error, a warning, or a failed test at the time.
- **Presence and reachability.** The maker says its review confirmed that the intended hardware source was in the binary. The review "did not verify end-to-end symbol resolution and call reachability from wallet seed generation." The maker also says: "We were unaware of the bug until today."
- **Effect, as stated by the maker.** The advisory gives "about 72 bits of entropy rather than the expected 128 bits" for seeds made on the later hardware models before the fixed releases, and the technical account gives a preliminary estimate of about 40 bits for the earlier models. The maker calls these estimates preliminary. The advisory says that attackers "exploited those weak seeds offline by regenerating the corresponding private keys."
- **The fix, as stated by the maker.** The fixed releases exclude the fallback generator from the build. They add a build-time check that fails unless the board-specific code defines the random function and the fallback object defines no symbols. That check is a fail-closed selection at build time.
- **What the fix does not change.** The maker says that an update does not repair a seed that affected firmware generated.

**Unresolved.** This note did not read the firmware source or rebuild any release. The entropy figures are the maker's preliminary estimates, and the independent analysis gives different bounds under its own conditions. The maker's account says its investigation continues. Both accounts were read through a page reader, so the quotes need a check against the pages.

### 3.2 embit: the secp256k1 implementation

**Claim.** The embit library holds native bindings and a pure-Python implementation of its secp256k1 operations. The library selects between them at import. The full record is in [What Can You Check About an embit Release? §3.5](embit-release-records.md#35-two-implementations-in-one-release).

**Object and date.** The source tree at tag `v0.8.2` and the other sources listed in that record, read 9 October 2026.

**Result.** The points below summarize that record. The sources and the unresolved items are listed there.

- **The intended path.** Bindings to a native `libsecp256k1`.
- **The fallback.** A pure-Python implementation. Its main file states that it is copied from Bitcoin Core's test framework. The origin file describes itself as test-only.
- **Selection.** At import, inside a bare `except:` clause. Any error in loading the native bindings selects the pure-Python implementation. Since version 0.8.1, selection can also happen per function.
- **Signal.** The project's README describes the fallback. The files read contain no warning, log line, or function that reports the selection at run time.
- **What differs.** In the pure-Python implementation, signing nonces are derived deterministically, so signing does not draw on a random source. The pure-Python implementation has an empty context-randomization function, and the project's own open change states that the two implementations return different results at some call sites.
- **The project's open change.** A pull request opened in June 2026 removes the pure-Python implementation and makes import fail when no native library loads. That change is a fail-closed selection at import. It was not merged on the date read.
- **A downstream check.** One downstream project checks its built image for the native library and for the absence of the pure-Python module.

**Unresolved.** As listed in the embit note. This note adds no reading of its own.

## 4. What separates the two records

The two records share a pattern: a fallback took over, or can take over, with no signal. They differ in every other respect that a reviewer needs. Do not carry a conclusion from one record to the other.

| Question | Coldcard firmware seed generation | embit secp256k1 |
| :---- | :---- | :---- |
| **What job has a fallback?** | Producing random values for a new seed | Elliptic-curve operations, including signing |
| **When is selection made?** | At build time. Every device with an affected release used the same path. | At import. The result depends on the machine, and can differ between two copies of one file. |
| **Was the fallback meant to be reachable?** | No, by the maker's account. The configuration was meant to exclude it. | Yes. The project's README describes it. |
| **What does the fallback weaken?** | The unpredictability of the seed, by the maker's account | Not established by the records read. The origin file warns about side channels and key protection. The project's open change cites differing results. |
| **Who could make use of the difference?** | Anyone, offline, by the maker's account | Not established by the records read |
| **Is the effect permanent?** | Yes for an affected seed. A firmware update does not change a seed already made. | Not established. The implementation in use can change when the environment changes. |
| **What is the fail-closed form?** | A build check, in the fixed releases | An import that raises an error, in an open change |

The first record is an incident with a stated effect. The second is a design property with no reported incident in the records read. The pattern is the same. The consequence is established for one and not for the other.

## 5. What a project can show

Each control below is paired with the artifact that would let a reader check it. A control without an artifact is a description of intent.

| Control | What it does | Artifact a reader can check |
| :---- | :---- | :---- |
| **Remove the fallback** | Leaves one way to do the job | The source tree, and a check of the built file for the removed code |
| **Fail closed at build time** | Stops the build when the intended function is not the one linked | The build check, and a record of it failing on a wrong configuration |
| **Fail closed at start** | Stops the program when the intended library does not load | The selection code, and a test that runs without the library and expects an error |
| **Report the selection** | Lets a caller ask which implementation is active | The function that reports it, and its use in a downstream check |
| **Test reachability** | Shows that the job calls the intended code, not only that the code is present | A test that traces the call from the job to the intended function |
| **Check the product** | Confirms the selection in the finished image | A downstream script that inspects the built image |

## 6. Questions that make a claim reviewable

| If the claim is | Request |
| :---- | :---- |
| **It uses the hardware random source** | Show the build configuration and the function that seed generation calls. Show a check that fails when another function is linked. |
| **The secure code is in the binary** | Show that the job in question reaches it. Presence is not reachability. |
| **It uses the native library** | Show what happens when the library does not load: an error, a warning, or a silent change of implementation. |
| **The fallback is only for other platforms** | Show what excludes it from this build, and a check that the exclusion took effect. |
| **The fallback gives the same results** | State which results were compared. Parity of results does not cover timing behavior or random values. |
| **The tests pass** | State whether any test fails when the fallback is in use. A test that passes on both ways does not show which one ran. |
| **It was reviewed** | State whether the review traced the call from the job to the code, and on which build. |
| **It is fixed** | State what the fix changes for material made before it. A fix to selection does not change keys already generated. |

## 7. Limits and source use

- This note describes a pattern and reads two records. Two records do not establish how common the pattern is. The note makes no claim about frequency.
- The Coldcard record rests on the maker's own account and one independent analysis, both written during an investigation that the maker says continues. Later accounts can differ.
- The embit record is a summary of another note in this repository. That note states that it does not assess the library's code.
- No conclusion about one record carries to the other. §4 lists the differences.
- No organization or person is assessed. The note names a product and a library because their public records are the evidence.
- Loss figures, attribution, and the course of the incident are out of scope.
- A fallback is not a fault. The note asks whether its selection can be seen and checked.

## 8. References

Each source is listed once with the date consulted and the sections that cite it. These pages are not pinned to a version; each was read on 9 October 2026.

| Reference | What it defines | Cited in |
| :---- | :---- | :---- |
| [COLDCARD Security Advisory](https://blog.coinkite.com/coldcard-mk3-seed-generation-warning/) | The maker's advisory: affected versions, stated effect, and guidance. First published 30 July 2026. | §3.1, §4 |
| [Technical Deep Dive into the Entropy Issue](https://blog.coinkite.com/entropy-technical-backgrounder/) | The maker's account of the cause, the fix, and the earlier review. First published 30 July 2026. | §3.1, §4, §5 |
| [Predictable RNG fallback and 32-bit reseed in COLDCARD firmware](https://engineering.block.xyz/blog/predictable-rng-fallback-and-32-bit-reseed-in-coldcard-firmware) | An independent analysis of the configuration, the guard, and the fallback generator. Published 30 July 2026. | §3.1 |
| [What Can You Check About an embit Release? §3.5](embit-release-records.md#35-two-implementations-in-one-release) | The embit record and its pinned sources | §3.2, §4, §5 |

## Changelog

Newest first. Versioning follows [STYLE.md](STYLE.md).

| Version | Change |
| :---- | :---- |
| 0.1 — 9 October 2026 | First draft. Terms for fallback, selection, and presence versus reachability. Four kinds of fallback. Two public records: Coldcard firmware seed generation, from the maker's account and one independent analysis, and the embit secp256k1 implementation, summarized from the embit note. A table of what separates the two records, a table of controls with the artifact for each, and a list of reviewable claims. |
