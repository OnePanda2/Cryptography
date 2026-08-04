# 08 — Designing a World-Class PRNG Evaluation Pipeline

*Final file of this report — see `00-INDEX-executive-summary.md` for the map. This file assumes everything in `01`–`07` of this report and, by direct reference, every companion report in this conversation. It is the concrete payoff this entire conversation's research program has been building toward, and it commits to specifics rather than staying at the level of general principle.*

---

## Before the pipeline: what a PRNG's evaluation must cover that a general cipher's doesn't

A PRNG is not just "a cipher used differently" — per companion ARX report `06` §1, it has properties no cipher-evaluation pipeline alone checks: forward secrecy, backward secrecy, fork safety, and a specified reseeding/entropy-accumulation discipline. Any evaluation pipeline for a PRNG that's simply borrowed from cipher evaluation, without these additions, is incomplete by construction, independent of how rigorously it's otherwise executed. The pipeline below is built with this specifically in mind.

---

## The Pipeline

### Phase 0 — Specification completeness (gate before anything else begins)

Before any testing: does the specification fully, unambiguously define — the internal permutation or cipher used for state-stretching; the exact state-update and output-extraction functions; the fast-key-erasure or equivalent forward-secrecy mechanism, stated precisely enough to implement independently; the reseeding schedule (trigger conditions and entropy-mixing function); and explicit fork-safety guidance or an internal mechanism addressing it? **If any of these five is missing or vague, stop here** — per `01` §2's requirements-engineering discipline (companion design-process report `01` §4), an evaluation of an underspecified target produces conclusions about whatever the evaluator happened to assume, not about the design itself, and this is not a defensible starting point for anything that follows.

### Phase 1 — Implementation validation (Stage 3 of `02`, PRNG-specific)

Known-answer tests covering: fixed (key/seed, output) pairs for the base generation function; explicit test vectors for the fast-key-erasure rekey step specifically (not just steady-state output — the rekey transition is exactly where an implementation bug is most likely to hide, since it's exercised less frequently in casual testing than plain output generation); and explicit test vectors for the reseeding/entropy-mixing function given a known entropy input. **Two independent implementations**, built from the specification alone, must agree on all three vector sets before Phase 2 begins.

### Phase 2 — Internal-permutation cryptanalysis (Stage 7 of `02`, the evidence-tier work)

This phase applies, in full, the entirety of companion ARX report `04`–`05` and the entire companion cryptanalyst-mindset report — not summarized here, but explicitly required, not optional, per this report's `04` §2.5 point that no amount of output-layer statistical rigor (Phase 4 below) substitutes for this phase. Concretely, minimum required coverage:

- [ ] Diffusion metrics (SAC/BIC/avalanche/branch number) on the internal permutation, with `03` §2's confidence intervals and `03` §4's multiple-comparisons correction applied
- [ ] Full MILP or SAT-derived differential/linear trail-search evidence across the internal permutation's full round count, with stated margin
- [ ] Rotational-XOR cryptanalysis check specifically (companion ARX report `04` §2.4), if the internal permutation is ARX-based
- [ ] Explicit application of the warning-sign catalog (companion cryptanalyst-mindset report `03`) against the specific state-update and extraction functions — with particular attention to "poor extraction" and "predictable carries," the two entries on that catalog most specifically relevant to a PRNG's own extraction step
- [ ] Explicit check of whether the internal permutation's original security case (if it's a reused, previously-published permutation) was made for a *keyed* context — companion cryptanalyst-mindset report `04`'s Keccak case study is the standing reminder that an unkeyed hash-function security case does not automatically transfer to a keyed generator use

### Phase 3 — Forward-secrecy, backward-secrecy, and fork-safety verification

Not covered by Phase 2 or Phase 4 — these are properties of the *surrounding construction*, not the internal permutation alone (companion ARX report `06` §1.5). Required evidence: **(a)** an explicit demonstration or proof that state compromise at time T does not reveal output generated before T (forward secrecy) — for a fast-key-erasure design, this means demonstrating the old key material is genuinely, verifiably overwritten and unrecoverable, not merely logically discarded (companion ARX report `07` §2.2's compiler-optimization-elimination risk applies directly here: a "secure" memzero that an optimizing compiler can legally eliminate is not evidence of forward secrecy, it's an unverified claim of it); **(b)** an explicit statement and, where feasible, experimental demonstration of backward-secrecy behavior given the specified reseeding schedule, with honest acknowledgment (per companion ARX report `06` §1.3) that backward secrecy without fresh entropy is mathematically impossible, not a gap in this specific design; **(c)** explicit fork-safety testing — snapshot the generator's internal state, resume it twice independently, and confirm the design's stated fork-detection/recovery mechanism actually produces divergent output streams from the two resumptions, not identical ones.

### Phase 4 — Output-stream statistical evaluation, with full rigor

PractRand run to a stated, disclosed maximum sample size (`04` §2.2) — not merely "no anomalies found," but "no anomalies found up to N bytes, with any marginal flags re-run under independent seeding per `03` §6." TestU01/BigCrush run as a full battery, with the same disclosure standard. **The explicit epistemic labeling this phase's results must carry, stated directly in whatever report or paper results this pipeline**: these results support a claim of *statistical uniformity*, not of *cryptographic security* — restating `01` §3 and `04` §2.5 one final time, because this is precisely the point at which an evaluation pipeline most commonly, silently upgrades a measurement into more than it actually is, and this pipeline is built specifically to prevent that upgrade from happening unnoticed.

### Phase 5 — Benchmarking, with statistical rigor

Cycles-per-byte, throughput, and latency (companion ARX report `07` §3.1, this report `05` §1) reported with repeated-trial variance disclosure across at minimum two structurally different platforms (one representing a target with SIMD/hardware acceleration available, one representing a genuinely constrained target, per companion ARX report `01` §1.6's original ARX motivation) — a single-platform benchmark is explicitly flagged as scoped to that platform only, per `05` §1.3, not generalized.

### Phase 6 — Formal argument, where claimed

If the design claims a provable security reduction for its generation/extraction construction (companion ARX report `05` §3, this report `05` §2): the reduction, its idealized-model dependencies, and its concrete-security bound (tied explicitly to Phase 2's trail-search evidence, per `05` §2.2) are all published in full, checkable form — not asserted by reference to a similar prior construction's proof (companion design-process report `04` §1.2's "asserting rather than establishing" mistake, restated here in the PRNG context specifically).

### Phase 7 — Independent replication and peer review

Every phase above repeated, in full or in relevant part, by a party independent of the original design team (`02` §2's control-methodology discipline; `03` §8's genuine-independence requirement) before any claim from this pipeline is treated as established rather than provisional.

---

## What Evidence Would Be Sufficient to Publish

A design that has cleared Phases 0–7 with: comfortable, explicitly-stated margin at every Phase-2 cryptanalytic checkpoint (not merely "no attack found," but a stated gap between the best found attack and the design's actual parameters, per companion design-process report `03` §3.2); Phase-3 properties demonstrated, not merely asserted, including compiler-optimization-resistant memory clearing; Phase-4 statistical results disclosed with sample size and re-run verification for any flagged result; Phase-5 benchmarks disclosed across multiple platforms with variance reporting; and Phase-7 independent replication actually completed, not merely invited — this is a design this report considers **ready for publication and serious peer review**, in the sense that every internally-checkable question has been asked and answered as rigorously as this entire conversation's six reports collectively know how to specify.

## What Would Still Be Insufficient, Stated Honestly

Even a design clearing every phase above has **not** thereby achieved `01` §3's "security" tier — it has achieved calibrated, evidence-grounded *confidence*, appropriately strong for a newly-published design that has not yet received years of broad, independent, multi-specialization external scrutiny (companion cryptanalyst-mindset report `07` §1.3; companion history report `07` §1.2–1.3's SIKE and Rainbow lessons). Specifically still missing, and *unavailable* to any design at the moment of its own first publication, no matter how rigorously that publication was prepared: the test of time and breadth this report has argued, throughout, cannot be substituted for — differential-neural cryptanalysis applied by researchers with no connection to the original design team; RX-cryptanalysis pushed by a group specifically motivated to find something wrong; the accumulated scrutiny of a live NIST-style competition process, if the design is ambitious enough to enter one; and, per the AI-cryptanalysis thread traced throughout companion ARX report `04` §2.10 and `09` §3.3 and companion history report `05`, whatever the field's cryptanalytic capability looks like several years from now, which by definition cannot be checked against today. **This is not a flaw in the pipeline** — it is `01` §3's epistemic hierarchy holding, honestly, all the way to the end: proof and rigorous evidence produce strong, genuine, warranted confidence, and confidence is still not the same thing as security, for this design or any other, and a pipeline that claimed otherwise would be violating the exact discipline this entire report has argued for from its first page.

---

This closes the eight-file report on evaluating cryptographic primitives rigorously, and, with it, the six-report research program this conversation has built across general cryptography, ARX-specific design and cryptanalysis, chronological history, the design process, the cryptanalyst's mindset, and now evaluation methodology itself. If your stated goal — a portable ARX-based permutation and PRNG, designed and evaluated to a standard that would satisfy the field's own best current practice — is still where this leaves you, the pipeline above is the most concrete, actionable answer this conversation has to give.
