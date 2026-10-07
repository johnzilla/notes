# Style guide

Rules for editing [Who Can Move Your Bitcoin?](who-can-move-your-bitcoin.md). They apply to every change, by any author. Check a change against this page before you commit it.

## Readers

The document has two readers.

- **People choosing how to hold bitcoin.** Most are not technical. They read "Start here."
- **Technical reviewers.** They read the full document.

Write "Start here" and any plain-language text for the first reader. Write the technical sections for the second reader, and keep them as clear as the content allows.

## Content rules

1. **No names.** Do not name products, companies, people, or specific incidents. Describe configurations, not providers.
2. **Conditions, not verdicts.** State what a protocol does and what evidence a claim needs. Do not state that a product or design is safe or unsafe. Use only the result terms in §1.1.
3. **Cite mechanism claims.** A statement about how a protocol or signature scheme works needs a public specification. Pin each source to a version in §8.
4. **Label citations by what the source defines.** A link label names what the specification actually specifies. Do not label an opcode as a recovery mechanism, or an output type as an escrow protocol.
5. **No frequency claims, in either direction.** Do not claim how often something fails without cited incident evidence. Do not treat a record without known failures as evidence of soundness.
6. **Keep conditions with their claims.** If a claim holds "only if" something, keep the claim and the condition in one sentence or one table cell. A reader must not be able to quote the claim without its condition.

## Structure rules

1. **Do not renumber.** Existing section and scenario numbers stay fixed, because reviewers cite them. Add new material inside an existing section, at the end of a list, or under an unnumbered heading.
2. **Version every change.** Change the minor number when a claim, consequence, citation, or reader-facing text changes. Change the major number when numbering or structure changes in a way that breaks references. Typo-only edits do not change the version.
3. **Record every version.** Add a changelog row that links the content commit. Tag the commit that contains the changelog row as `vX.Y`. Update the version line in the README.
4. **Keep the filename.** The filename is `who-can-move-your-bitcoin.md`. It stays fixed, so links keep working. Versions before 0.18 used `what-a-custody-pitch-has-to-show.md`; their tags keep that name, and a short file at that path links to the current one.

## Plain language

These rules follow Simplified Technical English (ASD-STE100) in part. They do not adopt its controlled dictionary. Apply them strictly in "Start here" and plain-language text. In the technical sections, apply them when you edit a passage.

1. **One word, one meaning.** Use the same word for the same thing everywhere. Use the canonical terms below. Add new terms to §1.2.
2. **One idea per sentence.** Aim for 20 words or fewer in an instruction and 25 or fewer in a description. Rule 6 under Content rules overrides this limit.
3. **No noun stacks.** Do not put more than three nouns in a row. Say who does what to what.
4. **Active voice.** Name the actor. Use the imperative for steps the reader does.
5. **Explain terms.** In plain-language text, explain each technical term the first time you use it. In technical sections, link the term to §1.2 or explain it.
6. **Clear references.** Do not start a sentence with "it" or "this" when the reader could be unsure what it refers to.

## Canonical terms

| Use | For | Do not use for the same thing |
| :---- | :---- | :---- |
| **holder** | The person whose bitcoin is at stake | owner, customer, user (except "account holder" in Scenario H) |
| **service** | An organization that holds a key, co-signs, runs signing infrastructure, or runs recovery | provider, firm, vendor (plain-language text says "company" throughout and mentions once that the technical sections call it a "service") |
| **key** | A private key that authorizes signatures | seed, share, backup (these are different things; see §1.2) |
| **signer** | The component that uses a key to sign | device, wallet (unless the device itself is meant) |
| **spending path** | One complete way to satisfy an output's spending conditions | route, branch, leaf (a leaf is a Taproot term; see §1.2) |
| **freeze** | Signer freeze or platform freeze, as defined in §1.2 | lock, block (unless describing a timelock or a block in the chain) |

A term inside a quoted claim, such as "Multiple vendors or devices" in §6, keeps the claim's wording.

## Before you push

- Every BIP or other specification cited in the text has a row in §8, and every "Cited in" entry matches the sections that cite it.
- Every link in the Contents resolves to a heading.
- Every new external link loads.
- The changelog row, tag, and README version line match.
