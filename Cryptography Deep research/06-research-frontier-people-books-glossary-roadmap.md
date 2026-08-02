# Part 6 — Research Frontier, People, Reading Roadmap, Glossary & Learning Path

*Final part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes Parts 1–5.*

---

## 1. Research Frontier

### 1.1 Zero-knowledge proofs and succinct arguments

Foundationally: a **zero-knowledge proof** lets a prover convince a verifier a statement is true while revealing nothing else — formalized (Part 1, 1.4) by Goldwasser–Micali–Rackoff (1985) and shown to exist for all of NP by Goldreich–Micali–Wigderson. The modern research and deployment landscape has moved decisively toward **non-interactive** variants with **succinctness** (proof size and verification time far smaller than the statement/witness itself):

- **zk-SNARKs** (Succinct Non-interactive ARguments of Knowledge): typically built via arithmetic-circuit representation of the statement, polynomial commitment schemes, and (for many constructions, notably the Groth16 family used by Zcash) a **trusted setup** — a one-time ceremony producing public parameters, with the property that if *any* participant in a multi-party ceremony honestly destroys their secret contribution, the whole setup remains sound even if every other participant was malicious ("powers of tau"-style ceremonies). Newer SNARK constructions (PLONK, Halo2, and related "universal"/"updatable" setup schemes) reduce or eliminate the need for a *per-application* trusted setup, a major practical and trust-model improvement over Groth16-era systems.
- **zk-STARKs**: avoid trusted setup entirely (relying only on collision-resistant hashing, "transparent" in the field's terminology) at the cost of larger proof sizes than comparable SNARKs, and are additionally believed **post-quantum secure** by construction (since they rely on hash functions and information-theoretic techniques rather than pairing-based or discrete-log-based polynomial commitments) — a genuinely relevant property given Part 3's broader PQC migration context.
- **Bulletproofs**: a different succinct-proof family (no trusted setup, logarithmic-size range proofs) particularly suited to confidential-transaction amount-range proofs, used in Monero-adjacent and other privacy-transaction contexts.
- **Deployment reality as of 2026**: the overwhelming majority of *production* ZK usage is in blockchain scaling (**rollups** — batching many transactions off-chain and posting a single succinct correctness proof on-chain, dramatically reducing the on-chain data/verification cost per transaction) and privacy-preserving transactions (Zcash, and privacy-focused Layer-2 systems). Emerging, less mature applications include **verifiable computation** generally (proving a computation was performed correctly without re-executing it — relevant to outsourced/cloud computation trust) and early **verifiable-AI** proposals (proving an AI model's output was genuinely produced by a specific, committed model without revealing the model weights or input data) — this latter category is real active research (arithmetizing neural-network inference into a provable circuit) but should be read as an emerging direction, not a mature, widely-deployed capability, and much of the "ZK + AI" content circulating in 2025–2026 industry commentary leans more toward speculative market framing than peer-reviewed maturity — a point worth flagging explicitly given how much lower-quality, market-cap-driven content surrounds this specific intersection online.
- **Open problems**: proof-generation time and hardware cost remain the dominant practical bottleneck (heavy ongoing research into GPU/FPGA-accelerated multi-scalar-multiplication and proof-generation pipelines, ZK-specific hardware accelerators, and pipelined/batched proving systems); reducing or eliminating trusted setups further without sacrificing proof size; formally standardizing ZK proof-system interoperability (no equivalent yet of NIST's PQC-style open competition/standardization process exists for the ZK proof-system landscape, which remains comparatively fragmented across many competing, non-interoperable systems — a genuine, often-noted gap relative to how the rest of applied cryptography standardizes).

### 1.2 Fully Homomorphic Encryption (FHE)

**FHE** allows arbitrary computation directly on encrypted data, returning an encrypted result decryptable only by the data owner, without the computing party ever seeing plaintext. **Craig Gentry's 2009 breakthrough** gave the first FHE construction (via ideal lattices and a "bootstrapping" technique to refresh accumulated ciphertext "noise" indefinitely, enabling unlimited-depth computation); the field has since converged on a handful of dominant scheme families, each suited to different workload shapes:
- **BFV/BGV**: exact integer arithmetic — suited to lookups, exact computation.
- **CKKS**: approximate real/complex-number arithmetic — suited to machine-learning-style workloads (this is the scheme family behind most "privacy-preserving ML inference" FHE research).
- **TFHE**: fast bootstrapping, well-suited to Boolean-logic/comparison-heavy computation — the scheme family Zama's production Ethereum-confidential-token deployment (mainnet since December 2025) is built on, and the natural choice per the "logic and comparisons want TFHE" rule of thumb that's become fairly standard guidance in the applied-FHE community as of 2026.
- **Real production deployments as of 2024–2026** (a meaningfully more concrete list than existed even two years prior): Apple's **Live Caller ID Lookup** (iOS 18+, BFV-based private-information-retrieval-style lookup checking an unknown caller against a provider's database without revealing the queried number) and **Enhanced Visual Search** (matching landmarks in photos against a server-side index via FHE plus differential privacy — a deployment that also drew real, documented public criticism in early 2025 over being enabled by default without an explicit opt-in, a useful reminder that strong cryptographic privacy engineering doesn't automatically satisfy separate consent/UX-ethics expectations); Microsoft Edge's **Password Monitor** (homomorphic breach-corpus credential checking); and Zama's TFHE-based confidential-token protocol on Ethereum mainnet.
- **Performance reality**: general-purpose FHE computation remains roughly 100–1000× slower than equivalent plaintext computation (some sources cite figures around 1000× for general workloads), which is precisely why every production deployment to date is a narrow, specific operation (a lookup, a comparison) rather than general-purpose outsourced computation — the gap between FHE's theoretical generality and its practical performance ceiling remains the field's central, actively-researched open problem, with steady, real, incremental progress (e.g., published ~33% improvements to homomorphic-AES-evaluation benchmarks in 2025 via new "p-encoding" techniques, part of an ongoing, friendly, published international competition among research groups specifically to speed up the homomorphic evaluation of AES as a standard benchmark task) but no near-term prospect of closing the gap to plaintext-comparable speed for general workloads.
- **Standardization**: unlike PQC, FHE has **no NIST-style open competition or finalized government standard** as of 2026 — an acknowledged real gap, with industry consortium efforts (the HomomorphicEncryption.org community standardization effort) providing informal, non-governmental convergence on parameter/API conventions instead, and interoperability/benchmarking work (cross-library benchmarking of SEAL, OpenFHE, HElib, Lattigo) an active research area precisely because no single canonical implementation or format yet exists.

### 1.3 Secure Multi-Party Computation (MPC)

**MPC** lets n parties jointly compute a function over their private inputs, learning only the output — the general-purpose ancestor concept both FHE (the two-party case, where one party contributes only the "computation," not private input) and threshold cryptography (1.4 below, MPC applied specifically to key-holding/signing operations) can be seen as specialized instances of. Foundational general-purpose constructions: **Yao's garbled circuits** (two-party, encrypting a circuit's truth tables so evaluation can proceed obliviously) and the **GMW protocol** (Goldreich–Micali–Wigderson, secret-sharing-based, generalizing to n parties). Modern practical MPC research and deployment has genuinely matured over roughly the past decade, per the field's own retrospective framing (Yehuda Lindell's widely-cited assessment: after ~20 years with no real applications in sight, the past decade has seen MPC become "fast enough to be used in practice" and receive real industry adoption) — the dominant production use case as of 2026 is **institutional digital-asset custody** (threshold-signature-based multi-party control of cryptocurrency private keys, letting an organization require, e.g., 3-of-5 device/party cooperation to produce a valid signature, with no single device ever holding the complete private key — explicitly marketed and adopted as a response to the scale of digital-asset theft, which industry tracking cited over $2 billion in the first half of 2025 alone), alongside growing use in **privacy-preserving analytics** (secure computation across siloed/regulated data — healthcare, finance — without centralizing the raw data) and **private set intersection (PSI)** (letting two parties learn only the intersection of their respective datasets, without revealing anything else about either set — used in contact-discovery features, advertising-measurement privacy compliance, and related applications). Current top-venue research (per the 2026 Theory and Practice of Multi-Party Computation workshop program) continues pushing on: reducing communication overhead (a historically dominant practical bottleneck for MPC relative to plaintext computation), minimal-assumption constructions (two-round MPC protocols from minimal primitives like plain oblivious transfer), and MPC-specific hardware/systems co-design (dedicated MPC compilers like MP-SPDZ, and systems research specifically on making MPC deployable by non-cryptography-expert engineering teams — still an acknowledged, real usability gap per Lindell's own assessment).

### 1.4 Threshold cryptography

Distributes a cryptographic **capability** (not just a computation, though the line blurs with MPC) — most commonly decryption or signing — across n parties such that any t of them (a threshold) can jointly exercise it, while fewer than t learn nothing. **Threshold ECDSA/Schnorr** signature schemes are today's dominant real-world instance (per 1.3's digital-asset-custody discussion). A genuinely open, active research area per Part 3's PQC-open-problems list: **threshold variants of the new NIST PQC standards** (threshold ML-KEM, threshold ML-DSA) are meaningfully less mature than their classical threshold-ECDSA/Schnorr counterparts, and building efficient, formally-secure threshold PQC signing/decryption — needed for exactly the same HSM/custody-style deployments currently using threshold ECDSA — is considered one of the field's higher-priority near-term research gaps precisely because production custody systems will need it once (not if) they migrate to PQC.

### 1.5 Verifiable Delay Functions (VDFs) and Verifiable Random Functions (VRFs)

- **VDFs**: a function requiring a specified, *unparallelizable* amount of sequential computation to evaluate, but whose result is quickly verifiable once computed (Wesolowski's and Pietrzak's constructions, both based on groups of unknown order — typically class groups or RSA groups — being the standard references). Used primarily in blockchain consensus designs needing an unbiasable, unpredictable, but publicly-verifiable source of delay/randomness (defending against an adversary who could otherwise bias an outcome by selectively withholding a fast-to-compute value).
- **VRFs**: a keyed pseudorandom function whose output is simultaneously deterministic (given the key and input), publicly verifiable as correctly computed (via an accompanying proof), and — critically — unpredictable to anyone without the secret key before it's revealed. The standard tool for **committee/leader selection in blockchain consensus** (a party proves, verifiably and unpredictably in advance, that they were selected without anyone being able to grind/bias the selection).

### 1.6 Accumulators

A cryptographic **accumulator** compactly represents a (potentially large, dynamically-changing) set, supporting efficient membership (and, for some constructions, non-membership) proofs without revealing the whole set — RSA accumulators (based on the same group-of-unknown-order structure VDFs use) and Merkle-tree-based accumulators (simpler, hash-based, but with logarithmic rather than constant-size proofs) are the two dominant families. Used for compact certificate-revocation representation, anonymous-credential systems, and as a building block inside several ZK-proof-system designs.

### 1.7 AI and cryptography — both directions

This relationship runs two ways, and both directions are live, current research areas as of 2026, not speculative future topics:

**AI as a cryptanalysis tool** (attacking cryptography): Part 3/Part 4's HAWK/reduced-round-AES event (July 2026) is the clearest, most concrete, most current example — a frontier AI model conducting substantial, largely autonomous mathematical research that produced a genuine, verified, standards-altering cryptanalytic result. This sits within a broader, longer-running research thread on machine-learning-assisted cryptanalysis (ML-based differential-characteristic search, ML-assisted side-channel attacks — using neural networks to classify power/EM traces, which has itself become a standard, published technique in the CHES research community over the past several years) — the HAWK event is best understood as a significant escalation in *autonomy and research-program duration* relative to this prior work, not as the field's first contact with AI-assisted cryptanalysis generally.

**Cryptography for AI** (protecting/verifying AI systems): FHE and MPC applied to privacy-preserving machine-learning inference/training (1.2, 1.3 above) is the most mature sub-thread; verifiable-AI-inference via ZK proofs (1.1 above) is the least mature but most actively hyped, and readers should apply real skepticism to specific production-readiness claims in this space given the amount of speculative/market-driven content currently circulating about it; differential privacy (a distinct but closely related field, formally guaranteeing that a computation's output doesn't reveal too much about any single individual's contribution to a dataset, via calibrated noise addition — not covered in depth in this cryptography-focused report but essential adjacent literacy for anyone working at the AI/privacy intersection) is a mature, NIST-and-academically-well-established complementary technique frequently combined with the cryptographic techniques above (exactly as Apple's Enhanced Visual Search deployment combines FHE with differential privacy, per 1.2).

**A genuinely open, actively-debated research and policy question**, worth naming explicitly as this report's closing frontier item: as frontier AI systems' capacity for sustained, semi-autonomous mathematical research grows, does the field's traditional timeline for "a new cryptographic scheme earns trust through years of accumulated public cryptanalytic scrutiny" need to be recalibrated — and if so, how, given that the thing that would need recalibrating (how much scrutiny is "enough") is exactly the kind of judgment call the field has never before had to make against a research-pace variable this uncertain. This report takes no position beyond documenting that the question is live, current, and, as of the HAWK event, no longer purely hypothetical.

---

## 2. People — Major Contributors and Their Specific Contributions

| Person | Specific, citable contribution(s) |
|---|---|
| **Claude Shannon** | Founded information theory (1948); "Communication Theory of Secrecy Systems" (1949) — perfect secrecy, proved the one-time pad's optimality and its key-length lower bound, confusion/diffusion |
| **Auguste Kerckhoffs** | Kerckhoffs's principle (1883) — security must not depend on algorithm secrecy, only key secrecy |
| **Whitfield Diffie & Martin Hellman** | "New Directions in Cryptography" (1976) — invented public-key cryptography and Diffie-Hellman key exchange |
| **Ralph Merkle** | Independently originated public-key-exchange ideas ("Merkle puzzles"); Merkle trees (hash trees, foundational to countless later systems including blockchain and many PQC hash-based signatures) |
| **Ron Rivest, Adi Shamir, Leonard Adleman** | RSA (1977/78) — first full public-key cryptosystem and practical digital signature scheme |
| **James Ellis, Clifford Cocks, Malcolm Williamson** | GCHQ, independently discovered public-key-cryptography-equivalent ideas years before Diffie-Hellman/RSA, declassified 1997 |
| **Joan Daemen & Vincent Rijmen** | Designed Rijndael, selected as AES (2000/2001); Daemen also co-designed Keccak (SHA-3) with Bertoni, Peeters, Van Assche, and co-designed the "wide trail strategy" methodology underlying both |
| **Daniel J. Bernstein** | Salsa20/ChaCha20 (stream ciphers), Poly1305 (MAC), Curve25519/Ed25519 (elliptic curve crypto), Classic McEliece and sntrup (PQC), NaCl/libsodium's design lineage, and a long, influential record of applied-cryptography engineering emphasizing speed, simplicity, and misuse-resistance |
| **Phillip Rogaway & Mihir Bellare** | Formalized the random oracle model (1993); OAEP, PSS padding schemes; foundational concrete/exact-security provable-security methodology broadly |
| **Oded Goldreich, Silvio Micali, Avi Wigderson** | Proved zero-knowledge proofs exist for all of NP (1986/1991) |
| **Shafi Goldwasser & Silvio Micali** | Formal definitions of semantic security/probabilistic encryption (1984); Goldwasser, Micali, Rackoff also founded zero-knowledge proofs (1985); both received the 2012 Turing Award for this foundational work |
| **Neal Koblitz & Victor Miller** | Independently proposed elliptic curve cryptography (1985) |
| **Tanja Lange & Daniel J. Bernstein** (jointly, plus their respective wider collaborator networks) | Leading, sustained post-quantum cryptography research programs; Lange is a central figure in the PQCRYPTO research community and PQC standardization discourse specifically |
| **Nigel Smart** | Extensive contributions across elliptic curve cryptography, MPC, and applied provable security; prominent in the European (ECRYPT) cryptographic research coordination community |
| **Peter Shor** | Shor's algorithm (1994) — the specific quantum-computing result that motivates the entire PQC migration effort |
| **Lov Grover** | Grover's algorithm (1996) — the quantum search speedup motivating symmetric-key-size doubling recommendations |
| **Craig Gentry** | First fully homomorphic encryption construction (2009), via ideal lattices and bootstrapping |
| **Oded Regev** | Learning With Errors (LWE) and its worst-case-to-average-case quantum reduction (2005) — the foundational hardness assumption underlying ML-KEM, ML-DSA, and most modern lattice cryptography |
| **Yehuda Lindell** | Leading figure in secure multi-party computation theory and, notably, its practical/industrial maturation over the past decade |
| **Vitalik Buterin** (as a widely-cited applied-cryptography communicator/engineer rather than a foundational theorist) | Not a cryptographic researcher in the Shannon/Diffie-Hellman sense, but a highly influential figure in driving real-world zk-SNARK/rollup engineering adoption via Ethereum's design and public technical writing — included here because the report's "who matters in the field today" list would be incomplete without acknowledging how much of the current ZK research agenda's funding and applied-engineering momentum traces to the blockchain-scaling use case specifically |
| **Bruce Schneier** | Twofish (AES finalist), Blowfish, Fortuna/Yarrow DRBG designs (with Ferguson and Kelsey), and, arguably more broadly influential, decades of widely-read public cryptography and security communication/education |
| **Moxie Marlinspike & Trevor Perrin** | Signal Protocol (X3DH, Double Ratchet) — the reference design for modern secure asynchronous messaging; Perrin separately created the Noise Protocol Framework |
| **Mitsuru Matsui** | Linear cryptanalysis (1993), including the first published practical full break of DES faster than exhaustive search |
| **Eli Biham & Adi Shamir** | Differential cryptanalysis (public rediscovery/formalization, 1990) |

---

## 3. Curated Reading Roadmap

### 3.1 Must Read

- **Shannon, "Communication Theory of Secrecy Systems" (1949).** Why it matters: the field's actual founding document. Difficulty: moderate (assumes basic probability). Prerequisites: none beyond undergraduate math. Reading time: 2–3 hours. Current relevance: still the correct place to internalize perfect secrecy and confusion/diffusion, concepts every later primitive either satisfies computationally or explicitly departs from.
- **Diffie & Hellman, "New Directions in Cryptography" (1976).** Why it matters: the paper that invented public-key cryptography. Difficulty: low-to-moderate. Prerequisites: group theory basics. Reading time: 1–2 hours. Current relevance: still the cleanest statement of the trapdoor-function idea underlying every public-key scheme since.
- **Rivest, Shamir, Adleman, "A Method for Obtaining Digital Signatures and Public-Key Cryptosystems" (1978).** Why it matters: RSA, in the authors' own words. Difficulty: low. Prerequisites: modular arithmetic. Reading time: 1 hour.
- **Regev, "On Lattices, Learning with Errors, Random Linear Codes, and Cryptography" (2005/2009 journal version).** Why it matters: introduces LWE and its worst-case-to-average-case reduction — the single most important paper underlying the current PQC generation. Difficulty: high. Prerequisites: Part 1's lattice section, basic quantum-computing familiarity for the full reduction. Reading time: several sessions.
- **NIST FIPS 203, 204, 205 (and 206 once final).** Why it matters: the actual, current, authoritative specifications you'll implement against. Difficulty: moderate (specification-document style, not narrative). Prerequisites: Part 3 of this report. Reading time: several hours each, ongoing reference use.
- **Bellare & Rogaway, "Random Oracles are Practical" (1993).** Why it matters: introduces the ROM as a proof tool, still the field's most-used (and most-debated) proof heuristic. Difficulty: moderate. Prerequisites: basic provable-security vocabulary (Part 1, 2.6–2.7).
- **Daemen & Rijmen, "The Design of Rijndael" (book, expanded from their AES submission documentation).** Why it matters: the wide-trail strategy, and the clearest worked example of *provable-margin* cipher design. Difficulty: moderate-high. Prerequisites: Part 1's finite-field/Boolean-algebra material.

### 3.2 Highly Recommended

- Biham & Shamir, "Differential Cryptanalysis of DES-like Cryptosystems" (1990/1991).
- Matsui, "Linear Cryptanalysis Method for DES Cipher" (1993).
- Bellare & Namprempre, "Authenticated Encryption: Relations among notions and analysis of the generic composition paradigm" (2000) — the Encrypt-then-MAC result from Part 2.5.1.
- Bernstein, "The Salsa20 Family of Stream Ciphers" and the ChaCha paper — clean examples of ARX design reasoning.
- Bernstein, Duif, Lange, Schwabe, Yang, "High-speed high-security signatures" (Ed25519, 2011).
- Bernstein, "Curve25519: new Diffie-Hellman speed records" (2006).
- Marlinspike & Perrin, the X3DH and Double Ratchet specification documents (available directly from signal.org/docs) — read as primary specifications, not secondary summaries.
- Goldwasser, Micali, Rackoff, "The Knowledge Complexity of Interactive Proof-Systems" (1985) — founding ZK paper.
- Boneh & Franklin, "Identity-Based Encryption from the Weil Pairing" (2001).
- NIST SP 800-90A (DRBG mechanisms) and SP 800-90B (entropy source assessment) — the practitioner's actual randomness-engineering reference.

### 3.3 Advanced

- Canetti, "Universally Composable Security: A New Paradigm for Cryptographic Protocols" (2001) — the UC framework underlying modern simulation-based composability proofs.
- Groth, "On the Size of Pairing-based Non-interactive Arguments" (2016) — Groth16, the SNARK construction behind Zcash's original design.
- Ben-Sasson et al., the STARK papers ("Scalable, transparent, and post-quantum secure computational integrity," 2018) — the STARK construction and its explicit post-quantum framing.
- Castryck & Decru, "An efficient key recovery attack on SIDH" (2022) — the SIKE break; essential reading for understanding both isogeny cryptography's specific risks and, more generally, how a "young" PQC scheme can fail catastrophically after surviving multiple competition rounds.
- Bogdanov, Khovratovich, Rechberger, "Biclique Cryptanalysis of the Full AES" (2011).
- Any of the current NIST PQC-round status NISTIRs (8309/8413/8528/8545/8610) for anyone tracking the standardization process as a live process rather than a historical event.

### 3.4 Historical Classics

- Kahn, *The Codebreakers* (1967 book, not a paper) — the classic narrative history of cryptography through WWII; dated in places but still the standard historical reference.
- Al-Kindi's frequency-analysis treatise (in translation) — for a direct sense of how old the field's oldest surviving technique actually is.
- Kerckhoffs, *La Cryptographie Militaire* (1883, in translation).

### 3.5 Latest Research (2025–2026)

- Anthropic's public HAWK-attack and reduced-round-AES writeups (referenced throughout Parts 3–4 of this report) — read directly rather than only via press coverage, for the actual technical claims and human-verification methodology.
- Recent (2025) NIST pqc-forum threads on FIPS 206 (FN-DSA) parameter/design choices — a live view into how a standard is actually finalized, not just its eventual output.
- Recent (2025) IETF TLS working-group drafts on ML-KEM/hybrid groups (`draft-ietf-tls-mlkem`, `draft-ietf-tls-ecdhe-mlkem`) and their cited formal-security-analysis papers (Chen/Peng/Wang/Bian on CPA-secure-KEM TLS composition; Zhou/Jiang/Zhao's Asiacrypt 2024 companion result) — the live state of the "is CCA security actually necessary" debate flagged in Part 3.
- Current IACR ePrint listings under "lattice cryptanalysis," "side-channel," and "post-quantum" tags — the single best way to track this report's fastest-moving material past its own publication date.

---

## 4. Books

| Book | Author(s) | Level | Notes |
|---|---|---|---|
| *Serious Cryptography* | Jean-Philippe Aumasson | Beginner–intermediate | Clear, modern, minimal-prerequisite; excellent first full-length book; slightly light on formal proofs by design |
| *Understanding Cryptography* | Christof Paar & Jan Pelzl | Beginner–intermediate | Strong on the engineering/implementation side; comes with free lecture videos; weaker on provable-security formalism |
| *Introduction to Modern Cryptography* | Jonathan Katz & Yehuda Lindell | Intermediate–advanced | The standard graduate-course textbook; rigorous definitions and proofs; the correct next step after a first practitioner-oriented book |
| *A Graduate Course in Applied Cryptography* | Dan Boneh & Victor Shoup | Advanced (free online) | Extremely comprehensive, continuously updated, free — arguably the best single current reference for someone past the beginner stage; covers PQC-adjacent material more currently than most printed textbooks can |
| *The Design of Rijndael* | Joan Daemen & Vincent Rijmen | Advanced | Primary-source-adjacent; essential for anyone wanting to actually understand wide-trail-strategy design reasoning rather than just AES's specification |
| *Handbook of Applied Cryptography* | Menezes, van Oorschot, Vanstone | Reference (free online) | Older (1996) but still a superb, dense reference for classical number-theoretic and protocol material; needs supplementing with modern PQC/provable-security material published since |
| *Post-Quantum Cryptography* (edited volume) | Bernstein, Buchmann, Dahmen (eds.) | Advanced | Pre-dates NIST's final standards but remains the best single-book grounding in the mathematical foundations across all five PQC families |
| *The Codebreakers* | David Kahn | General/historical | The classic narrative history; not technical, but essential context |
| *Applied Cryptography* | Bruce Schneier | Historical/general | Hugely influential in its time (1990s); now dated in specific algorithm recommendations (includes since-broken/deprecated ciphers presented uncritically by the standards of today) — read for historical context and breadth of exposure to primitive *types*, not as a current implementation reference |
| *Real-World Cryptography* | David Wong | Intermediate | Modern, protocol-and-systems-focused (TLS, Signal, PQC basics); a strong complement to Katz–Lindell's more theory-first approach |

---

## 5. Software

| Project | Language | Notes |
|---|---|---|
| **libsodium** | C (bindings everywhere) | Misuse-resistant high-level API philosophy; widely audited; the default recommendation for most application-level needs |
| **OpenSSL** | C | Dominant installed base; historically criticized low-level API, actively modernizing (3.x provider architecture); heavily fuzzed via OSS-Fuzz |
| **BoringSSL** | C | Google's OpenSSL fork; explicitly no external API-stability promise; leading early production PQC-hybrid experimentation |
| **AWS-LC** | C | Amazon's fork; FIPS-140-validated; early ML-KEM production shipper |
| **HACL\*** | F* (compiled to C) | Formally verified for both functional correctness and constant-time execution; used inside Mozilla NSS |
| **RustCrypto** | Rust | Community-maintained pure-Rust primitive implementations; memory-safe by construction |
| **Signal Protocol libraries** (`libsignal`) | Rust (current), historically Java/Swift/C | Reference implementation of X3DH/PQXDH/Double Ratchet/Triple Ratchet |
| **liboqs / Open Quantum Safe project** | C | The standard open-source testbed for experimenting with NIST PQC algorithms across many languages/protocols before/during standardization |
| **OpenFHE, Microsoft SEAL, HElib, Lattigo** | C++/Go | Leading FHE libraries, each with different scheme-family strengths (SEAL: BFV/CKKS; HElib: BGV/CKKS; Lattigo: Go-native, MPC-friendly variants) |
| **MP-SPDZ** | C++/Python | The most widely used general-purpose MPC research/deployment framework |
| **circom / snarkjs, Halo2** | Various (circuit DSLs, Rust) | Dominant tooling for building zk-SNARK circuits in production blockchain contexts |
| **age** | Go/Rust | Simple, modern, misuse-resistant file encryption (see Part 5) |
| **WireGuard** | C (Linux kernel) / various | Reference VPN implementation (see Part 5) |

---

## 6. Terminology — Glossary

**AEAD** — Authenticated Encryption with Associated Data; a cipher mode providing confidentiality, integrity, and authenticity simultaneously, plus authentication of unencrypted associated data. **Asymmetric distinguisher** — an attack that distinguishes a cipher's output from random with better-than-negligible advantage, without necessarily recovering the key. **Avalanche effect** — the property that a small input change causes a large, unpredictable change in output; a diffusion-quality indicator. **Bit security** — the log2 of the expected number of operations needed for the best known attack. **Birthday bound** — the point (≈√N draws from a space of size N) at which collisions become likely; governs hash-output-length requirements. **Ciphertext indistinguishability (IND-CPA/IND-CCA)** — the standard game-based encryption security notion. **Collision resistance** — hardness of finding any two inputs mapping to the same hash output. **Commitment scheme** — hides a value until later revealed, while binding the committer to it. **Confusion / diffusion** — Shannon's two design properties; see Part 1. **Constant-time** — execution independent of secret data, defending against timing/cache side channels. **CRQC** — cryptographically relevant quantum computer; one large/reliable enough to run Shor's algorithm against deployed key sizes. **DRBG** — Deterministic Random Bit Generator; NIST's term for a CSPRNG. **EUF-CMA** — existential unforgeability under chosen-message attack; the standard signature security notion. **Feistel network** — a block-cipher structure alternating halves via a round function; see Part 2. **Forward secrecy** — past session confidentiality survives future long-term-key compromise. **HNDL** — harvest-now-decrypt-later; the PQC-migration-motivating threat model. **HSM** — Hardware Security Module. **IBE** — Identity-Based Encryption. **Kerckhoffs's principle** — security must rest only in the key, not algorithm secrecy. **KDF** — Key Derivation Function. **KEM** — Key Encapsulation Mechanism; the PQC-era standard abstraction for public-key-based shared-secret establishment. **Lattice** — see Part 1, 2.8. **LWE** — Learning With Errors; see Part 1/Part 3. **MAC** — Message Authentication Code. **Merkle-Damgård** — an iterated hash-construction paradigm; see Part 2. **MITM (cryptanalysis)** — meet-in-the-middle attack. **MITM (protocol)** — man-in-the-middle attack. **MPC** — secure Multi-Party Computation. **Nonce** — a number used once; critical uniqueness requirement across many modes/signatures. **OWF** — one-way function. **PCS** — post-compromise security; future-message security after a past state compromise, via key ratcheting. **PKI** — Public Key Infrastructure. **PQC** — Post-Quantum Cryptography. **Preimage resistance** — hardness of finding an input mapping to a given hash output. **PRF/PRG/PRP** — pseudorandom function / generator / permutation. **QROM** — Quantum Random Oracle Model. **Reduction** — a proof technique showing one problem's hardness implies another's; see Part 1, 2.6. **ROM** — Random Oracle Model. **Salt** — random, non-secret value used to prevent precomputation attacks (e.g., against password hashes). **Semantic security** — Goldwasser-Micali's formalization of "ciphertext reveals nothing computationally useful about plaintext." **Side channel** — information leakage via a physical implementation property (timing, power, EM, etc.) rather than the algorithm's mathematics. **SPN** — Substitution-Permutation Network. **Trapdoor function** — a one-way function with a secret that makes inversion easy. **UC** — Universal Composability, Canetti's simulation-based composable-security framework. **VDF** — Verifiable Delay Function. **VRF** — Verifiable Random Function. **Zero-knowledge proof** — see Part 1, 1.4 and this Part, 1.1.

*(This glossary covers terms used across this report; it is not exhaustive of the entire field's vocabulary — the Katz–Lindell and Boneh–Shoup textbooks in §4 above have fuller glossaries for terms not needed elsewhere in this report.)*

---

## 7. Learning Roadmap — Beginner to Researcher

**Stage 1 — Mathematical literacy (parallel with Stage 2 is fine).** Discrete math, modular arithmetic, group/ring/field theory basics, elementary probability, big-O notation. *Milestone:* comfortably read and reproduce the RSA and Diffie-Hellman constructions from Part 1 of this report without needing to look anything up.

**Stage 2 — Practitioner foundations.** Read Aumasson's *Serious Cryptography* or Paar & Pelzl's *Understanding Cryptography* cover to cover. Implement (for learning, never for production) toy versions of: a Caesar/Vigenère cipher and its cryptanalysis, AES from the FIPS 197 specification directly, RSA key generation and OAEP padding, an HMAC. *Milestone:* can explain, from memory, why ECB mode is unsafe, why nonce reuse breaks GCM, and why padding-oracle attacks work.

**Stage 3 — Provable security formalism.** Katz & Lindell's *Introduction to Modern Cryptography*, cover to cover, doing the exercises. Learn game-based definitions (IND-CPA/CCA, EUF-CMA) and basic reduction-proof writing. *Milestone:* can write a correct reduction proof for a simple construction (e.g., proving a PRF-based MAC is EUF-CMA-secure given the PRF's security).

**Stage 4 — Specialization entry points** (pick based on interest, per the ranked opportunities below):
- *PQC track:* work through the lattice-cryptography sections of Boneh & Shoup, then Regev's LWE paper, then the ML-KEM/ML-DSA FIPS specifications directly, then implement a toy LWE-based encryption scheme.
- *Cryptanalysis track:* work through Biham-Shamir and Matsui's original papers, then a modern MILP-based automated-search tutorial, then attempt reduced-round attacks against a toy cipher using SAT/MILP tooling.
- *Protocol/systems track:* read the TLS 1.3 RFC and a Tamarin-prover tutorial, then attempt to formally model and verify a small toy protocol yourself.
- *ZK/MPC track:* work through the Groth16 and STARK papers, then build a small circuit in circom or Halo2 for a toy statement.

**Stage 5 — Research engagement.** Start reading IACR ePrint's daily new-paper feed in your specialization area; attend (in person or via recorded talks) a major venue — CRYPTO, Eurocrypt, Asiacrypt (the three flagship IACR conferences), CHES (implementation/hardware), Real World Crypto (deployment-focused), or PQCrypto (post-quantum-specific); participate in CTF (Capture The Flag) cryptography challenges (picoCTF for beginners, then more advanced CTFs' crypto categories) as a low-stakes way to practice actually breaking things under time pressure; contribute to an open-source cryptographic library's issue tracker or documentation as a way into the practitioner community before attempting original research; identify an open problem from Part 3's "Open Problems" section or §8 below that matches your specialization and attempt a small, scoped piece of it — even a negative result (a parameter regime you tried and couldn't break, written up carefully) is legitimate research output and a reasonable first target before attempting a full novel construction or attack.

**Suggested overall timeline** for someone starting from a strong general CS/math background (per this report's audience assumption): Stage 1–2 in parallel, 3–6 months; Stage 3, 4–8 months; Stage 4, 6–12 months depending on depth; Stage 5 begins overlapping Stage 4 and continues indefinitely — this is a genuinely open-ended field, and "graduating" to independent research is better thought of as a threshold of confidence and community engagement than a fixed curriculum endpoint.

---

## 8. Research Opportunities — Ranked

| # | Opportunity | Novelty | Difficulty | Potential impact | Barrier to entry |
|---|---|---|---|---|---|
| 1 | Threshold variants of ML-KEM/ML-DSA (efficient, formally-secure threshold PQC signing/decryption) | Medium | High | High (directly needed for PQC-migrating custody/HSM systems) | Medium — requires both PQC and threshold-crypto fluency, but well-scoped problem statements exist in current literature |
| 2 | Concrete side-channel-hardening of Falcon/FN-DSA's Gaussian sampling | Low-medium (known open problem, actively worked) | High | High (directly blocks safe production FN-DSA deployment) | Medium — requires implementation-security + PQC fluency; well-defined, timely target given FIPS 206's imminent finalization |
| 3 | Structured-lattice cryptanalysis (continued pressure-testing of Module-LWE/Ring-LWE beyond current best attacks) | High | Very high | Very high (would directly inform the single-scheme-risk-concentration debate) | High — genuinely frontier lattice-cryptanalysis expertise required |
| 4 | AI-assisted/AI-autonomous cryptanalysis methodology (following the Anthropic HAWK precedent) | Very high (essentially newly opened) | High | Potentially very high, direction still forming | Medium-high — requires both cryptanalysis fluency and comfort directing/interpreting AI-research-agent output; genuinely new enough that established practice doesn't yet exist |
| 5 | FHE performance (bootstrapping speed, hardware acceleration, compiler tooling) | Medium (active but far from solved) | High | High (currently the single largest practical barrier to broader FHE adoption) | Medium — active competitive research area with clear published benchmarks to beat |
| 6 | ZK proof-system interoperability/standardization | Medium | Medium | Medium-high (currently fragmented, unlike PQC's standardized landscape) | Low-medium — more a systems/standards-process problem than a pure cryptanalysis one, genuinely approachable for a strong engineer-researcher |
| 7 | MPC usability (making it deployable by non-expert engineering teams) | Low-medium | Medium | High (Lindell's own assessment names this as the field's key remaining barrier) | Low — a real, acknowledged gap, approachable from a systems/HCI-adjacent angle without requiring frontier cryptanalytic novelty |
| 8 | Post-quantum secure-messaging ratchet efficiency (reducing SPQR/Triple-Ratchet-style bandwidth overhead) | Medium | Medium-high | Medium (real but narrower deployment surface than the KEM/signature standards themselves) | Medium |
| 9 | Verifiable AI inference via succinct proofs (arithmetizing realistic-scale neural network inference efficiently) | High | Very high | Uncertain/high-variance — could be transformative or could stall on performance the way general FHE has | High — genuinely unsolved efficient-arithmetization problem at real model scale |
| 10 | Isogeny-based cryptography's post-SIKE rehabilitation (further scrutiny and, if it holds, renewed confidence-building for SQIsign and related constructions) | Medium-high | Very high | Medium (isogeny crypto's smallest-key advantage remains valuable if trust can be rebuilt) | High — requires genuinely deep isogeny-theory background, a narrower specialist community than lattices |

---

## Closing note

This report is a map, current as of August 2, 2026, of a field that is — per its own Executive Summary — in the middle of its largest structural transition since 1976. Treat every "current status" claim across all seven files as time-stamped, not timeless: PQC standardization will continue moving (FIPS 206 finalization, the additional-signatures track's eventual selections), quantum-hardware and quantum-algorithm resource estimates will continue being revised, and the AI-cryptanalysis thread opened in July 2026 is, by this report's own account, too new to have a settled trajectory yet. The mathematics in Part 1, and most of the primitive-design reasoning in Part 2, will still be exactly this true in five years. Re-check Parts 3, 4 (the AI-cryptanalysis and side-channel material specifically), and 5's deployment-status tables against primary sources — NIST CSRC, IACR ePrint, and the IETF datatracker — before relying on this report's specific current-state claims for anything operationally important.
