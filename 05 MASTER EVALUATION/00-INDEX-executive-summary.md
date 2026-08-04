# Evaluating Cryptographic Primitives Rigorously

**Query:** "How should a modern cryptographic primitive be rigorously evaluated before anyone claims it is secure or superior?" — evaluation methodology, measurement science, statistical rigor, reproducibility, and, as the capstone, a concrete evaluation pipeline for a new portable PRNG.
**Report date:** August 4, 2026

---

## Read this before the rest — this is the sixth report in this line, and it says so honestly

You now have five other reports in this conversation's file history, and this one overlaps with several of them more than any prior addition has. Specifically: companion ARX report `03` already covers every diffusion metric in exhaustive technical depth; `05` already covers MILP/SAT and formal evaluation; `06` already covers PractRand/TestU01/Dieharder/NIST-STS in full, including their blind spots; `07` already covers benchmark methodology in full. Companion design-process report `03` already covers the evidentiary escalation ladder that decides when a designer stops iterating, and `04` covers reviewer expectations. Companion cryptanalyst-mindset report `02` already reconstructs an evaluation-style workflow (there, framed around finding weaknesses; here, framed around establishing confidence), and `07` already covers failed research and reviewer perspective on attack claims specifically.

**This report does not re-derive any of that, and says so explicitly at the top of every file where it applies rather than pretending otherwise.** What it adds that is genuinely, substantively new across all six reports in this line: **(1) an explicit statistical-methodology layer** — confidence intervals, power analysis, multiple-comparisons/false-discovery correction, effect size — applied specifically to cryptographic testing contexts (diffusion metrics, randomness test suites, benchmark variance, trail-probability estimates), which none of the prior reports built out in this form even though several of them use these tests; **(2) a sharpened epistemic vocabulary** distinguishing measurement, evidence, proof, confidence, and security as five genuinely different things routinely conflated in casual discussion of "is this secure"; and **(3) a single, concrete, PRNG-specific evaluation pipeline** (`08`) that pulls every thread from all six reports in this conversation into one actionable answer to your stated long-term goal — not a general "how to evaluate any primitive" restatement, but a pipeline built and ordered specifically for a portable PRNG, with an explicit sufficient-versus-insufficient evidence line drawn at the end.

---

## How this report is organized

| File | Covers | Relationship to companions |
|---|---|---|
| `00-INDEX-executive-summary.md` | This file | — |
| `01-philosophy-of-evaluation-and-the-epistemic-hierarchy.md` | Why evaluation exists, and the genuine differences between measurement, evidence, proof, confidence, and security | New conceptual material |
| `02-the-evaluation-pipeline-and-controls.md` | The full evaluator's workflow, and positive/negative controls done rigorously | Reframes cryptanalyst-mindset report `02`'s workflow around confidence-building rather than weakness-hunting; genuinely new control-methodology depth |
| `03-statistical-methodology-for-cryptographic-evaluation.md` | Sampling, confidence intervals, power analysis, multiple comparisons, false discovery, applied to crypto testing specifically | Entirely new — the report's core original contribution |
| `04-diffusion-and-randomness-evaluation-with-statistical-rigor.md` | The statistical-rigor layer on top of diffusion metrics and randomness test suites | Cross-references companion ARX report `03` and `06` heavily; adds only the rigor layer |
| `05-benchmark-methodology-and-formal-evaluation-with-rigor.md` | The statistical-rigor layer on top of benchmarking and formal/provable-security evaluation | Cross-references companion ARX report `05` and `07` heavily; adds only the rigor layer |
| `06-reproducibility-evaluation-failures-and-modern-frameworks.md` | Reproducibility as a formal discipline, a catalog of evaluation failures, and how NIST/CAESAR/PQC/SHA-3 actually select finalists | Cross-references companion history report and cryptanalyst-mindset report `07` |
| `07-research-opportunities-and-software-ecosystem.md` | Open problems specifically in evaluation methodology, and the tool landscape | Cross-references companion ARX report `09` and cryptanalyst-mindset report `06` |
| `08-designing-a-world-class-prng-evaluation-pipeline.md` | The capstone — a concrete, ordered, actionable evaluation pipeline for a brand-new portable PRNG specifically | Synthesizes all six reports in this conversation into one PRNG-specific deliverable |

---

## Executive Summary

The central philosophical finding, developed in full in `01`: **"secure" is not a measurement outcome, and treating it as one is the single most common category error this entire report catalogs.** A test suite produces a *measurement* (this PRNG's output passed BigCrush; this round function's SAC deviation is 0.0003). A cryptanalytic evaluation produces *evidence* (no MILP-found trail below weight 128 exists for this round count, under stated assumptions). A reduction produces *proof*, but only of a conditional statement (if this hard problem is hard, this construction is secure) — never of security in an absolute sense. What a design team, a reviewer, and eventually a user actually have, after all of this, is *confidence* — a calibrated, always-revisable epistemic state built from measurement, evidence, and proof combined, never a certainty. Keeping these five words — measurement, evidence, proof, confidence, security — genuinely distinct, rather than treating them as synonyms of increasing strength, is this report's single most load-bearing idea, and it recurs in every subsequent file as the test against which a specific piece of evaluation practice is judged: does this practice produce a measurement being mistaken for evidence, or evidence being mistaken for proof, or proof being mistaken for a guarantee of "security" the proof never actually claimed?

The second finding, and the report's core original technical contribution (`03`): **cryptographic evaluation has a statistical-rigor gap that is under-discussed relative to its actual consequences.** A SAC dependency-matrix entry computed from 2^16 random trials has a specific, computable confidence interval around it — and that interval is very rarely reported in practice, meaning a design that "looks" SAC-compliant may simply have been under-sampled relative to the deviation size that would actually matter. A randomness-test-suite run against many output streams, or a diffusion check against many bit-position pairs, is a **multiple-comparisons problem** in the exact statistical sense that field has a name and correction techniques for — run enough independent tests and some will show a "significant" deviation by chance alone, and a paper reporting "we found no anomalies in N tests" without correcting for N is making a weaker claim than it appears to be making. None of this is exotic statistics; it is the same experimental-design discipline biology, psychology, and physics have spent decades formalizing specifically because their fields learned, repeatedly and expensively, what happens when it's skipped — and cryptographic evaluation, per this report's synthesis, has been slower to import it explicitly than the stakes probably warrant.

The third finding, delivered concretely in `08`: **a genuinely sufficient PRNG evaluation pipeline is a specific, orderable, and — this report argues — currently under-specified-in-the-literature sequence**, and this report's capstone commits to stating exactly what that sequence is, what counts as sufficient evidence at each stage, and what would still be insufficient even after every stage is cleared — a level of concrete, PRNG-specific commitment that companion documents in this conversation, focused on ciphers and permutations generally, did not attempt with this specific target.

---

## A note on scope, stated once and applied throughout

Given the extent of prior coverage in this conversation, every file below opens with an explicit statement of what it assumes from its companions and does not re-derive. This is a deliberate choice to keep this report's actual length proportionate to its actual new content, rather than padding it back up to match its five predecessors' size through repetition — a discipline this report considers part of practicing the reproducibility and honesty standards it argues for in `06` and `09`.

Part `01` begins with the philosophical foundation the rest of the report is built on.
