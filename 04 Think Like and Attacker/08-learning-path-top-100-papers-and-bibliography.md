# 08 — Learning Path, Top Attack Papers & Bibliography

*Final file of this report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`07`. Companion ARX report `10` already provides a ~100-paper roadmap heavily weighted toward attack papers; this file does not duplicate that list — it names the specific papers this report newly surfaced (TinyJambu, Rocca, Keccak's cube/zero-sum lineage) as required additions, and focuses its own contribution on the *learning path*, not another parallel paper list.*

---

## 1. How Working Cryptanalysts Actually Built Their Intuition

### 1.1 The mathematics that matters most, prioritized by this report's own findings

Restating companion ARX report `01`'s mathematical-foundations material with this report's discovery-process priorities layered on: **probability and the Lipmaa-Moriai-style exact-propagation formalism** matter most immediately and directly, because `01`–`05` of this report have shown, repeatedly, that the actual discovery moment is almost always "does this specific propagation/correlation/degree-growth claim hold," a question these formalisms answer precisely; **basic algebraic-degree and Boolean-function theory** (companion history report `06`'s nonlinearity-metric thread) comes next, directly underlying the low-algebraic-degree warning sign (`03`) that drives cube and integral attacks; **elementary information theory** (entropy, statistical distance) underlies the diffusion and randomness-testing checks (`02` Stages 4, `06`); and, notably, **less** of the deep algebraic-geometry/lattice-theory machinery than a PQC-focused researcher would need (companion ARX report `01` §2.8's lattice material is comparatively less load-bearing for the ARX/symmetric-cryptanalysis discovery process this report has focused on specifically) — worth stating explicitly since it's a genuine, useful narrowing for anyone whose stated goal, like yours, is specifically ARX permutation and PRNG design/evaluation rather than the full breadth of modern cryptography.

### 1.2 Papers considered foundational, beyond the ARX report's own list

Companion ARX report `10` (Section A, "Foundational") already covers Lipmaa-Moriai, Wallén, and the Lai-Massey-Murphy Markov-cipher paper as the load-bearing mathematical foundations; this report adds, specifically because of its focus on the *discovery process* rather than the finished technique: **Lai's original 1994 higher-order-derivative paper** (the direct root of both integral and cube attacks, and the specific technique Aumasson and Meier applied to Keccak in 2009, `04`) deserves reading as a primary source specifically to see the technique in its most general, unspecialized form before encountering its many cipher-specific applications.

### 1.3 Which conferences to follow, and why, restated with this report's emphasis

Companion ARX report `08` §3.1 covers CRYPTO/EUROCRYPT/ASIACRYPT/FSE/ToSC/CHES's respective scopes in full; the discovery-process-specific addition worth making: **ToSC and FSE specifically publish the overwhelming majority of the "someone questioned a specific assumption" narratives this report has reconstructed** (the TinyJambu, Rocca, and Keccak-keyed-mode papers are all ToSC or FSE-lineage venues, or EUROCRYPT for the highest-profile instances like the original cube-attack-like Keccak work) — for a researcher specifically trying to build the discovery-process intuition this report describes, ToSC's continuously-published issue structure (rather than an annual conference's single yearly batch) makes it the single best venue to read *serially*, in publication order, to watch the funnel in `02` play out across many different targets over time.

### 1.4 Which researchers consistently produce influential discovery-process work

Beyond companion history report `08`'s major-researcher profiles (which cover design-side contributors more than attack-side ones): the TinyJambu discovery team (Saha, Sasaki, Shi, Sibleyras, Sun, Zhang) and the Rocca-cryptanalysis lineage (Hosoyamada, Isobe, Sasaki, and collaborators; Derbez, Fouque, Rahman, Schrottenloher for the key-committing-attack extension) both newly surfaced by this session's research, are worth following directly as active, current contributors to exactly the discovery-process pattern this report has centered — Sasaki's name recurring across both lineages is itself a useful, concrete data point for `01` §4's claim that intuition compounds with repeated, direct engagement across many real targets, not from any single breakthrough. Aumasson and Meier's foundational Keccak zero-sum work, and Meier's broader, sustained contribution to statistical-distinguisher methodology across many designs, likewise exemplifies the same compounding pattern.

### 1.5 Books, repositories, and the community layer

Inherited from companion ARX report `09` §2's toolkit list (CLAASP, CryptoSMT, the university lecture-note sources) without modification — this report's distinct addition is a point about *community*, not resources: the Keccak team's own public third-party-cryptanalysis cataloging page (`04`) is a genuinely underused learning resource specifically for the discovery-process focus of this report — reading through years of cataloged findings, in the order they were submitted, with the team's own brief characterization of each result's actual significance attached, is close to the most direct available window into exactly the "distinguisher versus break" calibration judgment `01` §4 and `07` §2.3 both identify as the hardest-to-teach skill in this entire report.

### 1.6 A roadmap from intermediate researcher to independent cryptanalyst, specific to this report's focus

Directly extending companion ARX report `09` §7's general learning roadmap with this report's discovery-process emphasis: after building the mathematical and practitioner foundations that roadmap describes, spend a dedicated period — genuinely, not figuratively — working through `04`'s nine case studies **in primary-source form**, for each one explicitly writing out, before reading the paper's own stated contribution, your own guess at which of `03`'s warning signs you'd have noticed first and which technique you'd have reached for; then compare your guess against what the actual discoverers report. This exercise, repeated across enough real cases, is this report's most direct, actionable answer to "how do I build the intuition faster" — not a shortcut around the practice `01` §4 insists doesn't have one, but a structured, deliberate version of that practice rather than unstructured exposure.

---

## 2. Required Additions to the Companion ARX Report's Paper List

The following did not appear in companion ARX report `10`'s ~100-paper list (which predates this session's research) and should be treated as direct additions for anyone using both reports' bibliographies together:

| Paper | Why it matters here specifically | Status |
|---|---|---|
| Saha, Sasaki, Shi, Sibleyras, Sun, Zhang, "On the Security Margin of TinyJAMBU with Refined Differential and Linear Cryptanalysis" (ToSC 2020 / ePrint 2020/1045) | `04`'s central, most-teachable discovery-process case study — the independence-assumption story | 🟢 required |
| Sakamoto, Liu, Nakano, Kiyomoto, Isobe, "Rocca: An Efficient AES-based Encryption Scheme for Beyond 5G" (ToSC 2021/2022) | Original Rocca design and its own stated countermeasure rationale (the AES-round-vs-quadratic-function choice) | 🟢 required |
| Hosoyamada, Isobe, Sasaki, et al., "Cryptanalysis of Rocca and feasibility of its security claim" (ToSC 2022) | The key-recovery-relevant gap that produced Rocca-S | 🟢 required |
| Derbez, Fouque, Isobe, Rahman, Schrottenloher, "Key committing attacks against AES-based AEAD schemes" (ToSC 2024) | The cross-design attack-family extension (Rocca, Rocca-S, AEGIS, Tiaoxin simultaneously) | 🟢 required |
| Eichlseder, Nageler, Primas, "Analyzing the Linear Keystream Biases in AEGIS" (ToSC 2019) | Direct background to the earlier AES-round-based-AEAD statistical-bias pattern Rocca's design responded to | 🟢 required, read before the Rocca papers above |
| Aumasson, Meier, "Zero-sum Distinguishers for Reduced Keccak-f and for the Core Functions of Luffa and Hamsi" (NIST hash forum, 2009) | The founding Keccak zero-sum result | 🟢 required |
| Dinur, Morawiecki, Pieprzyk, Srebrny, Straus, "Cube Attacks and Cube-attack-like Cryptanalysis on the Round-Reduced Keccak Sponge Function" (EUROCRYPT 2015) | The keyed-mode cube-attack-like methodology, the direct ancestor of the conditional-cube-tester lineage | 🟢 required |
| Huang, Wang, Xu, Wang, Zhao, "Conditional Cube Attack on Reduced-Round Keccak Sponge Function" (EUROCRYPT 2017) | The refined technique reaching further than the original 2015 result | 🟢 required |
| Guo, Liu, Song, "Linear Structures: Applications to Cryptanalysis of Round-Reduced Keccak" (ASIACRYPT 2016) | The linearization technique extending zero-sum distinguishers and enabling reduced-round preimage attacks | 🟢 required |
| Duan, Lai, "Improved zero-sum distinguisher for full round Keccak-f permutation" (ePrint 2011/023) | The direct, dated follow-up sharpening Aumasson-Meier's original parameters | 🟢 required |
| Practical related-key forgery paper on full TinyJAMBU-192/256 (ePrint 2022/1122) | The related-key extension showing the single-key analysis alone was incomplete | 🟢 required |

---

## 3. Closing Note

This report and its three companions in this conversation together cover, between them, essentially the full stack this specific research question implies: what the field's current designs and attacks actually are (the general and ARX-specific reports), how that landscape came to exist (the chronological history), how a designer invents something new within it (the design-process report), and, in this report, how a cryptanalyst goes looking for the place a new design's claims might be false. If your stated long-term goal — designing a portable ARX-based permutation and PRNG — is still where you land after all four of these, the single most concrete, actionable next step this report can leave you with is the one `01` §4 already named directly: there is no substitute for personally working through real specifications, applying `03`'s warning-sign catalog and `02`'s funnel yourself, and checking your own instincts against what actually happened in cases like the ones `04` reconstructs. Every report in this conversation has, in its own way, been assembling the map. The territory still has to be walked.
