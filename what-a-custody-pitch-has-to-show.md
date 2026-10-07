**CONFORMANCE NOTE  ·  PUBLIC SCENARIOS**

**What A Bitcoin Custody Pitch Has To Show**

*A claim-versus-artifact note. Patterns, not providers. No finding in this document is a statement about any firm's intent, solvency, or character.*

| Document | what-a-custody-pitch-has-to-show.md |
| :---- | :---- |
| **Date** | 6 October 2026 |
| **Audience** | A new holder choosing among public pitches, and a security reviewer asked to look at one. A new holder can start at §1.3 and §6, and use the glossary in §1.2 for the rest. |
| **Method** | Same shape as an evidence-based audit: stated claim, scope, artifact that would establish the claim, what public materials typically supply instead. |
| **Revision** | Replaces the single-standard analogy with an audit posture. Adds a glossary, a summary matrix, a threshold-signature scenario, and a section on privacy and coercion. Corrects the on-chain verification advice and the quorum arithmetic. Tightens the multisig count, the B and G matrix cells, and the two 2-of-2 cases to match the prose. Still no firm, person, or product names. |
| **Not this** | Not a vendor review. Not a ranking. Not an allegation. Names are omitted on purpose. A reader who maps a pattern onto a firm has done their own work. |

# **1\. Why this shape**

A newcomer is not offered a script. They are offered a sentence. "No single party can move your funds." "On-chain verifiable." "Different hardware vendors, so a supply-chain bug cannot reach you." "Insured by a market you have heard of." Each sentence can be true, false, or true of a different object than the one the listener pictured.

The useful review is the one an auditor already knows how to do. A claim has a scope. The scope has an artifact. The opinion covers what the evidence supports, for that scope, at that date. Anything outside the scope is not a finding of wrongdoing. It is unexamined. Custody pitches deserve the same treatment.

The posture is borrowed from security validation and assurance work generally, not from any one standard. A validated module has a boundary, a stated mode of operation, and a policy that says what was demonstrated. A brochure that says "audited" or "compliant" without the report and its scope has not established the claim. The same standard is applied here. There is no lab in this market. The holder, or a reviewer the holder trusts, is the lab. Absence of the artifact is recorded as "not established." It is not recorded as misconduct.

## **1.1 What a claim needs**

| To establish | The artifact |
| :---- | :---- |
| **Who can spend** | The output descriptor or the miniscript, with every leaf and the condition that makes it valid. A diagram is not this artifact. |
| **Who can freeze** | The same descriptor, read for paths that require a party the holder does not control, and for when those paths expire. |
| **A control is consensus-enforced** | The condition appears in the script. A video call, a ticket queue, an allowlist in a database, and a hardware authenticator presented to a website do not. |
| **A control is device-enforced** | The signer stores the cosigner set and displays destination, amount, fee, and change. The coordinator's screen does not count. |
| **An assurance report applies** | Report, scope, date, and type. A Type I attestation is design at a point in time. It is not operating effectiveness, and it is not a certificate that the script matches the brochure. |
| **Insurance changes the outcome** | The insuring clause, the exclusions, the proof of loss, and the limit. The name of the market is not this artifact. Insurance is a contract. It is not a reduction in attack probability. |

Verdicts used below are only these: Established, Partial, Not established, Not applicable. A reviewer who disagrees should replace the verdict and keep the artifact column. That is the point of writing it this way.

## **1.2 Terms**

| Term | Meaning here |
| :---- | :---- |
| **Descriptor** | A short text record of the spending rules: which keys, which threshold, which paths, which timelocks. With it, compatible software can find the coins and build a spend. Signing still needs the keys. Without it, seeds alone may not be enough. Also called "the map" below. |
| **Leaf** | One way to spend. A script can have several, each with its own signers and its own timelock. Each leaf is checked on its own. |
| **Extended public key (xpub)** | The public half of a key, from which every receive address can be derived. Not a spending secret. Anyone holding it can see the balance and history. |
| **Miniscript** | A structured way to write spending rules with several leaves. More expressive than a plain multisig, and supported by fewer wallets. |
| **Coordinator** | The software that assembles the wallet at setup and builds each transaction for the devices to sign. It sees everything and signs nothing. |
| **Timelock** | A condition that makes a leaf valid only after a time. Absolute timelocks open at a fixed block or date. Relative timelocks open a number of blocks after each coin confirms, and restart when the coin moves. |
| **Module boundary** | Everything a claim depends on: keys, devices, software, files, people, procedures. Not just the parts in the pitch. |

## **1.3 At a glance**

One row per scenario, plus a baseline for comparison. Each cell is the claim the detail section checks. Where a cell says "varies," the descriptor and the devices decide.

| | Who can spend today | Who can freeze | Holder exit without the service | Map required | Setup-substitution exposure | Independent display | On-chain quorum |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Baseline: single-sig + passphrase** | Holder (anyone with seed and passphrase) | No one | Yes, widest tooling | No | Device only | One device | None |
| **A: distributed institutional** | Any 2 of 3 institutions | Any 2 institutions, by refusal | No | Held by the service | Service side | Not applicable, holder does not sign | 2-of-3, holder not in it |
| **B: collaborative, holder quorum** | Holder alone; or 1 holder key plus firm | No one, while holder has both keys, the configuration file, and a coordinator other than the firm's | Yes, with both keys, the configuration file, and another coordinator | Yes | Yes, the web application assembles the wallet | Holder devices, varies | 2-of-3, holder holds 2 |
| **C: time-layered joint** | Holder plus firm; later recovery partner plus firm; later holder alone | Firm, until the sovereign leaf matures | After expiry, if other software supports the script | Yes, and coordinator support for it | Yes | Varies | Per leaf |
| **D: self-directed 2-of-3** | Any 2 of the holder's 3 | No one | Yes | Yes, critical | Yes, the coordinator | Only if devices register the cosigner set | 2-of-3, all holder |
| **E: decaying 2-of-2** | Holder plus provider; holder alone after the timelock, per coin | Provider, until maturity, restarting on change | After maturity | Recovery data and tool | The provider's application | None, the phone does everything | 2-of-2, decaying to 1 |
| **F: share-split seed** | Anyone with the device and PIN, or a threshold of shares | No one | Needs an implementation of the share format | No | Device only | One device | None |
| **G: threshold signature (MPC)** | Per the provider's policy; the chain sees one key | Provider, if its share is needed to meet the threshold | Depends on key export or recovery | Not applicable | Provider side | The provider's application | None visible |

# **2\. Seven scenarios, as they are usually pitched**

These are composites of language a new holder will meet. They are not profiles of any company. Several real products mix two of them and market the mix under one name. When that happens, assess each leaf of the script on its own. The brand is not the boundary. Two of the seven, F and G, are not multisig on-chain. A through E are, including the decaying 2-of-2. They are included because they are offered in the same sentence as multisig.

## **2.1 Scenario A — distributed institutional custody**

Pitch. Three independent institutions each hold a key. Two must sign. No single institution can move the funds. The holder never touches a signing device. The holder initiates, and each institution verifies the holder, often on a video call, often with a hardware authenticator. Insurance against institutional failure is offered as an add-on.

Module boundary, as pitched. The holder hears "my bitcoin, checked by me." The objects in the pitch are an account, an authenticator, a verification procedure, and three institutional signers.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| No single institution can spend | **Partial** | True of a 2-of-3 if the keys are actually split and the ceremony holds. The artifact is the descriptor plus a ceremony summary. Marketing asserts both. Newcomers are rarely shown either. |
| The institutions are independent | **Not established** | Independence is ownership, jurisdiction, and operations, not logos. Two signers under one court, one parent, or one hosting provider are one failure for a legal order or an outage. The artifact is who holds each key, where, and under which law. This is the argument of §3, applied to institutions. |
| Only the holder can cause a spend | **Not established** | Initiation and a video check are application controls. They constrain the institution that chooses to follow them. They do not constrain the script. If the three keys are institutional, any two institutions can produce a consensus-valid spend without the holder. |
| The holder can exit without the institutions | **Not established** | Nothing in the usual pitch gives the holder a key. Freeze and recovery are the same fact, read from two sides. |
| Insurance removes the custody risk | **Not applicable** | Insurance prices a defined loss after a claim. It does not change who can sign. Exclusions, proof of loss, and adjudication sit outside the module. |

How to read it. This scenario can be a reasonable product. It is institutional custody with a separation-of-duties improvement over a single custodian, plus an optional contract. It is not self-custody, and it is not "only I can move it" unless a holder key is in the script. Asking for the descriptor is the whole review.

## **2.2 Scenario B — collaborative custody, holder holds the quorum**

Pitch. A 2-of-3. The holder has two keys, on two devices, with two seed backups. The firm holds the third, in cold storage, as backup and as a co-signer of convenience. The holder can spend with both of their own keys and never call the firm. The firm cannot spend with its one key. If the holder loses one key, the firm co-signs a recovery. A configuration file lets the holder rebuild the wallet in other software if the firm is gone.

Module boundary, as pitched. Two holder secrets, one firm secret, a configuration file that is not a secret, and a web application that assembles the wallet at setup and builds the transaction the device is asked to sign.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| The firm cannot move the funds alone | **Established** | Follows from 2-of-3 if the third key is the only key the firm has, and no other leaf exists. This is a consensus fact. It is the property Scenario A does not have. It assumes the two holder keys in the descriptor are the keys on the holder's devices. See the next row. |
| The holder keys in the wallet are the holder's | **Partial** | At setup, the web application chose which extended public keys entered the wallet. A substituted holder key gives whoever substituted it two of three. Containment is each holder device registering the cosigner set and showing it, as in Scenario D. Where the devices do that, and the holder compared, this is established. |
| The holder can leave without the firm | **Partial** | True with both holder keys and the configuration file. Seeds alone do not identify the firm's extended public key or the paths. The file is an availability-critical part of the module, and it is often treated as paperwork. |
| Losing one key is safe | **Partial** | Losing one key does not strand the funds. It also does not leave theft impossible. One holder key plus the firm's signer is a valid spend. The firm's approval of that co-sign is a procedure. Procedures fail differently from scripts. |
| The application cannot redirect a payment | **Not established** | The web application constructs the transaction. Destination integrity is whatever the signing device displays. The firm cannot force the holder to read the device. Devices differ in what they show. |

How to read it. The on-chain claim is the strongest of the custody scenarios, and it is narrow. It says the firm is not a custodian of the spending quorum. It does not say the firm's website is outside the trust boundary, at setup or at spend, and it does not say a co-sign request is reviewed by anything stronger than the firm's own process. Legal process can reach the firm. It cannot reach the UTXO with one key. The holder exit is the control, and only while the holder still has both keys and the file.

## **2.3 Scenario C — time-layered joint custody**

Pitch. During an insured term, neither the holder nor the firm can spend alone. The holder has keys. The firm co-signs. If the holder loses every key, a recovery partner and the firm can restore access after a delay. When the term ends, a further delay passes and the holder can spend alone. The rules are in Bitcoin script, so they survive the firm. Inheritance and disaster recovery are described as built in.

Module boundary, as pitched. A miniscript with several leaves, each valid at a different time. A recovery partner. A policy term that is a calendar fact, not a block the holder picked. A sovereign leaf after expiry.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| Nobody can spend alone | **Partial** | Often true of the leaf that is valid today, and false of a leaf that becomes valid later. "Nobody" has to be checked per leaf. A recovery leaf that two service parties can satisfy is a second quorum, opened by a timelock. |
| The holder is not frozen if the firm disappears | **Partial** | True after the sovereign leaf matures, and only if the holder still has the keys that leaf requires. Until then, refusal to co-sign is a freeze. Time-bounded is not the same as absent. |
| The rules survive the firm | **Partial** | The rules survive in script. Spending under them needs a coordinator other than the firm's that can read this miniscript, build a spend for the leaf in question, and get the holder's devices to sign it. Fewer wallets support this than support plain multisig. The artifact is a named second implementation that has done it, not the statement that one could. |
| The holder can verify this without trusting the firm | **Not established** | Verifiable means the holder has the descriptor and has checked each leaf, including the recovery leaf and its timelock. A page that says the vault is verifiable has not performed the verification. |
| The layers match the brochure in force today | **Not established** | Public descriptions of this pattern have been more specific in some places than in others, and products under one brand have changed shape. The artifact is the descriptor for this vault, not the best explanation anyone has posted. |

How to read it. A recovery leaf is not a scandal. It is the mechanism that makes total key loss recoverable. It is also the mechanism by which two service parties can spend once the delay has passed. Both descriptions are the same script. Insurance, if purchased, is the compensation for that leaf, not a deletion of it. Score the leaf. Do not score the motive.

## **2.4 Scenario D — self-directed 2-of-3 with a desktop coordinator**

Pitch. Three hardware devices, ideally three manufacturers. A free desktop application builds the wallet and the transactions. Any two devices spend. No company holds a key. The holder can lose one device or one seed. Different vendors mean a flaw at one company cannot expose the funds.

Module boundary, as pitched. Three seeds, three devices, one descriptor, one coordinator on one computer, one operator. The pitch draws the boundary around the three logos. The module includes the coordinator, the computer, the descriptor backup, and the person who compared the fingerprints.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| Two signatures are required | **Established** | Consensus, once the descriptor is the one the devices think it is. This part is real. |
| The coordinator cannot redirect funds | **Not established** | At setup, the coordinator chooses which extended public keys enter the wallet. A substituted key makes the attacker a cosigner. At spend, the coordinator builds the transaction. Containment is each device storing the cosigner set and displaying the output. Several devices commonly recommended in the same breath do not store the set. The laptop screen is not containment. |
| One lost key is survivable | **Partial** | One lost seed is survivable. A lost descriptor is not, even with two seeds. There is no firm to call. The map is part of the module. |
| Mixed vendors mitigate supply-chain attack | **Partial** | See §3. The narrow claim is true. The sentence as pitched is not. |

## **2.5 Scenario E — decaying 2-of-2**

Pitch. A key on the holder's phone or desktop, and a second key held by the application provider. Both must sign. A second factor — a code, a prompt, an authenticator — protects the provider's signature. If the provider disappears, or the second factor is lost, the holder can spend alone after a wait. The wait is described as about a year.

Module boundary, as pitched. Two keys. A relative timelock on each deposit, starting when that deposit confirms, on the order of 26,000 to 65,000 blocks. Relative timelocks cap at 65,535 blocks, about fifteen months, so "about a year" sits near the consensus ceiling. A second factor that the holder experiences as part of sending. An open recovery tool for the case where the service is gone and the timelock has matured. One application that holds the holder key, builds the transaction, and shows it.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| The provider cannot move the funds | **Established** | True while the script is a 2-of-2 and the provider holds only one key. This is a consensus fact, and it is better than Scenario A on this one point. |
| The second factor is a second key | **Not established** | The factor gates the provider's willingness to sign. It is not a leaf in the script. A failure of that check is a failure of the provider's procedure, not of consensus. |
| The holder approves what they see | **Not established** | The holder key, the transaction builder, and the display are the same application on the same phone. Nothing independent of that phone shows the destination. The provider's co-sign is the only second check, and it is the provider's procedure. This is a weaker display position than Scenario B or D. |
| The holder can exit today | **Not established** | Not this scenario. Exit without the provider waits on the timelock for those specific coins. Moving the coins starts the wait over, and every send returns change with a fresh timer. A wallet in regular use always has coins inside the window. This is a freeze until maturity, not a holder quorum. |
| This is the layered joint-custody scenario | **Not applicable** | No recovery partner can spend without the holder. The sovereign leaf is the holder's own key, later. Scenario C has a leaf other parties can satisfy. This one does not. |

How to read it. A reasonable phone wallet with a time-bounded co-signer. Not self-custody today, and not institutional custody either. The question to ask is which deposits, and which change outputs, are still inside the window. A device that can register an ordinary multisig and export that registration is not this scenario. That device, used as one signer in a desktop coordinator, is Scenario D. The export is a second copy of the map. It helps descriptor loss. It does not add a spend rule.

## **2.6 Scenario F — one seed, split into shares**

Pitch. Remove the single point of failure without the complexity of multisig. One wallet, split into several recovery shares. Any threshold of shares restores the wallet. Shares can be stored apart. No configuration file. No second device required to send. Often offered by a hardware maker as the backup a new holder should use instead of a quorum.

Module boundary, as pitched. One secret. Shares of that secret. One device that holds the whole secret and signs. A share format that any restoring device has to implement.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| This is multisig, simplified | **Not established** | Multisig is several keys, and consensus requires a threshold of signatures. This is one key, split so that a threshold of shares rebuilds it. One device holds the whole key and signs alone. The chain cannot tell a share-split wallet from any other single-signature wallet. |
| Losing one share does not lose the funds | **Partial** | True of the backup, if the threshold was set above one and the remaining shares still meet it. False of signing. The device that holds the secret, usually the one that created it, holds the wallet at all times. So does whoever gathers the threshold of shares in one place. |
| No vendor boundary | **Partial** | The shares are an application format. If it is an open format with several implementations, exit is narrower than a seed in the common wordlist but not tied to one vendor. If it is proprietary, it is tied. The artifact is a second implementation that has restored from these shares. |
| No map to lose | **Established** | There is no descriptor, because there is no cosigner set. That advantage is real, and it is the advantage of not having a quorum. |

How to read it. A backup scheme for a single-signature wallet. Loss tolerance of the paper is real. Theft tolerance is the tolerance of one device, every day, not only at the moment of signing. A newcomer who accepts the pitch has changed modules without being told. Do not score it on the multisig rubric. Score it as single-signature plus a split backup.

## **2.7 Scenario G — threshold signatures, no seed phrase**

Pitch. No seed phrase to lose. The key is split among the holder's phone, the provider, and sometimes a backup party. No single party ever holds the full key, not even during signing. Policies — spending limits, approvals, allowlists — protect the funds. Recovery runs through a cloud backup or the provider.

Module boundary, as pitched. One key on-chain. Several key shares held by several parties. A threshold-signing protocol run by the provider's software on each party's device. A policy engine on the provider's servers.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| No single party can move the funds | **Partial** | True of the shares if the protocol and its implementation hold. The chain sees one key and one signature. The threshold is enforced by software, not by consensus. Implementation flaws in threshold-signing libraries are a publicly documented class. The artifact is the protocol specification, an audit of this implementation, and the list of share holders. A descriptor will not help, because the descriptor is single-key. |
| The provider cannot freeze the holder | **Partial** | Depends on whether the provider's share is needed to meet the threshold. If the holder's shares and a backup party can sign without it, refusal by the provider is not a freeze, though the signing software may still be the provider's. If the provider's share is required, refusal is a freeze until an export or recovery path is used. The artifact is the share map: who holds which share, and which combinations sign. |
| Policies protect the funds | **Not established** | Limits, approvals, and allowlists are application controls. The chain accepts any valid signature. |
| The holder can exit without the provider | **Not established** | Depends on a documented key export or recovery that produces a full key outside the provider's software. Where one exists, using it ends the scheme. What comes out is a single-signature wallet. |
| This is multisig | **Not established** | Same answer as Scenario F, from the other side. F has one key and several backups. G has several shares and one key. Neither is a quorum the chain enforces. |

How to read it. Multi-party custody with the quorum in the application, not the script. It can be a reasonable product, especially where the parties are institutions with their own audits. It is not "only I can move it," and "no seed phrase" means the recovery path is someone else's procedure. Score it on the software and the share holders, because there is no script to score.

# **3\. The mixed-vendor sentence**

The sentence a newcomer hears: use hardware from different manufacturers, so a supply-chain attack or a bug at one company cannot take the funds.

The narrow claim, which is established. An independent second implementation contains a failure of one implementation's random-number generator or signing oracle, provided the other seeds were generated on other implementations and not recombined by a shared passphrase. Seed-generation defects in wallet hardware and software have been publicly disclosed more than once, including in 2026. Any one of them is enough to demonstrate the class. Firmware updated after the fact does not repair a seed created on the affected build. In a mixed 2-of-3 that defect is one share. In a single-signature wallet, or in three devices of that same build, it is the wallet. Diversity is a real containment control for correlated implementation failure. Calling it nothing would be wrong.

The pitched claim, which is not established.

* One purchase channel, one setup computer, and one operator are one supply chain, regardless of the logos on the boxes.

* Different brands do not imply different secure elements, different recovery-word code, or different transaction parsers.

* The coordinator and the host sit above the vendor boundary. A substituted cosigner key at setup is not stopped by which factory made the genuine devices.

* The descriptor and the backup ritual are single-implementation by nature.

* Entropy the operator adds — dice rolled once and copied, a passphrase reused on all three — puts the correlation back.

An auditor would record the configuration that was actually tested: which devices register the cosigner set, where the descriptor is stored, whether setup happened on a machine the operator is willing to trust with a substitution attack. "Three brands" is not that configuration.

# **4\. Does a larger quorum help**

Pitch. 2-of-3 is the start. 3-of-5 is more secure, because an attacker needs more keys and the holder can lose more keys. The cost is more devices and more management, which is framed as the price of the improvement.

On the two axes that are actually about the quorum, the larger one wins. An attacker needs three shares instead of two. The holder can lose two shares instead of one. It is the smallest quorum that survives a double loss while still requiring three keys to steal. A 2-of-4 also survives a double loss, at a theft bar of two. The witness is larger, so the fee is higher. That is the entire consensus difference.

It does not change the class of control. A substituted cosigner key at setup, a coordinator that builds the transaction, a map nobody stored, a co-sign desk that approves on a procedure, and a recovery leaf two service parties can satisfy are all still present at 3-of-5. They were never quorum problems.

The overhead is not neutral. A 2-of-3 with a seed backup per key is six items to place. A 3-of-5 on the same rule is ten. The setup comparison Scenario D already fails is now done across five devices instead of three. Five seeds generated on one machine, five backups in one safe, or one passphrase reused across the set collapse toward one component. Correlated failure does not honor the threshold.

Who holds the extra keys matters more than the count. Two more keys in the holder's other locations raise the theft bar and the loss bar together. Two more keys at service parties raise the collusion surface and can remove the holder quorum. That is arrangement, already the difference between Scenario A and Scenario B. A larger threshold does not convert one into the other.

Verdict. Partial, and only for an operator who already runs a clean 2-of-3, already stores the map away from the seeds, and specifically needs to survive two independent losses. For everyone else it is more management on the same module. The failures that take funds are not the ones the extra keys touch. A 2-of-2 is the other direction, and it should not be sold as the safer small quorum. A plain 2-of-2 with no timelock is lost forever if either key is lost. Scenario E softens one side only: a lost provider key is a freeze until maturity, and a lost holder key is still forever.

# **5\. Two things the pitches leave out**

Privacy. Any party holding the extended public keys or the configuration file can derive every address, and with it the balance and history. That includes the firm in B and C, the institutions in A, and the provider in E and G. In D, a server the coordinator queries usually sees the addresses it was asked about, and sees the extended public keys only if the coordinator sent them. That is a narrower finding, and still a targeting finding. None of this is a spend risk. It is a targeting risk, and for many holders the likelier one. Ask who holds the public keys after setup, and for how long.

Coercion. A holder-held quorum is a strength against a firm and a weakness against a person standing in the holder's kitchen. Whatever the holder can do alone, the holder can be made to do. A co-signer that applies a delay or a review, or a timelock nobody can shorten, is a real control against that attack. It is the same control that makes A, C, and E able to freeze, and G where the provider's share is required. Score it in both directions. The scenarios are not ranked, because the threat a given holder faces decides which side of that trade matters.

# **6\. What a pleb can do with this**

No scenario above is disqualified. Each is a different module. The mistake available to a new holder is accepting a sentence from one scenario as a property of another.

| If the pitch says | Ask for |
| :---- | :---- |
| **Only you can initiate** | Show me the leaf that fails without a signature from a key I hold. |
| **No single party can move it** | List every party that holds a key. Then list every leaf, including the ones that are not valid yet. |
| **Independent institutions** | Who holds each key, in which jurisdiction, under which parent company? |
| **You can always recover** | Who signs that recovery, after how long, and what do I hold while I wait? |
| **Verifiable on-chain** | The descriptor, and the address I derive from it in software the firm does not run, matching the address that holds my coins. An explorer shows a hash until the coins move, and some leaves never appear on-chain at all. Not a screenshot of a dashboard. |
| **Insured** | The insuring clause and the exclusions. Separately, who can sign. Those are different documents. |
| **We are audited** | Which report, which scope, which date, Type I or Type II. Then whether the scope included the script. |
| **Use three brands** | Which of the three devices will show me the other keys, and the destination, on its own screen? |
| **You can lose one key** | What else, besides a key, reconstructs this wallet? Where is that copy? |
| **Simpler than multisig** | After I restore, how many signatures does the chain require? If the answer is one, it is not a quorum. |
| **No seed phrase** | Who holds the shares, what enforces the threshold, and how do I get a usable key out if you are gone? |
| **We co-sign, and you can recover later** | Who can satisfy the recovery leaf, and which of my coins, including change, are still inside the window? |
| **We only see your public keys** | Then you can see every address and balance. Who else can, and how long is it kept? |
| **3-of-5 is safer** | Safer against which failure? Show the one a third key actually closes. |

A firm that answers with the artifact has supplied what the claim needs. A firm that answers with a restatement of the pitch is asking the holder to validate the brochure. Those are observably different. Neither answer is a character reference.

# **7\. Limits**

* Patterns were taken from public marketing and public technical descriptions current in 2026\. They will drift. The method does not. Re-read the descriptor, not this note.

* No script was pulled from the chain. A holder with a vault can close every "not established" in an afternoon. This note cannot.

* A reader in this market will recognize some patterns. Recognition is not attribution. The document does not name a firm, a person, or a product, and it does not invite that mapping as a conclusion.

* Nothing here alleges an incident, a theft, a false statement made knowingly, or an inability to pay a claim. "Not established" means the artifact was not in the pitch.

*End of note. The verdicts are replaceable. The artifact column is the part to keep.*
