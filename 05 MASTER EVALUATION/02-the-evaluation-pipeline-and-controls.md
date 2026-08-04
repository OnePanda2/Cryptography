# 02 — The Evaluation Pipeline and Controls

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`'s five-level epistemic hierarchy. Companion cryptanalyst-mindset report `02` already reconstructs a similar-looking staged workflow — that file is written from the *attacker's* vantage point (which technique do I reach for to break this). This file is the *evaluator's* vantage point: not "how do I find a weakness" but "what sequence of checks lets me report a calibrated confidence level, honestly, to someone who will rely on it" — a related but genuinely different question, since an evaluator's job includes reporting a clean result as a clean result, not just hunting for a dirty one.*

---

## The Pipeline, Stage by Stage

### Stage 1 — Specification review, read for testable claims

Restating `01` §4's framing operationally: read the specification specifically extracting every claim that could, in principle, be measured or falsified — a stated security level, a stated diffusion property, a stated performance target, a stated randomness guarantee. **The evaluator's specific discipline here, distinct from the cryptanalyst's (companion cryptanalyst-mindset report `02` Stage 1's assumption-hunting)**: the evaluator is building a checklist of claims to *test*, not yet a list of suspected weaknesses — the posture is closer to "what would I need to measure to responsibly report a confidence level on each of these specific claims" than "which of these claims do I doubt."

### Stage 2 — Threat model, made explicit and bounded

Directly restating companion design-process report `01` §4's requirement that a design state its threat model explicitly, from the evaluator's receiving end: if the specification leaves the threat model vague, the evaluator's first responsible move is not to guess generously on the design's behalf, but to state, in their own evaluation report, exactly what threat model they are testing under — because an evaluation's conclusions are only meaningful relative to a stated threat model, and a vague threat model produces evaluation conclusions that are vague in exactly the same proportion, however precise the underlying measurements look.

### Stage 3 — Implementation validation and reference vectors

Before any security-relevant testing begins: does the reference implementation actually compute what the specification says it computes? This is checked via **known-answer tests (KATs)** — fixed input/output pairs, ideally including edge cases (all-zero input, maximum-value input, boundary block-length cases) that an independent implementation, built from the specification alone without looking at the reference code, should reproduce exactly. **Why this stage exists as a distinct, early gate rather than being assumed**: every subsequent stage's measurements are meaningless if run against an implementation that doesn't actually match the specification — a diffusion test run against a buggy implementation measures the bug, not the design, and this is a genuinely common, entirely preventable source of wasted downstream effort. Reference vectors should be **published alongside the specification**, not held privately, specifically so independent evaluators can perform this check themselves rather than trusting the design team's own claim that their implementation is correct.

### Stage 4 — Correctness testing beyond the KATs

KATs check specific fixed points; correctness testing more broadly checks structural properties that should hold universally — for a PRNG specifically, does encrypt-then-decrypt (or, for a generator, generate-then-verify-against-an-independent-reimplementation) round-trip correctly across a wide, randomized sample of inputs, not just the fixed KAT set? This stage exists to catch implementation bugs that happen to not touch any of the specific KAT inputs, a real and non-hypothetical gap KATs alone leave open.

### Stage 5 — Diffusion analysis

Companion ARX report `03` in full technical detail; `04` of this report adds the statistical-rigor layer this stage specifically needs and generally doesn't receive in published practice.

### Stage 6 — Randomness evaluation

Companion ARX report `06` in full technical detail; `04` of this report adds the same rigor layer here.

### Stage 7 — Cryptanalysis

Companion ARX report `04`–`05` and the entirety of the companion cryptanalyst-mindset report in full technical and process depth — not re-derived here.

### Stage 8 — Benchmarking

Companion ARX report `07` in full technical detail; `05` of this report adds the statistical-rigor layer.

### Stage 9 — Statistical validation of the *entire evaluation itself*

The stage this report exists to add real weight to, covered in full in `03`: not a single test in the pipeline, but a cross-cutting check applied to every prior stage's results jointly — has the pipeline run enough independent tests that some "significant" findings are expected by chance alone, and has that been corrected for? Are confidence intervals reported wherever a stage's result is a sampled estimate rather than an exact computation?

### Stage 10 — Independent replication

Restating companion ARX report `08` §1.3's replication discipline and companion cryptanalyst-mindset report `07` §1.2's negative-control point from the evaluation-pipeline's own vantage point: has every prior stage's result been reproduced by someone who built their own implementation from the specification, independent of the original design team's code? A result that exists only in the original team's own codebase, however carefully that codebase was written, has not yet cleared this stage.

### Stage 11 — Peer review

The stage where everything above gets exposed to reviewers who did not participate in the design or the original evaluation — covered in reviewer-expectation depth in companion design-process report `04` and companion cryptanalyst-mindset report `07`; from the evaluation-pipeline's vantage point specifically, this stage's function is providing the *external* scrutiny `01` §1 named as the whole reason evaluation exists in the first place, closing the loop the pipeline opened at Stage 1.

---

## Positive and Negative Controls, Done Rigorously

### Why controls are essential, stated precisely

A test result is only informative relative to what it would have looked like if the thing being tested were different — this is the entire logical structure a control provides, and its absence is the single most common reason a technically-correct measurement fails to actually constitute evidence (`01` §3's measurement/evidence distinction, operationalized).

### Positive controls

**Definition, applied to this domain**: run your exact evaluation methodology against a target where the *correct* answer is already known and expected to show the effect you're testing for — does your MILP model correctly find the already-published best-known trail for a well-studied cipher before you trust it on your novel design? Does your PractRand harness correctly flag a deliberately-weakened generator (a linear congruential generator, say) as non-random before you trust its "no anomaly" verdict on your real candidate? **Why this matters specifically for a first-time evaluator, stated directly**: a methodology that has never been checked against a known-positive case can have an undetected bug that makes it silently incapable of finding *any* effect, of any kind — a "clean" result from such a methodology is not evidence of a clean design; it's evidence of a broken test, and the two are indistinguishable without a positive control.

### Negative controls

**Definition**: run your exact evaluation methodology against a target where the effect you're testing for is known to be *absent* — does your differential-trail search correctly report "nothing found" against a deliberately over-strengthened variant with far more rounds than any realistic attack could reach? Does your statistical test correctly *fail* to flag genuinely uniform random data as anomalous, at the expected false-positive rate? **The specific, easily-missed value of a negative control**: it's the only way to directly measure your methodology's actual false-positive rate on your own setup, rather than trusting a test suite's documented theoretical false-positive rate applies unchanged to your specific implementation, sample size, and parameter choices.

### Known-answer tests and regression tests

Distinct from Stage 3's *correctness*-focused KATs: a **regression test** specifically re-runs a previously-established result (a previously-computed differential-trail bound, a previously-measured benchmark figure) after any change to the codebase or methodology, checking that the change didn't silently alter a result that should have stayed fixed — this is a software-engineering discipline as much as a cryptographic one, and its absence is a common, quietly compounding source of the "our numbers changed between paper drafts and we're not sure why" problem that undermines confidence in a result even when the final reported numbers happen to be correct.

### Cross-validation and independent implementations

Restating Stage 10 with its specific control-theoretic framing: an independent implementation, built from the specification alone, is itself a control on the *specification's* clarity and correctness, distinct from a control on any specific test's methodology — if two independently-built implementations disagree on any KAT, the specification itself (not just one team's code) has a genuine, evaluation-relevant problem.

### Historical examples where controls prevented incorrect conclusions

Restating and reframing cases already covered in companion reports through this file's control-specific lens: **TinyJambu's discovery (companion cryptanalyst-mindset report `04`)** is, read through this lens, a case where the *original designers'* methodology lacked an adequate negative control — their MILP model was never checked against a target where the independence assumption's failure would have been visible, meaning the model's "no better trail found" result was silently a false negative, not genuine evidence of security. **Keccak's zero-sum-distinguisher cataloging discipline (same companion report)** is, by contrast, a positive institutional example of controls working as intended — the Keccak team's explicit, standing practice of checking every third-party finding against "does this actually threaten the sponge construction's real security claim" is functionally a negative control applied at the level of an entire research program, run continuously rather than once.

Part `03` (`03-statistical-methodology-for-cryptographic-evaluation.md`) now develops the statistical rigor this pipeline's Stage 9 requires, in full technical depth — this report's central original contribution.
