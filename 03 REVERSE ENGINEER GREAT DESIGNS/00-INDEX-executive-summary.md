# How Expert Cryptographers Actually Design New Primitives

**Query:** "How do expert cryptographers actually design new cryptographic primitives? Not how they implement or explain them — how they invent, think, evaluate, and know when a design is mature."
**Report date:** August 2, 2026

---

## Read this before the rest — how this report relates to its companions

You now have three other reports from this conversation sitting in the same file history: a general cryptography survey, an ARX-specific deep dive (design, cryptanalysis, tooling, reviewer expectations, a ~100-paper roadmap), and a chronological intellectual history (1993–2026). Between them, they already contain the *factual bedrock* this report would otherwise need to re-derive: what ChaCha's quarter-round actually is, what Sparkle's long-trail strategy proves, what a CT-RSA reviewer expects to see, which papers matter and when they appeared.

**This report does not re-derive that bedrock. It stands on it and asks a different question.** Where the ARX-specific report answers "what is the current state of ARX design and cryptanalysis," this report answers "what was the *designer's own reasoning process*, at the moment of decision, that produced that state" — the difference between a field guide to a landscape and a reconstruction of the surveying expedition that mapped it. Every case study, every rotation-constant discussion, every reviewer-expectation section in this report is written from *inside* the designer's head at the decision point, not from the finished artifact backward — and where the factual grounding is identical to a companion report, this report says so and points there, rather than repeating it, so that what you're reading here is genuinely new synthesis, not padding.

---

## How this report is organized

| File | Answers |
|---|---|
| `00-INDEX-executive-summary.md` | This file |
| `01-beginning-with-nothing-requirements-and-architecture-choice.md` | How a designer starts from an unstated need, turns it into explicit requirements, and picks ARX vs. SPN vs. sponge vs. counter-based *before* any round function exists |
| `02-state-topology-and-round-function-invention.md` | How state size/word size/lane count get chosen, how a round function is actually invented and candidates rejected, and how topology (columns/diagonals/butterfly networks) gets designed |
| `03-rotation-constants-performance-and-the-design-evaluation-loop.md` | How rotation constants are actually searched (not just what the search methods are), how performance and security get traded off in real time during design, and what evidence makes a designer keep going versus abandon a candidate |
| `04-reviewer-expectations-and-design-mistakes.md` | What a reviewer's red flags actually look like from the *designer's* side of the table, and the specific, named mistakes that kill otherwise-promising independent designs |
| `05-case-studies-reverse-engineered.md` | The actual reconstructed decision sequence — not just the outcome — behind ChaCha, Salsa20, BLAKE2/3, Ascon, Xoodoo, Sparkle, SipHash, Threefry, Philox, and NORX |
| `06-design-philosophies-of-major-designers.md` | How Bernstein, Daemen, Rijmen, Rogaway, Aumasson, and Pornin each actually think, where they agree, and where they genuinely disagree |
| `07-automatic-design-and-synthesis.md` | The current real state of AI-assisted, evolutionary, and solver-guided primitive design — what exists, what's still aspirational |
| `08-checklists-decision-trees-and-bibliography.md` | The consolidated design workflow, decision trees, checklists, and comparison tables you can actually use when you sit down to design something |

---

## Executive Summary — what expert design actually looks like, compressed

The single biggest correction this report makes to how design is usually taught: **a new cryptographic primitive is not invented by having an idea and then testing whether it's secure.** Every reconstructed case study in this report (`05`) shows the same inverted order — the designer starts from a **specific, narrow, often almost embarrassingly concrete problem** (Bernstein needed something that wouldn't leak through cache timing on software-only AES; the Keccak team needed a hash function structurally immune to Merkle–Damgård's length-extension weakness; the Sparkle team needed to prove, not just claim, an ARX design could match AES's wide-trail-strategy rigor) and the *entire subsequent design process* — architecture choice, state size, round function, rotation constants — is a sequence of decisions each one made to specifically serve that starting problem, with security evaluation running *continuously alongside* every decision, not applied once at the end. Security evaluation is not a phase; it is the medium the whole design conversation happens in. A designer who treats "design it, then test it" as two sequential steps has already made the report's most commonly cited mistake (`04`) before writing a single line of specification.

The second correction: **novelty is not the goal, and elegance is not evidence.** Every design philosophy profiled in `06` — however much Bernstein, Daemen, and Aumasson otherwise disagree — converges on treating "this is a genuinely new idea" as, at best, neutral and, more often, a yellow flag demanding *extra* scrutiny, not a mark in the design's favor; the field's actual reward structure (traced in this conversation's companion history report) pays out for designs that survive scrutiny, and a design's only route to survival is being boring enough, in its individual moving parts, that reviewers and independent cryptanalysts can actually reason about it. Sparkle/Alzette's long-trail strategy (`05`, `06`) is this report's clearest illustration: its *methodological* contribution — a way to prove a bound, not merely assert one — is more valued by the field than the specific permutation it was demonstrated on, and this is not an accident of taste; it's what "mature" means in this field, operationalized (`03`'s "how designers know a design is ready" section is built entirely around this).

The third: **the design process has a genuine, repeatable shape**, even though no two designers narrate it identically. Every case study in `05`, reconstructed as faithfully as the historical record allows, passes through the same sequence: an unstated need becomes an explicit requirement (`01`); the requirement rules out entire architecture families before any specific design exists (`01`); a state size and word size get chosen as a joint function of the target platform's register/SIMD width and the desired security margin (`02`); a round function gets *invented* through a process this report reconstructs as closer to constrained brainstorming-and-elimination than sudden insight (`02`); rotation constants and topology get searched, not guessed, using tools that escalate from hand-reasoning to automated search as the design matures (`02`–`03`); and the whole thing is subjected to continuous, escalating security evaluation whose *specific* evidentiary bar (`03`–`04`) determines when the designer stops iterating and starts writing the paper. Internalizing this shape — not memorizing any specific cipher's rotation schedule — is what "thinking like a designer rather than a student" (your stated final objective) actually means.

---

## A note on confidence and sourcing

Reconstructing a *thought process* rather than reporting a *finished artifact* is inherently less certain than the fact-reporting this conversation's other reports do — designers' own retrospective accounts (talks, interviews, design-rationale sections of papers) are the primary evidence this report relies on, and retrospective accounts of one's own reasoning are known, in general, to smooth over false starts and present a cleaner narrative than the messier real-time process actually was. Where this report reconstructs a decision sequence from a designer's own published design-rationale prose (Bernstein's Salsa20/ChaCha notes, the Sparkle/Alzette paper's explicit methodology framing, Ascon's submission documentation), confidence is high — these are close to primary testimony. Where this report infers a likely reasoning process from the *pattern* of decisions visible in a specification, absent explicit designer commentary, that inference is flagged as such rather than presented with equal confidence, per this conversation's established practice of not overstating certainty.

Part `01` begins with the actual starting point: how a designer gets from an unstated need to a design brief.
