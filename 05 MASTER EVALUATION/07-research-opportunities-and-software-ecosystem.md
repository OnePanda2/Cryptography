# 07 — Research Opportunities and Software Ecosystem

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`06`. Companion ARX report `09` already ranks open problems in ARX design/cryptanalysis broadly; this file is scoped specifically to evaluation *methodology* — problems about how we measure and validate, not problems about specific ciphers.*

---

## 1. Research Opportunities, Ranked

| # | Opportunity | Novelty | Difficulty | Potential impact | Tractability for an independent researcher |
|---|---|---|---|---|---|
| 1 | A standardized, published practice for reporting confidence intervals and sample sizes alongside every diffusion-metric figure in a new-design paper | High — this report's search record and companion documents found no existing field-wide standard for this specifically | Low-medium — the statistical machinery (`03` §2) already exists in general statistics; the work is field-specific adaptation and advocacy | Medium-high — would directly close the false-precision gap `03`–`04` identify | **High** — genuinely approachable; largely a matter of building and publishing a reference methodology paper plus reference implementation |
| 2 | A formal, general treatment of the multiple-comparisons problem specifically for full dependency-matrix (SAC/BIC) evaluation, with a published, ready-to-use correction procedure | Medium-high | Low-medium | Medium — a real, currently-uncorrected gap per `03` §4 | **High** — a well-scoped statistics-meets-cryptography contribution |
| 3 | Extending PractRand/BigCrush-style built-in power-analysis discipline (`04` §2.2) explicitly to diffusion testing, not just randomness testing | Medium | Medium | Medium | Medium-high |
| 4 | A formal treatment of exactly how Markov-cipher-assumption uncertainty (`03` §9) should be quantified and reported alongside MILP/SAT trail-search results, as a distinct uncertainty category from sampling uncertainty | High — genuinely underspecified in current published practice, per companion ARX report `05` §1.7's flag | High — requires real new theoretical work, not just adaptation of existing statistical tools | High — would directly strengthen the concrete-security bridge this report's `05` §2.2 describes | Medium — a harder, more theoretical target than #1–3, but well-scoped |
| 5 | Cross-platform benchmark-variance standardization — a published, reusable methodology for separating within-platform measurement noise from genuine across-platform true-value variation (`05` §1.3) | Medium | Low-medium | Medium — primarily valuable for fair, standardized cross-design performance comparison | High — an empirical, systems-oriented contribution, approachable without deep new cryptanalytic theory |
| 6 | A reusable, open-source reference implementation of this report's full statistical checklist (`03` §10), built to plug directly into existing tools like CLAASP and PractRand's output | High — no equivalent tool currently exists, per this report's search record | Medium | High — would lower the barrier to adopting this report's entire statistical layer from "know the theory" to "run the tool" | **High** — a concrete, buildable software contribution with an unusually direct path from idea to usable artifact |

## 2. Software Ecosystem

Companion ARX report `09` §2 and companion cryptanalyst-mindset report `06` §2 already catalog CLAASP, CryptoSMT, PractRand, TestU01, Gurobi/CPLEX, and CryptoMiniSat in full — not repeated here. This section adds the specifically statistical/methodological tooling those catalogs don't cover:

| Tool | Relevance to this report |
|---|---|
| **R, Python's `scipy.stats`/`statsmodels`** | General-purpose statistical computing — confidence intervals, power analysis, multiple-comparisons correction (`03` §2, §4, §5) are directly computable with standard functions in either ecosystem; no cryptography-specific tool is needed for this layer, which is itself part of this report's point — the machinery is standard, general statistics, not exotic |
| **`statsmodels`' multiple-testing correction module (Bonferroni, Benjamini-Hochberg)** | Directly implements `03` §4's correction techniques, ready to apply to a dependency-matrix p-value array |
| **Criterion (Rust) / Google Benchmark (C++)** | Purpose-built benchmarking harnesses that already implement much of `05` §1.2's repeated-trial, variance-reporting discipline by default — a genuinely good practical starting point for benchmark rigor specifically, since they handle much of the statistical bookkeeping automatically rather than requiring it to be built by hand |
| **Jupyter/R Markdown notebooks for reproducible analysis pipelines** | Directly supports `06` §1.1–1.3's reproducibility discipline — an analysis pipeline published as an executable, re-runnable notebook (rather than a static results table) is a concrete, low-friction way to meet the artifact-evaluation bar `06` §1.2 describes |
| **DVC (Data Version Control) or similar data-versioning tools** | Directly supports `06` §1.3's data-preservation point for raw evaluation results, not just summary statistics |

Part `08` (`08-designing-a-world-class-prng-evaluation-pipeline.md`), the report's capstone, now pulls every thread from this report and its five companions into one concrete, ordered, actionable evaluation pipeline for a brand-new portable PRNG.
