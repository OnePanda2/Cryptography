# Part 3 — Public-Key Cryptography & Post-Quantum Cryptography

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. This is the fastest-moving part of the report; standards-status details reflect August 2026 and should be re-checked against NIST's live CSRC pages periodically.*

---

## 1. Classical Public-Key Cryptography

### 1.1 RSA

Security rests on the presumed hardness of **integer factorization** (and the closely related but not proven-equivalent **RSA problem**). Key generation: pick large primes p, q, compute n = pq and φ(n) = (p−1)(q−1), pick public exponent e coprime to φ(n) (commonly 65537 for a good speed/security balance), compute private exponent d ≡ e⁻¹ (mod φ(n)). Encryption: c = m^e mod n. Decryption: m = c^d mod n. Signing is the mirror operation with roles of e/d swapped.

**Padding is not optional and is the source of most real RSA vulnerabilities historically.** "Textbook RSA" (no padding) is deterministic, malleable (RSA's homomorphic multiplicative structure means c1·c2 = (m1·m2)^e mod n, which breaks semantic security outright and enables real attacks like Bleichenbacher's) and vulnerable to a long catalog of structural attacks: low-exponent attacks (Håstad's broadcast attack against e=3 with insufficient padding across multiple recipients), common-modulus attacks, Wiener's attack against small private exponents, and more. The field's converged answer:
- **OAEP** (Optimal Asymmetric Encryption Padding, Bellare–Rogaway, standardized in PKCS#1 v2) for encryption — provides IND-CCA2 security in the random oracle model.
- **PSS** (Probabilistic Signature Scheme, also Bellare–Rogaway) for signatures — provides a tight security reduction in the ROM, unlike the older PKCS#1 v1.5 signature padding, which remains widely deployed for compatibility reasons despite the field generally preferring PSS for new systems.
- **PKCS#1 v1.5** encryption padding is the subject of the classic **Bleichenbacher (1998) million-message/padding-oracle attack** and its many descendants (ROBOT, 2018, showed the attack class was *still* practically exploitable against numerous production TLS implementations two decades later) — a long-running, repeatedly-relearned lesson that a padding scheme's *error-handling behavior* (does decryption failure look identical regardless of failure reason, in constant time?) is as security-critical as the padding's mathematical structure.

Key size recommendations have grown over time as factoring records advance (the General Number Field Sieve is the best known classical factoring algorithm, sub-exponential but not polynomial); **RSA-2048** is the current practical minimum for new systems, **RSA-3072/4096** for longer-horizon needs, though per Part 3.4 below, the entire RSA family is slated for eventual retirement in favor of PQC regardless of classical key-size adequacy, due to Shor's algorithm.

### 1.2 Diffie–Hellman

Two parties with a public group (generator g, prime p or elliptic curve parameters) each pick a secret exponent (a, b), exchange g^a and g^b, and both compute the shared secret g^(ab) — computable by each party but (believed) infeasible for an eavesdropper to derive from g^a and g^b alone without solving the discrete log problem. **Finite-field DH** requires large primes (2048+ bits) due to sub-exponential index-calculus attacks; a notable, embarrassing-in-retrospect real-world failure was the **Logjam attack (2015)**, which showed that widespread reuse of a small number of *fixed*, well-known 512/768-bit DH primes across huge swaths of the internet made a one-time, expensive precomputation against those specific primes reusable for cheap, fast attacks against any connection using them — a lesson in why parameter *diversity*, not just parameter *size*, matters. **Elliptic-curve DH (ECDH)** avoids this scale problem with much smaller, faster parameters (Part 1, 2.3).

DH as described is **unauthenticated** — it protects against passive eavesdroppers but not an active man-in-the-middle who can run independent DH exchanges with each party — so real protocols (TLS, Signal, SSH) always layer DH under an authentication mechanism (certificates/signatures, or an out-of-band-verified identity key), a distinction that matters conceptually: "key exchange" and "authentication" are different primitives that must be composed deliberately, not assumed to come bundled.

### 1.3 ECC in practice: Ed25519/Curve25519 vs. NIST curves

Covered mathematically in Part 1, 2.3; the practical/deployment picture as of 2026: **Curve25519** (for ECDH, as X25519) and **Ed25519** (EdDSA signatures over the twisted Edwards form of the same curve) have become the de facto default for new protocol design — SSH, WireGuard, Signal, and the majority of new TLS deployments prefer them — due to their rigid, fully-reproducible ("nothing up my sleeve") parameter generation, resistance to a wide class of implementation pitfalls via complete addition formulas (no exceptional/invalid-point cases to mishandle, unlike Weierstrass-form curve arithmetic), deterministic nonce generation in EdDSA (removing an entire historical bug class — see 1.4 below), and competitive-to-superior performance. **NIST P-256/P-384/P-521** remain mandatory in many government/CNSA-compliant and FIPS-140-validated contexts and are far from cryptanalytically broken, but carry the residual, never-fully-resolved provenance-trust question noted in Part 1.

### 1.4 The nonce-reuse catastrophe in (EC)DSA

**(EC)DSA signing requires a fresh, secret, uniformly random per-signature nonce (k).** If k is ever reused across two different messages under the same key — or, more subtly, if k is only *partially* predictable/biased (even a few bits leaked, e.g., from a flawed hardware RNG or a side channel) — an attacker can recover the private signing key via straightforward linear algebra (lattice attacks, in the partial-leakage case). This is not a hypothetical: it caused the **2010 Sony PlayStation 3 signing-key compromise** (Sony reused a *fixed* k across all firmware signatures, a maximally severe instance of the bug) and repeated real-world Bitcoin wallet-draining incidents from weak or duplicated ECDSA nonces in poorly-implemented wallet software. **EdDSA/Ed25519** eliminates this entire bug class by deriving the nonce *deterministically* from the private key and message (via a hash), removing randomness generation from the signing-time attack surface entirely — one of Ed25519's most concretely valuable engineering properties, not just a performance nicety. (Deterministic nonce generation is also available as an option for classical ECDSA per RFC 6979, but is not universally implemented or defaulted-to across the ECDSA ecosystem the way it is built into EdDSA by design.)

### 1.5 Pairings and pairing-based cryptography

Bilinear pairings (Part 1, 2.3) enable primitives with no efficient non-pairing equivalent: **identity-based encryption** (Boneh–Franklin, 2001 — a public key can be an arbitrary string like an email address, with a trusted authority deriving the corresponding private key, eliminating certificate-distribution overhead at the cost of introducing a powerful trusted-authority key-escrow point), short **BLS signatures** (aggregatable — many signatures over possibly-different messages can be compressed into a single short signature, heavily used in blockchain consensus protocols including Ethereum's proof-of-stake validator signature aggregation), and much of the polynomial-commitment machinery underlying zk-SNARK constructions (Part 6). Pairing-based security rests on a distinct assumption family (variants of bilinear Diffie–Hellman) from plain ECDLP and should be tracked as a separate trust boundary when auditing a system's cryptographic dependencies.

---

## 2. Post-Quantum Cryptography

### 2.1 Why quantum computers matter here specifically

**Shor's algorithm** (1994) solves integer factorization and the discrete logarithm problem (both finite-field and elliptic-curve) in polynomial time on a sufficiently large, fault-tolerant quantum computer — which breaks RSA, (EC)DH, DSA, and ECDSA outright, not merely weakens them. **Grover's algorithm** (1996) gives only a quadratic speedup for generic unstructured search, which halves the effective *bit-security* of symmetric primitives (AES-128 → roughly AES-64-equivalent against a quantum adversary running Grover) but does not break them outright; doubling key/output sizes (AES-256, 256-bit hash outputs) restores the original margin. This asymmetry — public-key cryptography catastrophically broken, symmetric cryptography merely weakened and easily compensated — is *why* the entire post-quantum migration effort is concentrated on key-establishment and signatures, not on replacing AES or SHA-2/3.

### 2.2 The five PQC hard-problem families

| Family | Representative hardness assumption | NIST-relevant schemes | Relative maturity |
|---|---|---|---|
| Lattices | LWE / Module-LWE / Ring-LWE / NTRU / SIS | ML-KEM (Kyber), ML-DSA (Dilithium), FN-DSA (Falcon), FrodoKEM | Best-studied, most efficient; the primary NIST picks |
| Hash-based | Collision/preimage resistance of an underlying hash | SLH-DSA (SPHINCS+), stateful XMSS/LMS (SP 800-208) | Most conservative — security reduces *only* to the hash function, no novel structured-algebraic assumption |
| Code-based | Syndrome decoding of random-looking linear codes | Classic McEliece, HQC, BIKE | Classic McEliece dates to 1978 — extremely long track record; large public keys historically limited adoption |
| Isogeny-based | Hardness of finding isogenies between (supersingular) elliptic curves | SIKE (catastrophically broken, 2022); SQIsign (candidate, round 3 of additional signatures) | Smallest keys of any PQC family when it works, but the 2022 SIKE break was a serious, structure-specific blow to confidence in the whole family |
| Multivariate | Hardness of solving systems of multivariate quadratic (MQ) equations | Rainbow (broken, 2022); MAYO, SNOVA, QR-UOV, UOV (round 3 candidates) | Long history of proposed schemes being broken; survivors are heavily scrutinized |

### 2.3 The NIST PQC standardization process — full status as of August 2026

NIST opened the process in **2016** (announced at PQCrypto 2016), received **82 initial submissions** by the late-2017 deadline, of which 69 were deemed complete for round 1. This is an explicit, deliberate repeat of the AES/SHA-3 open-competition model: multiple public rounds, cryptanalysis invited from the entire global research community, transparent published selection rationale (NIST Internal/Interagency Reports, "NISTIR," at every round).

**Track 1 — Key Encapsulation Mechanisms (KEMs) and general encryption:**
- Round 3 (2020) finalists included CRYSTALS-Kyber (lattice), plus alternates NTRU, SABER, Classic McEliece, BIKE, FrodoKEM, HQC, SIKE.
- **Kyber selected as the primary standard**, published August 13, 2024 as **FIPS 203 — Module-Lattice-Based Key-Encapsulation Mechanism Standard**, renamed **ML-KEM** in the final standard (ML-KEM-512/768/1024 parameter sets, corresponding roughly to AES-128/192/256-equivalent security).
- A **fourth round** continued evaluating structurally-independent backups (BIKE, Classic McEliece, HQC, SIKE) specifically as a hedge against the possibility that structured lattices are someday found to have an exploitable weakness that would otherwise simultaneously threaten both ML-KEM *and* ML-DSA (both lattice-based) — this hedge rationale is explicit in NIST's public reporting and is the direct institutional answer to the "single-scheme risk concentration" debate flagged in the Executive Summary.
- **SIKE was catastrophically broken in 2022** — not by a subtle statistical weakness but by a classical (non-quantum!) polynomial-time attack (Castryck–Decru, building on Kani's theorem) that recovered the secret isogeny entirely, executed on an ordinary laptop within about an hour for parameters previously believed to offer over 100 bits of security. This is one of the most dramatic cryptanalytic breaks in the entire PQC program's history and is a standing caution against over-trusting a "young" scheme regardless of how many rounds of a competition it has survived — SIKE had reached round 4 before being broken.
- **HQC (code-based) was selected for standardization on March 11, 2025** as the fourth-round winner, explicitly chosen over BIKE and the remaining candidates for the structurally-independent-backup role; draft standardization work is ongoing as of mid-2026, with a full FIPS expected around 2027.

**Track 2 — Digital Signatures (initial round):**
- Finalists CRYSTALS-Dilithium (lattice), Falcon (NTRU-lattice), SPHINCS+ (hash-based, stateless) were all selected for standardization in 2022.
- **Dilithium → FIPS 204, "ML-DSA"** (Module-Lattice-Based Digital Signature Standard), finalized August 13, 2024.
- **SPHINCS+ → FIPS 205, "SLH-DSA"** (Stateless Hash-Based Digital Signature Standard), finalized August 13, 2024 — the most conservative of the three, valuable precisely *because* its security reduction touches only hash-function properties, with no reliance on any lattice or other structured-algebraic hardness assumption.
- **Falcon → FIPS 206, "FN-DSA"** (FFT-over-NTRU-Lattice-Based Digital Signature Algorithm): the slowest to finalize of the four, because Falcon's signing procedure relies on **floating-point discrete Gaussian sampling**, which is unusually difficult to implement in constant time and free of side-channel leakage — the standard implementation approach (an FFT-based sampler over an "LDL tree," per NIST's own FIPS 206 status presentations) genuinely requires more implementation-guidance work than a typical FIPS draft. NIST submitted the draft for approval on **August 28, 2025**; per NIST's own September 2025 PQC Standardization Conference materials and multiple vendor trackers (DigiCert, Encryption Consulting) as of mid-2026, the Initial Public Draft has been published and industry commentary (including detailed technical feedback threads on NIST's pqc-forum mailing list, e.g. on hash-then-sign parameter choices and randomized-vs-hedged nonce derivation) is ongoing, with the **final FIPS 206 standard expected in late 2026 or early 2027**. FN-DSA offers by far the smallest signatures and public keys of the four NIST PQC signature standards, making it especially attractive for bandwidth-constrained contexts (certificate chains, blockchain transaction signatures) once finalized — but NIST and vendors alike currently recommend it for testing and evaluation only, not production deployment, pending finalization.

**Track 3 — Additional Digital Signature Schemes** (opened by a September 2022 call, specifically seeking signature schemes with security foundations *other than* structured lattices, and/or shorter signatures with faster verification than the existing standards):
- Round 1: 40 submissions.
- Round 2 (announced October 24, 2024): 14 candidates advanced, including CROSS, LESS, and MAYO among others.
- Round 3 / current round: **NIST's own status reporting (NISTIR 8610, updated as recently as May 2026) lists nine candidates — FAEST, HAWK, MAYO, MQOM, QR-UOV, SDitH, SNOVA, SQIsign, and UOV** — spanning zero-knowledge-derived (Fiat–Shamir-transformed) constructions, multivariate systems, isogeny-based SQIsign, and lattice-based HAWK.
- **HAWK was withdrawn from consideration in late July 2026**, directly as a result of the Anthropic Claude Mythos Preview cryptanalysis event described in 2.6 below — a genuinely unusual, very recent case of an AI-discovered attack directly altering the outcome of a live national-standards process while it was still underway, rather than after deployment.

**Governing internal reports** for anyone tracking this process directly: NIST IR 8309 (round 2), IR 8413 (round 3), IR 8545 (round 4, KEMs), IR 8528 (additional signatures round 1), IR 8610 (additional signatures round 2), and SP 800-227 (September 2025, guidance on secure KEM use, definitions, and composition — worth reading directly for anyone implementing ML-KEM or HQC in a real system, since correct KEM usage patterns, e.g. around re-encapsulation and IND-CCA composition with TLS, are genuinely non-trivial and actively researched, per the CPA-vs-CCA-secure-KEM debate referenced in Part 5).

### 2.4 Deployment status (2024–2026) — hybrid, not pure, PQC

Every major production deployment as of 2026 uses **hybrid key exchange**: combine a classical ECDH group with an ML-KEM encapsulation, deriving the session key from *both* outputs concatenated (e.g., X25519MLKEM768), so that the connection remains secure as long as **at least one** of the two component algorithms is unbroken — explicit, deliberate risk-hedging against the possibility that ML-KEM (comparatively young relative to the decades of scrutiny X25519/ECDH has had) harbors an as-yet-undiscovered weakness. This compositional-security property (security holds if either component holds) has been given formal cryptographic proofs in recent literature (covered in Part 5's TLS section).

Concretely, as of mid-2026:
- **Chrome** enabled X25519MLKEM768 by default for desktop clients from version 124 (April 2024); **Firefox** followed with version 132. Cloudflare Radar telemetry reported by industry trackers put hybrid-PQC TLS 1.3 handshakes at over 30% of measured global traffic in early 2026, up from roughly 2% in early 2024 — a genuinely fast ramp for internet-scale cryptographic migration by historical standards.
- **BoringSSL and AWS-LC** ship ML-KEM as a default/available group.
- **The IETF TLS working group** has draft standards (`draft-ietf-tls-mlkem`, `draft-ietf-tls-ecdhe-mlkem`) formally specifying X25519MLKEM768, SecP256r1MLKEM768, and SecP384r1MLKEM1024 as standardized hybrid NamedGroups for TLS 1.3, with formal security analysis (both classical ROM and quantum ROM) published for the compositional-security claims.
- **Microsoft** shipped hybrid post-quantum TLS 1.3 key exchange into production Windows Schannel via July 14, 2026 security updates (Windows 11 24H2/25H2, Windows Server 2025), disabled by default and requiring explicit enablement; Active Directory Certificate Services gained general-availability support for issuing ML-DSA certificates in May 2026.
- **Apple's iMessage PQ3 protocol** (shipped in iOS 17.4, 2024) uses ML-KEM (then still called Kyber) not just at initial session setup but woven into the ongoing per-message ratchet — described by Apple as the first messaging protocol to reach their self-defined "Level 3" post-quantum security (continuous protection, not just handshake protection); this pattern — continuous, not just initial, post-quantum protection — is exactly what Signal's Triple Ratchet (Part 5) later also adopted, and the two efforts are widely discussed together as defining the new state of the art for post-quantum secure messaging design.
- **The JDK's SunJSSE provider** ships ML-KEM/ECDHE hybrids as of early 2026.

**Signatures/certificates are lagging key exchange in deployment**, for a structural reason: PQC signatures (ML-DSA especially) are substantially larger than ECDSA/Ed25519 signatures, and certificate chains compound this — a full hybrid or pure-PQC certificate chain can be many times the size of a classical one, which has real effects on TLS handshake latency and on systems (embedded, constrained-bandwidth) with strict size budgets. This is precisely the practical motivation for FN-DSA/Falcon's small-signature design being earmarked especially for root/intermediate CA certificates (where the compact-signature benefit compounds across every certificate in a chain) once finalized, rather than as a general leaf-certificate replacement for ML-DSA — a nuance drawn directly from vendor engineering guidance (DigiCert) tracking the FN-DSA rollout.

### 2.5 Migration timelines and mandates

- **NIST**: recommends deprecating RSA-2048/ECC P-256-class classical algorithms starting 2030, with a hard prohibition target by 2035 (per NIST IR 8547 and related guidance cited across multiple 2026 tracking sources).
- **NSA / CNSA 2.0**: mandates PQC support for National Security Systems (defense contractors, federal systems handling classified/sensitive data) with phased deadlines — some sources report a January 1, 2027 requirement for new National Security System acquisitions to support CNSA 2.0 algorithms, and a 2035 full-transition target consistent with NIST's broader guidance.
- **A June 2026 U.S. executive order** reportedly set federal civilian deadlines of 2030 for key establishment and 2031 for digital signatures (per industry tracking of the order; readers should verify current federal requirements directly against the executive order and OMB/CISA guidance, since this is exactly the kind of fast-moving policy detail this report's sourcing caveats apply to most strongly).
- **Google** announced a 2029 internal target for full PQC migration across its infrastructure in March 2026, explicitly framed as accelerated relative to prior planning, citing both quantum-hardware progress and the resource-estimate reductions discussed in 2.7 below; **Cloudflare** has stated a comparable target, and the two companies' public alignment is treated in industry coverage as a notable coordination point given their combined share of internet-facing TLS termination.
- **Enterprise adoption lags mandates substantially**: industry surveys cited in 2026 tracking pieces report only around 13% of organizations had moved PQC into production as of that point, with a majority not yet meaningfully begun migration — a real, acknowledged gap between standards/mandate maturity and operational reality that mirrors historical patterns for major cryptographic transitions (TLS 1.3 adoption itself took years to become dominant after RFC 8446).

### 2.6 The July 2026 AI-cryptanalysis event (HAWK / reduced-round AES)

This deserves its own subsection given its recency and significance. On **July 28, 2026**, Anthropic disclosed that its **Claude Mythos Preview** model — used with periodic human direction and review, but doing the bulk of the mathematical exploration autonomously — had, over roughly 60 hours of dedicated effort, discovered a previously unknown mathematical symmetry in the lattice structure underlying **HAWK**, a lattice-based signature scheme that was at the time a live candidate in round 3 of NIST's "additional digital signatures" track (see 2.3 above). The discovery enabled a new key-recovery attack that **effectively halves HAWK's claimed security margin** for the HAWK-256 parameter set — described by multiple outlets as an end-to-end key-recovery attack achievable in roughly 3 hours 42 minutes on a 96-core server once the underlying mathematical insight was in hand. Separately, the same research effort produced a **200–800× speedup on the best previously known attack against a 7-round (of 10) reduced variant of AES-128**, by eliminating a guessing step from an existing meet-in-the-middle attack technique — a meaningful cryptanalytic improvement on an *academic, artificially weakened* AES variant, not any threat to full-round, deployed AES-128.

**What this does and does not mean**, synthesized across the reporting (Anthropic's own disclosure, CSO Online, Heise, The Quantum Insider, SC Media, and cryptocurrency-industry coverage tracking implications for Bitcoin/Ethereum):
- It does **not** affect any deployed production cryptography. HAWK was never standardized or used in any shipped system; the AES result targets a reduced-round research variant that has never been the deployed cipher.
- It does **not** affect Bitcoin or Ethereum directly — both rely on secp256k1 ECDSA and SHA-256, neither targeted by this work, and HAWK was not among the algorithms under consideration for Bitcoin's own (separate, ongoing) post-quantum migration discussions.
- It **does** directly affect NIST's live standardization process: the HAWK development team withdrew their algorithm from consideration as a direct consequence, meaning HAWK will not be standardized or deployed — a real, concrete, standards-altering outcome, not merely an academic curiosity.
- Anthropic reportedly notified HAWK's developers privately in June 2026 (ahead of public disclosure) and consulted NIST's public pqc-forum mailing list as part of responsible-disclosure practice before the July 28 announcement, following the field's normal coordinated-disclosure norms for cryptanalytic findings.
- Human researchers reportedly spent "hundreds of hours" independently validating the AI-produced results before publication — an important detail: this was not published as an unverified AI claim, but as a human-reviewed, human-validated result with the AI credited for the discovery process, consistent with how the field expects any novel cryptanalytic claim (AI-assisted or not) to be handled before publication.
- It **is** a serious, concrete, non-hypothetical data point in the long-running (previously mostly speculative) discussion of what happens once frontier AI systems can sustain sequential, semi-autonomous mathematical research over many hours/days on hard, adversarial problems — cryptanalysis being an unusually clean domain to observe this in, because results are objectively checkable (does the attack actually recover the key or not) in a way many other research domains are not.

🟡 **Medium-to-high confidence overall**: the event itself is well-corroborated across multiple independent outlets citing Anthropic's own disclosure and its effect on HAWK's NIST status is a matter of public record via the withdrawal. What remains genuinely uncertain, and is explicitly flagged as an active debate in this report's Executive Summary, is how far to *generalize* from this one event — whether it signals a durable, ongoing capability shift in cryptanalytic research pace (which would argue for re-evaluating confidence in other "young" PQC candidates that haven't yet received comparable scrutiny) or is better understood as a single, notable but non-representative result. As of this writing (days after disclosure), independent replication and broader community reaction are still developing, and this report's treatment should be expected to age faster than most other sections — check IACR ePrint and the NIST pqc-forum directly for the current state of discussion.

### 2.7 The quantum-hardware timeline itself — what changed recently

Separately from the AI-cryptanalysis thread, 2025–2026 saw a meaningful compression in expert estimates of *how many physical qubits* are actually needed to run Shor's algorithm against RSA-2048-class parameters, driven primarily by **algorithmic** (not hardware) improvements to circuit constructions:
- Estimates that stood at roughly **20 million physical qubits** a few years prior fell to **around 1 million**, then, per multiple 2026 tracking sources citing newer published resource-estimate work (including a widely-discussed Google paper revising down the logical-qubit/gate requirements specifically for 256-bit elliptic curves like P-256 and secp256k1), toward **100,000**, and — per the most recent (and least independently verified, since it originates from a single newer research group, Oratomic) figures circulating as of mid-2026 — potentially as low as **10,000** physical qubits under favorable architecture assumptions.
- This matters because it is explicitly an *algorithmic* story as much as a hardware one: even without a single additional physical qubit being built, better circuit constructions for Shor's algorithm directly lower the bar a real quantum computer needs to clear — a distinct and, to several practitioners, more alarming category of progress than incremental hardware scaling, because it can happen suddenly via a single clever paper rather than gradually via engineering.
- Concretely, the largest current quantum processors (Google's Willow, at roughly 105 physical qubits, cited as the state of the art in mid-2026 tracking) remain many orders of magnitude short of even the lowest of these revised estimates, and **no cryptographically relevant quantum computer (CRQC) exists as of this writing.** But several serious practitioners — cryptography engineer Filippo Valsorda's widely-circulated public commentary being a specific, named example from the search record — have publicly revised their own risk assessments upward in direct response to this specific combination of algorithmic-estimate compression and the observation that resource-estimate reductions have themselves been accelerating (20M → 1M → 100K → potentially 10K, each step arriving faster than the last), independent of any change in physical hardware.
- 🟡 **Medium confidence on the specific numbers** (they come from a mix of peer-reviewed and pre-print/industry-tracked sources, with the lowest figure — 10,000 — the least independently corroborated as of this writing), **high confidence on the qualitative trend**: quantum-timeline estimates compressed meaningfully during 2025–2026, and this is a genuine, not merely marketing-driven, shift reflected in independent expert commentary as well as vendor announcements (Google's own accelerated 2029 internal migration target, discussed in 2.5, is itself a direct institutional response to this trend, by Google's own stated reasoning).

---

## 3. Comparison Tables

### 3.1 NIST PQC standards — status snapshot (August 2026)

| Standard | Algorithm | Type | Hard problem | Status |
|---|---|---|---|---|
| FIPS 203 | ML-KEM (Kyber) | KEM | Module-LWE | **Final** (Aug 13, 2024) |
| FIPS 204 | ML-DSA (Dilithium) | Signature | Module-LWE / Module-SIS | **Final** (Aug 13, 2024) |
| FIPS 205 | SLH-DSA (SPHINCS+) | Signature | Hash-based (stateless) | **Final** (Aug 13, 2024) |
| — | HQC | KEM | Syndrome decoding (code-based) | Selected Mar 2025; draft standard in progress, final expected ~2027 |
| FIPS 206 (draft) | FN-DSA (Falcon) | Signature | NTRU lattice | Initial Public Draft published; final expected late 2026/early 2027 |
| — | FAEST, MAYO, MQOM, QR-UOV, SDitH, SNOVA, SQIsign, UOV | Signature | ZK-derived / multivariate / isogeny | Round 3 of "additional signatures" track, ongoing |
| — (withdrawn) | HAWK | Signature | Lattice | Withdrawn July 2026 after AI-discovered attack |
| — (broken) | SIKE | KEM | Isogeny | Broken by classical cryptanalysis, 2022 |
| — (broken) | Rainbow | Signature | Multivariate | Broken, 2022 |

### 3.2 Classical vs. post-quantum security-level rough mapping

| Classical scheme (bits of security) | PQC KEM equivalent | PQC signature equivalent |
|---|---|---|
| RSA-3072 / ECC-256 (~128-bit) | ML-KEM-512 | ML-DSA-44, FN-DSA-512 |
| RSA-7680 / ECC-384 (~192-bit) | ML-KEM-768 | ML-DSA-65 |
| RSA-15360 / ECC-521 (~256-bit) | ML-KEM-1024 | ML-DSA-87, SLH-DSA (all variants target ≥128-bit with much larger signatures) |

### 3.3 Key/ciphertext/signature size comparison (approximate, illustrative)

| Scheme | Public key | Ciphertext / Signature |
|---|---|---|
| X25519 (classical ECDH) | 32 B | 32 B (shared output) |
| ML-KEM-768 | 1184 B | 1088 B ciphertext |
| Ed25519 | 32 B | 64 B signature |
| ML-DSA-65 | 1952 B | ~3300 B signature |
| SLH-DSA-128s | 32 B | ~7856 B signature (much larger, but most conservative security foundation) |
| FN-DSA-512 (draft) | ~897 B | ~666 B signature (smallest PQC signature) |

*(Figures are approximate/representative of published parameter sets and intended to illustrate relative scale, not to substitute for the authoritative FIPS specification values when implementing.)*

---

## 4. Open Problems and Research Gaps in PQC

- **Structured-lattice risk concentration**: both ML-KEM and ML-DSA rest on module-lattice hardness; a structural break of that assumption family would compromise both simultaneously — the core justification for HQC and SLH-DSA as independent hedges, and an area where continued cryptanalytic pressure-testing (exactly the kind of work the HAWK event exemplifies, even though HAWK itself was a different lattice family) is considered high-value, ongoing research.
- **Constant-time, side-channel-free implementation of Gaussian sampling** (Falcon/FN-DSA) remains a genuinely open engineering-research problem, not fully solved even in the draft-standard implementations circulating for testing.
- **KEM composition correctness**: recent (2025) formal work on whether IND-CPA-secure KEMs are sufficient (versus requiring full IND-CCA2) for secure TLS 1.3 composition is still an active refinement area, with real efficiency implications (up to ~45% key-exchange-layer speedup claimed by CPA-secure-KEM proposals) — not yet fully settled in deployed standards.
- **Crypto-agility** as a systems-engineering research problem: many deployed systems (embedded/IoT/SCADA, firmware-locked hardware) cannot easily rotate algorithms even once a weakness is found, which is a structural, not purely cryptographic, open problem the field increasingly treats as first-class (part of why NIST's SP 800-227 and related guidance emphasize composable, swappable KEM design).
- **Isogeny-based cryptography's future** after SIKE: SQIsign remains a live signature candidate with much smaller keys than lattice alternatives, but the field's confidence in isogeny-based hardness assumptions generally took a real hit in 2022 and rebuilding assurance is an active, explicitly acknowledged research program, not a solved matter.
- **Quantum-safe threshold and multi-party variants** of the new standards (threshold ML-KEM/ML-DSA, needed for HSM/custody-style deployments that currently use threshold ECDSA/Schnorr) are an active, not yet mature, research area (see Part 6).

Part 4 (`04-cryptanalysis-and-randomness.md`) covers the attack techniques referenced throughout this Part in full technical depth, plus randomness generation and testing.
