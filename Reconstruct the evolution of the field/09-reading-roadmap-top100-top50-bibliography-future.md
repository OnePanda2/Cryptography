# 09 — Reading Roadmap, Top 100 Papers, Top 50 Milestones & Future Directions

*Final file of a multi-file chronological history — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`08`.*

Your outline asked for the reading order to reconstruct the field's actual intellectual journey rather than simply rank papers by importance — so this file does not separate "the reading roadmap" from "the top 100 papers" into two different lists with two different orderings. **The chronological reading order below *is* the top 100 papers list**: reading it start to finish, in order, is reading this report's entire intellectual history at primary-source depth, with each entry building on what came before it exactly as it did historically.

---

## Chronological Reading Order / Top 100 Papers

Grouped by the era chapters (`01`–`05`) they belong to, in reading order within each group. Where a paper is discussed in depth elsewhere in this report or its companion documents, that's noted rather than repeated.

### Era 1: 1993–2001 (`01`) — 14 entries

1. Matsui, "Linear Cryptanalysis Method for DES Cipher" (EUROCRYPT 1993)
2. Biham & Shamir, "Differential Cryptanalysis of DES-like Cryptosystems" (Journal of Cryptology 1991, foundational to this era's methodology)
3. Bellare & Rogaway, "Random Oracles are Practical: A Paradigm for Designing Efficient Protocols" (CCS 1993)
4. Rivest, "The RC5 Encryption Algorithm" (FSE 1994)
5. Shor, "Algorithms for Quantum Computation: Discrete Logarithms and Factoring" (FOCS 1994)
6. Bellare & Rogaway, "Optimal Asymmetric Encryption — How to Encrypt with RSA" (OAEP, EUROCRYPT 1994)
7. NIST, FIPS 180-1 (SHA-1 specification, 1995)
8. Grover, "A Fast Quantum Mechanical Algorithm for Database Search" (STOC 1996)
9. Kocher, "Timing Attacks on Implementations of Diffie-Hellman, RSA, DSS, and Other Systems" (CRYPTO 1996)
10. NIST, AES call for candidates (1997)
11. Kocher, Jaffe, Jun, "Differential Power Analysis" (CRYPTO 1999)
12. Bellare & Rogaway, "The Exact Security of Digital Signatures — How to Sign with RSA and Rabin" (PSS, EUROCRYPT 1996) — read alongside #6
13. Daemen & Rijmen, Rijndael/AES submission documentation (1998–2000) and NIST's AES selection announcement (2000)
14. NIST, FIPS 197 (AES, 2001)

### Era 2: 2002–2008 (`02`) — 16 entries

15. Wang, Feng, Lai, Yu, "Collisions for Hash Functions MD4, MD5, HAVAL-128 and RIPEMD" (2004)
16. Canetti, Goldreich, Halevi, "The Random Oracle Methodology, Revisited" (JACM 2004, STOC 1998 original)
17. Wang, Yin, Yu, "Finding Collisions in the Full SHA-1" (CRYPTO 2005)
18. Bernstein, "The Salsa20 Family of Stream Ciphers" (eSTREAM submission, 2005)
19. Bernstein, "Cache-timing attacks on AES" (2005)
20. Bernstein, "Curve25519: new Diffie-Hellman speed records" (PKC 2006)
21. NIST, SP 800-90A (Dual_EC_DRBG and other DRBG mechanisms, 2006)
22. Bertoni, Daemen, Peeters, Van Assche, Keccak/sponge-construction submission and design papers (2007–2008)
23. Shumow & Ferguson, public presentation on Dual_EC_DRBG's potential backdoor (CRYPTO rump session, 2007)
24. NIST, SHA-3 competition call (2007)
25. Bernstein, "ChaCha, a variant of Salsa20" (2008)
26. Bertoni, Daemen, Peeters, Van Assche, "On the Indifferentiability of the Sponge Construction" (EUROCRYPT 2008)
27. Bellare & Namprempre, "Authenticated Encryption: Relations among Notions and Analysis of the Generic Composition Paradigm" (Journal of Cryptology 2008, ASIACRYPT 2000 original) — read here for its full consequences, though the original result predates this sub-window
28. Aumasson, Henzen, Meier, Phan, "SHA-3 proposal BLAKE" (2008, SHA-3 first-round submission)
29. Boneh & Franklin, "Identity-Based Encryption from the Weil Pairing" (SIAM J. Computing 2003, CRYPTO 2001 original) — read here as background to pairing-based cryptography's continuing role
30. Vaudenay, "Security Flaws Induced by CBC Padding — Applications to SSL, IPSEC, WTLS..." (EUROCRYPT 2002) — the padding-oracle attack family's founding paper, essential AEAD-thread background

### Era 3: 2009–2015 (`03`) — 20 entries

31. eSTREAM final portfolio report (2008–2009)
32. Gentry, "Fully Homomorphic Encryption Using Ideal Lattices" (STOC 2009)
33. Intel, AES-NI specification and rollout documentation (2010)
34. Mouha, Wang, Gu, Preneel, "Differential and Linear Cryptanalysis Using Mixed-Integer Linear Programming" (Inscrypt 2011)
35. NIST, Keccak selected as SHA-3 winner, announcement and rationale (2012)
36. Aumasson, Neves, Wilcox-O'Hearn, Winnerlein, "BLAKE2: simpler, smaller, fast as MD5" (ACNS 2013)
37. Aumasson & Bernstein, "SipHash: a fast short-input PRF" (INDOCRYPT 2012)
38. Bernstein, Duif, Lange, Schwabe, Yang, "High-speed high-security signatures" (Ed25519, CHES 2011 / J. Cryptographic Engineering 2012)
39. Snowden disclosures begin; reporting on RSA/BSAFE and Dual_EC_DRBG (2013, primary journalism and subsequent technical analysis)
40. NSA/Beaulieu, Shors, Smith, Treatman-Clark, Weeks, Wingers, "The SIMON and SPECK Families of Lightweight Block Ciphers" (2013, ePrint)
41. Aumasson, Jovanovic, Neves, "NORX: Parallel and Scalable AEAD" (ESORICS 2014)
42. IETF, CAESAR competition call (2014)
43. IETF TLS WG, early TLS 1.3 drafts (2014 onward)
44. Zinzindohoué, Bhargavan, Protzenko, Beurdouche, background papers leading to HACL* (formal-verification thread, work beginning this period)
45. NIST, FIPS 202 (SHA-3, 2015)
46. Salmon, Moraes, Dror, Shaw, "Parallel Random Numbers: As Easy as 1, 2, 3" (SC 2011, Threefry/Philox) — placed here for its full adoption-era context
47. Nir & Langley, "ChaCha20 and Poly1305 for IETF Protocols" (RFC 7539, 2015; later RFC 8439)
48. NIST, formal withdrawal of Dual_EC_DRBG from SP 800-90A (2014)
49. Fu, Wang, Guo, Sun, Hu, "MILP-Based Automatic Search Algorithms for Differential and Linear Trails for Speck" (FSE 2016) — read here as the direct extension of #34/#54's methodology to ARX specifically
50. Sun, Hu, Wang, Wang, Qiao, Ma, Song, "Automatic Security Evaluation and (Related-key) Differential Characteristic Search..." (ASIACRYPT 2014)

### Era 4: 2016–2020 (`04`) — 22 entries

51. NIST, Post-Quantum Cryptography Standardization call for proposals (2016)
52. Ashur & Liu, "Rotational Cryptanalysis in the Presence of Constants" (FSE 2016, RX-cryptanalysis founding paper)
53. Regev, "On Lattices, Learning with Errors, Random Linear Codes, and Cryptography" (STOC 2005 / J. ACM 2009) — read here, positioned against the PQC process it underlies
54. Stevens, Bursztein, Karpman, Albertini, Markov, "The First Collision for Full SHA-1" ("SHAttered," CRYPTO 2017)
55. Rescorla et al., RFC 8446 (TLS 1.3, 2018)
56. Bellare & Namprempre generic-composition consequences as reflected in TLS 1.3's ciphersuite removals (2018 design documentation)
57. ISO/IEC 29192-2:2019 standardization record and public discussion of the Speck/Simon rejection (2018)
58. NIST, Lightweight Cryptography competition call (2018)
59. Gohr, "Improving Attacks on Round-Reduced Speck32/64 Using Deep Learning" (CRYPTO 2019)
60. Beierle, Biryukov, Cardoso dos Santos, Großschädl, Perrin, Udovenko, Velichkov, Wang, "Lightweight AEAD and Hashing using the Sparkle Permutation Family" (ToSC 2020)
61. Beierle, Biryukov, et al. (same authorship group), "Alzette: a 64-Bit ARX-box" (CRYPTO 2020)
62. Beierle, Leander, Todo, "Improved Differential-Linear Attacks with Applications to ARX: Application to SPECK, ChaCha, and Others" (CRYPTO 2020)
63. Daemen, Hoffert, Peeters, Van Assche, Van Keer, Xoodoo/Xoodyak submission documentation (2019–2020)
64. Dobraunig, Eichlseder, Mendel, Schläffer, Ascon v1.2 NIST LWC submission (2019)
65. Wu & Huang, TinyJambu NIST LWC submission (2019)
66. Castryck, Lange, Martindale, Panny, Renes, "CSIDH: An Efficient Post-Quantum Commutative Group Action" (ASIACRYPT 2018) — isogeny-based background essential to understanding both SIKE's promise and its 2022 fall
67. O'Connor, Aumasson, Neves, Winnerlein, "BLAKE3" design documentation (2020)
68. Beullens, Rainbow's early cryptanalytic scrutiny papers (background to 2022's break, this period's groundwork)
69. Signal, X3DH and Double Ratchet specification documents (signal.org, ongoing maintenance through this period, primary-source read)
70. Maurer, Renner, Holenstein, "Indifferentiability, Impossibility Results on Reductions..." (TCC 2004) — read here against its Sparkle/Xoodyak-era applications
71. Courtois & Pieprzyk, "Cryptanalysis of Block Ciphers with Overdefined Systems of Equations" (XSL attack, ASIACRYPT 2002) — read here as this history's dead-end case study, `07` §1.1
72. Independent verification/rebuttal literature on the XSL attack's actual complexity (various, 2003–2005-era, read alongside #71)

### Era 5: 2021–2026 (`05`) — 28 entries

73. Benamira, Gerault, Peyrin, Tan, "A Deeper Look at Machine Learning-Based Cryptanalysis" (EUROCRYPT 2021)
74. Gohr, "An Assessment of Differential-Neural Distinguishers" / "aaaa-blinding" work (ePrint 2022)
75. Castryck & Decru, "An Efficient Key Recovery Attack on SIDH" (EUROCRYPT 2022)
76. Beullens, "Breaking Rainbow Takes a Weekend on a Laptop" (CRYPTO 2022)
77. NIST, PQC round 3/4 selection announcement — Kyber, Dilithium, Falcon, SPHINCS+ (2022)
78. NIST, Lightweight Cryptography competition — Ascon selected as winner, announcement and rationale (2022)
79. Bellini, Gerault, Grados, Peyrin, et al., "CLAASP: a cryptographic library for the automated analysis of symmetric primitives" (ePrint 2023/622)
80. Coutinho & Peyrin, "Boosting differential-linear cryptanalysis of ChaCha7 with MILP" (ToSC 2023)
81. Coutinho, Passos, Grados Vásquez, de Mendonça, de Sousa Jr, Borges, "Latin Dances Reloaded" + Forró proposal (ASIACRYPT 2022)
82. Bellini et al., ToSC 2023 5-round ChaCha differential-linear distinguisher paper
83. Signal, PQXDH specification and deployment documentation (2023)
84. Bao, Zhu, et al., "More Insight on Deep Learning-Aided Cryptanalysis" (ASIACRYPT/AICrypt 2023)
85. Apple, PQ3 design documentation ("iMessage with PQ3," 2024)
86. NIST, FIPS 203, 204, 205 (2024)
87. Xu, Xu, Tan, Qi, "Differential-Linear Cryptanalysis of Reduced Round ChaCha" (ToSC 2024)
88. Chrome/Firefox/BoringSSL/AWS-LC hybrid PQC deployment documentation (2024, primary vendor engineering-blog sources)
89. NIST, HQC selection announcement and rationale (2025)
90. NIST, SP 800-232 (Ascon, 2025)
91. Signal, Triple Ratchet / SPQR design and formal-security-proof papers (Eurocrypt 2025, USENIX Security 2025)
92. Bellini, Gerault, Grados, Peyrin, "The Window Heuristic: Automating Differential Trail Search in ARX Ciphers with Partial Linearization Trade-offs" (CT-RSA 2025)
93. Automated RX-differential search framework paper, Speck 17/24-round results (Designs, Codes and Cryptography, 2025)
94. Chen, Zhu, Xiang, Xu, Zeng, Zhang, "Rotational-XOR Differential Rectangle Cryptanalysis on Simon-like Ciphers" (CT-RSA 2023) — read here for its full 2025-tooling-era context
95. Flórez-Gutiérrez & Todo, "Improved Cryptanalysis of ChaCha: Beating PNBs with Bit Puncturing" (EUROCRYPT 2025)
96. Deep-learning-plus-RX-cryptanalysis convergence paper on Simon32/64 and Simeck32/64 (arXiv 2025)
97. Quantum-hardware resource-estimate revision literature, 2025–2026 (multiple sources, read as a cluster rather than a single paper — the compressing-estimate story `05` traces)
98. Google, 2029 internal PQC-migration-target announcement and stated rationale (2026)
99. Anthropic, public disclosure of the Claude Mythos Preview HAWK attack and reduced-round-AES speedup (July 28, 2026) — read directly, not via secondary coverage
100. NIST pqc-forum public discussion thread on HAWK's withdrawal (July 2026) — the live, primary-source record of the standards process actually responding in real time

---

## Paper Dependency Graph (Text Form)

A full graph-rendering of 100 papers' interdependencies would be illegible as a diagram; what follows are the field's genuine major lineages — the chains where one paper's *existence* is a direct precondition for the next, traced as explicit "led to" arrows. Read each chain top to bottom as cause-and-effect, not mere chronological adjacency.

```
DIFFERENTIAL/LINEAR CRYPTANALYSIS → AUTOMATION → ARX-SPECIFIC AUTOMATION → CHACHA'S CREEPING BREAK
Biham-Shamir (differential, 1990/91)
  → Matsui (linear, 1993)
    → Lai-Massey-Murphy (Markov cipher assumption, 1991, underlies both)
      → Mouha-Wang-Gu-Preneel (MILP for active S-boxes, 2011)
        → Sun et al. (MILP for ARX addition, 2014)
          → Fu-Wang-Guo-Sun-Hu (MILP for Speck specifically, 2016)
            → Bellini-Gerault-Grados-Peyrin (Window Heuristic, 2025)
        → Aumasson-Fischer-Khazaei-Meier-Rechberger (PNBs, 2008)
          → Beierle-Leander-Todo (differential-linear + PNB for ChaCha, 2020)
            → Coutinho-Peyrin (MILP-boosted differential-linear, 2023)
              → Xu-Xu-Tan-Qi (7.5-round ChaCha, 2024)
                → Flórez-Gutiérrez-Todo (bit puncturing, 2025)

HASH FUNCTION STRUCTURE → SPONGE CONSTRUCTION → LIGHTWEIGHT CRYPTOGRAPHY
Merkle-Damgård structural unease (2002-2003, informal)
  → Wang et al. (MD5/SHA-1 breaks, 2004-2005)
    → NIST SHA-3 call (2007)
      → Bertoni-Daemen-Peeters-Van Assche (sponge + indifferentiability, 2008)
        → Keccak selected as SHA-3 (2012)
          → Daemen et al. Xoodoo/Xoodyak (2019-2020)
          → Dobraunig et al. Ascon (2019) → NIST LWC winner (2022) → SP 800-232 (2025)
        → Beierle et al. Sparkle/Alzette long-trail strategy (2020, ARX analogue of wide-trail via Daemen's own earlier AES work)

QUANTUM ALGORITHMS → LATTICE HARDNESS → PQC STANDARDIZATION → DEPLOYMENT
Shor (1994)
  → field's slow-building urgency assessment
    → Regev, LWE + worst-case-to-average-case reduction (2005)
      → NIST PQC call (2016)
        → Kyber/Dilithium/Falcon/SPHINCS+ selected (2022)
          → FIPS 203/204/205 (2024)
            → Chrome/Firefox/BoringSSL hybrid TLS deployment (2024-2026)
            → Signal PQXDH (2023, pre-standardization) → Triple Ratchet/SPQR (2025)
            → Apple PQ3 (2024)
        → SIDH/SIKE parallel track: Castryck-Lange et al. CSIDH (2018) → SIKE reaches round 4 → Castryck-Decru break (2022)
        → HQC selected as structurally-independent backup (2025)

PROVENANCE TRUST → CURVE CHOICE → STANDARDIZATION POLITICS
Bernstein, Curve25519 (2006, motivated by speed/safety, not yet provenance)
  Shumow-Ferguson, Dual_EC backdoor demonstration (2007)
    → Snowden disclosures + RSA/BSAFE reporting (2013)
      → NIST withdraws Dual_EC_DRBG (2014)
      → Curve25519/Ed25519 adoption accelerates (2013-2016), now explicitly FOR its provenance transparency
      → ISO rejects Speck/Simon (2018), provenance suspicion outweighing clean cryptanalytic record
      → NIST PQC process run with explicit, heightened open-competition rigor (2016 onward), directly citing this trust environment

CRYPTANALYSIS AUTOMATION, PART 2: NEURAL AND BEYOND
Gohr, differential-neural distinguisher (CRYPTO 2019)
  → Benamira-Gerault-Peyrin-Tan, interpretability study (2021)
    → Gohr, aaaa-blinding self-critique (2022)
      → Bao-Zhu et al., related-key extensions (2023)
        → DbitNet, architecture-agnostic distinguishers (2024)
          → deep-learning + RX-cryptanalysis convergence (arXiv 2025)
  [separate, converging thread:]
  Ashur-Liu, RX-cryptanalysis founding (2016)
    → automated RX-differential search frameworks (2025)
      → Chen et al., RX + rectangle combination (2023)
        → deep-learning + RX convergence (arXiv 2025) [same node as above — the two threads meet here]
  [both threads feed into, without directly causing:]
  Anthropic, Claude Mythos Preview HAWK attack (July 2026) — a different technique (autonomous mathematical research, not a trained distinguisher) applied to a different target (HAWK's lattice structure, not Speck/Simon/ChaCha), but positioned at the end of the same automation-of-cryptanalysis lineage this whole graph traces
```

---

## Top 50 Historical Milestones

A condensed, date-first list — events, not papers specifically, though many map directly to entries above — for anyone who wants the skeleton of this history without the full paper-level texture.

1. 1993 — Matsui's linear cryptanalysis breaks full DES
2. 1993 — Bellare-Rogaway formalize the random oracle model
3. 1994 — Shor's algorithm
4. 1995 — SHA-1 standardized
5. 1996 — Grover's algorithm
6. 1996 — Kocher's timing-attack paper opens side-channel cryptanalysis
7. 1997 — AES competition opens
8. 1998 — DES falls to brute force (Deep Crack)
9. 1999 — Differential Power Analysis published
10. 2000 — Rijndael selected as AES
11. 2001 — FIPS 197 (AES) finalized
12. 2004 — Practical MD5 collisions published
13. 2004 — Random oracle model's theoretical insufficiency proven
14. 2005 — Theoretical SHA-1 collision attack published
15. 2005 — Salsa20 published
16. 2005 — Regev introduces Learning With Errors
17. 2006 — Curve25519 published
18. 2006 — Dual_EC_DRBG standardized
19. 2007 — SHA-3 competition opens
20. 2007 — Dual_EC_DRBG backdoor potential publicly demonstrated
21. 2008 — ChaCha published
22. 2008 — Sponge construction and its indifferentiability proof published
23. 2009 — Gentry's first FHE construction
24. 2010 — AES-NI ships
25. 2011 — MILP first applied to differential cryptanalysis
26. 2012 — Keccak selected as SHA-3
27. 2013 — Snowden disclosures; Dual_EC_DRBG backdoor substantiated
28. 2013 — NSA publishes Speck and Simon
29. 2014 — Heartbleed disclosed
30. 2014 — CAESAR competition opens; TLS 1.3 drafting begins
31. 2015 — FIPS 202 (SHA-3) finalized
32. 2016 — NIST opens PQC standardization
33. 2016 — Rotational-XOR cryptanalysis founded
34. 2017 — SHA-1 practically broken (SHAttered)
35. 2018 — TLS 1.3 finalized (RFC 8446)
36. 2018 — ISO rejects Speck/Simon for standardization
37. 2018 — NIST Lightweight Cryptography competition opens
38. 2019 — Gohr publishes the first differential-neural cryptanalysis result
39. 2020 — Sparkle/Alzette introduces the long-trail strategy
40. 2022 — SIKE catastrophically broken
41. 2022 — Rainbow broken
42. 2022 — NIST selects Kyber, Dilithium, Falcon, SPHINCS+
43. 2022 — NIST selects Ascon for Lightweight Cryptography
44. 2023 — Signal deploys PQXDH
45. 2023 — CLAASP unifies automated ARX/symmetric-primitive analysis tooling
46. 2024 — FIPS 203/204/205 finalized; Apple ships PQ3
47. 2025 — NIST selects HQC; finalizes Ascon as SP 800-232
48. 2025 — Signal deploys the Triple Ratchet/SPQR
49. 2025 — The Window Heuristic advances MILP-based ARX addition modeling
50. July 2026 — Anthropic's Claude Mythos Preview finds a real attack against HAWK; HAWK withdrawn from NIST consideration

---

## Future Research Directions

Synthesizing this entire history's trajectory rather than repeating this report's companion documents' own detailed open-problems lists (which remain the more actionable, technically-scoped reference for a researcher — see the general-cryptography report's Part `06` §8 and the ARX-specific report's Part `09` §1): this history's own throughlines suggest three genuinely open, historically-motivated questions worth naming as this report's closing word. **First**: does the automation-of-cryptanalysis lineage this report has traced continuously from 2011's first MILP application through 2019's differential-neural distinguishers to July 2026's substantially-autonomous AI research effort represent a smoothly continuing trend, or did something qualitatively different happen in 2026 specifically — and if the latter, what does the field's next competition-standardization cycle (whatever it turns out to be) need to do differently in response, given this history's repeated lesson that institutional process, not just mathematics, is what ultimately earns trust. **Second**: will the provenance-trust dynamic this history has traced from Dual_EC_DRBG (2006) through Snowden (2013) through Speck/Simon's rejection (2018) extend to AI-assisted design and cryptanalysis specifically — does a construction found or validated with substantial AI involvement need its own, new category of provenance scrutiny, analogous to how NSA authorship became its own scrutiny category after 2013, or does the field's existing open-competition, open-cryptanalysis machinery already handle this adequately because the *mathematics itself* is what ultimately gets checked, regardless of what process produced the initial insight. **Third, and most directly practical**: this history's pattern of theoretical-warning-then-practical-confirmation (SHA-1's 2005-to-2017 arc being the clearest instance) suggests the field's current PQC-migration urgency debate — genuinely, honestly unresolved as this report is written, per its companion document's treatment — will very likely resolve the same way: not through a single dramatic proof either way, but through accumulating evidence (further resource-estimate revisions, further hardware progress, or the absence of either) that gradually shifts expert consensus one direction or the other, the way it always has in this history, rather than through the kind of sudden rupture a less historically-grounded reading of the current moment might expect.

---

This closes the nine-file intellectual history of modern cryptography, 1993–2026. Return to `00-INDEX-executive-summary.md` for the full map and the Evolution Map table if you're using this report as a reference rather than reading it start to finish.
