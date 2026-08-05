# 01 — Research Strategy and Problem Selection

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map.*

---

## 1. Interesting Versus Important — the First Distinction

### 1.1 What each word actually means, used precisely

**Interesting** describes a problem's pull on the researcher's own curiosity — it's a fact about the researcher, not the field. **Important** describes a problem's actual consequence for the field if solved — a fact about the field, largely independent of who's asking. These are correlated, sometimes strongly, but they are not the same axis, and every case study this report examines in `07` shows successful multi-year programs built on repeatedly, deliberately checking one against the other rather than trusting that personal fascination reliably tracks field-level importance.

### 1.2 Why conflating them is the specific, recurring mistake

A researcher who only follows what's personally interesting risks accumulating a body of work that's technically impressive and field-irrelevant — a novel cryptanalytic technique applied to a toy cipher no one deploys, elegant but answering a question the field wasn't actually asking. A researcher who only chases what's institutionally "important" (whatever a funding body or advisor currently prioritizes) without any genuine personal engagement risks the opposite failure: technically competent but uninspired work, more likely to be abandoned at the first real difficulty because there's no intrinsic motivation carrying it through. **The actual skill, reconstructed from this report's case studies**: hold both questions open simultaneously and look specifically for their intersection — a problem you're genuinely curious about *and* can articulate, in one sentence, why the field needs answered.

### 1.3 A concrete test for "important," borrowed and generalized from this conversation's companion reports

Companion cryptanalyst-mindset report `01` §1's independence-claim-hunting framework gives a genuinely portable test: **a problem is important if it targets a load-bearing assumption something else depends on** — companion evaluation-methodology report `03`'s entire statistical-rigor contribution is important precisely because confidence intervals and multiple-comparisons correction are load-bearing for every diffusion and randomness claim the field makes, not because statistics is intrinsically more exciting than cryptanalysis. Applying this test to a candidate problem before committing time to it — *what, specifically, does this problem being solved change about what the field can subsequently trust* — is a cheap, early, high-value check this report recommends running explicitly, in writing, before any other planning step.

## 2. Estimating Research Value

### 2.1 Value as a function of three things, not one

Restating and generalizing companion research-platform report `02` §2.6's multi-objective framing at the level of problem selection itself: a candidate research direction's value is a function of **(a) how consequential the answer would be if found** (§1.3's test), **(b) how much the field currently lacks any answer at all** (a well-mapped, heavily-worked area yields diminishing returns per additional researcher-hour even for an important question — companion ARX report `09` §1's ranked-opportunities table is explicitly built around exactly this "novelty" axis for this reason), and **(c) how tractable a genuine answer actually is given available time, tools, and expertise**. A problem strong on (a) and (b) but weak on (c) is a bet, not a plan — worth naming explicitly as a distinct category from a problem strong on all three, since a mature research strategy usually includes some deliberate mix of both, not exclusively one or the other.

### 2.2 Estimating tractability specifically

The single most reliable, portable heuristic this report's case studies (`07`) support: **can you state, concretely, what a first, small, genuinely completable milestone toward this problem looks like, reachable within weeks rather than years?** A problem where no such milestone is nameable is either not yet well-enough understood to start (a scoping problem, addressed in `02`) or is being approached at the wrong grain (too large a bite, needing decomposition before it's a startable project at all).

## 3. Novelty Versus Feasibility

### 3.1 The tradeoff, stated as a genuine curve, not a binary choice

Restating companion design-process report `01` §3's novelty-evaluation discipline at the strategic-planning level: **novelty should be scarce and deliberate**, concentrated at one carefully-chosen point in a research program rather than spread across every dimension simultaneously — a program attempting a structurally novel cipher design *and* a novel proof technique *and* a novel evaluation methodology all at once is not more ambitious in a way the field rewards; per companion design-process report `08` §3.5, it's harder to review and harder to trust, because there's no independently-verified anchor point for a reader (or the researcher's own future self, revisiting the work after a setback) to reason from. **The strategic version of this lesson**: pick the one axis where genuine novelty is the actual point of the program, and deliberately hold every other axis as conventional, well-understood, and independently reusable as possible — directly mirroring companion research-platform report `01` §1.2's finding that mature *evaluation* tooling should simply be integrated, not reinvented, so that a program's genuinely novel effort (its *generation*-side contribution, in that report's framing) isn't diluted across territory that's already solved.

### 3.2 Feasibility as an honest, revisited estimate, not a one-time gate

A feasibility estimate made once, at the start, and never revisited is a known failure mode this report's `03` develops in full — feasibility should be re-estimated at every planned milestone (`02` §3), because new information (a cheap early check that came back worse than expected, a newly-published paper that changes the landscape) genuinely changes the honest answer, and a program that only assessed feasibility once is, structurally, unable to notice when its own premise has quietly become outdated.

## 4. How Successful Researchers Actually Balance These, Synthesized

Restating this file's three sections as a single, usable sequence: **(1)** generate candidate problems from genuine curiosity, without initially filtering for importance — this report's case studies consistently show the *best* eventual programs starting from real personal fascination, not a cold importance calculation alone; **(2)** apply §1.3's load-bearing-assumption test to each candidate, in writing, before committing further time; **(3)** for candidates surviving that filter, apply §2.1's three-factor value estimate and §2.2's concrete-first-milestone tractability check; **(4)** for the surviving candidate (or small set of candidates) a researcher actually commits to, apply §3.1's novelty-concentration discipline explicitly when scoping the program's actual technical plan. This sequence is not a one-time gate run before Part `02`'s planning begins — per §3.2, it should be revisited at every subsequent milestone, which `02` develops directly.

Part `02` (`02-hypothesis-formation-and-research-planning.md`) now covers what happens once a problem clears this filter: turning it into a testable hypothesis and a genuine multi-year plan.
