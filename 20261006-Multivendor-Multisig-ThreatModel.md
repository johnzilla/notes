**CONFORMANCE NOTE  ·  PUBLIC SCENARIOS**

**Assessing Bitcoin custody scenarios a pleb might encounter**

*A claim-versus-artifact note. Patterns, not providers. No finding in this document is a statement about any firm's intent, solvency, or character.*

| Document | CN-BTC-SCENARIOS-2026-10-06 |
| :---- | :---- |
| **Date** | 6 October 2026 |
| **Audience** | A new holder choosing among public pitches, and a security reviewer asked to look at one. |
| **Method** | Same shape as a module validation: stated claim, module boundary, artifact that would establish the claim, what public materials typically supply instead. |
| **Revision** | Adds decaying 2-of-2, single-seed share-splitting, and the quorum-size question. Still no firm, person, or product names. |
| **Not this** | Not a vendor review. Not a ranking. Not an allegation. Names are omitted on purpose. A reader who maps a pattern onto a firm has done their own work. |

# **1\. Why this shape**

A newcomer is not offered a script. They are offered a sentence. "No single party can move your funds." "On-chain verifiable." "Different hardware vendors, so a supply-chain bug cannot reach you." "Insured by a market you have heard of." Each sentence can be true, false, or true of a different object than the one the listener pictured.

The useful review is the one a conformance lab already knows how to do. A cryptographic module is not "trusted" or "untrusted." It has a boundary, a stated mode of operation, and a security policy. The certificate covers that boundary in that mode. Operating outside it is not a moral event. It is an unvalidated configuration. Custody pitches deserve the same treatment.

FIPS 140-3 is the analogy, used narrowly. The Cryptographic Module Validation Program does not publish a view on whether a vendor is a good firm. It publishes whether the claims in the security policy were demonstrated for a defined module, at a defined level, in an approved mode. A brochure that says "FIPS compliant" without a certificate and a security policy has not established the claim. The same standard is applied here. Absence of the artifact is recorded as "not established." It is not recorded as misconduct.

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

# **2\. Six scenarios, as they are usually pitched**

These are composites of language a new holder will meet. They are not profiles of any company. Several real products mix two of them and market the mix under one name. When that happens, assess each leaf of the script on its own. The brand is not the boundary. Two of the six are not multisig. They are included because they are offered in the same sentence as multisig.

## **2.1 Scenario A — distributed institutional custody**

Pitch. Three independent institutions each hold a key. Two must sign. No single institution can move the funds. The holder never touches a signing device. The holder initiates, and each institution verifies the holder, often on a video call, often with a hardware authenticator. Insurance against institutional failure is offered as an add-on.

Module boundary, as pitched. The holder hears "my bitcoin, checked by me." The objects in the pitch are an account, an authenticator, a verification procedure, and three institutional signers.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| No single institution can spend | **Partial** | True of a 2-of-3 if the keys are actually split and the ceremony holds. The artifact is the descriptor plus a ceremony summary. Marketing asserts both. Newcomers are rarely shown either. |
| Only the holder can cause a spend | **Not established** | Initiation and a video check are application controls. They constrain the institution that chooses to follow them. They do not constrain the script. If the three keys are institutional, any two institutions can produce a consensus-valid spend without the holder. |
| The holder can exit without the institutions | **Not established** | Nothing in the usual pitch gives the holder a key. Freeze and recovery are the same fact, read from two sides. |
| Insurance removes the custody risk | **Not applicable** | Insurance prices a defined loss after a claim. It does not change who can sign. Exclusions, proof of loss, and adjudication sit outside the module. |

How to read it. This scenario can be a reasonable product. It is institutional custody with a separation-of-duties improvement over a single custodian, plus an optional contract. It is not self-custody, and it is not "only I can move it" unless a holder key is in the script. Asking for the descriptor is the whole review.

## **2.2 Scenario B — collaborative custody, holder holds the quorum**

Pitch. A 2-of-3. The holder has two keys, on two devices, with two seed backups. The firm holds the third, in cold storage, as backup and as a co-signer of convenience. The holder can spend with both of their own keys and never call the firm. The firm cannot spend with its one key. If the holder loses one key, the firm co-signs a recovery. A configuration file lets the holder rebuild the wallet in other software if the firm is gone.

Module boundary, as pitched. Two holder secrets, one firm secret, a configuration file that is not a secret, and a web application that builds the transaction the device is asked to sign.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| The firm cannot move the funds alone | **Established** | Follows from 2-of-3 if the third key is the only key the firm has, and no other leaf exists. This is a consensus fact. It is the property Scenario A does not have. |
| The holder can leave without the firm | **Partial** | True with both holder keys and the configuration file. Seeds alone do not identify the firm's extended public key or the paths. The file is an availability-critical part of the module, and it is often treated as paperwork. |
| Losing one key is safe | **Partial** | Losing one key does not strand the funds. It also does not leave theft impossible. One holder key plus the firm's signer is a valid spend. The firm's approval of that co-sign is a procedure. Procedures fail differently from scripts. |
| The application cannot redirect a payment | **Not established** | The web application constructs the transaction. Destination integrity is whatever the signing device displays. The firm cannot force the holder to read the device. Devices differ in what they show. |

How to read it. The on-chain claim is the strongest of the custody scenarios, and it is narrow. It says the firm is not a custodian of the spending quorum. It does not say the firm's website is outside the trust boundary, and it does not say a co-sign request is reviewed by anything stronger than the firm's own process. Legal process can reach the firm. It cannot reach the UTXO with one key. The holder exit is the control, and only while the holder still has both keys and the file.

## **2.3 Scenario C — time-layered joint custody**

Pitch. During an insured term, neither the holder nor the firm can spend alone. The holder has keys. The firm co-signs. If the holder loses every key, a recovery partner and the firm can restore access after a delay. When the term ends, a further delay passes and the holder can spend alone. The rules are in Bitcoin script, so they survive the firm. Inheritance and disaster recovery are described as built in.

Module boundary, as pitched. A miniscript with several leaves, each valid at a different time. A recovery partner. A policy term that is a calendar fact, not a block the holder picked. A sovereign leaf after expiry.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| Nobody can spend alone | **Partial** | Often true of the leaf that is valid today, and false of a leaf that becomes valid later. "Nobody" has to be checked per leaf. A recovery leaf that two service parties can satisfy is a second quorum, opened by a timelock. |
| The holder is not frozen if the firm disappears | **Partial** | True after the sovereign leaf matures, and only if the holder still has the keys that leaf requires. Until then, refusal to co-sign is a freeze. Time-bounded is not the same as absent. |
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
| Mixed vendors mitigate supply-chain attack | **Partial** | See the next section. The narrow claim is true. The sentence as pitched is not. |

## **2.5 Scenario E — decaying 2-of-2**

Pitch. A key on the holder's phone or desktop, and a second key held by the application provider. Both must sign. A second factor — a code, a prompt, an authenticator — protects the provider's signature. If the provider disappears, or the second factor is lost, the holder can spend alone after a wait. The wait is described as about a year.

Module boundary, as pitched. Two keys. A relative timelock on each deposit, starting when that deposit confirms, on the order of 26,000 to 65,000 blocks. A second factor that the holder experiences as part of sending. An open recovery tool for the case where the service is gone and the timelock has matured.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| The provider cannot move the funds | **Established** | True while the script is a 2-of-2 and the provider holds only one key. This is a consensus fact, and it is better than Scenario A on this one point. |
| The second factor is a second key | **Not established** | The factor gates the provider's willingness to sign. It is not a leaf in the script. A failure of that check is a failure of the provider's procedure, not of consensus. |
| The holder can exit today | **Not established** | Not this scenario. Exit without the provider waits on the timelock for those specific coins. Moving the coins starts the wait over. This is a freeze until maturity, not a holder quorum. |
| This is the layered joint-custody scenario | **Not applicable** | No recovery partner can spend without the holder. The sovereign leaf is the holder's own key, later. Scenario C has a leaf other parties can satisfy. This one does not. |

How to read it. A reasonable phone wallet with a time-bounded co-signer. Not self-custody today, and not institutional custody either. The question to ask is which deposits are still inside the window. A device that can register an ordinary multisig and export that registration is not this scenario. That device, used as one signer in a desktop coordinator, is Scenario D. The export is a second copy of the map. It helps descriptor loss. It does not add a spend rule.

## **2.6 Scenario F — one seed, split into shares**

Pitch. Remove the single point of failure without the complexity of multisig. One wallet, split into several recovery shares. Any threshold of shares restores the wallet. Shares can be stored apart. No configuration file. No second device required to send. Often offered by a hardware maker as the backup a new holder should use instead of a quorum.

Module boundary, as pitched. One secret. Shares of that secret. One device that reconstructs the secret and signs. A share format that the reconstructing device has to implement.

### **Claim check**

| Claim | Verdict | Why |
| :---- | :---- | :---- |
| This is multisig, simplified | **Not established** | Multisig is several keys, and consensus requires a threshold of signatures. This is one key, split so that a threshold of shares rebuilds it. After reconstruction, one device signs alone. The chain cannot tell a share-split wallet from any other single-signature wallet. |
| Losing one share does not lose the funds | **Partial** | True of the backup, if the threshold was set above one and the remaining shares still meet it. False of signing. Whoever holds the threshold of shares in one place holds the wallet. |
| No vendor boundary | **Not established** | The shares are an application format. Reconstruction needs an implementation that understands them. That is a narrower exit than a seed written in the common wordlist. |
| No map to lose | **Established** | There is no descriptor, because there is no cosigner set. That advantage is real, and it is the advantage of not having a quorum. |

How to read it. A backup scheme for a single-signature wallet. Loss tolerance of the paper is real. Theft tolerance at the moment of signing is the tolerance of one device. A newcomer who accepts the pitch has changed modules without being told. Do not score it on the multisig rubric. Score it as single-signature plus a split backup.

# **3\. The mixed-vendor sentence**

The sentence a newcomer hears: use hardware from different manufacturers, so a supply-chain attack or a bug at one company cannot take the funds.

The narrow claim, which is established. An independent second implementation contains a failure of one implementation's random-number generator or signing oracle, provided the other seeds were generated on other implementations and not recombined by a shared passphrase. A seed-generation defect disclosed by a hardware vendor in 2026 is enough to demonstrate the class. Firmware updated after the fact does not repair a seed created on the affected build. In a mixed 2-of-3 that defect is one share. In a single-signature wallet, or in three devices of that same build, it is the wallet. Diversity is a real containment control for correlated implementation failure. Calling it nothing would be wrong.

The pitched claim, which is not established.

* One purchase channel, one setup computer, and one operator are one supply chain, regardless of the logos on the boxes.

* Different brands do not imply different secure elements, different recovery-word code, or different transaction parsers.

* The coordinator and the host sit above the vendor boundary. A substituted cosigner key at setup is not stopped by which factory made the genuine devices.

* The descriptor and the backup ritual are single-implementation by nature.

* Entropy the operator adds — dice rolled once and copied, a passphrase reused on all three — puts the correlation back.

A conformance lab would record the configuration that was actually tested: which devices register the cosigner set, where the descriptor is stored, whether setup happened on a machine the operator is willing to trust with a substitution attack. "Three brands" is not that configuration.

# **4\. Does a larger quorum help**

Pitch. 2-of-3 is the start. 3-of-5 is more secure, because an attacker needs more keys and the holder can lose more keys. The cost is more devices and more management, which is framed as the price of the improvement.

On the two axes that are actually about the quorum, the larger one wins. An attacker needs three shares instead of two. The holder can lose two shares instead of one. It is the smallest quorum that survives a double loss. The witness is larger, so the fee is higher. That is the entire consensus difference.

It does not change the class of control. A substituted cosigner key at setup, a coordinator that builds the transaction, a map nobody stored, a co-sign desk that approves on a procedure, and a recovery leaf two service parties can satisfy are all still present at 3-of-5. They were never quorum problems.

The overhead is not neutral. A 2-of-3 with a seed backup per key is six items to place. A 3-of-5 on the same rule is ten. The setup comparison Scenario D already fails is now done across five devices instead of three. Five seeds generated on one machine, five backups in one safe, or one passphrase reused across the set collapse toward one component. Correlated failure does not honor the threshold.

Who holds the extra keys matters more than the count. Two more keys in the holder's other locations raise the theft bar and the loss bar together. Two more keys at service parties raise the collusion surface and can remove the holder quorum. That is arrangement, already the difference between Scenario A and Scenario B. A larger threshold does not convert one into the other.

Verdict. Partial, and only for an operator who already runs a clean 2-of-3, already stores the map away from the seeds, and specifically needs to survive two independent losses. For everyone else it is more management on the same module. The failures that take funds are not the ones the extra keys touch. A 2-of-2 is the other direction: either share lost is a freeze until a timelock, or forever, and it should not be sold as the safer small quorum.

# **5\. What a pleb can do with this**

No scenario above is disqualified. Each is a different module. The mistake available to a new holder is accepting a sentence from one scenario as a property of another.

| If the pitch says | Ask for |
| :---- | :---- |
| **Only you can initiate** | Show me the leaf that fails without a signature from a key I hold. |
| **No single party can move it** | List every party that holds a key. Then list every leaf, including the ones that are not valid yet. |
| **You can always recover** | Who signs that recovery, after how long, and what do I hold while I wait? |
| **Verifiable on-chain** | The descriptor, and a walkthrough of each leaf against a block explorer. Not a screenshot of a dashboard. |
| **Insured** | The insuring clause and the exclusions. Separately, who can sign. Those are different documents. |
| **We are audited** | Which report, which scope, which date, Type I or Type II. Then whether the scope included the script. |
| **Use three brands** | Which of the three devices will show me the other keys, and the destination, on its own screen? |
| **You can lose one key** | What else, besides a key, reconstructs this wallet? Where is that copy? |
| **Simpler than multisig** | After I restore, how many signatures does the chain require? If the answer is one, it is not a quorum. |
| **We co-sign, and you can recover later** | Who can satisfy the recovery leaf, and which of my coins are still inside the window? |
| **3-of-5 is safer** | Safer against which failure? Show the one a third key actually closes. |

A firm that answers with the artifact is operating an approved mode. A firm that answers with a restatement of the pitch is asking the holder to validate the brochure. Those are observably different. Neither answer is a character reference.

# **6\. Limits**

* Patterns were taken from public marketing and public technical descriptions current in 2026\. They will drift. The method does not. Re-read the descriptor, not this note.

* No script was pulled from the chain. A holder with a vault can close every "not established" in an afternoon. This note cannot.

* A reader in this market will recognize some patterns. Recognition is not attribution. The document does not name a firm, a person, or a product, and it does not invite that mapping as a conclusion.

* Nothing here alleges an incident, a theft, a false statement made knowingly, or an inability to pay a claim. "Not established" means the artifact was not in the pitch.

*End of note. The verdicts are replaceable. The artifact column is the part to keep.*