# 04 — Diffusion and Randomness Evaluation, With Statistical Rigor

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`03`. Companion ARX report `03` (every diffusion metric) and `06` (every randomness test suite) are assumed in full and not re-derived — this file applies `03`'s statistical layer directly to the specific tests those companion sections document.*

---

## 1. Diffusion Metrics, With the Rigor Layer Applied

### 1.1 SAC and BIC

Covered mathematically in companion ARX report `03` §2.1–2.2. Applying `03` §2 and §4 of this report directly: every dependency-matrix entry needs its sample size stated and its confidence interval computable; every full-matrix evaluation needs multiple-comparisons correction before any individual cell is treated as a finding rather than noise. **The specific addition this report makes to companion ARX report `03` §2.8's blind-spots list**: that list already names "empirical dependency-matrix estimation via random sampling has its own statistical-confidence blind spot" as a known limitation — this file's `03` supplies the actual quantitative machinery (the standard-error table, the multiple-comparisons correction) that turns that named limitation from an acknowledged caveat into something an evaluator can concretely correct for, rather than merely flag and move past.

### 1.2 Branch number

Companion ARX report `03` §2.3 — worth noting explicitly that branch number is an **exact, deterministic linear-algebra computation**, not a sampled statistic, and none of this file's statistical machinery applies to it; it belongs instead in `03` §9's "measurement uncertainty in non-statistical contexts" category, alongside MILP-computed trail weights.

### 1.3 Avalanche-weight-distribution testing

Companion ARX report `03` §2.7. This is a genuinely good candidate for the full power-analysis treatment in `03` §5: the null hypothesis (avalanche weight follows a binomial distribution centered on n/2 for an n-bit output) has a precisely computable expected variance, meaning a rigorous evaluator can state, in advance, exactly how many trials are needed to detect a deviation of a specified size from the ideal binomial shape — a calculation this report has not seen made explicit in typical published avalanche-testing practice, and one that would directly strengthen it.

### 1.4 Entropy-growth and information-theoretic diffusion metrics

Companion ARX report `03` §2.4. These metrics are frequently more expensive to estimate accurately at real design scale than SAC/BIC, per that section's own statement — meaning the power-analysis discipline in `03` §5 of this report is *more*, not less, important here: an entropy-growth estimate computed from an inadequately-sized sample carries wide, often unstated uncertainty, and reporting a single entropy figure without its associated confidence interval risks exactly the false-precision problem `03` §2 warns against generally.

## 2. Randomness Test Suites, With the Rigor Layer Applied

### 2.1 The core reframing

Companion ARX report `06` §2.5's core finding — that passing statistical tests demonstrates uniformity, not cryptographic security — remains this report's most important single point about randomness testing and is not qualified or weakened by anything in this file. What this file adds: **even within the narrower claim these suites can legitimately support** (statistical uniformity), the specific way that claim gets reported is itself often under-rigorous in exactly the ways `03` describes, independent of the separate, larger point about statistical uniformity not implying cryptographic security.

### 2.2 PractRand specifically

Companion ARX report `06` §2.1, §2.4. PractRand's own design — testing at escalating, doubling sample sizes until an anomaly appears or a specified maximum is reached — is, in effect, a form of **built-in power analysis**: rather than committing to one fixed sample size in advance, it runs until either a failure is detected or a very large sample size is reached without one, directly addressing `03` §5's under-powering concern by construction. **The rigor-layer addition this file makes**: an evaluator using PractRand should still report the specific maximum sample size actually reached before stopping (not just "no anomalies found"), because per companion ARX report `06` §2.4's final bullet, a report that omits this figure is unfalsifiable in exactly the way `01` §3's evidence-versus-measurement distinction warns against — "no anomalies found" at 2^30 bytes and at 2^40 bytes are different-strength claims, and only one of them is usually actually true for a given evaluation run.

### 2.3 TestU01 / BigCrush / SmallCrush / Rabbit

Companion ARX report `06` §2.5. Directly applying `03` §6's multiple-comparisons discussion: BigCrush's own internal structure already represents dozens of simultaneous tests, and a full BigCrush "pass" should be understood, precisely, as "no sub-test crossed its threshold after the suite's own internal handling of this issue" — not as "zero risk of any false positive anywhere in the battery," a distinction worth stating explicitly because "passed BigCrush" is sometimes reported with more absolute confidence than the underlying multi-test structure actually supports.

### 2.4 NIST STS, Dieharder, ENT

Companion ARX report `06` §2.1. The same multiple-comparisons and re-run-on-flag discipline (`03` §4, §6) applies identically; these suites are not qualitatively different from PractRand/TestU01 in this respect, only differently scoped and differently sensitive.

### 2.5 Internal state versus output testing, with rigor applied

Companion ARX report `06` §2.3's two-layer methodology (cryptanalytic evaluation of the internal permutation, separately from statistical testing of the output stream) is, from this file's vantage point, exactly the right structural response to `01` §3's evidence hierarchy: the internal-permutation cryptanalysis (companion ARX report `04`–`05`, and the entirety of the companion cryptanalyst-mindset report) is capable of producing genuine *evidence* about security, in the sense `01` §3 defines the term, while output-stream statistical testing alone caps out at *measurement* of uniformity — and no amount of additional statistical rigor applied to the output-testing layer (however carefully §1–2 of this file are followed) can promote it into the evidence tier the internal-permutation analysis occupies. This is worth stating as directly as possible because it's the single most consequential thing this file wants a PRNG evaluator specifically to internalize before `08`'s capstone pipeline: **rigor applied to the wrong layer doesn't compensate for skipping the right layer.**

Part `05` (`05-benchmark-methodology-and-formal-evaluation-with-rigor.md`) now applies the same statistical-rigor layer to benchmarking and formal/provable-security evaluation.
