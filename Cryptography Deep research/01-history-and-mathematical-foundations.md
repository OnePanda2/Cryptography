# Part 1 — History of Cryptography & Mathematical Foundations

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map, methodology, and confidence ratings.*

---

## 1. History of Cryptography

### 1.1 Origins and the classical era

Cryptography begins wherever there is a reason to hide meaning: Egyptian scribes used non-standard hieroglyphs for effect (not real secrecy) around 1900 BCE; the Spartan **scytale** (a transposition device — wrap a strip around a rod of a given diameter) is the earliest attested *military* cryptographic device; Hebrew **atbash** is a simple substitution used in parts of the Old Testament. None of these are cryptography in the modern sense — they lack a formal notion of key or adversary — but they establish the two structural primitives that recur for the next two thousand years: **substitution** (replace symbols) and **transposition** (reorder symbols).

**Caesar cipher** (c. 58–51 BCE): a shift substitution, key space of 25 (for the Latin alphabet). Trivially broken by brute force or frequency analysis; historically important mainly as the namesake for the entire class of shift ciphers and as the standard first example in every cryptography course, because it cleanly demonstrates *key space* and *brute-force attack* before students need any other machinery.

**Frequency analysis**, developed by Arab scholars — most notably **Al-Kindi** in his 9th-century treatise *Risalah fi Istikhraj al-Mu'amma* ("On Deciphering Cryptographic Messages") — is the first systematic cryptanalytic method and arguably the first piece of *statistical* science applied to an adversarial problem. It defeats any monoalphabetic substitution cipher because single-letter (and later digram/trigram) frequencies in natural language are highly non-uniform and are preserved, merely relabeled, by substitution.

**Polyalphabetic substitution** was the first serious response to frequency analysis. The **Vigenère cipher** (popularized in the 16th century, misattributed to Blaise de Vigenère but substantially the work of Giovan Battista Bellaso) uses a repeating key to select a different Caesar shift per position, flattening single-letter frequencies. It was called *le chiffre indéchiffrable* for roughly three centuries until **Friedrich Kasiski** (1863) and, independently and earlier but unpublished, **Charles Babbage** (1850s) showed that repeated ciphertext fragments reveal the key length (via the distance between repeats sharing a common factor), after which each of the resulting Caesar-shifted sub-streams falls to ordinary frequency analysis. This is the first clear historical instance of a general pattern that recurs throughout the field: **any construction that reduces to a small number of independent, individually-weak components can be broken by decomposing it back into those components** — a lesson re-learned in modern block-cipher and mode-of-operation design.

**One-time pad**: proposed practically by Frank Miller in 1882 and reinvented for telegraphy by Gilbert Vernam in 1917; Joseph Mauborgne recognized that a *truly random, single-use, as-long-as-the-message* key gives **perfect (information-theoretic) secrecy** — a fact later proven rigorously by Shannon. The catch, then and now, is key distribution and reuse discipline: the pad must be as long as all messages ever sent under it, generated with true randomness, and never reused. The catastrophic consequence of reuse (a **two-time pad**) is that XORing two ciphertexts cancels the key and leaves the XOR of the two plaintexts, which is usually crackable via crib-dragging — this is exactly what happened to the Soviet **VENONA** traffic in the 1940s when pad material was reused under wartime pressure, giving US/UK cryptanalysts a multi-decade intelligence coup.

### 1.2 Mechanization and WWII

The interwar and WWII period is where cryptography becomes an industrial, state-scale activity, and where cryptanalysis becomes a matter of national survival rather than intellectual curiosity.

**Enigma** (German, commercial origins in the 1920s, adopted and progressively hardened by the Wehrmacht): an electromechanical rotor cipher machine implementing a complex, key-dependent polyalphabetic substitution via a signal path through rotors, a reflector, and (in later/more secure configurations) a plugboard (*Steckerbrett*). Poland's **Biuro Szyfrów** (Marian Rejewski, Jerzy Różycki, Henryk Zygalski) achieved the first breaks in the 1930s using group-theoretic analysis of the rotor permutations, then handed their techniques and a reconstructed machine to the British and French in 1939. At **Bletchley Park**, **Alan Turing** and Gordon Welchman industrialized the attack with the electromechanical **Bombe**, exploiting operational weaknesses (known/likely plaintext "cribs," operator errors, message-setting procedures) as much as mathematical structure — a recurring lesson: real-world breaks are very often a *combination* of a mathematical/structural weakness and an operational/procedural one, and it is often cheaper to attack the procedure.

**Lorenz cipher (Tunny)**: a stream cipher used for high-level German strategic communications, attacked using statistical methods pioneered by **Bill Tutte** (who reconstructed the machine's logical structure purely from ciphertext, without ever seeing the device — one of the great feats of applied cryptanalysis) and broken operationally with **Colossus**, arguably the first programmable electronic digital computer, built specifically for this cryptanalytic task in 1943–44 under Tommy Flowers.

**SIGABA / ECM Mark II** (US): unlike Enigma, never known to have been broken by the Axis, largely due to its irregular, non-repeating rotor-stepping design.

**Purple** (Japanese diplomatic cipher): broken by US Army cryptanalysts (William Friedman's team) via pure cryptanalysis without capturing a machine, producing the MAGIC intelligence program.

The overall lesson of WWII cryptography, still taught explicitly today: cryptographic security is a *systems* property, not a machine property — key management, operator discipline, message indicators, and traffic analysis matter as much as the cipher's mathematical structure, and the eventual breaks were achieved by combining mathematics, engineering (purpose-built computing machines), and operational intelligence.

### 1.3 The Shannon–Kerckhoffs foundation

**Auguste Kerckhoffs** (1883), in *La Cryptographie Militaire*, articulated what is now **Kerckhoffs's principle**: a cryptosystem should remain secure even if everything about it *except the key* is public knowledge. This is not merely a convenience — it reframes the entire field. It rules out "security through obscurity" as a foundation (though it doesn't forbid it as a *supplement*), and it forces designers to locate all secrecy in a compact, replaceable, information-theoretically-analyzable object: the key.

**Claude Shannon**, "*Communication Theory of Secrecy Systems*" (1949, building on his unpublished wartime work) is the true founding document of cryptography as a mathematical science. Shannon:
- Defined **perfect secrecy**: a cipher has perfect secrecy if the ciphertext reveals *zero* information about the plaintext, i.e., for every plaintext and ciphertext, the posterior distribution over plaintexts given the ciphertext equals the prior. He proved the one-time pad achieves this, and — critically — proved that perfect secrecy *requires* a key at least as long as the message (a fundamental limit, not an engineering shortcoming).
- Introduced **confusion and diffusion** as the two properties a good cipher needs: confusion makes the relationship between key and ciphertext as complex as possible (hiding statistical structure); diffusion spreads the influence of each plaintext/key bit over many ciphertext bits (so that local statistical patterns in the plaintext are smeared out). Every modern block cipher — DES's Feistel network, AES's substitution-permutation network — is explicitly engineered to deliver confusion (via nonlinear S-boxes) and diffusion (via linear mixing layers/permutations) in alternating rounds.
- Founded **information theory** in the companion 1948 paper, giving cryptography its formal vocabulary for **entropy**, redundancy, and unicity distance (the amount of ciphertext beyond which a unique plaintext/key becomes determinable, for ciphers weaker than perfect secrecy — most real ciphers, since perfect secrecy's key-length requirement is impractical).

Shannon + Kerckhoffs together define the field's founding contract: **security must be provable (or at least soundly arguable) against a fully-informed adversary who knows everything but the key**, using **information-theoretic** language where possible and, where perfect secrecy is infeasible, a fallback to **computational** security (the subject of Part 2.6 below).

### 1.4 The public-key revolution

Until 1976, all cryptography was **symmetric**: the same key encrypts and decrypts, so it must be shared secretly in advance — the fundamental *key distribution problem*, which scales as O(n²) shared secrets for n parties and requires an out-of-band secure channel that itself needs protecting.

**Whitfield Diffie and Martin Hellman**, "*New Directions in Cryptography*" (1976), solved this with an idea of stunning conceptual simplicity: use a function that is easy to compute in one direction but computationally infeasible to invert without extra information (a **trapdoor**), and split the key into a **public** part (safe to broadcast) and a **private** part (kept secret). Their concrete construction, **Diffie–Hellman key exchange**, lets two parties agree on a shared secret over a fully public channel using the presumed hardness of the **discrete logarithm problem** in a finite cyclic group. (Ralph Merkle independently originated closely related ideas — "Merkle's puzzles" — around the same time, and is credited by Diffie and Hellman themselves; the concept is sometimes called Diffie–Hellman–Merkle key exchange in recognition.)

**Ron Rivest, Adi Shamir, and Leonard Adleman**, "*A Method for Obtaining Digital Signatures and Public-Key Cryptosystems*" (1978), gave the first full public-key **cryptosystem** (not just key exchange) — **RSA** — based on the presumed hardness of **integer factorization**, and, as a byproduct, the first practical digital signature scheme. RSA's elegance (encryption and signing are essentially the same modular-exponentiation operation with public/private exponents swapped) made it the workhorse of public-key cryptography for the next four decades.

It later emerged that **James Ellis, Clifford Cocks, and Malcolm Williamson** at Britain's GCHQ had discovered the core ideas of public-key cryptography (including a scheme equivalent to RSA, by Cocks in 1973, and Diffie–Hellman-equivalent key exchange, by Williamson) several years earlier, but the work was classified and only declassified in 1997. This is a genuinely important historical footnote: it shows the ideas were, in some sense, "in the air" given the state of number theory and computing at the time, and it raises the still-debated question of how much credit and priority should attach to classified-but-unpublished discovery versus open publication that actually shapes the field.

**Elliptic curve cryptography (ECC)** was proposed independently by **Neal Koblitz** and **Victor Miller** in 1985: the same discrete-logarithm-based constructions (Diffie–Hellman, signatures) can be run in the group of points on an elliptic curve over a finite field instead of the multiplicative group of integers mod p. The elliptic-curve discrete-log problem (ECDLP) has no known sub-exponential attack (unlike factoring and finite-field discrete log, both of which fall to the sub-exponential **general number field sieve** / **index calculus** family), so ECC achieves equivalent security to RSA with dramatically shorter keys (a 256-bit ECC key is roughly comparable in strength to a 3072-bit RSA key) — smaller keys, faster operations, less bandwidth. This made ECC the practical default for new deployments from roughly the mid-2000s onward.

**Provable security** matured through the late 1980s–2000s: **Shafi Goldwasser and Silvio Micali** ("*Probabilistic Encryption*," 1984) gave the first rigorous definitions of semantic security (what it *means* for an encryption scheme to hide information, formalized as computational indistinguishability rather than Shannon's information-theoretic notion); Goldwasser, Micali, and **Charles Rackoff** introduced **zero-knowledge proofs** (1985); **Oded Goldreich, Silvio Micali, and Avi Wigderson** showed zero-knowledge proofs exist for all of NP (1986/1991), a foundational result for the entire modern ZK/succinct-proofs research program covered in Part 6. **Mihir Bellare and Phillip Rogaway** formalized the **random oracle model** (1993) and pioneered practical **provable security for real-world constructions** (OAEP, PSS, and the whole "concrete security"/exact-security research program that lets practitioners read off actual bit-security levels from a reduction rather than just asymptotic statements).

### 1.5 Modern standardization

**DES** (Data Encryption Standard, 1977), based on IBM's Lucifer cipher with NSA-influenced modifications (a shortened, contested 56-bit key and altered S-boxes later shown to *harden*, not weaken, against differential cryptanalysis — a technique the public research community wouldn't rediscover until the late 1980s), was the first publicly standardized, openly specified block cipher and the direct ancestor of the entire modern block-cipher research tradition. Its 56-bit key became practically breakable by brute force by the late 1990s (the EFF's purpose-built "Deep Crack" machine broke a DES challenge in 56 hours in 1998), driving the interim **3DES** (triple-application) patch and, ultimately, a fully open public competition.

**AES (Advanced Encryption Standard)**: NIST ran an open, international, multi-round public competition (1997–2000) — a first for a cryptographic standard of this importance — that selected **Rijndael**, designed by **Joan Daemen and Vincent Rijmen**, over strong finalists including Serpent, Twofish, RC6, and MARS. The competition model itself (open call, public cryptanalysis by the entire world's research community, transparent selection criteria) became the template NIST reused for SHA-3 (2007–2012) and for the PQC standardization process (2016–ongoing), and is now considered close to a best practice for how a national standards body should develop cryptography it expects the world to trust.

**Public-key standardization** (RSA, DH, ECC via NIST curves and later Curve25519/Ed25519) proceeded more incrementally through ANSI X9, IEEE P1363, and NIST SP 800-56 series rather than open competitions, which is itself a point of some retrospective criticism (see the Dual_EC_DRBG history in Part 4) — competitions surface adversarial scrutiny that closed-door standardization processes structurally cannot.

### 1.6 The post-quantum era

**Peter Shor** (1994) showed that a sufficiently large, fault-tolerant **quantum computer** can factor integers and compute discrete logarithms in polynomial time, which would break RSA, (finite-field and elliptic-curve) Diffie–Hellman, DSA, and ECDSA outright — essentially all deployed public-key cryptography as of the mid-1990s. **Lov Grover** (1996) showed a quantum computer gives a quadratic (not exponential) speedup for unstructured search, which halves the effective security of symmetric ciphers and hash functions (AES-128 drops to roughly AES-64-equivalent security against a quantum adversary) — serious, but survivable by doubling key/output sizes (hence AES-256 and 256-bit hash outputs being recommended as the quantum-safe symmetric baseline).

Shor's result is theoretical but was never in doubt as a *motivation*; what changed over 2015–2024 was the field's assessment of *how soon* it would matter, driven by (a) steady, if slower-than-hyped, progress in physical quantum computing hardware, and (b) growing recognition of the **harvest-now-decrypt-later** threat model, under which today's still-classical adversary already benefits from recording today's traffic for future decryption. NIST opened its **Post-Quantum Cryptography Standardization** process in 2016 (announced at PQCrypto 2016), received 82 initial submissions, and — after multiple public rounds of open cryptanalysis, exactly the AES/SHA-3 playbook — finalized **FIPS 203/204/205** on August 13, 2024. This is covered in full technical and status detail in Part 3.

### 1.7 Major historical milestones — timeline

| Era | Milestone |
|---|---|
| ~1900 BCE | Non-standard hieroglyphs (Egypt) — earliest attested intentional obscuring of writing |
| ~500 BCE | Spartan scytale (transposition) |
| 9th c. CE | Al-Kindi formalizes frequency analysis |
| 1553 | Bellaso (popularized later as "Vigenère") polyalphabetic cipher |
| 1854 | Playfair cipher (digraph substitution) |
| 1863 | Kasiski defeats Vigenère-family ciphers generally |
| 1917 | Vernam one-time pad (telegraphy); ZIMMERMANN telegram intercepted/decrypted, pulls US into WWI |
| 1918–1930s | Enigma commercialized, then militarized |
| 1932–1939 | Polish Biuro Szyfrów breaks Enigma; hands methods to Britain/France |
| 1939–1945 | Bletchley Park; Bombe; Colossus; Tunny/Lorenz break |
| 1948–1949 | Shannon: information theory + "Communication Theory of Secrecy Systems" |
| 1970s | VENONA decrypts declassified; DES standardized (1977) |
| 1976 | Diffie–Hellman public-key key exchange |
| 1977–1978 | RSA |
| 1982–1985 | GCHQ's classified Ellis/Cocks/Williamson work (declassified 1997) |
| 1984–1986 | Goldwasser–Micali semantic security; GMR zero-knowledge |
| 1985 | Koblitz and Miller independently propose elliptic curve cryptography |
| 1990 | Differential cryptanalysis publicly formalized (Biham–Shamir) |
| 1993 | Linear cryptanalysis (Matsui); random oracle model (Bellare–Rogaway) |
| 1994 | Shor's algorithm |
| 1996 | Grover's algorithm |
| 1997–2000 | AES public competition; Rijndael selected |
| 2001 | AES becomes FIPS 197 |
| 2004–2005 | Wang et al. break MD5 and SHA-1 collision resistance (practically demonstrated for MD5 immediately; SHA-1 practically broken by Google/CWI's SHAttered in 2017) |
| 2007–2015 | SHA-3 competition → Keccak selected (2012) → FIPS 202 (2015) |
| 2013 | Snowden disclosures; Dual_EC_DRBG backdoor suspicion publicly substantiated |
| 2014 | Curve25519/Ed25519 gain rapid real-world adoption (Bernstein et al., published 2006/2011) |
| 2016 | NIST opens PQC standardization |
| 2018–2020 | TLS 1.3 (RFC 8446) finalized; Signal Double Ratchet widely deployed |
| 2022 | NIST selects Kyber/Dilithium/SPHINCS+/Falcon; SIKE (isogeny KEM) catastrophically broken by classical cryptanalysis |
| 2023 | Signal deploys PQXDH |
| Aug 2024 | FIPS 203/204/205 finalized |
| Mar 2025 | HQC selected as backup KEM |
| 2025 | Signal Triple Ratchet/SPQR; SLAP/FLOP Apple-silicon side channels; FN-DSA draft submitted |
| Jul 2026 | Anthropic's Claude Mythos Preview finds novel HAWK attack + AES-128(7-round) speedup; HAWK withdrawn from NIST consideration |

---

## 2. Mathematical Foundations

This section is the field's actual prerequisite spine. Everything in Parts 2–5 of this report assumes fluency here. It is written as a dense reference, not a first-pass tutorial — pair it with the textbooks in Part 6 if any section is unfamiliar.

### 2.1 Modular arithmetic

The ring **ℤ/nℤ** (integers mod n) underlies essentially all classical and much modern number-theoretic cryptography. Key facts a cryptographer must have cold:
- **Euler's theorem**: if gcd(a, n) = 1, then a^φ(n) ≡ 1 (mod n), where φ is Euler's totient function. This is the mathematical engine of RSA: choosing e, d with ed ≡ 1 (mod φ(n)) makes (a^e)^d = a^(ed) ≡ a (mod n).
- **Fermat's little theorem** is the p-prime special case (φ(p) = p−1); it underlies Fermat primality testing and is a building block of Miller–Rabin.
- **Extended Euclidean algorithm** computes modular inverses in O(log n) time — needed constantly, for RSA key generation (computing d) and for ECC scalar arithmetic.
- **Chinese Remainder Theorem (CRT)**: ℤ/nℤ ≅ ℤ/pℤ × ℤ/qℤ for n = pq with gcd(p,q)=1. Used both as an *optimization* (RSA-CRT: doing the private-key operation mod p and mod q separately, then recombining, is roughly 4× faster than working mod n directly) and as a source of real implementation vulnerabilities (RSA-CRT computations are much more exposed to fault attacks — a single faulty computation of one CRT branch, combined with the correct other branch, leaks the private key via a simple GCD, the **Bellcore attack**).
- **Quadratic residues** and the **Legendre/Jacobi symbols** underlie some signature and identification schemes and several primality tests.
- **Discrete logarithm problem (DLP)**: given g, h in a cyclic group G with h = g^x, find x. Believed hard in "generic" groups (formalized via the generic group model) and in specific groups like (ℤ/pℤ)* and elliptic curve groups when parameters are chosen well; *not* hard in additive groups (trivial) or in groups where a sub-exponential algorithm like index calculus applies (multiplicative groups of finite fields — this is *why* finite-field DH needs much larger parameters than elliptic-curve DH for equivalent security).

### 2.2 Groups, rings, fields

- **Group**: a set with an associative binary operation, identity, and inverses. Cryptographically relevant groups: (ℤ/nℤ, +), ((ℤ/pℤ)*, ×), elliptic curve point groups, ideal class groups (isogeny-based crypto).
- **Ring**: a set with two operations (+, ×) where + forms an abelian group and × distributes over +; need not have multiplicative inverses. ℤ, ℤ/nℤ, and polynomial rings F[x] are the workhorses.
- **Field**: a ring where every nonzero element has a multiplicative inverse. Finite fields (Galois fields) **GF(p)** (p prime) and **GF(2^n)** (used pervasively — AES's S-box arithmetic is GF(2^8)) are essential. Every finite field has order p^k for prime p and is unique up to isomorphism for a given order — this uniqueness is why "GF(256)" is an unambiguous phrase.
- **Vector spaces and lattices** are built over these fields/rings; see 2.6.

### 2.3 Elliptic curves

An elliptic curve over a field K (char ≠ 2, 3, for the simple case) is the set of solutions to y² = x³ + ax + b (plus a point at infinity, O), which forms an **abelian group** under a geometrically-defined "chord-and-tangent" addition law (three collinear points on the curve sum to the identity). Over a finite field GF(p) or GF(2^m), this group is finite, and its order is bounded by the **Hasse bound**: |#E(F_q) − (q+1)| ≤ 2√q.

Why cryptography cares: scalar multiplication (computing kP for a point P and integer k, via double-and-add) is efficient, but the inverse — the **elliptic curve discrete logarithm problem (ECDLP)**, given P and kP find k — has no known sub-exponential classical algorithm for well-chosen curves (the best generic attacks, Pollard's rho and baby-step-giant-step, are fully exponential, O(√n)). This is *why* ECC gets away with much smaller keys than RSA/finite-field-DH for equivalent security.

Curve choice matters enormously and is a recurring source of controversy:
- **NIST curves** (P-256, P-384, P-521; the "Suite B" / CNSA curves): generated via a seeded, supposedly-unbiased process, but the seed generation was never fully justified publicly, feeding persistent (never proven, never fully dismissed) suspicion after the Dual_EC_DRBG revelations that *some* NIST-era parameter choices might have been engineered for exploitability. This suspicion, warranted or not, is a major reason the field moved toward curves with fully auditable, "nothing-up-my-sleeve" generation.
- **Curve25519 / Ed25519** (Daniel J. Bernstein et al.): designed explicitly for rigid, reproducible parameter generation, resistance to a wide array of implementation pitfalls (twist-security, no need for point validation in the same way, complete/constant-time addition formulas via the Montgomery/Edwards curve forms), and speed. Rapidly became the de facto preferred choice for new protocol design (SSH, Signal, WireGuard, TLS 1.3's default groups) once available.
- **Pairing-friendly curves** (BN curves, BLS12-381, etc.) support **bilinear pairings** e: G1 × G2 → GT, an additional algebraic structure enabling identity-based encryption (Boneh–Franklin), short (BLS) signatures, and much of the machinery behind zk-SNARKs (see Part 6). Pairing-based cryptography rests on a *different, stronger* hardness assumption family (bilinear Diffie-Hellman variants) than plain ECDLP, worth knowing as a distinct trust boundary.

### 2.4 Polynomial arithmetic, Boolean algebra, coding theory

- **Polynomial rings** F[x]/(f(x)) for an irreducible f are how finite fields GF(p^n) are constructed for n > 1, and are the algebraic home of Reed–Solomon codes, BCH codes, and — critically for PQC — **Ring-LWE** and **NTRU** lattice constructions, which work in rings like ℤ_q[x]/(x^n+1).
- **Boolean functions**: block cipher S-boxes are Boolean functions (or vectorial Boolean functions, F_2^n → F_2^m), and their cryptographic quality is measured by properties like **nonlinearity** (distance from the nearest affine function — resists linear cryptanalysis), **differential uniformity** (resists differential cryptanalysis), and the **algebraic degree** (resists algebraic/higher-order attacks). AES's S-box, built from the multiplicative inverse in GF(2^8) composed with an affine transformation, was specifically chosen for near-optimal values across all of these simultaneously — a genuinely elegant piece of applied algebra.
- **Coding theory**: error-correcting codes (Hamming, BCH, Goppa, Reed–Solomon) are dual-use in cryptography — they're the basis of an entire PQC family (code-based cryptography: McEliece, HQC — see Part 3) because *decoding a random-looking linear code is NP-hard in the worst case and believed hard on average for well-chosen parameters*, while decoding a code with known efficient-decoding structure (a Goppa code, say) is easy for the holder of the structure and hard for everyone else — exactly the asymmetric trapdoor structure public-key cryptography needs.

### 2.5 Probability and information theory

- **Entropy** H(X) = −Σ p(x) log₂ p(x): the fundamental measure of unpredictability, in bits. Distinguish **Shannon entropy** (average-case, what most people mean by "entropy") from **min-entropy** H_∞(X) = −log₂ max_x p(x) (worst-case, the relevant quantity for cryptographic key material, since an adversary only needs to guess correctly *once*).
- **Statistical distance / total variation distance** between two distributions is the standard way to bound "how distinguishable" a pseudorandom output is from truly random, and underlies the leftover hash lemma and randomness-extraction arguments used in DRBG design.
- **Birthday bound**: among k uniformly random draws from a space of size N, a collision appears with probability ≈ 1 − e^(−k²/2N), meaningfully likely once k ≈ √N. This single fact explains why hash function output length must be roughly *double* the desired collision-resistance security level (a 256-bit hash gives ~128-bit collision resistance, not 256-bit) and drives the block size / rekeying limits on many symmetric modes (e.g., why 64-bit block ciphers like 3DES/Blowfish become risky at large data volumes — the "Sweet32" attack class).
- **Kolmogorov complexity** and pseudorandomness: a sequence is "random" in the Kolmogorov sense if it has no shorter description than itself; cryptographic pseudorandomness is a computational relaxation of this — a PRG output need not be *incompressible* in the absolute sense, only *indistinguishable from random to any efficient adversary*.

### 2.6 Computational complexity and hard problems

Modern cryptography's security claims are almost all **computational**, not information-theoretic (with hash-based signatures and a few other schemes as partial exceptions in their reduction targets, though the underlying hash function is still a computational assumption). The standard toolkit:

- **Asymptotic notation and polynomial-time**: security is defined against adversaries running in probabilistic polynomial time (PPT) in a security parameter λ, with advantage required to be **negligible** (smaller than the inverse of every polynomial in λ) — not "small," a specific asymptotic notion.
- **One-way functions (OWFs)**: easy to compute, hard to invert on average for a PPT adversary. Their existence is the single most important open foundational question in the field (P ≠ NP is necessary but almost certainly not sufficient for OWFs to exist; OWFs' existence is exactly equivalent to the existence of essentially all of "Minicrypt" — pseudorandom generators, pseudorandom functions, symmetric encryption, digital signatures, commitments — per Håstad–Impagliazzo–Levin–Luby and related work). Everything downstream of Part 2–5 of this report is, formally, conditional on some hardness assumption; nothing in classical (non-quantum-resistant, and arguably even PQC) cryptography is *unconditionally* proven secure against all polynomial-time adversaries, because that would resolve major open complexity-theoretic questions.
- **Trapdoor one-way functions**: OWFs with an extra secret that makes inversion easy — the structural requirement for public-key encryption.
- **The standard hard-problem menu**: integer factorization; RSA problem (given N, e, c, find m with m^e ≡ c mod N — believed possibly *easier* than full factoring, though no separation is proven); discrete log (finite field and elliptic curve); computational and decisional Diffie-Hellman (CDH/DDH); Learning With Errors (LWE) and its ring/module variants; the Short Integer Solution (SIS) problem; syndrome decoding (code-based); the isogeny problem between supersingular elliptic curves; multivariate quadratic (MQ) system solving.
- **Reductions**: the core proof technique. A security proof typically shows "if adversary A breaks scheme S with non-negligible advantage, we can build adversary B that solves hard problem P with related advantage," i.e., breaking S is *at least as hard* as solving P. The quality of a reduction matters a great deal in practice — a *tight* reduction (advantage/runtime loss is a small constant) lets you choose parameters close to the raw hardness of P; a *loose* reduction (loss scales with, e.g., the number of adversary queries) forces larger, more conservative parameters to compensate, which is a live, contested area of "concrete/exact security" research, not a settled matter.
- **Random oracle model (ROM)**: a proof heuristic where a hash function is modeled as a truly random function that all parties can query. Many practical, efficient schemes (RSA-OAEP, RSA-PSS, many Fiat–Shamir-derived signatures including much of the PQC signature landscape) only have proofs in the ROM. This is a real, acknowledged gap: **Canetti, Goldreich, and Halevi** (2004) constructed artificial (not "natural") schemes that are secure in the ROM but insecure for *any* real hash function instantiation, proving ROM security doesn't *imply* real-world security. In practice the field treats ROM proofs as strong evidence, not proof, and prefers **standard-model** proofs (no idealized oracle, just the raw hardness assumption) when the efficiency cost is acceptable — a real, ongoing design tension, not resolved.
- **Quantum random oracle model (QROM)**: the ROM adapted so the (quantum) adversary can query the oracle in superposition — the correct model to prove PQC schemes secure against quantum adversaries with quantum access to hash queries, and technically much harder to work in than the classical ROM (many classical ROM proof techniques, like rewinding, don't naively work against a quantum adversary who can un-compute).

### 2.7 Formal security models: game-based vs. simulation-based

Two dominant *styles* of security definition, both essential vocabulary:

- **Game-based (indistinguishability-based) definitions**: define security via an explicit game between a challenger and adversary — e.g., IND-CPA (indistinguishability under chosen-plaintext attack): adversary picks two messages, challenger encrypts one at random, adversary must guess which with advantage better than 1/2. Extended to IND-CCA1/CCA2 (adversary gets a decryption oracle, before/after the challenge respectively), EUF-CMA (existential unforgeability under chosen-message attack, the standard signature security notion), and many more. These are the dominant style for encryption and signature security because they're relatively easy to state precisely and to prove tight reductions for.
- **Simulation-based definitions**: require that whatever an adversary can do in the real protocol, a simulator with only "ideal" access (e.g., to a trusted functionality) could achieve equally well — "real world indistinguishable from ideal world." This is the dominant style for multi-party computation and general protocol composition (Universal Composability, Canetti 2001) because game-based definitions don't compose well automatically (a scheme proven secure "standalone" via a game can become insecure when run alongside other protocol instances) whereas UC-secure components compose safely by construction. The cost is that simulation-based/UC proofs are typically much more involved to construct.

### 2.8 Lattices

A **lattice** L ⊂ ℝ^n is the set of all integer linear combinations of a set of basis vectors. Lattice-based cryptography's hardness rests on problems like:
- **Shortest Vector Problem (SVP)**: find the shortest nonzero vector in L.
- **Closest Vector Problem (CVP)**: given a target point, find the closest lattice point.
- **Learning With Errors (LWE)** (Regev, 2005): given many samples (a_i, b_i = ⟨a_i, s⟩ + e_i mod q) for a secret s and small errors e_i, find s. Regev proved a **worst-case to average-case reduction**: solving LWE on average is at least as hard as solving certain lattice problems (like SVP) in the *worst case*, on a *quantum* computer for the general reduction (a classical reduction exists for weaker parameter regimes). This worst-case-to-average-case connection is philosophically important and fairly rare in cryptography — most hardness assumptions (factoring, discrete log) are only conjectured average-case hard, with no proof they're as hard as their worst-case variant.
- **Ring-LWE / Module-LWE**: LWE instantiated over a polynomial ring (Ring-LWE) or over modules over such a ring (Module-LWE, used by ML-KEM/Kyber and ML-DSA/Dilithium) for efficiency — smaller keys, faster arithmetic (via NTT, the Number-Theoretic Transform, a finite-field analogue of the FFT) — at the cost of relying on the extra ring structure not being exploitable. Whether this structure creates an exploitable weakness compared to "plain" LWE is exactly the "structured vs. unstructured lattices" debate flagged in the Executive Summary.
- **NTRU**: a different, older (1996, Hoffstein–Pipher–Silverman) lattice-adjacent construction based on polynomial ring arithmetic, predating the LWE framework; Falcon/FN-DSA is built on an NTRU lattice.
- **Practical lattice-reduction algorithms** — **LLL** (Lenstra–Lenstra–Lovász, 1982) and **BKZ** (Block Korkine-Zolotarev) — are the actual attack tools cryptanalysts use to try to find short lattice vectors in practice; parameter selection for lattice-based PQC schemes is done by estimating the concrete cost of the best known BKZ-family attacks (via tools like the "lattice estimator" maintained by the PQC research community), not by asymptotic statements alone.

### 2.9 Hash functions as mathematical objects

A cryptographic hash function H: {0,1}* → {0,1}^n needs (at minimum) **preimage resistance** (given y, hard to find x with H(x)=y), **second-preimage resistance** (given x, hard to find x'≠x with H(x')=H(x)), and **collision resistance** (hard to find *any* x≠x' with H(x)=H(x')) — collision resistance is the strongest and implies the others aren't trivially broken, but is not strictly implied by them, and is subject to the birthday bound (2.5 above), which is why output length must roughly double the target security level. Modern constructions:
- **Merkle–Damgård**: iterate a fixed-size compression function over padded message blocks, chaining outputs. Simple and provably reduces collision-resistance of the whole to the compression function, but suffers **length-extension**: given H(m) and len(m) but not m, one can compute H(m ‖ pad ‖ m') for attacker-chosen m' without knowing m — this broke naive "H(key ‖ message)" MACs built on MD5/SHA-1/SHA-2 and is *why* HMAC (Bellare–Canetti–Krawczyk, using a specific nested construction) rather than naive concatenation is the standard MAC-from-hash construction, and part of why SHA-3's sponge design was deliberately chosen not to have this property.
- **Sponge construction** (Keccak/SHA-3): absorb input into a large internal state via a fixed permutation, then squeeze output; naturally resists length-extension, naturally supports arbitrary output length (making it the basis of the **SHAKE** extendable-output functions), and has a clean security proof reducing to the underlying permutation being modeled as ideal — covered in full in Part 2 (of the report, i.e., the file `02-...`).

This concludes the mathematical spine. Part 2 (`02-primitives-and-symmetric-cryptography.md`) builds the actual primitive zoo — block ciphers, stream ciphers, hash functions, MACs, and modes — directly on top of everything above.
