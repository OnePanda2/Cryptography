# Modern Cryptography: A Research Map for the Aspiring Cryptographer

**Query:** "What is everything an expert cryptographer should know about modern cryptography?"
**Scope:** Exhaustive / graduate-researcher depth. Technical-historical-mathematical angle. Time horizon: classical era → August 2026, with emphasis on the last 10/5/2/1 years.
**Searches run:** 9 live web-search waves (landscape, targeted deep-dive, and verification), grounding every claim about the *current state of the field* (2024–2026) in primary sources — NIST, IETF, IACR ePrint, Signal, and reputable technical press. Timeless mathematics, classical history, and settled 20th-century results are presented from well-established literature and are not individually re-verified by search (they don't change).
**Report date:** August 2, 2026

---

## How this report is organized

This is not one document — it's a shelf. A single file at this depth would be unusable (impossible to navigate, impossible to update one section without touching all of it). It is split into **seven files**, each independently readable, cross-referenced by part number:

| File | Covers | Roughly maps to your outline |
|---|---|---|
| `00-INDEX-executive-summary.md` | This file — map, findings, confidence, methodology | — |
| `01-history-and-mathematical-foundations.md` | History of cryptography; all mathematical prerequisites | PART 1, PART 2 |
| `02-primitives-and-symmetric-cryptography.md` | The primitive zoo; block/stream ciphers; hash functions; modes; ARX/SPN/Feistel/sponge | PART 3, PART 4 |
| `03-public-key-and-post-quantum-cryptography.md` | RSA/DH/ECC to lattices, codes, isogenies, hash-based sigs; full PQC standardization status | PART 5, PART 6 |
| `04-cryptanalysis-and-randomness.md` | Every major attack family; RNG design and testing | PART 7, PART 8 |
| `05-protocols-implementation-realworld-standards.md` | TLS/Noise/SSH/Signal; constant-time engineering; deployed systems; standards bodies | PART 9, PART 10, PART 11, PART 12 |
| `06-research-frontier-people-papers-books-glossary-roadmap.md` | Open problems, people, curated reading list, books, software, glossary, learning roadmap, ranked research opportunities | PART 13, People, Papers, Books, Software, Terminology, Roadmap, Research Opportunities |

Read them in order if you're building foundations top-down; jump directly to `03` or `04` if you already have the math and want the current primitive/attack landscape; jump to `06` if you want the "what do I do next" map right now.

---

## Executive Summary

Modern cryptography in 2026 is in the middle of the largest primitive-level transition since the public-key revolution of the 1970s: the migration away from cryptography whose security rests on integer factorization and the discrete-logarithm problem (RSA, Diffie–Hellman, ECDSA/ECDH) toward **post-quantum cryptography (PQC)**, whose security rests on structured lattices, hash functions, and (to a lesser extent) codes and multivariate systems. 🟢 **High confidence.** NIST finalized its first three PQC standards — FIPS 203 (ML-KEM, lattice-based key encapsulation), FIPS 204 (ML-DSA, lattice-based signatures), and FIPS 205 (SLH-DSA, hash-based signatures) — on August 13, 2024, after an eight-year, multi-round open competition. A fourth KEM (HQC, code-based) was selected in March 2025 as a structurally-independent backup to ML-KEM, and a fourth signature scheme (FN-DSA, based on Falcon) is in final draft as FIPS 206, expected late 2026/early 2027. Hybrid deployment (classical + post-quantum combined) is already default or opt-in in Chrome, Firefox, BoringSSL, AWS-LC, the JDK, Windows Schannel, Signal, iMessage (PQ3), and roughly 20–30%+ of measured TLS 1.3 handshakes on the public web as of early-to-mid 2026, per Cloudflare Radar telemetry.

The urgency behind this migration is *not* that a cryptographically relevant quantum computer (CRQC) exists — it does not, as of this writing. It is **harvest-now-decrypt-later (HNDL)**: adversaries who record today's encrypted traffic can decrypt it retroactively once a CRQC arrives, so anything with a confidentiality lifetime longer than the time-to-CRQC is already at risk. 🟡 **Medium confidence on timing, high confidence on the mechanism.** Expert timelines for a CRQC vary widely — most surveyed cryptographers still centered on the 2030s–2035 as of 2024 — but 2026 brought a wave of algorithmic resource-estimate reductions (the physical-qubit cost of breaking RSA-2048 fell from roughly 20 million to under 1 million, and by some newer estimates toward 10,000–100,000, driven largely by improved Shor-algorithm circuit constructions and new hardware approaches like neutral-atom arrays) that have visibly compressed several serious practitioners' timelines. This is one of the most consequential *recent* developments in the field and is covered in depth in Part 3 and Part 6.

A second major and very recent development: **AI models are becoming active participants in cryptanalysis**, not just tools that assist humans. On July 28, 2026, Anthropic disclosed that its Claude Mythos Preview model, largely working autonomously with periodic human direction, discovered a previously unknown mathematical symmetry in the lattice structure of HAWK — a NIST post-quantum signature candidate then in round 3 of the "additional signatures" track — yielding a key-recovery attack that halves HAWK's effective security margin. The same effort also produced a 200–800× speedup on the best known attack against 7-round (of 10) AES-128. Neither result threatens deployed systems, but the HAWK team withdrew their algorithm from NIST consideration as a direct result, and the event is a serious data point for a question the field has debated abstractly for years: what happens to cryptanalysis and cryptographic design once frontier AI systems can run sustained, semi-autonomous mathematical research programs. This is covered in Part 4 and Part 6.

Beyond PQC, the report tracks five other threads in depth: (1) the maturing but still non-trivial engineering of authenticated encryption and misuse-resistant modes (AES-GCM-SIV, OCB, Ascon as the new NIST lightweight standard); (2) the practical arrival of zero-knowledge proof systems (zk-SNARKs, zk-STARKs, Halo2/PLONK-family) as production infrastructure for blockchains and, increasingly, verifiable-computation and privacy-preserving identity use cases outside crypto-assets; (3) fully homomorphic encryption's slow but real crossing from "10,000× slower than plaintext" toward shipped, narrow, production deployments (Apple's Live Caller ID Lookup and Enhanced Visual Search, Microsoft Edge Password Monitor); (4) modern secure messaging's move to continuous, not just initial-handshake, post-quantum protection (Signal's Triple Ratchet / SPQR, Apple's PQ3); and (5) the steady drumbeat of microarchitectural side-channel research (SLAP and FLOP against Apple M-series chips, 2025) showing that implementation-level attacks remain as fertile a research area as ever, arguably more so as CPUs grow more speculative and PQC schemes introduce new side-channel-sensitive operations (Falcon/FN-DSA's floating-point Gaussian sampling being the standing example).

**What separates a practitioner from a researcher**, per this report's synthesis: practitioners need to correctly *select and deploy* well-analyzed constructions and get the boring parts (randomness, constant-time code, key management, protocol composition) right, which is already hard. Researchers need the reduction-based and simulation-based proof toolkit, comfort with the hard-problem landscape (lattices, isogenies, MQ systems, codes) well enough to *invent or break* schemes, and the judgment to know which of today's "unbroken" assumptions are actually well-scrutinized versus merely young. The gap between the two is largely Part 2 of this report (mathematical foundations) plus sustained paper-reading (Part 6).

---

## Research Map

Investigated in depth:
- Historical evolution from classical ciphers through Shannon/Kerckhoffs to the public-key revolution and PQC era
- Full mathematical toolkit: modular arithmetic, groups/rings/fields, elliptic curves, lattices, coding theory, complexity theory, probability/information theory, provable-security frameworks (game-based, simulation-based, random oracle, standard model)
- Symmetric primitives: block ciphers (DES→AES and the AES finalists), stream ciphers (Salsa/ChaCha), hash functions (MD/SHA family, Keccak/SHA-3, BLAKE3, Ascon), MAC constructions, modes of operation (CBC/CTR/GCM/SIV/OCB/XTS) and their failure modes
- Public-key cryptography: RSA, Diffie–Hellman, ECC (including Curve25519/Ed25519 vs. NIST curves), pairings
- Post-quantum cryptography across all five hard-problem families (lattices, hash-based, codes, isogenies, multivariate), full current NIST standardization status as of August 2026
- Cryptanalysis: differential/linear/algebraic/statistical attack families; side-channel and microarchitectural attacks (including 2025 results); the AI-cryptanalysis development
- Randomness: entropy sources, CSPRNG/DRBG designs, historical RNG failures (Dual_EC_DRBG, Debian OpenSSL), randomness testing and its limits
- Protocols: TLS 1.3 and its PQC hybrid rollout, Noise, SSH, Signal (X3DH→PQXDH→Triple Ratchet), WebAuthn/FIDO2/passkeys, PKI
- Implementation security: constant-time programming, memory safety and language choice, formal verification, fuzzing
- Real-world deployed systems: Bitcoin/Ethereum/Zcash/Monero, Tor, WireGuard, libsodium/OpenSSL/BoringSSL, HSMs/Secure Enclave/TPM
- Standards ecosystem: NIST/FIPS, IETF/RFC, ISO, CFRG, NSA Suite B → CNSA 2.0
- Research frontier: FHE, MPC, ZK/succinct proofs, threshold cryptography, VDFs/VRFs, AI+cryptography in both directions
- People (20+ major contributors and their specific contributions), curated paper reading roadmap, book recommendations, software landscape, full glossary, a beginner-to-researcher learning roadmap, and ranked research opportunities

Explicitly scoped down (see "What's Missing" in each part, and consolidated at the end of Part 6):
- Exhaustive enumeration of *every* cipher ever proposed (hundreds of AES/eSTREAM/CAESAR/NIST-LWC candidates) — covered via the significant/surviving/instructive ones, with pointers to competition archives for the rest
- Line-by-line proofs of every theorem cited — this is a map, not a textbook; each claim points to where the proof lives
- Non-English-language research communities and venues are underrepresented, consistent with the field's general (and worth-noting) skew toward English-language IACR/NIST/IETF venues
- Quantum cryptography *proper* (QKD, quantum money, quantum computing algorithms themselves) is covered only where it bears directly on classical PQC — it is a large field of its own

---

## Key Findings

| # | Finding | Confidence | Where |
|---|---|---|---|
| 1 | NIST's core PQC suite (ML-KEM, ML-DSA, SLH-DSA) has been final since August 13, 2024; a 4th KEM (HQC) and 4th signature (FN-DSA/Falcon) are in the pipeline, expected final 2026–2027 | 🟢 | Part 3 |
| 2 | Hybrid classical+PQC key exchange is already the practical default in major browsers/TLS stacks; pure-PQC is not yet standard practice, by design, as a hedge | 🟢 | Part 3, Part 5 |
| 3 | No cryptographically relevant quantum computer exists in 2026; expert timelines cluster in the 2030s but have compressed meaningfully in 2025–2026 due to algorithmic (not just hardware) resource-estimate improvements | 🟡 | Part 3, Part 6 |
| 4 | An Anthropic AI model autonomously found a real cryptanalytic weakness in a NIST PQC candidate (HAWK) and a large speedup on reduced-round AES, in July 2026 — a first-of-its-kind, verified event, not a hypothetical | 🟢 | Part 4, Part 6 |
| 5 | AES, SHA-2, SHA-3, and ChaCha20 remain unbroken in any practically exploitable sense; best cryptanalytic results are on reduced-round variants and don't threaten deployed full-round use | 🟢 | Part 2, Part 4 |
| 6 | Falcon/FN-DSA's floating-point discrete Gaussian sampling is the standing example of a PQC scheme whose *implementation* (not its mathematics) is the main open risk — this is a recurring theme across the new PQC generation | 🟢 | Part 3, Part 4 |
| 7 | Zero-knowledge proof systems (SNARKs/STARKs) have crossed from theory into heavy production use, overwhelmingly in blockchain scaling (rollups) first, with verifiable-computation/verifiable-AI applications emerging | 🟢 | Part 6 |
| 8 | FHE remains 100–1000× slower than plaintext computation for general workloads but has real, narrow production deployments (private set intersection style lookups) as of 2024–2026 | 🟢 | Part 6 |
| 9 | Secure messaging has moved beyond "post-quantum handshake only" to continuous post-quantum forward secrecy (Signal's Triple Ratchet/SPQR, 2025; Apple PQ3, 2024) | 🟢 | Part 5 |
| 10 | Modern microarchitectural side-channel research continues to find new classes of leakage on current-generation CPUs (Apple M3/M4: SLAP, FLOP, both 2025), independent of the algorithm-level security of the cryptography running on them | 🟢 | Part 4 |

---

## Confidence & Source-Quality Notes

Per the source-tier discipline used throughout: **S-tier** (NIST/FIPS/IETF-RFC/IACR ePrint/peer-reviewed) is preferred for every factual and mathematical claim about standards and settled science. **A-tier** (Reuters-grade tech press, vendor engineering blogs from primary implementers — Cloudflare, Signal, Apple, Google, Microsoft, DigiCert, PQShield) is used for *current deployment status and timelines*, which is not yet in peer-reviewed literature by nature. **B-tier** sources (industry blogs, market-research-style PQC vendor content) appear only for framing/context and are flagged inline where used — several search results in the "PQC 2026" space were transparently vendor-marketing content and were down-weighted or excluded accordingly. **C-tier** (forums, unverified social content, cryptocurrency-market-cap-driven ZK "top projects" listicles) appears nowhere as evidentiary support in this report; where it surfaced in search results (some of the zero-knowledge-proof "market cap" content) it was excluded, not cited.

---

## Consensus vs. Debate (report-wide)

**Consensus:** RSA/DH/ECC must eventually be replaced due to Shor's algorithm; lattice-based cryptography is currently the best-understood and most efficient PQC foundation; hash-based signatures (SLH-DSA) are the most conservative fallback because their security reduces only to hash-function collision/preimage resistance; constant-time, side-channel-aware implementation is as important as algorithm selection; AES/SHA-2/SHA-3/ChaCha20 remain sound.

**Active debate:** (1) *How urgent is migration, really* — this splits along quantum-timeline estimates, which genuinely differ among serious experts, not just marketing. (2) *Structured vs. unstructured lattices* — module-lattice schemes (Kyber/ML-KEM, Dilithium/ML-DSA) trade some theoretical worst-case-hardness elegance for efficiency versus less-structured (and slower) alternatives; whether the extra algebraic structure will eventually be exploited is an open research question, not resolved. (3) *Single-scheme risk concentration* — critics have noted that ML-KEM and ML-DSA both rely on module-lattice problems, so a single mathematical breakthrough against structured lattices could simultaneously compromise both; this is precisely why NIST pursued HQC (code-based) and SLH-DSA (hash-based) as structurally independent hedges, but the debate over how much weight to put on hedge-diversity versus efficiency continues. (4) *AI in cryptanalysis* — is the HAWK/AES result evidence of a step-change that should accelerate re-evaluation of "young" PQC candidates, or a one-off that shouldn't be over-extrapolated? Both positions are held by serious people as of this writing.

---

## What's Missing / Gaps

- This report cannot substitute for working through primary proofs; it is a curated map with pointers, not the destination.
- Some PQC deployment statistics (e.g., exact % of global TLS traffic using hybrid key exchange) come from vendor telemetry (Cloudflare Radar) rather than independently audited measurement, and should be read as directionally, not precisely, correct.
- The most recent NIST FIPS 206 (FN-DSA) text is still in draft; specifics may change before finalization (expected late 2026/early 2027) — treat all FN-DSA implementation detail as provisional.
- Anthropic's HAWK/AES cryptanalysis result is days old at the time of writing (disclosed July 28, 2026); independent replication/peer review is ongoing, standard practice for any new cryptanalytic claim, and this report treats it accordingly.
- Non-Western research programs (particularly Chinese SM2/SM3/SM4 national standards, and non-English cryptanalysis literature) are acknowledged but not covered in comparable depth — a real gap in most English-language surveys of the field, this one included.

---

## Recommended Next Steps

1. If you're building foundations: start at `01`, work through the math before the primitives — nearly everything downstream assumes it.
2. If you want the fastest path to "can evaluate a new paper": read `02` + `03` for primitive literacy, then jump straight into the Part 6 paper roadmap's "Must Read" tier.
3. If PQC migration is an operational concern for you (not just intellectual curiosity): `03` has the standards-status table and `05` has the deployed-protocol status; both are the fastest-moving parts of this report and worth re-checking against NIST's live CSRC pages every few months.
4. If you want to track whether the AI-cryptanalysis story is a one-off or a trend: watch the IACR ePrint archive and NIST's pqc-forum mailing list directly — this is unfolding in real time and this report's Part 4/Part 6 treatment will age faster than the rest.
