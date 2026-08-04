# 06 — Reproducibility, Evaluation Failures, and Modern Frameworks

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`05`. Companion history report covers competition timelines and companion cryptanalyst-mindset report `07` covers failed-research examples in cryptanalysis specifically — this file's reproducibility material is new; its failures and frameworks material is a targeted synthesis, not a re-derivation.*

---

## 1. Reproducibility as a Formal Discipline

### 1.1 What reproducibility actually requires, stated precisely

Restating `03` §8's statistical framing as a concrete, checkable practice: reproducibility means an independent party, given the specification and the claimed methodology but *not* the original team's exact code or exact random seeds, can arrive at results consistent with the original claims within stated uncertainty bounds. This is a meaningfully higher bar than "the code runs" — code that runs and produces the same output every time on the same machine demonstrates **determinism**, a necessary but not sufficient condition; genuine reproducibility additionally requires that the *specification itself* (not just one team's implementation of it) contains enough information for an independent implementation to converge on the same claims.

### 1.2 Artifact evaluation

Companion ARX report `08` §1.4 covers this from the design-process side; the evaluation-methodology-specific point: an artifact-evaluation package for an *evaluation* claim (not a design claim) should include the actual test harness, the exact random-seed values or seed-generation procedure used, the specific tool versions (a MILP solver's version can affect search completeness and runtime; a compiler version affects benchmark figures), and, per `03` §10's checklist, the raw data underlying any reported confidence interval — not merely the final interval itself, so an independent reviewer can recompute it and check the calculation.

### 1.3 Version control, determinism, and data preservation

Genuinely software-engineering-adjacent disciplines that nonetheless directly bear on evaluation credibility: a claim's supporting code should be tagged to a specific, immutable version (not "the current state of a live repository," which can silently change after a paper is published); wherever true randomness isn't essential to the test itself, a fixed, documented seed makes exact reproduction possible rather than merely statistical reproduction within a confidence interval; and raw results (not just summary statistics) should be preserved and, ideally, published, since a summary statistic computed with an undisclosed error can't be independently caught without access to what it was computed from.

### 1.4 Cross-platform reproducibility, distinct from within-platform reproducibility

Directly extending `05` §1.3's distinction: a benchmark result reproducing exactly on the *same* hardware/compiler/OS combination the original team used demonstrates far less than a result that reproduces (within `03`'s statistical bounds) across genuinely different platforms — the former mainly rules out a determinism bug; the latter is actual evidence the underlying claim (about the design's performance, not about one specific measurement) is robust in the sense `03` §7 defines.

### 1.5 Conference and venue requirements, as an institutional reproducibility mechanism

Companion cryptanalyst-mindset report `08` §1.3 names ToSC/FSE/CHES as the field's primary venues; the reproducibility-specific point worth adding: these venues' growing artifact-evaluation processes (companion ARX report `08` §1.4) function as an *institutional* enforcement mechanism for everything §1.1–1.4 describe — converting reproducibility from an individual researcher's personal discipline (which varies) into a structural expectation the venue itself checks before publication, which is precisely the kind of external-scrutiny mechanism `01` §1 argues evaluation exists to provide in the first place, applied recursively to the evaluation process itself.

## 2. A Catalog of Evaluation Failures

Restating and synthesizing failures already documented across this conversation's companion reports, organized specifically around *which stage of `02`'s pipeline the failure occurred at* — because, per this report's overall framing, a failure's lesson is most actionable when tied to the specific pipeline stage a future evaluator should reinforce.

| Failure | Pipeline stage it exposes | Companion report |
|---|---|---|
| TinyJambu's independence-assumption gap | Stage 7 (cryptanalysis) — a methodology bug, not a sampling issue | cryptanalyst-mindset `04` |
| The XSL attack's overclaimed complexity | Stage 10 (independent replication) — the initial claim wasn't independently re-verified before wide attention | history `07` §1.1, cryptanalyst-mindset `07` §2.1 |
| Dual_EC_DRBG passing every statistical test while backdoored | Stage 6 (randomness evaluation) — the sharpest possible illustration of `04` §2.5's layer-conflation point in this report | general-cryptography `04` §2.4–2.5, this report `04` §2.5 |
| Rocca's original security claim outrunning its own proof | Stage 1 (specification review) — a gap between prose claim and formal scope that specification-review discipline exists specifically to catch | cryptanalyst-mindset `04` |
| Rainbow and SIKE surviving multiple competition rounds before catastrophic breaks | Stage 11 (peer review) interpreted too optimistically — round-survival mistaken for proof rather than accumulating-but-still-partial confidence, exactly `01` §3's proof/confidence conflation | general-cryptography `03` §2.3, history `07` §1.2–1.3 |
| Heartbleed | Stage 3–4 (implementation validation/correctness testing) — a memory-safety bug invisible to every algorithm-level evaluation stage, the standing reminder that this report's entire pipeline evaluates the *algorithm*, and a structurally separate implementation-review discipline (companion ARX report `07` §2.4) is still required regardless of how rigorously the algorithm itself was evaluated | history `03` |

### 2.1 The synthesized lesson

Every failure in this table maps to a *specific, nameable* pipeline stage, not to "insufficient rigor" in some diffuse, unlocated sense — which is this report's central practical argument for why `02`'s staged pipeline, with `01`'s epistemic hierarchy applied at every stage, is worth following explicitly rather than trusting general diligence alone: general diligence doesn't tell you *which* specific stage needs the most scrutiny for a given design, while this table's stage-by-stage mapping does.

## 3. Modern Evaluation Frameworks — What Evidence Actually Matters for Finalist Selection

Companion history report covers the AES/SHA-3/CAESAR/PQC/NIST-LWC timelines in full; this section's distinct contribution is the *evidentiary* question specifically — not when finalists were chosen, but what kind of evidence actually moved a design from "candidate" to "finalist" to "winner" across these processes.

**Cryptanalytic cleanliness** (per this report's Stage 7, and the entirety of companion ARX report `04`–`05` and the companion cryptanalyst-mindset report) is necessary but, per companion design-process report `05`'s Ascon-over-Sparkle case study, **not sufficient** — every NIST competition this conversation's reports trace has, in its own published selection rationale, explicitly weighed implementation footprint, side-channel posture, API flexibility, and *breadth* of received scrutiny (not just its absence of findings) alongside pure cryptanalytic results. **Breadth of scrutiny specifically** functions, in practice, as this report's `01` §3 confidence concept made institutionally concrete: a design receiving sustained attention from differential/linear specialists, algebraic/SAT specialists, side-channel specialists, and (increasingly, per companion ARX report `04` §2.10) differential-neural specialists earns a *qualitatively* different, more calibrated confidence level than one that's simply avoided attracting any attention at all — and NIST's own multi-round, open-call structure is best understood as a deliberate institutional mechanism for manufacturing exactly this breadth, not merely for running the clock long enough for attacks to surface.

Part `07` (`07-research-opportunities-and-software-ecosystem.md`) now identifies open problems specifically in evaluation methodology, and surveys the tool landscape this report's statistical layer needs to actually be practiced with.
