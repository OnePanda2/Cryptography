# 07 — Dead Ends & Five-Year Research Trends

*Part of a multi-file chronological history — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`06`.*

---

## 1. Dead Ends — What Looked Promising and Failed

A history told only through what succeeded is dishonest by omission. These are directions the field invested real, serious effort in, believing them promising at the time, that did not pan out the way their proponents expected — and, in every case, what the field actually learned from the failure, which is usually more durable than the failed idea itself.

### 1.1 Algebraic (Gröbner-basis) attacks against AES — the XSL attack

**Why researchers believed in it**: Courtois and Pieprzyk's 2002 XSL attack claimed that representing AES as a large system of low-degree multivariate polynomial equations and solving via a specific algebraic technique (the "eXtended Sparse Linearization" method) could recover AES keys faster than brute force — a genuinely exciting, if the claim held, direct algebraic break of the field's most important cipher, arriving barely a year after AES's own standardization. **What later disproved it**: the claimed complexity analysis rested on assumptions about the linear independence of the generated equations that turned out not to hold at real AES parameters; independent verification and subsequent analysis found the attack's actual complexity, correctly computed, was far worse than exhaustive search — it simply didn't work as claimed. **What survived**: not the specific technique, but the general research program it opened — algebraic and, later, SAT-solver-based attacks (`04`–`05` of this report's companion documents) remain a standard, legitimate part of the cryptanalytic toolkit, just correctly scoped to what they can actually achieve (mainly reduced-round or structurally weak targets), rather than the sweeping full-AES break XSL originally claimed. **The lesson**: an attack's asymptotic/theoretical claim and its actual, independently-verified practical applicability are different things, and the field's subsequent, much stricter insistence on independent replication before accepting a break (a norm this history traces hardening across the following two decades) owes something directly to having been burned by XSL's initial, uncritically-exciting reception.

### 1.2 Isogeny-based cryptography's SIDH/SIKE branch

**Why researchers believed in it**: supersingular isogeny Diffie-Hellman offered, by a wide margin, the smallest keys of any post-quantum key-exchange candidate, and the underlying hard problem (finding an isogeny between supersingular elliptic curves) had a plausible, if comparatively young, security case; SIKE survived four full rounds of NIST's open PQC competition (`04`–`05`), accumulating years of public scrutiny. **What later disproved it**: Castryck and Decru's 2022 attack (`05`) — not a subtle statistical weakness discovered through years of grinding cryptanalysis, but a classical, polynomial-time break connecting SIKE's structure to a decades-old theorem (Kani's, predating SIDH entirely) that simply hadn't been applied to it before. **What survived**: isogeny-based cryptography as a *field* didn't die — SQIsign, a differently-structured isogeny-based signature scheme, remains an active NIST "additional signatures" round-3 candidate as of 2026 — but the specific SIDH/SIKE construction, and a meaningful fraction of the field's confidence in isogeny-based hardness assumptions generally, did not survive intact. **The lesson**, explicitly drawn and repeatedly cited throughout this report's companion documents: competition survival across multiple rounds is strong evidence of *scrutiny received*, not proof of *security*, and a young mathematical foundation deserves continued suspicion regardless of how many rounds it has cleared — a lesson the field now applies explicitly and by name to every remaining "young" PQC candidate family.

### 1.3 Multivariate quadratic (MQ) signature schemes — the Rainbow branch

**Why researchers believed in it**: multivariate-quadratic-system-solving is NP-hard in the worst case, and Rainbow (an "unbalanced oil and vinegar"-family multivariate signature scheme) had been studied for years, reached NIST's PQC signature-standardization rounds, and offered competitive performance. **What later disproved it**: a 2022 attack (Beullens) exploited structural weaknesses specific to Rainbow's parameter choices, breaking it well below its claimed security level — not a break of multivariate cryptography as a hard-problem family generally, but a break of this *specific* construction's particular structural choices. **What survived**: multivariate-based schemes (MAYO, SNOVA, QR-UOV, UOV) remain active in NIST's ongoing "additional signatures" round as of 2026 — the family survived; the specific early flagship candidate did not, a pattern directly parallel to the isogeny story above.

### 1.4 Fully closed, "security through obscurity" hardware key protection

**Why some vendors and standards bodies believed in it, at various points across this history**: the intuition that keeping an algorithm's *design*, not just its key, secret adds a genuine extra layer of security is intuitively appealing and was a real, if increasingly minority, design instinct across parts of this history, especially in some proprietary telecom and pay-TV cryptographic systems from the 1990s–2000s that never went through this history's open-competition processes at all. **What later disproved it, repeatedly**: essentially every closed-design proprietary cipher from this era that was eventually reverse-engineered (the various proprietary GSM and pay-TV ciphers being the most cited examples) turned out to have real, exploitable weaknesses that open review would very likely have caught — direct, repeated, empirical vindication of Kerckhoffs's 1883 principle, playing out across this entire modern history as a live, tested hypothesis rather than a settled matter. **What survived**: the field's near-total consensus, by 2026, that any cryptography intended for serious deployment should be openly published and openly analyzed — closed-design cryptography still exists in some proprietary/legacy niches, but it is essentially never chosen by anyone building a new, serious system, a genuinely completed cultural shift this history's four major open-competition case studies (AES, SHA-3, PQC, Lightweight Cryptography) both reflect and reinforced.

### 1.5 NIST curve provenance as a "probably fine" assumption

**Why the field mostly accepted it, for a while**: NIST's elliptic-curve parameters (P-256 and siblings) were generated via a seeded process that NIST described as unbiased, and — prior to 2013 — the field's default posture toward a major national standards body's published parameters was closer to trust than suspicion, absent specific evidence of a problem. **What eroded this**: Shumow and Ferguson's 2007 Dual_EC_DRBG demonstration (`02`) planted the specific concern that NIST-associated parameter generation *could*, in principle, be engineered; the 2013 Snowden disclosures (`03`) substantiated that *a* NIST-standardized generator had, in fact, been backdoored in a shipped product — and while no equivalent proof exists, as of 2026, that the NIST elliptic curves specifically are compromised, the seed-generation process was never fully, independently justified publicly, and the suspicion, while never resolved either way, never went away either. **What survived**: not a resolved verdict on the NIST curves' safety, but a permanently changed field-wide default posture — provenance-unauditability is now treated as an active cost, not a neutral unknown, directly driving Curve25519's displacement of NIST curves in new protocol design (`03`) even though, to be precise about this genuinely unresolved case, no one has ever proven the NIST curves are actually backdoored.

---

## 2. Five-Year Research Trends — What Each Window Was Actually Excited About

### 1993–1997: Formalizing the informal

Conferences dominated by the transition from ad hoc, "we tried to break it and couldn't" security arguments toward the first generation of rigorous, definitional cryptography — semantic security, the ROM, and Matsui/Biham-Shamir's statistical-attack formalization all crystallizing the field's move from craft toward science. The AES call (1997) closes this window by converting the field's growing formal maturity into an institutional, competition-scale demonstration.

### 1998–2002: Competition and provable padding

Dominated by the AES competition itself — an unprecedented volume of comparative, public cryptanalysis applied to a shared set of serious candidates — alongside the maturing OAEP/PSS provable-security padding-scheme literature and DPA's arrival (1999) opening the implementation-security research thread that would grow steadily for the rest of this history without ever again receding.

### 2003–2007: The hash function reckoning

Dominated, almost entirely, by the MD5/SHA-1 collapse and its aftermath — Wang et al.'s attacks, the resulting SHA-3 competition's opening, and the parallel, quieter but consequential ROM-limits critique (Canetti-Goldreich-Halevi). This is the window where "hash functions are a solved problem" complacency died, permanently, and never returned.

### 2008–2012: Sponges, ARX, and side channels maturing together

Three genuinely distinct research threads matured in parallel and with surprisingly little direct interaction at the time: the SHA-3 competition's sponge-construction theory-building; ARX's post-Salsa20 consolidation via ChaCha and eSTREAM's portfolio selection; and side-channel research's steady professionalization (DPA countermeasures, masking schemes) into what would become CHES's dominant subject matter. Also this window: Gentry's 2009 FHE breakthrough, opening a research program (homomorphic computation) that would run, largely on its own separate track, for the following fifteen-plus years.

### 2013–2017: Trust, provenance, and the automation of trail search

Dominated, unmistakably, by the Snowden/Dual_EC_DRBG reckoning and its direct, cascading consequences — Curve25519's adoption acceleration, the eventual (2018) ISO rejection of Speck/Simon, and a field-wide, permanent tightening of what counts as trustworthy provenance. Running in parallel, quieter at the time but more durable in its ultimate consequences: MILP-based automated cryptanalysis (2011 onward) steadily professionalizing from a novel technique into the field's expected standard practice.

### 2018–2022: Post-quantum urgency becomes concrete

Dominated by NIST's PQC standardization process reaching its selection rounds, TLS 1.3's finalization institutionally validating a decade-plus of protocol-hardening work, and — arriving almost as a coda to this window rather than its centerpiece at the time — Gohr's 2019 differential-neural cryptanalysis result, whose full significance wouldn't be widely appreciated as a durable new sub-field until the following window.

### 2023–2026: Standardization completes, and automation gets a mind of its own

Dominated by the actual finalization of what the previous window only selected — FIPS 203/204/205 (2024), HQC and Ascon's SP 800-232 (2025) — alongside secure messaging's move to continuous (not just handshake) post-quantum protection, automated-search tooling's continued professionalization (CLAASP, the Window Heuristic), differential-neural cryptanalysis's maturation into a fully competitive sub-field, and, closing this window and this entire 33-year history, the first verified instance of substantially autonomous AI-conducted cryptanalytic research altering a live standardization outcome.

---

## 3. Changing Assumptions — A Direct List

For quick reference, the specific assumptions this history has shown the field revising, with the year the revision crystallized:

- "A cipher's key length is the main thing that determines its practical lifespan" → qualified by 1990s differential/linear cryptanalysis showing *structure* can fail independent of key length, though DES's actual death (1998) was still, in the end, a key-length brute-force event, not a structural one.
- "Hash functions, once well-designed, are durably solved" → decisively revised, 2004–2005.
- "Security proven in the random oracle model is a mathematical guarantee" → revised to "strong heuristic evidence," 2004.
- "A national standards body's published cryptographic parameters can be trusted by default absent specific evidence otherwise" → revised to active, ongoing skepticism, 2007–2013.
- "Cryptanalysis is a hand-craft skill that doesn't mechanize" → decisively revised, 2011–2014, and further revised again, 2019 onward (neural), and again, July 2026 (substantially autonomous AI research).
- "ARX designs can't have provable security margins the way SPN designs can" → decisively revised, 2020.
- "Post-quantum cryptography is a distant, theoretical concern" → revised to "an active, deadline-driven engineering program," gradually across 2016–2024, then sharply re-revised toward greater urgency across 2025–2026 specifically due to compressing algorithmic resource estimates.
- "Competition survival across multiple public rounds is strong evidence a scheme is secure" → sharply qualified, 2022 (SIKE), reinforced by Rainbow's break the same year.

Part `08` (`08-research-culture-and-major-researchers.md`) turns to how the field's institutions and norms — not just its ideas — changed across this same span, and profiles the researchers whose individual contributions this history has traced throughout.
