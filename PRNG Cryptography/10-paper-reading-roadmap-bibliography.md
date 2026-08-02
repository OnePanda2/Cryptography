# 10 — Paper Reading Roadmap & Bibliography

*Final file of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`09`.*

A note on **citation counts**: this report does not fabricate specific citation-count figures for papers not individually re-verified via search — doing so would risk presenting invented numbers as fact. Instead, each entry's **relative field impact** is indicated qualitatively (foundational / heavily cited / actively cited / emerging), which is the honest, defensible version of what your outline's citation-count request is actually trying to convey. Where you need an exact current citation count for a specific paper, Google Scholar or Semantic Scholar will give you a live, accurate figure faster and more reliably than this report can.

**Reading time** estimates assume the graduate-level, math-comfortable, programming-comfortable audience this whole report targets (per this conversation's established audience calibration) — someone already through Stage 2–3 of a roadmap like this one's, not a first-time reader of any cryptography paper.

**Status** flags: 🟢 remains state-of-the-art / directly relevant; 🟡 foundational but superseded in its specific numeric results by later work (still essential reading for the ideas); 🔵 historical/context, not a current technical reference point.

---

## A. Foundational (10)

| Paper | Why it matters | Difficulty | Prerequisites | Time | Status |
|---|---|---|---|---|---|
| Shannon, "Communication Theory of Secrecy Systems" (1949) | Confusion/diffusion — the conceptual root of every ARX design decision in `01`–`02` | Moderate | Basic probability | 2–3h | 🔵 essential context |
| Rivest, "The RC5 Encryption Algorithm" (1994) | First deliberately-ARX-style (add/rotate/XOR) block cipher; the direct conceptual ancestor of the whole paradigm | Low | None | 1h | 🔵 historical, ARX origin point |
| Bernstein, "Salsa20 specification" (2005, eSTREAM submission docs) | The paradigm's modern founding design; read as primary source, not summary | Low-moderate | `01` | 1–2h | 🟡 superseded by ChaCha in practice, essential reading |
| Bernstein, "ChaCha, a variant of Salsa20" (2008) | The field's most-referenced ARX design; every later case study compares against it | Low-moderate | Salsa20 paper | 1h | 🟢 |
| Lipmaa & Moriai, "Efficient Algorithms for Computing Differential Properties of Addition" (FSE 2001) | THE foundational result — exact linear-time differential probability of modular addition; everything in `04`–`05` builds on this | High | Boolean algebra, basic automata reasoning | 3–4h, re-read | 🟢 the single most load-bearing paper in this entire report |
| Wallén, "Linear Approximations of Addition Modulo 2^n" (FSE 2003) | The linear-cryptanalysis analogue of Lipmaa–Moriai | High | Lipmaa–Moriai | 3h | 🟢 |
| Lai, Massey, Murphy, "Markov Ciphers and Differential Cryptanalysis" (EUROCRYPT 1991) | The Markov-cipher assumption underlying every trail-probability computation in `03` §1.5, `05` | Moderate-high | Basic differential cryptanalysis | 2h | 🟢 assumption still in active use, worth understanding its limits |
| Daemen & Rijmen, wide-trail strategy (AES design documentation / "The Design of Rijndael") | The direct ancestor of the ARX-adapted long-trail strategy in `03` §1.4 | Moderate-high | `01`, general-cryptography-report Part `02` | Several sessions | 🟢 |
| Bellare & Rogaway, random oracle model papers (1993 onward) | Underlies the ideal-permutation-model formal-security framework in `05` §3 | Moderate | Basic provable security | 2h | 🟢 |
| Aumasson, Fischer, Khazaei, Meier, Rechberger, "New Features of Latin Dances" / PNB introduction (FSE 2008) | Introduces Probabilistic Neutral Bits — the technique underlying essentially every subsequent ChaCha/Salsa20 attack in `04` §2.3 | Moderate-high | Differential-linear basics | 2–3h | 🟢 foundational technique, still the base every 2023–2025 paper builds on |

## B. Design — Case Studies (15)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Aumasson, Henzen, Meier, Phan, "SHA-3 proposal BLAKE" (2010) | BLAKE's original design, direct ChaCha-G-function ancestry | Moderate | 2h | 🔵 |
| Aumasson, Neves, Wilcox-O'Hearn, Winnerlein, "BLAKE2" (2013) | `02` §2.3 | Moderate | 1–2h | 🟢 |
| O'Connor, Aumasson, Neves, Winnerlein, "BLAKE3" (2020) | `02` §2.3 — the Merkle-tree parallelism redesign | Moderate | 2h | 🟢 |
| Aumasson & Bernstein, "SipHash: a fast short-input PRF" (2012) | `02` §2.4 | Low-moderate | 1h | 🟢 |
| Salmon, Moraes, Dror, Shaw, "Parallel Random Numbers: As Easy as 1, 2, 3" (Random123/Threefry/Philox, SC 2011) | `02` §2.5, `06` §1.1 | Moderate | 2h | 🟢 |
| Daemen, Hoffert, Peeters, Van Assche, Van Keer, "Xoodoo cookbook" / Xoodyak submission (2018–2019) | `02` §2.6 | Moderate-high | 2–3h | 🟢 |
| Dobraunig, Eichlseder, Mendel, Schläffer, "Ascon v1.2" submission + NIST SP 800-232 (2025) | `02` §2.7 — required reading given Ascon's standardization | Moderate | 2h + spec reference use | 🟢 the finalized standard |
| Aumasson, Jovanovic, Neves, "NORX" (2014) + designers' 2019 withdrawal statement | `02` §2.8 | Moderate | 1–2h | 🔵 withdrawn but instructive |
| Beaulieu, Shors, Smith, Treatman-Clark, Weeks, Wingers, "The SIMON and SPECK Families of Lightweight Block Ciphers" (NSA, 2013) | `02` §2.9 — read as primary source given how much subsequent literature targets these designs | Low-moderate | 1–2h | 🟢 the single most-cited benchmark-target paper in the whole automated-search literature |
| Hong, Lee, Kim, Kwon et al., "LEA: A 128-Bit Block Cipher for Fast Encryption on Common Processors" (2013) | `02` §2.10 | Moderate | 1h | 🟢 |
| Wu & Huang, "TinyJambu" NIST LWC submission | `02` §2.11 | Low-moderate | 1h | 🔵 not selected, still instructive |
| Beierle, Biryukov, Cardoso dos Santos, Großschädl, Perrin, Udovenko, Velichkov, Wang, "Lightweight AEAD and Hashing using the Sparkle Permutation Family" (ToSC 2020) / "Alzette: a 64-Bit ARX-box" (CRYPTO 2020) | `02` §2.12 — this report's central provable-margin design case study; required reading | High | `03` §1.4 helps first | Several sessions | 🟢 |
| Beaulieu et al., ISO/IEC 29192-2:2019 rejection discussion (public record/mailing-list, not a single paper but worth tracing) | `02` §2.9's process-vs-mathematics lesson | Low | 30min–1h | 🔵 |
| Bernstein, "The Poly1305-AES message-authentication code" (2005) | Companion to Salsa/ChaCha, general-cryptography-report Part `02` §5.1 cross-reference | Low-moderate | 1h | 🟢 |
| Nir & Langley, "ChaCha20 and Poly1305 for IETF Protocols" (RFC 8439) | The standardized, deployed specification — required for any implementation work | Low | 1h, reference use | 🟢 |

## C. Classical Cryptanalysis of ARX (12)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Choudhuri & Maitra, "Significantly Improved Multi-bit Differentials for Reduced Round Salsa and ChaCha" (ToSC 2016) | Long-standing prior-best benchmark for ChaCha attacks | High | 2–3h | 🟡 superseded, worth reading for the technique lineage |
| Dey & Sarkar, "Improved analysis for reduced round Salsa and Chacha" (2017) | Direct ancestor of later PNB refinements | High | 2h | 🟡 |
| Coutinho & Souza Neto, "New multi-bit differentials to improve attacks against ChaCha" (ePrint 2020/350) | Bridges to the 2022–2024 line | High | 2h | 🟡 |
| Coutinho, Passos, Grados Vásquez, de Mendonça, de Sousa Jr, Borges, "Latin Dances Reloaded" (ASIACRYPT 2022) + Forró proposal | `04` §2.3 — direct PNB-selection-algorithm improvement, plus a new design response | High | 3h | 🟢 |
| Beierle, Leander, Todo, "Improved Differential-Linear Attacks with Applications to ARX Ciphers" (CRYPTO 2020) | `04` §2.3 — the 3.5-round differential | High | 2–3h | 🟢 |
| Bellini et al., ToSC 2023 5-round differential-linear distinguisher paper | `04` §2.3 | High | 2–3h | 🟢 |
| Coutinho & Peyrin, "Boosting differential-linear cryptanalysis of ChaCha7 with MILP" (ToSC 2023) | `04` §2.3, `05` §1.6 — first MILP-automated differential-linear distinguisher optimization | High | 3h | 🟢 |
| Dey, Garai, Maitra, "Cryptanalysis of Reduced Round ChaCha — New Attack & Deeper Analysis" (ePrint 2023) | `04` §2.3 | High | 2–3h | 🟢 |
| Xu, Xu, Tan, Qi, "Differential-Linear Cryptanalysis of Reduced Round ChaCha" (ToSC 2024) | `04` §2.3 — current published state of the art (7.5 rounds) at time of this report | High | 3h | 🟢 the number to beat |
| Flórez-Gutiérrez & Todo, "Improved Cryptanalysis of ChaCha: Beating PNBs with Bit Puncturing" (EUROCRYPT 2025) | `04` §2.3 — most recent published advance in this line | Very high | 3–4h | 🟢 |
| Ashur & Liu, "Rotational Cryptanalysis in the Presence of Constants" / RX-cryptanalysis founding paper (FSE 2016) | `04` §2.4 — founds the entire RX-cryptanalysis line | High | 2–3h | 🟢 |
| Khovratovich & Nikolić, "Rotational Cryptanalysis of ARX" (FSE 2010) | Pre-RX rotational cryptanalysis foundations, direct RX ancestor | High | 2–3h | 🟢 |

## D. Rotational-XOR (RX) Cryptanalysis, Current (8)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Chen, Zhu, Xiang, Xu, Zeng, Zhang, "Rotational-XOR Differential Rectangle Cryptanalysis on Simon-like Ciphers" (CT-RSA 2023) | `04` §2.4 — first RX+rectangle combination | High | 2–3h | 🟢 |
| (Authors per this report's search record), "A new automatic framework for searching rotational-XOR differential characteristics in ARX ciphers" (Designs, Codes and Cryptography, 2025) | `04` §2.4, `05` §2.5 — the 17/24-round Speck RX results | Very high | 3–4h | 🟢 current state of the art for automated RX search |
| "On the Probability and Automatic Search of Rotational-XOR Cryptanalysis on ARX Ciphers" (The Computer Journal, 2022) | `04` §2.4 — RX-offset formalization | High | 2–3h | 🟢 |
| "Rotational Differential-Linear Cryptanalysis Revisited" | `04` §2.4 — RX + differential-linear combination | Very high | 3h | 🟢 |
| (2025 arXiv) "Enhancing Deep Learning-Based Rotational-XOR Attacks on Lightweight Block Ciphers Simon32/64 and Simeck32/64" | `04` §2.4, §2.10 — the RX+neural convergence | High | 2–3h | 🟢 most current combined-technique paper referenced in this report |
| Lu, Liu, et al., "Improved (Related-Key) Differential-Based Neural Distinguishers for SIMON and SIMECK Block Ciphers" (Computer Journal, 2024) | Bridges RX-family target ciphers to neural methodology | High | 2h | 🟢 |
| Sadeghi, Rijmen, Bagheri, "Proposing an MILP-based method for the experimental verification of difference-based trails: application to SPECK, SIMECK" (2021) | RX-trail experimental verification, catching incompatible published trails | High | 2h | 🟢 |
| Wang, Feng, Bin, Guan, Shi, Zhang, "New method for combining Matsui's bounding conditions with sequential encoding method" (2023) | Search-efficiency technique referenced across the RX/MILP literature | High | 2h | 🟢 |

## E. Differential-Neural / AI-Assisted Cryptanalysis (10)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Gohr, "Improving Attacks on Round-Reduced Speck32/64 Using Deep Learning" (CRYPTO 2019) | `04` §2.10 — founding paper of the entire technique | High | 3h | 🟢 required reading |
| Gohr, "An Assessment of Differential-Neural Distinguishers" (ePrint 2022) + "Real Differences Experiment"/"aaaa-blinding" work | `04` §2.10 — the crucial "what is it actually learning" methodology paper | High | 2–3h | 🟢 essential for the open-problem #8 question |
| Benamira, Gerault, Peyrin, Tan, "A Deeper Look at Machine Learning-Based Cryptanalysis" (EUROCRYPT 2021) | `04` §2.10 | High | 2h | 🟢 |
| Bao, Zhu, et al., "More Insight on Deep Learning-Aided Cryptanalysis" (ASIACRYPT/AICrypt 2023) | `04` §2.10 — related-key setting, 14-round Speck32/64 | High | 2–3h | 🟢 |
| Bellini et al., "DbitNet" (ePrint/FSE 2024) | `04` §2.10, `05` §1.6 — architecture-agnostic distinguisher, also introduced the generic auto-training tool referenced in section F | High | 2–3h | 🟢 |
| Hou, Ren, Chen, "Practical attacks of round-reduced Simon based on deep learning" (Computer Journal, 2023) | `04` §2.10 | Moderate-high | 2h | 🟢 |
| Wang & Wang, "A New (Related-Key) Neural Distinguisher Using Two Differences for Differential Cryptanalysis" (IET Info Security, 2024) | `04` §2.10 | Moderate-high | 2h | 🟢 |
| Hou, Liu, Han, Ma, Ye, Nie, "Improving deep learning-based neural distinguisher with multiple ciphertext pairs for Speck and Simon" (Sci Reports, 2025) | `04` §2.10 — most recent architecture-improvement paper referenced | Moderate-high | 2h | 🟢 |
| Huang, Li, Fu, Chen, Song, "Improving Differential-Neural Cryptanalysis for Large-State SPECK" (ICICS 2024) | Extends the technique beyond toy-scale Speck32/64 | High | 2–3h | 🟢 |
| (This conversation's companion general-cryptography report, `03` §2.6) Anthropic's July 2026 HAWK/reduced-round-AES disclosure | Not an ARX-specific paper, but the essential current context for `09` §3.3's discussion — read the primary Anthropic disclosure directly, not secondary coverage | Moderate (as reported) | 1h | 🟢 days-old at report-writing time; track for updates |

## F. MILP for ARX (8)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Mouha, Wang, Gu, Preneel, "Differential and Linear Cryptanalysis Using Mixed-Integer Linear Programming" (2011) | `05` §1.1 — founding MILP-for-cryptanalysis paper | High | 2–3h | 🟢 |
| Sun, Hu, Wang, Wang, Qiao, Ma, Song, "Automatic Security Evaluation and (Related-key) Differential Characteristic Search: Application to SIMON, PRESENT, LBlock, DES(L) and Other Bit-oriented Block Ciphers" (ASIACRYPT 2014) | `05` §1.1, §1.6 — the full ARX/bit-oriented MILP modeling foundation | Very high | 4h+ | 🟢 required reading, dense |
| Fu, Wang, Guo, Sun, Hu, "MILP-Based Automatic Search Algorithms for Differential and Linear Trails for Speck" (FSE 2016) | `05` §1.6 — Speck-specific MILP application, standard reference | High | 2–3h | 🟢 |
| Liu et al. (Bel-T-focused refinement paper, ~2019) | `05` §1.6, §1.7 — carry-bit-level refinement beyond the independent-assumption baseline | High | 2h | 🟢 |
| Zhang, Sun, Cai, Hu, "Speeding up MILP aided differential characteristic search with Matsui's strategy" (2018) | Search-efficiency technique | High | 2h | 🟢 |
| Sadeghi, Rijmen, Bagheri (see D above, cross-listed) | MILP verification of RX trails | High | 2h | 🟢 |
| Bellini, Gerault, Grados, Peyrin, "The Window Heuristic: Automating Differential Trail Search in ARX Ciphers with Partial Linearization Trade-offs" (CT-RSA 2025 / ePrint 2024/1743) | `05` §1.4, §1.6 — the current state-of-the-art addition-modeling technique | Very high | 3–4h | 🟢 most current MILP-modeling paper |
| Bellini et al., "CLAASP: a cryptographic library for the automated analysis of symmetric primitives" (ePrint 2023/622) | `05` §1.6, `09` §2.2 — the field's current unified tooling framework, most directly actionable for a new-design researcher | High (as a tool to learn, not just read) | Several sessions of hands-on use | 🟢 |

## G. SAT/SMT for ARX (6)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Mironov & Zhang, "Applications of SAT Solvers to Cryptanalysis of Hash Functions" (2006) | Early foundational SAT-for-cryptanalysis paper | Moderate-high | 2h | 🔵 historical, still instructive |
| Soos, Nohl, Castelluccia, "Extending SAT Solvers to Cryptographic Problems" (SAT 2009) | CryptoMiniSat's origin paper | High | 2–3h | 🟢 |
| Kölbl, Leander, Tiessen, "Observations on the SIMON Block Cipher Family" (CRYPTO 2015) | Includes SAT-based analysis, standard SIMON reference | High | 2–3h | 🟢 |
| Sun, Huang, Yang (per `05` §2.1 search record), MILP/MIQCP fully-automatic differential-linear distinguisher search for SIMON-like ciphers (CT-RSA 2023, IET Info Security 2024 follow-up) | `05` §2.1 | High | 2–3h | 🟢 |
| Song, Guo, "Cube-Attack-Like Cryptanalysis of Round-Reduced Keccak Using MILP" (ToSC 2018) | Sponge-permutation-specific MILP/cube-attack combination, relevant to Xoodoo/Sparkle-adjacent evaluation | High | 2–3h | 🟢 |
| CryptoSMT documentation and associated papers (Kölbl) | `05` §1.6, §2.3, `09` §2.3 — the practical SMT tool | Moderate (tool docs, not a paper) | Several hours hands-on | 🟢 |

## H. Randomness and PRNG Design (8)

Cross-reference this conversation's general-cryptography report Part `06` §3 for the general (non-ARX) DRBG/randomness bibliography (NIST SP 800-90A/B, Fortuna/Yarrow papers); this list covers ARX-specific/PRNG-design-specific material only.

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Bernstein, public design notes on fast-key-erasure RNG construction | `06` §1.2 — the core current-best-practice pattern; read directly from the primary source, not secondary summaries | Moderate | 1–2h | 🟢 |
| Cheng, Vigna, "Scrambled Linear Pseudorandom Number Generators" and related xoshiro/xoroshiro family papers | Widely-used non-cryptographic (statistical-quality) ARX-adjacent PRNGs, useful contrast case for `06` §2's crypto-vs-statistical distinction | Moderate | 1–2h | 🟢 useful contrast, not itself cryptographically secure |
| Salmon et al. (Random123, see B above, cross-listed) | `06` §1.1 | Moderate | 2h | 🟢 |
| L'Ecuyer & Simard, "TestU01: A C Library for Empirical Testing of Random Number Generators" (2007) | `06` §2.1 — the BigCrush/SmallCrush reference | Moderate | 1–2h, reference use | 🟢 |
| Doty-Humphrey, PractRand documentation and associated design notes | `06` §2.1, §2.4 | Moderate (tool docs) | 1–2h | 🟢 |
| Linux kernel `random.c`/`chacha.c` design discussion (kernel mailing list, LWN coverage of the ChaCha20-RNG transition) | `06` §1.6 — the field's best real-world worked example | Moderate | 2–3h | 🟢 |
| Ferguson, Schneier, Kohno, *Cryptography Engineering* — Fortuna chapter | Cross-reference to general-cryptography-report Part `06` §4; the PRNG-design chapter specifically is directly ARX-relevant reading | Moderate | 2–3h | 🟢 |
| NIST SP 800-90B, entropy source assessment | Cross-reference general-cryptography report; required for the entropy-input side of any new PRNG proposal per `06` §1.4 | Moderate-high | Reference use | 🟢 |

## I. Diffusion & Permutation Theory (8)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Webster & Tavares, "On the Design of S-boxes" (CRYPTO 1985) | SAC/BIC founding paper — `03` §2.1–2.2 | Moderate | 1–2h | 🟢 |
| Daemen, Govaerts, Vandewalle, "Correlation Matrices" (FSE 1994) | Linear-diffusion analysis foundations | High | 2h | 🟢 |
| Beierle, Biryukov, et al., long-trail strategy paper (see B/`02` §2.12, cross-listed) | `03` §1.4 — required reading, again | High | Several sessions | 🟢 |
| Maurer, Renner, Holenstein, "Indifferentiability, Impossibility Results on Reductions, and Applications to the Random Oracle Methodology" (TCC 2004) | `05` §3.3 — the formal indifferentiability tool | Very high | 3–4h | 🟢 |
| Bertoni, Daemen, Peeters, Van Assche, "On the Indifferentiability of the Sponge Construction" (EUROCRYPT 2008) | `05` §3.2–3.3, direct sponge-specific application | Very high | 3h | 🟢 |
| Lai, "Higher Order Derivatives and Differential Cryptanalysis" (1994) | Higher-order/integral-attack foundations, `04` §2.9 | High | 2h | 🟢 |
| Daemen, "Cipher and Hash Function Design Strategies based on Linear and Differential Cryptanalysis" (PhD thesis, 1995) | The deep primary-source origin of the wide-trail strategy | Very high | Multi-session | 🟢 the wide-trail strategy's original, full statement |
| Nyberg, "Perfect Nonlinear S-Boxes" (EUROCRYPT 1991) | Nonlinearity-metric foundations, background for understanding why ARX's addition-based nonlinearity is structurally different from S-box nonlinearity | High | 2h | 🟢 |

## J. Automatic Analysis Frameworks — General (6)

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Sun, Liu, Sasaki, Qiao, "Automatic Search of Bit-Based Division Property for ARX Ciphers and Word-Based Division Property" | Integral/division-property automated search — the ARX-specific integral-attack automation counterpart to `05`'s differential/linear focus | Very high | 3h | 🟢 |
| Song, Huang, Yang, "Automatic Differential Analysis of ARX Block Ciphers with Application to SPECK and LEA" (ACISP 2016) | `05` §1.6 — cross-referenced foundational tool paper | High | 2–3h | 🟢 |
| Gérault, Lafourcade, Minier, Solnon, "Revisiting AES related-key differential attacks with constraint programming" (2018) | CP-based (not just MILP/SAT) automated search, directly relevant to `05` §2.2's CP-SAT discussion | High | 2h | 🟢 |
| Bellini, Gerault, Grados, Protopapa, Rossi, "Monte Carlo tree search for automatic differential characteristics search: application to SPECK" (INDOCRYPT 2022) | A distinct, heuristic (not exact-solver) automated-search methodology worth knowing alongside MILP/SAT | High | 2–3h | 🟢 |
| Fu, Wang, Guo, Sun, Hu (see F above, cross-listed) | — | — | — | 🟢 |
| Bagherzadeh & Ahmadian, "MILP-Based Automatic Differential Searches for LEA and HIGHT" (ePrint 2018/948) | Additional standard-benchmark-cipher application | Moderate-high | 2h | 🟢 |

## K. Side Channels & Implementation (8)

Cross-reference this conversation's general-cryptography report Part `04` §1.5 for the general side-channel bibliography; ARX-specific and directly `07`-relevant material only here.

| Paper | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| Kocher, "Timing Attacks on Implementations of Diffie-Hellman, RSA, DSS, and Other Systems" (1996) | Foundational timing-attack paper; `01` §1.2, `07` §1.1's ARX-avoids-this-class discussion | Moderate | 1–2h | 🟢 essential background even though ARX is largely immune |
| Bernstein, "Cache-timing attacks on AES" (2005) | The specific historical motivation for ARX's constant-time design choice, `01` §1.2 — read this to understand precisely what ARX is designed to avoid | High | 2h | 🟢 |
| HACL* project papers (Zinzindohoué, Bhargavan, Protzenko, Beurdouche, "HACL*: A Verified Modern Cryptographic Library," CCS 2017) | Cross-reference general-cryptography-report Part `05` §2.4 — formally-verified constant-time ARX implementation, direct `07` §1.1/§1.6 relevance | High | 2–3h | 🟢 |
| Fog, "Instruction tables" and "The microarchitecture of Intel, AMD, and VIA CPUs" (ongoing technical reference, not a paper) | The standard practical reference for `07` §1.2's instruction-latency/throughput claims | Reference | Ongoing reference use | 🟢 |
| Intel/ARM SIMD intrinsics guides (official vendor documentation) | Required practical reference for `07` §1.4 hand-vectorization work | Reference | Ongoing reference use | 🟢 |
| Bernstein & Schwabe, "NEON crypto" (CHES 2012) | ARM NEON-specific ARX implementation techniques | High | 2h | 🟢 |
| CHES proceedings generally, lightweight-cipher implementation comparison papers (per `02`'s NIST LWC finalist comparisons) | `07` §3.2's energy/footprint benchmarking methodology reference class | Varies | Varies | 🟢 |
| Trail of Bits / NCC Group public cryptographic-implementation-audit reports (various, ongoing) | Real-world, practitioner-grade implementation-security case studies, directly complementing the academic literature with audit-practice perspective your outline specifically requested | Moderate | 1h each | 🟢 |

## L. Benchmarking (4)

| Reference | Why it matters | Difficulty | Time | Status |
|---|---|---|---|---|
| SUPERCOP / eBASH benchmarking framework documentation | `09` §2.4, `07` §3.4 — the standard cross-implementation benchmark harness | Reference | Hands-on setup time | 🟢 |
| eSTREAM final portfolio report | Historical but methodologically instructive competition-scale performance comparison | Moderate | 1–2h | 🔵 historical, methodology still relevant |
| NIST Lightweight Cryptography finalist-round comparison reports (IR 8454 and associated NIST presentation materials) | `02`, `07` §3.2 — the most current, most rigorous head-to-head ARX-vs-SPN-vs-AND-RX-vs-NLFSR performance/footprint comparison available | Moderate | 2–3h | 🟢 |
| Individual design papers' own benchmark sections (Sparkle, Xoodyak, Ascon, ChaCha per RFC 8439 appendices) | Primary-source performance claims to compare against independent measurement | Low-moderate | 1h each | 🟢 |

## M. Recent Breakthroughs, 2022–2026 (9)

A consolidated "if you only read the newest material" list, cross-referencing entries already detailed above by category:

1. Coutinho, Passos, Grados Vásquez, de Mendonça, de Sousa Jr, Borges — ASIACRYPT 2022 (§C)
2. Chen, Zhu, Xiang, Xu, Zeng, Zhang — CT-RSA 2023 RX-rectangle (§D)
3. Bellini, Gerault, Grados, Peyrin — CLAASP, ePrint 2023 (§F)
4. Coutinho & Peyrin — ToSC 2023 MILP-boosted differential-linear (§C)
5. Bellini et al. — DbitNet, 2024 (§E)
6. Xu, Xu, Tan, Qi — ToSC 2024, 7.5-round ChaCha (§C)
7. Automated RX-differential framework, Designs Codes and Cryptography 2025, 17/24-round Speck (§D)
8. Bellini, Gerault, Grados, Peyrin — Window Heuristic, CT-RSA 2025 (§F)
9. Flórez-Gutiérrez & Todo — EUROCRYPT 2025, bit puncturing (§C)

Plus, from this conversation's companion general-cryptography report: Anthropic's July 2026 HAWK/reduced-round-AES disclosure (§E's final entry) — not ARX-cryptanalysis specifically, but the essential current context for where AI-assisted cryptanalysis stands as this report is written.

---

## Closing note

This roadmap totals roughly **100 entries** across categories A–M, consciously weighted toward the fast-moving 2020–2025 literature this report's search record surfaced, since that material is both the least likely to already be part of a reader's existing training/background *and* the most directly load-bearing for the "responsibly design and publish a new ARX permutation in 2026" goal your original prompt specifically named. Pair this file with `08`'s explicit reviewer-expectation checklist when moving from *reading* toward *writing* — the checklist tells you which of this roadmap's technique-categories your own new design's evaluation needs to actually cover, not just be aware of.

This closes the ten-file ARX cryptography report. Return to `00-INDEX-executive-summary.md` for the full map, confidence ratings, and recommended next steps if you're starting a specific project from here.
