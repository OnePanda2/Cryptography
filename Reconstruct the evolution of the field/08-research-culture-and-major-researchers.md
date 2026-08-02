# 08 — Research Culture & Major Researchers

*Part of a multi-file chronological history — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`07`.*

---

## 1. How Research Culture Changed, 1993–2026

### 1.1 The rise of the open competition

The single most consequential cultural shift this entire history traces: from DES's substantially closed, NSA-involved 1970s development process to a field where, by 2026, **every major new symmetric or post-quantum standard has been produced through an open, multi-round, globally-scrutinized competition** — AES (1997–2000), SHA-3 (2007–2012), CAESAR (2014–2019, non-NIST but institutionally similar), NIST Lightweight Cryptography (2018–2025), and NIST PQC (2016–ongoing). Each competition explicitly reused and refined the previous one's template — SHA-3's process was consciously modeled on AES's; PQC's was consciously modeled on both. **What changed procedurally across these four iterations**: increasing formalization of multi-round elimination with published rationale at each round; increasing expectation that submissions include reference implementations, not just paper specifications; increasing weight given to implementation footprint and side-channel posture alongside pure cryptanalytic margin (a weighting shift visible in Ascon's 2022 win over the comparably-margined Sparkle, `05`); and, in the PQC process specifically, an explicit, unprecedented institutional commitment to hedging against single-hard-problem-family risk (HQC as a structurally-independent backup to lattice-based ML-KEM) that none of the earlier competitions needed, because none of the earlier competitions were replacing a hard-problem family under an active, known future threat (Shor's algorithm) the way PQC was.

### 1.2 Peer review expectations tightening

**What a "convincing" security argument required visibly escalated across this history**: 1990s-era papers could credibly claim security based substantially on "we tried the known attacks and this resists them"; by the 2010s, MILP/SAT-derived trail-search evidence became progressively expected, not optional; by the 2020s, per this report's companion ARX-specific document's detailed reviewer-mindset treatment, a serious FSE/ToSC/CHES submission is expected to provide reproducible artifacts (models, code, and, for neural-cryptanalysis work, training pipelines) alongside prose claims, formalized in part through explicit **artifact evaluation** processes some venues now run — a genuine, if gradual, tightening of what counts as adequate evidence, tracking directly with the automated-tooling maturation this history has traced as its own thread.

### 1.3 Responsible disclosure norms

**Established gradually, without a single crystallizing event the way some other cultural shifts in this history had one**: by the 2020s, the field's norm for a genuine, serious cryptanalytic finding against a live or deployed system is private notification to affected parties/maintainers, a reasonable remediation window, and coordinated public disclosure — visible directly in the July 2026 HAWK event (`05`), where Anthropic reportedly notified HAWK's developers privately in June 2026 and engaged NIST's public pqc-forum norms before the July 28 public announcement, and where "hundreds of hours" of independent human verification preceded publication — a textbook instance of exactly the disclosure discipline the field had spent the preceding two decades normalizing, applied, notably, to what may be the field's first substantially-AI-conducted cryptanalytic discovery, suggesting the norm generalized to a genuinely new kind of researcher without needing to be reinvented.

### 1.4 Open-source implementation as a trust signal

**A gradual but decisive shift**: 1990s-era cryptographic software was often proprietary or, even when published, treated as a secondary artifact relative to the paper describing it. By the 2010s–2020s, a reference implementation — increasingly, per `06`'s portable-optimization thread, a *portable, auditable* one specifically — became close to a mandatory accompaniment to any serious new design, and major, heavily-audited open-source libraries (libsodium, OpenSSL's post-Heartbleed modernization, BoringSSL, HACL*'s formally-verified code) became load-bearing trust infrastructure the whole field depends on, not optional conveniences.

### 1.5 Benchmarking standardization

**From ad hoc, inconsistently-reported performance claims** in earlier decades toward, by the 2010s–2020s, standardized cross-implementation benchmarking harnesses (SUPERCOP/eBASH, tracing to the eSTREAM/eBASH lineage) and an increasingly explicit expectation (detailed in this report's companion ARX-specific document) that reported cycles-per-byte figures disclose platform, compiler, optimization level, and measurement-variance methodology — a quieter but real professionalization running parallel to peer-review tightening.

### 1.6 Growth and internationalization of the research community

Not tied to a single event, but visible across this history's search-record footprint (this report's companion documents' bibliographies): the automated-search and RX-cryptanalysis literature specifically shows substantial, active contribution from research groups across Europe (Luxembourg's SnT, KU Leuven's COSIC, Bochum's HGI) and China through the 2010s–2020s, a genuinely more globally-distributed research community by 2026 than the field's earlier decades, when North American and a smaller set of Western European institutions more heavily dominated visible publication.

---

## 2. Major Researchers

For each: the specific contributions this history has traced, and — per your outline's specific request — research philosophy and long-term influence where the historical record supports characterizing it.

**Claude Shannon** — not active within this report's 1993 start-date window, but the entire period's intellectual grandfather; confusion/diffusion and information-theoretic secrecy (1948–1949) are the vocabulary every subsequent design decision this history traces is stated in.

**Whitfield Diffie, Martin Hellman, Ron Rivest, Adi Shamir, Leonard Adleman** — likewise pre-dating this history's window (1976–1978), but RSA and Diffie-Hellman are the exact public-key foundation whose eventual quantum-vulnerability (Shor, 1994, `01`) sets this entire history's second major throughline (the 22-year arc to PQC finalization) in motion.

**Daniel J. Bernstein** — the single most recurring individual name across this entire history. Salsa20 (2005), ChaCha (2008), Curve25519 (2006), Ed25519 (2011, with Duif, Lange, Schwabe, Yang), Poly1305 (2005), and a sustained, decades-long public design philosophy explicitly prioritizing speed, implementation-safety, side-channel resistance by construction, and transparent, reproducible parameter generation — a philosophy that reads, across this history's full arc, as remarkably prescient specifically about the provenance-trust dynamic that wouldn't become field-wide consensus until the 2013 Snowden episode validated concerns Bernstein's design choices had been implicitly addressing for years beforehand. **Long-term impact**: ARX's entire modern trajectory, as traced through this history and this report's companion ARX-specific document, is substantially a Bernstein-originated research program that the rest of the field then extended (ChaCha's own later cryptanalysis lineage, BLAKE2/3's adoption of ChaCha's core function, SipHash) — few individuals in this 33-year window have a comparably direct, traceable line from personal design choices to field-wide default practice.

**Joan Daemen and Vincent Rijmen** — Rijndael/AES (2000, `01`) and, separately, Daemen's continuing role (with Bertoni, Peeters, Van Assche) in Keccak/SHA-3 (2012, `03`) and the Xoodoo/Xoodyak lightweight-cryptography line (`04`–`05`'s companion document) — a genuinely unusual case of the same researcher(s) originating *two* of this history's most consequential structural paradigms (the wide-trail-strategy SPN tradition, and the sponge-construction tradition), each independently field-defining.

**Phillip Rogaway and Mihir Bellare** — the ROM's formalization (1993, `01`), OAEP and PSS (1994/1996), and the Encrypt-then-MAC generic-composition result (2000) that eventually, if belatedly, corrected TLS's CBC-mode vulnerability class (`04`'s AEAD-thread discussion in `06`) — a body of work whose defining character, across this history, is *methodological infrastructure-building*: not any single flashy break or design, but the proof techniques and definitional frameworks an entire generation of subsequent papers rest on.

**Xiaoyun Wang** (with Dengguo Feng, Xuejia Lai, Hongbo Yu, and, for the SHA-1 extension, Yiqun Lisa Yin) — the MD5 and SHA-1 collapse (2004–2005, `02`), arguably this history's single most consequential *cryptanalytic* (as opposed to design) contribution, directly triggering the SHA-3 competition and permanently ending the field's "hash functions are solved" complacency.

**Peter Shor and Lov Grover** — the two quantum algorithms (1994, 1996, `01`) whose asymmetric consequences (Shor breaking public-key cryptography outright, Grover only mildly weakening symmetric cryptography) define this history's entire second major throughline and directly explain why the post-quantum research program concentrated exclusively on public-key replacement.

**Oded Regev** — Learning With Errors and its worst-case-to-average-case quantum reduction (2005, background to `04`), the single mathematical result underlying essentially the entire lattice-based PQC generation (ML-KEM, ML-DSA, and this history's companion PQC-status material) — a case where a paper's full consequences (finalized national standards) didn't arrive until nineteen years later, a genuinely long fuse even by this history's standards.

**Beierle, Biryukov, Cardoso dos Santos, Großschädl, Perrin, Udovenko, Velichkov, Wang (Sparkle/Alzette's authorship team)** — the long-trail strategy (2020, `04`), this history's clearest instance of ARX design achieving methodological parity with SPN's decades-older provable-margin tradition, and a genuinely collaborative, multi-institution achievement (Luxembourg's SnT, among others) rather than a single-lab or single-researcher result.

**Aron Gohr** — the founding differential-neural cryptanalysis paper (2019, `04`) and the subsequent, methodologically self-critical follow-up work (the "aaaa-blinding" and "Real Differences" experiments) specifically interrogating what the technique actually learns — a research posture (introduce a powerful new technique, then rigorously interrogate your own claims about why it works) this history treats as a model instance of the field's mature, self-scrutinizing culture (§1.2 above) applied by a single researcher to their own breakthrough.

**Wouter Castryck and Thomas Decru** — the 2022 SIKE break (`05`), this history's sharpest illustration that competition survival is not proof of security, achieved by connecting a decades-old theorem (Kani's) to a live standardization candidate rather than by any fundamentally new cryptanalytic machinery — a reminder, worth stating explicitly, that major breaks in this history sometimes come from creative *application* of existing mathematics rather than the invention of new techniques.

**The Anthropic Claude Mythos Preview research effort** (July 2026, `05`) — not a human researcher, and deliberately listed last and separately for exactly that reason: the first entry in this history's roster of "major contributors" that is a substantially autonomous AI system rather than a human or human team, credited by Anthropic's own disclosure with the core mathematical discovery under human direction and extensive human verification. Whatever this ultimately means for how this list looks in a future edition of this history, its presence here, as of August 2026, is simply factual, and this report declines to speculate further about its long-term significance beyond what `05`'s closing discussion already states.

---

Part `09` (`09-reading-roadmap-top100-top50-bibliography-future.md`), the final file in this report, provides the chronological reading order, the top 100 papers and top 50 milestones your outline specifically requested, and closes with future research directions.
