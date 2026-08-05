# How World-Class Cryptographers Make Research Decisions

**Query:** "How do world-class cryptographers consistently make good research decisions over multi-year research programs?" — not cryptography, not attacks, not mathematics, but the decision-making process itself: what to investigate, what to abandon, what evidence convinces, when to publish, when to stop.
**Report date:** August 5, 2026

---

## Read this before the rest — the eighth report, and what's actually left to say

You now have seven other reports in this conversation. Several of them already touch this report's territory from a technical angle: companion design-process report `03` covers the evidentiary escalation ladder deciding when a *design* is mature enough to stop iterating; companion evaluation-methodology report `01`–`03` covers the epistemic hierarchy (measurement, evidence, proof, confidence) and statistical discipline underlying what "sufficient evidence" actually means; companion cryptanalyst-mindset report `07` covers reviewer perspective and failed research. **This report does not re-derive any of that — it's explicitly not about the technical evidence itself, but about the human, strategic, and institutional judgment surrounding it**: how a problem gets chosen in the first place, how a multi-year program gets planned and kept honest, how a principal investigator leads a group, which venue a result belongs in, what research ethics actually requires beyond "don't lie," and — because this conversation's whole arc points toward you working largely alone — a dedicated, concretely-grounded chapter on what it actually takes to contribute as an independent researcher, including real, current, named funding sources and their actual status as of this report's writing.

This turn's research surfaced genuinely current, actionable material for that last chapter specifically: **NLnet Foundation**, a well-established (since 1997) funder of open-source cryptography and internet-infrastructure work — including, historically, WireGuard and post-quantum cryptography projects — has, as of mid-2026, **paused new submissions** while transitioning its NGI Zero program to a successor effort, with new calls expected to reopen September 3, 2026 (deadline November 3, 2026). This is exactly the kind of time-sensitive, must-be-current detail this report is careful to flag precisely rather than present as evergreen advice.

---

## How this report is organized

| File | Covers |
|---|---|
| `00-INDEX-executive-summary.md` | This file |
| `01-research-strategy-and-problem-selection.md` | How problems get chosen — interesting versus important, novelty versus feasibility, estimating value and risk before starting |
| `02-hypothesis-formation-and-research-planning.md` | Constructing testable hypotheses and planning multi-year programs — milestones, dependencies, anticipated dead ends |
| `03-the-decision-loop-continue-stop-and-avoiding-bias.md` | When to continue, when to stop, and the specific psychological failure modes (confirmation bias, sunk cost) that corrupt the decision |
| `04-research-risk-and-negative-results.md` | The full risk taxonomy your outline names, and when a negative result is itself a genuine contribution |
| `05-research-leadership-and-publication-strategy.md` | How principal investigators actually lead groups, and which venue a given result actually belongs in |
| `06-research-ethics-and-anti-patterns.md` | Research integrity as a dedicated topic, and the specific, named failure patterns that recur across independent research |
| `07-historical-case-studies-of-decision-making.md` | The AES/SHA-3/CAESAR/PQC/ChaCha/Ascon/BLAKE programs, reread specifically for their decision points, not their technical content |
| `08-independent-research-and-the-decision-framework.md` | The capstone — realistic guidance for an independent researcher specifically, including current funding sources, plus the consolidated decision-making framework |

---

## Executive Summary

The finding that organizes this entire report: **the single most consistent thing separating researchers who make good decisions over a multi-year program from those who don't is not intelligence, technique, or even domain knowledge — it's having decided, in writing, before starting, what evidence would make them stop.** This report's companion design-process document already established this at the level of a single design (`03` §3.4's maturity criteria); this report generalizes it to the level of an entire research career. A researcher who can state, before beginning, "I will abandon this direction if X happens" has done something a researcher who merely "feels" their way toward stopping has not: they've pre-committed to a falsification condition immune to the exact psychological pressures (sunk cost, motivated reasoning, the sheer discomfort of admitting eighteen months were spent on a dead end) that `03` documents as the actual, empirically-recurring reason good researchers stay on bad directions too long.

The second finding: **"interesting" and "important" are genuinely different axes, and conflating them is the most common problem-selection error this report's case studies (`07`) surface.** A problem can be intellectually fascinating and contribute nothing the field actually needs (a novel but practically irrelevant attack against an already-well-margined toy cipher); a problem can be unglamorous and genuinely important (companion evaluation-methodology report `03`'s statistical-rigor gap, or the disciplined, unglamorous work of independently replicating someone else's claimed result). Every researcher this report profiles who sustained a productive multi-year program did so by explicitly, repeatedly checking their own excitement against a second, separate question: does the field actually need this answered, independent of whether I personally find it fun to work on.

The third, and for your specific situation the most consequential: **independent research in cryptography is genuinely possible, genuinely happening right now, and genuinely harder in specific, nameable ways this report doesn't soften** — no institutional peer network providing the "breadth of scrutiny" companion cryptanalyst-mindset report `07` §1.3 identifies as what actually builds confidence in a result, no default access to the compute or funding companion research-platform report's Year-1 roadmap assumes, and a credibility-building problem every unaffiliated researcher in every field faces. `08` does not pretend these problems away; it names them and works through what a realistic, current answer to each one looks like.

Part `01` begins with the actual first decision every research program starts from: what to work on.
