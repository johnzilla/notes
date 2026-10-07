**NOTE · PUBLIC CUSTODY WORKFLOWS**

# What A Bitcoin Custody Pitch Has To Show

*A threat-model and evidence-review framework. Configurations, not providers.*

| Document | what-a-custody-pitch-has-to-show.md |
| :---- | :---- |
| **Date** | 6 October 2026 |
| **Audience** | Security researchers and technically competent reviewers assessing Bitcoin custody workflows. |
| **Method** | Identify spending authority, trace the custody lifecycle, and distinguish protocol consequences from deployment claims requiring evidence. |
| **Revision** | Corrects spending-path, timelock, threshold-signature, recovery, passphrase, and privacy claims. Adds a full single-signature baseline, account custody, escrow, channel settlement, lifecycle checks, and public technical references. |
| **Scope** | Direct on-chain custody and its signing, backup, and recovery arrangements; a separate section covers payment-channel settlement. This is not a finding about any organization, an incident survey, or an exhaustive assessment of every off-chain protocol. |

## Contents

- [1. What the assessment establishes](#1-what-the-assessment-establishes)
  - [1.1 Evidence and verdicts](#11-evidence-and-verdicts)
  - [1.2 Terms and boundaries](#12-terms-and-boundaries)
  - [1.3 Coverage by dimension](#13-coverage-by-dimension)
- [2. Configuration assessments](#2-configuration-assessments)
  - [Baseline — holder-controlled single-signature custody](#baseline--holder-controlled-single-signature-custody)
  - [2.1 Scenario A — distributed institutional custody](#21-scenario-a--distributed-institutional-custody)
  - [2.2 Scenario B — collaborative custody with a holder quorum](#22-scenario-b--collaborative-custody-with-a-holder-quorum)
  - [2.3 Scenario C — custody with multiple timed spending paths](#23-scenario-c--custody-with-multiple-timed-spending-paths)
  - [2.4 Scenario D — holder-controlled 2-of-3](#24-scenario-d--holder-controlled-2-of-3)
  - [2.5 Scenario E — joint signing with delayed unilateral recovery](#25-scenario-e--joint-signing-with-delayed-unilateral-recovery)
  - [2.6 Scenario F — split backup of a single-signature secret](#26-scenario-f--split-backup-of-a-single-signature-secret)
  - [2.7 Scenario G — threshold or aggregate signing](#27-scenario-g--threshold-or-aggregate-signing)
  - [2.8 Scenario H — an account with external spending authority](#28-scenario-h--an-account-with-external-spending-authority)
  - [2.9 Scenario I — escrow and shared control](#29-scenario-i--escrow-and-shared-control)
  - [2.10 Payment channels and other off-chain arrangements](#210-payment-channels-and-other-off-chain-arrangements)
- [3. Review the complete custody lifecycle](#3-review-the-complete-custody-lifecycle)
  - [3.1 Deadlines, fees, and reorganizations](#31-deadlines-fees-and-reorganizations)
- [4. Independence, quorum size, and shared failures](#4-independence-quorum-size-and-shared-failures)
- [5. Privacy and coercion](#5-privacy-and-coercion)
- [6. Questions that make a claim reviewable](#6-questions-that-make-a-claim-reviewable)
- [7. Limits and source use](#7-limits-and-source-use)

## 1. What the assessment establishes

A custody claim needs a defined object: a particular output, wallet, signing policy, recovery arrangement, or account. “Only you can spend” and “no single party can spend” describe different properties. Neither follows from a device count, an approval screen, or a backup count.

This document uses public protocol specifications to establish the behavior of the mechanisms below. The configurations specify who controls the required material; they are not assertions that an unnamed deployment has that allocation. Consequences derived from those configurations are identified as such. No customer wallet, private ceremony record, implementation build, or insurance contract has been inspected.

For an actual assessment, identify the assets, authorized actions, participants, trust boundaries, and capabilities being evaluated. Distinguish at least unauthorized spending, refusal or inability to spend, permanent loss of recovery material, transaction misdirection, and disclosure of wallet activity. Record the exact keys, shares, devices, administrators, files, and services accessible to each participant. Physical devices and nominally separate organizations are not automatically independent control domains.

### 1.1 Evidence and verdicts

Each deployment finding should contain: **claim; configuration and date; public source or inspected artifact; verification performed; result; unresolved dependencies**. Record document revisions or source commits where available. A source describing a protocol is evidence of its specified behavior, not proof that a deployment implements it correctly.

| Result | Meaning |
| :---- | :---- |
| **Established within scope** | The inspected evidence and verification support the claim for the recorded scope. |
| **Partial** | A specified part is supported; remaining conditions are identified. |
| **Not established** | The necessary evidence was unavailable or not verified. |
| **Contradicted under the stated configuration** | The configuration or inspected evidence permits behavior inconsistent with the claim. |
| **Not applicable** | The claim does not concern this configuration. |

The scenario tables below state **protocol consequences and evidence requirements**, not deployment verdicts. Lack of evidence is not evidence of misconduct. Conversely, a claim disproved by the stated authorization rules is not merely an unanswered question.

| Claim to establish | Required evidence and verification |
| :---- | :---- |
| **Who can spend** | Complete output policy, matched to the relevant outputs; every spending path; a mapping from keys or shares to actual controllers, including backups and recovery authority. |
| **Who can block spending** | All available spending paths at the relevant time, plus required signing software, data, authentication, communication, and broadcast services. Separate signer refusal from other availability failures. |
| **A control is consensus-enforced** | Identify the consensus rule and show that no alternate path bypasses the claimed restriction. |
| **A signer verifies authorization** | Show how it authenticates the intended wallet policy and transaction. Verify destination, amount, fees, and change handling against trusted information. |
| **Recovery works without a service** | A documented restoration and spend using the retained material, with that service unavailable; record software versions and the path exercised. |
| **An audit applies** | Inspect the report's subject, scope, method, date or period, exclusions, and findings. A code review, operational assessment, and financial audit answer different questions. |
| **Insurance covers the claimed loss** | Inspect the applicable policy, insured party, covered event, exclusions, limits, and claim requirements. Signing authority and contractual compensation require separate findings. |

### 1.2 Terms and boundaries

| Term | Meaning here |
| :---- | :---- |
| **Key, signer, party** | A private key authorizes signatures; a signer is the component using it; a party controls components or credentials. Counting keys does not establish a count of independent parties. |
| **Spending path** | A complete way to satisfy an output's authorization conditions. Assess every alternative, including recovery and delayed paths. |
| **Taproot leaf** | A script in a Taproot tree. One leaf can contain multiple authorization alternatives; key-path spending is a separate route. |
| **Descriptor** | A structured description of output scripts and keys, including derivation information where applicable. Descriptors can contain private keys. “Public-only descriptor” below excludes them. |
| **Miniscript** | A structured subset of Script supporting analysis and construction of spending conditions; it is not synonymous with a Taproot tree. |
| **Coordinator** | The role that assembles wallet information and transactions. Whether the same application also signs is an implementation fact. |
| **Signing device and supply chain** | The signer's hardware, firmware, and software, and the route by which each reaches it: procurement, delivery, initialization, update authority, and build provenance. A genuine device running substituted firmware, or a remote signing service, is a different signer for review purposes. |
| **Freeze** | Two variants. *Signer freeze*: prevention of spending by withholding required signer cooperation. *Platform freeze*: suspension of an account or service workflow, such as withdrawals, authentication, or signing requests, regardless of any signer's willingness. Scenario tables mean signer freeze unless they say otherwise. Neither covers every source of unavailability. |
| **Loss versus compromise** | Loss removes legitimate access. Compromise gives an adversary access. A missing backup may be both; assess the consequences separately. |

These distinctions follow the descriptor, Miniscript, and Taproot specifications. [Descriptor format](https://bips.dev/380/), [Miniscript specification](https://bitcoin.sipa.be/miniscript/), [Taproot spending rules](https://bips.dev/341/)

For Taproot, inspect both the internal-key construction and the complete intended script tree. Establish who can produce a key-path signature, or document the construction that makes that path unavailable. A script-tree review alone cannot exclude a key-path bypass. Re-derive the output from the supplied policy and match its scriptPubKey to the actual UTXO. This verifies the commitment, not exclusive possession of private material. [Taproot descriptors](https://bips.dev/386/)

### 1.3 Coverage by dimension

Custody authority, signing mechanism, backup method, and recovery policy are separate dimensions. A split backup can protect one key of a multisig wallet. A threshold protocol can implement a signing key within a larger script. Device isolation and online exposure are further implementation choices.

| Configuration | Spending authority under the stated configuration | Unilateral holder exit, with retained material |
| :---- | :---- | :---- |
| **Baseline: holder single-signature** | The holder's signing key, including any usable copies | With key material, wallet metadata, and usable software |
| **A: distributed institutional 2-of-3** | Any two institutional keys | None provided to the holder |
| **B: collaborative 2-of-3** | Both holder keys, or one holder key and the service key | With both holder keys and sufficient wallet metadata |
| **C: multiple timed paths** | Each currently eligible path; earlier paths remain available as later ones mature | When the holder path is eligible and usable |
| **D: holder-controlled 2-of-3** | Any two of the holder's keys | With two keys and sufficient wallet metadata |
| **E: joint signing with delayed holder exit** | Holder plus service; holder alone after the specified delay | Per eligible UTXO |
| **F: split backup of a single-signature secret** | One signing key; sufficient backup shares can restore it | With sufficient shares, any required passphrase, metadata, and compatible recovery software |
| **G: threshold signing** | An authorized set of shares; consensus verifies the resulting signature | Depends on the available subset, protocol data, software, and recovery arrangement |
| **H: account with external spending authority** | The external controller's underlying signing arrangement | No unilateral exit unless separately demonstrated |
| **I: 2-of-3 escrow** | Any two of the transaction parties and dispute signer | Neither transaction party alone, absent another path |

These rows describe authorization, not relative safety. Sections 3–6 supply the workflow checks needed to evaluate a deployment.

## 2. Configuration assessments

### Baseline — holder-controlled single-signature custody

One key authorizes spending. Online software, an isolated signing device, and an offline signing process can all implement this authority arrangement; their exposure and verification procedures need separate examination. Single-signature descriptors exist and identify the script and derivation needed to locate funds. [Single-key output descriptors](https://bips.dev/382/)

| Review point | Consequence and evidence requirement |
| :---- | :---- |
| **Unauthorized spending** | A usable copy of the relevant private key is sufficient. Enumerate the active signer, backups, exports, and restore environments. |
| **Availability** | No external cosigner is required by this configuration. That does not establish independence from a particular application or recovery service. Demonstrate another usable spend workflow. |
| **Recovery** | Retain the applicable secret format, derivation paths, script type, account information, and any additional secret required to restore. Demonstrate address recovery and signing. |
| **Transaction integrity** | Establish how the recipient is authenticated and compared with the signed transaction. An accurate display cannot correct an already substituted payment instruction. |

Where a mnemonic passphrase is used, distinguish the mnemonic from the derived binary seed. Under BIP 39, both mnemonic and passphrase determine the seed; a copied derived seed or spending key does not additionally need that passphrase. A wrong passphrase produces another wallet rather than a universal “incorrect password” result. [Mnemonic derivation](https://bips.dev/39/)

### 2.1 Scenario A — distributed institutional custody

Three institutional controllers each have one key; an on-chain 2-of-3 policy is the only spending path. The holder has no signing key. The following consequences are deductions from that allocation and the specified multisig threshold. [Multisig descriptors](https://bips.dev/383/)

| Review point | Consequence and evidence requirement |
| :---- | :---- |
| **No single institution can spend** | Supported by the allocation only if no institution controls a second key or an alternate path. A descriptor establishes public keys, not their exclusive control. |
| **Only the holder can authorize movement** | Not enforced by the stated script: two institutional keys suffice. Assess customer authorization as an operational control, including who can override it. |
| **One institution can freeze the wallet** | One refusal leaves two signers; two refusals block this path. Shared dependencies can make multiple signers unavailable together. |
| **The holder can exit independently** | Contradicted by this configuration unless an additional exit mechanism exists. |

Review control over backups, administrative access, signing requests, and recovery ceremonies. Shared ownership, infrastructure, or jurisdiction are dependencies to investigate; they do not by themselves prove identical behavior or a common outage. Insurance or an assurance report does not replace this authority mapping.

### 2.2 Scenario B — collaborative custody with a holder quorum

The sole spending policy is 2-of-3. The holder controls two independent keys and a service controls the third. The holder can satisfy the threshold without the service; the service's single key cannot satisfy it alone. This is an allocation consequence, conditional on the actual keys and absence of other paths.

| Review point | Consequence and evidence requirement |
| :---- | :---- |
| **Setup substitution** | Replacing one holder key gives an unrelated attacker one key. It gives an attacker controlling the service key a quorum. State which keys and software the attacker controls. |
| **One holder key unavailable** | The remaining holder key and service can spend. The service now has a veto over that recovery path. |
| **One holder key compromised** | Unauthorized spending additionally requires the other holder key or the service's signature. Assess service authorization independently. |
| **Exit** | Demonstrate spending with both holder keys and retained wallet metadata, without service authentication or access to its servers. |

Verify each signer's actual key in the intended policy and compare the policy and derived address across trusted displays or independently authenticated records. A short fingerprint is an identifier, not sufficient proof of key identity. Registering an incorrect policy preserves the error. [Secure multisig setup](https://bips.dev/129/)

### 2.3 Scenario C — custody with multiple timed spending paths

Review the actual script rather than inferring timing from a contract term. For a configuration specifying a holder-and-service path, a delayed recovery-partner-and-service path, and a later holder-only path, enumerate which paths are eligible at each UTXO's current age or chain height/time. This is an analytical description of those conditions, not an assertion about a deployment.

Minimum-time conditions open paths; they do not disable earlier ones. The service and recovery partner can remain authorized after the holder-only path opens. A holder-only path therefore establishes an additional exit option, not exclusive control. Moving to a new output policy is required to remove an old path from the funds being moved. Delayed recovery constructions are publicly specified. [Absolute-lock recovery examples](https://bips.dev/65/), [Relative-lock examples](https://bips.dev/112/)

Require the exact policy, key-to-controller mapping, timing units, relevant UTXOs, and a demonstrated spend for each promised recovery route. For inheritance, separately inspect how successors obtain the required material and authority. A timelock does not verify death, incapacity, or entitlement.

### 2.4 Scenario D — holder-controlled 2-of-3

The holder controls all three keys. Any two satisfy the stated policy. Distinct devices do not establish distinct seed generation or separate backup access.

| Review point | Consequence and evidence requirement |
| :---- | :---- |
| **One key permanently unavailable** | The other two suffice if the complete spending policy can still be reconstructed. Loss of one device need not be loss of its key. |
| **Wallet metadata unavailable** | Two private keys do not necessarily supply the third public key, derivation paths, or original script. Inspect remaining exports, signer records, and backups before concluding permanent loss. |
| **Coordinator compromise** | Verify policy at setup, receiving addresses before funding, and transaction outputs and change at spending. |
| **Signer policy validation** | Require authenticated policy verification; persistent storage is one implementation choice. An independent display and stored registration are distinct properties. |

Metadata includes the actual key ordering or sorting rule and the script wrapper. Published derivation conventions assist interoperability but do not prove that a particular wallet used them. [Multisig derivation](https://bips.dev/48/), [Public-key ordering](https://bips.dev/67/)

The signer must recognize the intended policy and verify that change remains protected by it. Storage alone does not establish this behavior, and a stateless design is not automatically incapable of it. Inspect the actual implementation and user confirmation flow. [Wallet policies](https://bips.dev/388/)

### 2.5 Scenario E — joint signing with delayed unilateral recovery

An immediate path requires the holder and service; a delayed path requires the holder alone. A service-side second factor gates that service's participation. It is not an additional consensus signature requirement. Script-enforced delayed exit and a separately stored, pre-signed refund are different recovery arrangements; establish which is present. [Delayed recovery and refund mechanisms](https://bips.dev/65/)

For relative locks, record the encoded value and type. BIP 68 supports up to 65,535 blocks or 65,535 units of 512 seconds. The latter is about 388 days; the block-based limit is about 455 days at the nominal ten-minute interval, not a guaranteed calendar duration. Time-based eligibility uses median-time-past. [Relative-lock encoding](https://bips.dev/68/), [Lock-time clock](https://bips.dev/113/)

A fresh relative delay applies to new outputs carrying that policy, including change when created. Spending some UTXOs does not reset untouched UTXOs. A transaction need not create change. Determine exit eligibility per output.

Before maturity, service refusal blocks the joint path. After maturity, the holder can use the delayed path if the required key, metadata, and software remain available. The same delayed path is available to an attacker who has compromised that holder key. Provider refusal no longer prevents that spend. Permanent loss of every usable copy of the holder key defeats both stated paths.

The delayed holder path has no deadline of its own: once mature, it stays available. Deadlines arise where a competing path, or a counterparty's spend, is the event to prevent; see §3.1. Track maturity per output on the active chain, since a reorganization can shift it.

Do not infer display independence from this policy. Assess whether signing and transaction presentation share a compromised component, and what independent verification exists.

### 2.6 Scenario F — split backup of a single-signature secret

This is a backup method. Record which secret form is split: a mnemonic, the binary seed derived from it, an extended private key, or a raw private key. Each restores a different scope and carries different passphrase and derivation requirements. A split mnemonic still needs any mnemonic passphrase; a split derived seed does not; a split extended private key restores only its subtree; a split raw key restores one key. In the configuration assessed here, restoration yields material from which one signer derives its key. The backup threshold is not a consensus signing threshold. Threshold secret sharing and threshold signing are distinct operations. [Secret-sharing construction, Appendix C](https://www.rfc-editor.org/rfc/rfc9591.html#appendix-C)

SLIP-39 is a published share format for this purpose. It splits a master secret into mnemonic shares, supports two-level group thresholds, and applies its own passphrase encryption, under which every passphrase yields a valid but different wallet. It is not a share encoding of a BIP 39 mnemonic. Converting an existing BIP 39 wallet requires splitting the 512-bit derived seed, which produces longer shares and carries over only one mnemonic-and-passphrase combination; the specification instead recommends moving funds to a new SLIP-39 wallet. Record which share format is in use and which recovery software implements it. [SLIP-39 at commit 78c87bc](https://github.com/satoshilabs/slips/blob/78c87bc63ba1e4479dad7ffd3b18584430d8efb6/slip-0039.md)

For a simple t-of-n backup, losing one share preserves recoverability when n−1 is at least t. A threshold above one is not required for loss tolerance. Separately, compromise of sufficient shares can expose the restored secret, subject to any additional protection. Compromise of the active signer can bypass the need to obtain backup shares.

Assess share format, thresholds, any group structure, passphrase requirements, compatibility, and the environment where restoration collects secrets. Demonstrate restoration without the original service. Single-signature wallet metadata remains relevant; “no cosigner set” does not mean “no descriptor” or “no recovery metadata.”

Apply this backup assessment separately to each protected key when shares are used inside a multisig arrangement.

### 2.7 Scenario G — threshold or aggregate signing

A threshold-signature protocol allows an authorized subset of share holders to generate a signature under one public key. Its threshold is cryptographically enforced under the protocol's assumptions. Consensus verifies the resulting signature without separately checking the participant count. This differs from an application approval policy. Review key generation, share allocation, nonce handling, protocol version, and implementation evidence. [Threshold-signature specification (FROST, RFC 9591)](https://www.rfc-editor.org/rfc/rfc9591.html)

Do not assume every aggregate-signature scheme supports arbitrary t-of-n signing. Some require all participants: MuSig2 is an n-of-n scheme, while FROST supports t-of-n. Verify the actual protocol and access structure. [Aggregate multisignature specification (MuSig2, BIP 327)](https://bips.dev/327/)

| Review point | Consequence and evidence requirement |
| :---- | :---- |
| **No party has unilateral authority** | Map shares, usable backups, dealer-generated material, and recovery access to controllers. Absence of a mnemonic proves none of these properties. |
| **Policies protect spending** | Identify who enforces each limit or approval, who can change it, and whether another authorized subset bypasses it. |
| **Provider-independent exit** | Demonstrate an authorized subset signing outside the provider's infrastructure, or a documented recovery/export route. Full-key reconstruction is not inherently required for a withdrawal. |
| **Export retires old authority** | Exporting a full key does not invalidate existing shares. Move funds to a new policy to remove the old signing authority over those funds. |

The public output policy remains relevant, especially when the aggregate key is combined with script recovery paths. Record protocol data and derivation information needed for recovery. “One signature on-chain” establishes neither one controller nor the absence of a quorum.

### 2.8 Scenario H — an account with external spending authority

When the holder has account credentials but no usable signing or unilateral recovery path, assess withdrawal authorization and underlying custody separately. The external controller may use single-signature, multisig, or threshold signing. Those mechanisms do not by themselves give the account holder an exit path.

Require evidence connecting the account entitlement, withdrawal process, and underlying funds. Inspect deposit attribution, withdrawal destination changes, authentication recovery, privileged overrides, and reconciliation. A public address or valid signature is evidence about an output or key; neither alone establishes the completeness of account liabilities or the holder's contractual rights. Those are separate evidence requirements, outside this document's protocol findings.

Account-level controls can block withdrawal regardless of the underlying signing arrangement: account suspension, frozen withdrawals, identity-verification holds, withdrawal-address lockouts, or a legal or regulatory hold. Each is a platform freeze in the §1.2 sense. Identify who can impose each control, on what grounds, how it is lifted, and whether the holder has any path that does not pass through the account. Under this configuration, the expected answer to the last question is none.

### 2.9 Scenario I — escrow and shared control

A 2-of-3 escrow assigns keys to two transaction parties and a dispute signer. Any pair can satisfy the threshold. The dispute signer cannot spend alone, but can cooperate with either party; the script does not decide whether their action complies with the agreement. Neither transaction party has unilateral exit under the plain policy. [Multisignature escrow specification](https://bips.dev/11/)

Assess agreement and destination verification, signer identity, dispute authorization, availability, and any timeout or refund path. A pure 2-of-2 instead requires both keys and has no tolerance for permanent loss of either key unless another recovery mechanism exists. Pre-signed transactions can change what remains possible after key loss, so include them in the authority inventory.

### 2.10 Payment channels and other off-chain arrangements

A funding-output descriptor alone is insufficient for a payment channel. For the commitment-and-revocation protocol referenced here, review the current enforceable state, retained signatures, revocation material, unilateral-close transactions, delayed outputs, pending conditional payments, and the ability to monitor and respond before applicable deadlines. An old state backup is not interchangeable with a current state. Settlement may require several on-chain transactions and adequate fees. These requirements follow the documented commitment and settlement structure. [Channel transaction specification, BOLT 3 at commit 444805d](https://github.com/lightning/bolts/blob/444805d12ab98c30006173bb190cd9d6fce9e405/03-transactions.md)

For other off-chain arrangements, identify the exact published protocol before assigning findings. Record what the holder owns or controls, who controls the backing outputs, and every dependency of redemption. Do not transfer the conclusions of an on-chain multisig row to an unexamined off-chain design.

## 3. Review the complete custody lifecycle

Apply this worksheet to every configuration. Record actual evidence and observed results; a proposed test is not a passed test. Recovery exercises should use designated test funds and material rather than exposing production secrets to a review environment.

| Stage | Questions the review must resolve | Evidence to retain |
| :---- | :---- | :---- |
| **Creation** | Who generates each secret, on what device and firmware? How was that device obtained and its firmware authenticated? Who can copy, export, replace, or restore the secret? Do recovery administrators span a quorum? | Generation and backup procedures, authority map, implementation versions, procurement and firmware-verification records |
| **Setup and funding** | Do intended keys and policy match across participants? Is the verified receiving output the one funded? Are all alternate paths included? | Public-only policy, authenticated key records, address comparisons, funding outputs |
| **Routine spending** | Who requests and approves? How is the recipient authenticated? What exactly is signed, including fees and change? Who can update signer firmware or a remote signing service, and could an update change what is signed or displayed? | Approval rules, signer verification behavior, transaction records, update authority and firmware history |
| **Backup and restoration** | Which secrets, metadata, passwords, and software are required? Which common failures affect multiple copies? | Recovery inventory and a recorded restoration exercise |
| **Loss or compromise** | Which material is unavailable, and which may be held by an adversary? Can the remaining authority move funds to a safe policy? | Separate loss and compromise findings, migration procedure |
| **Recovery and succession** | Who obtains authority, by what evidence, after what delay? Does recovery bypass normal approvals? | Every recovery path, successor access procedure, demonstrated spend |
| **Rotation and exit** | Which outputs retain the old policy? Are old backups or shares still usable against them? Can the replacement workflow operate independently? | Old-to-new output mapping, new recovery records, exit exercise |
| **Broadcast and settlement** | Can valid transactions reach the network and confirm within any required window? How are fees and dependent transactions handled? | Broadcast alternatives, fee procedure and budget, fee-bumping outputs, confirmation and deadline monitoring (§3.1) |

A signer's verification behavior is only as trustworthy as the code performing it. Establish how firmware and signing software are authenticated before first use and at each update, who holds update-signing authority, and whether the build can be reproduced from published source. Where signing occurs in a remote or coordinated service, treat that service's operator, infrastructure, and administrators as control points in the authority map. Device diversity limits a defect to the devices sharing it; it does not authenticate any one device (§4).

Transaction interchange data can carry scripts, derivation information, partial signatures, and signing constraints. Verify the actual transaction and signature-hash behavior rather than treating an approval action as proof of what was authorized. Retain required pre-signed recovery transactions as recovery artifacts, with their covered outputs and restrictions. [Partially signed transaction format](https://bips.dev/174/)

### 3.1 Deadlines, fees, and reorganizations

Minimum-time conditions open paths; they do not close them. A delayed path therefore creates a deadline only when another path's maturity, or a protocol's response window, is the event to be prevented. Examples in this document: moving funds before a recovery path the holder no longer wants becomes eligible (Scenario C), responding to a revoked channel state within its delay (§2.10), and any pre-signed transaction that must confirm before a competing spend. For each deadline, record the triggering height or time, the transaction that must confirm before it, and who must act.

Feasibility depends on confirming at a feerate that is not known in advance. A pre-signed transaction commits to its fee when signed. Raising it later requires either a replacement signed by the required keys or a child transaction spending an output the acting party can sign. Record which outputs support fee bumping, which keys a replacement needs, and a fee budget set before the deadline approaches. If the only bumping route requires the counterparty whose action the deadline guards against, the fee plan depends on that counterparty.

Replacement, package acceptance, and the limits that make pinning possible are node relay policy, not consensus. A party able to spend an output of a pending transaction can sometimes attach transactions that make a replacement or child uneconomic or unrelayable under that policy. Policy differs across implementations and versions. Record the policy the procedure relies on and an alternative broadcast route. [Opt-in replacement signaling](https://bips.dev/125/), [Ancestor package relay](https://bips.dev/331/), [Topology restrictions for pinning](https://bips.dev/431/)

A chain reorganization can move or remove the confirmation of the transaction that created an output. A relative lock counts from that output's confirmation on the active chain, so maturity can arrive later than computed, and a spend valid at the old tip may be non-final at the new one. Time-based locks use the active chain's median-time-past. Monitor from before maturity, re-evaluate eligibility after a reorganization, and do not treat a single confirmation of the creating transaction, or of a deadline-sensitive spend, as final. [Relative-lock encoding](https://bips.dev/68/), [Lock-time clock](https://bips.dev/113/)

## 4. Independence, quorum size, and shared failures

For a plain t-of-n policy with distinct keys, no alternate spending route, and all other recovery dependencies available:

- A spend requires valid signatures from t distinct keys. An attacker may obtain these through key compromise or by inducing authorized signers to sign an unauthorized transaction.
- Up to n−t unavailable keys leave a signing quorum.
- Withholding n−t+1 keys prevents that path from being satisfied.

These are threshold counts, not probabilities or counts of independent organizations. For 2-of-3 the corresponding values are two, one, and two; for 3-of-5 they are three, two, and three. A two-key compromise therefore has a different outcome in the two configurations. Additional keys do not automatically correct a substituted policy, shared administrator, or missing recovery artifact. [Multisig threshold semantics](https://bips.dev/383/)

Implementation diversity can limit the reach of a defect confined to one implementation. That conclusion requires separate affected components and uncompromised verification elsewhere; different labels do not establish it. Record shared entropy sources, libraries, update authority, procurement, setup hosts, backup locations, and operator access. Do not claim a particular failure frequency without cited incident evidence.

Reusing a passphrase across independent mnemonics does not merge their seeds. It does create a common recovery dependency: loss of that passphrase can defeat all restores that require it. Disclosure removes that additional protection but does not reveal the separate mnemonics. Copying the same underlying seed is a different failure of independence. This distinction follows the mnemonic-to-seed derivation. [BIP 39](https://bips.dev/39/)

Compare transaction costs for the actual construction at the same feerate. A larger conventional multisig witness has a different size from a smaller one, but an aggregate key-path signature need not grow with participant count. Script choice and the exercised path matter. [Witness-script construction](https://bips.dev/382/), [Taproot signature validation](https://bips.dev/342/)

## 5. Privacy and coercion

An xpub exposes non-hardened descendant public keys in its subtree. It does not automatically reveal every account or every multisig address: deriving the latter also requires the other keys and policy. A complete public-only descriptor can expose the addresses within its defined range and branches. Record what each service actually receives, retains, and queries. [Hierarchical key derivation](https://bips.dev/32/)

Public data alone does not authorize spending. However, a parent xpub combined with a corresponding non-hardened descendant private key can expose the parent private key. Treat this documented compound failure separately from privacy loss and from compromise of a full wallet quorum. [Extended-key security implications](https://bips.dev/32/)

Wallet activity also leaks through transactions and services. Assess each separately:

- **Input clustering.** Spending several outputs in one transaction links them under the common assumption that one entity controls all inputs. Record the coin-selection behavior and whether the holder can control it.
- **Change identification.** Change can often be distinguished by script type, amount precision, or output ordering. Change returned under a distinctive policy can be linked to the wallet when later spent. Record how change is constructed.
- **Spend-time policy disclosure.** Spending a script-based multisig output publishes the complete witness script, including every public key in it and the threshold. A Taproot script-path spend reveals the executed leaf and its inclusion proof, not the other leaves; a key-path spend reveals no script. A distinctive policy template, such as an uncommon threshold or timelock, can make one wallet's outputs recognizable as a group once spent. [Taproot spending rules](https://bips.dev/341/)
- **Address reuse.** Receiving more than once to one address links those payments regardless of other measures.
- **Service-side retention.** Coordinators, signing services, and blockchain-data servers may record network addresses, queried addresses, xpubs, labels, transaction history, and identity linkage. Request what is collected and retained, for how long, and who can be compelled to produce it.
- **Broadcast origin.** The first relaying node or service can associate a transaction with a network location. Record the broadcast route.

For coercion, assess which signing material and approvals a person can access within the relevant time. A holder-controlled quorum may be geographically separated; a service approval may be obtainable under coercion. Neither ownership label establishes resistance. A delay constrains only paths that actually require it, and a mature fallback may bypass a service review. Record the available paths and practical access procedure rather than assigning a universal ranking.

## 6. Questions that make a claim reviewable

| If the claim is | Request |
| :---- | :---- |
| **Only I can authorize spending** | Show that every available spending path requires authority exclusively controlled by me. Identify copies, recovery overrides, and future paths. |
| **No single party can spend** | Map every authorized key/share combination to controllers, including administrators and backup access. |
| **The policy is verifiable on-chain** | Supply the complete public policy; independently derive and match the relevant outputs. Separately establish control of the secrets. |
| **My funds cannot be frozen** | Show a usable path without each dependency being assessed, including required data, software, authentication, and broadcast access. Address signer freeze and platform freeze separately. |
| **Recovery is guaranteed** | Specify the failure being survived, retained artifacts, timing, and an observed recovery result. |
| **A device verifies everything** | Demonstrate policy authentication, recipient verification, fee checks, and change recognition for the actual script. |
| **Backups remove the single point of failure** | Identify whether redundancy protects stored recovery material, active signing, or both. |
| **No mnemonic is needed** | Show the share/secret inventory, derivation data, recovery authority, and independent exit procedure. |
| **More signers are safer** | State which compromise and loss combinations change outcome, and identify shared dependencies. |
| **Audited or insured** | Provide the applicable report or contract and its scope. Keep technical control and contractual protection as separate findings. |

## 7. Limits and source use

The linked public specifications were consulted on 6 October 2026 and support the mechanisms discussed. They are not deployment attestations. Historical examples in a specification establish that a workflow is documented; they do not establish current adoption, implementation quality, or present-day relay behavior beyond the rule cited.

This review framework does not supply incident likelihoods, legal classifications, insurance interpretations, or conclusions about an unnamed provider. It deliberately leaves implementation and organizational findings unresolved until evidence is inspected. A protocol-valid spend, operational independence, and successful account withdrawal are separate claims.

For each actual assessment, preserve the source version and review date, wallet or protocol version, relevant policy and outputs, and the verification result. Reassess when funds move to a different policy, signing or recovery authority changes, or an implementation changes. An unexamined path remains an unexamined path; a familiar label does not close it.
