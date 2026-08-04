# 07 — Automatic Design and Synthesis

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`06`. Cross-references companion ARX report `05` (automated cryptanalysis tooling) and `09` §3.4 (future-of-the-field projection) — this file's distinct contribution is separating, precisely, what current tooling actually *generates* versus what it only *evaluates*, since that distinction is the single most important thing a designer needs to understand correctly before deciding how much of their own design process to delegate.*

---

## 1. The Distinction That Matters Most: Evaluation Versus Generation

Nearly every piece of "automated design" tooling that actually exists and is actually used in current (2026) practice is, precisely, an **evaluation** tool — given a candidate round function or rotation schedule a human already proposed, it efficiently and rigorously checks that candidate's properties (companion ARX report `05`'s full MILP/SAT treatment). **Genuine generation** — a tool that proposes the candidate in the first place, without a human having already sketched its structure — is a categorically different, and much less mature, capability, and conflating the two is the single most common misunderstanding about "AI-assisted cryptography design" this report can correct directly. Every subsequent section in this file is organized around being explicit about which side of this line a given technique actually sits on.

## 2. What Genuinely Exists and Works, as of 2026

### 2.1 Automatic parameter tuning (generation, narrowly scoped)

**Rotation-constant search** (companion ARX report `04` §1.2, this report's `03` §1) is the one area where genuine, unsupervised *generation* — not just evaluation — is real, mature, and routinely used: given a fixed round-function structure and topology, MILP/SAT-guided and evolutionary search can propose rotation-constant candidates, not merely check human-proposed ones, and this has been true since well before 2020. **The scope limit, stated precisely**: this generation capability operates over a *narrow* parameter space (rotation amounts, given everything else fixed), not over the full design space (round-function structure, topology, state layout) simultaneously — a designer can genuinely delegate rotation-constant selection to a solver today, in a way they categorically cannot yet delegate "invent a good round function" or "invent a good topology."

### 2.2 MILP/SAT-guided optimization (evaluation, mature; light generation, emerging)

Companion ARX report `05` covers this in full technical depth. The generation-relevant addition worth making explicit here: **the Window Heuristic and CLAASP-style frameworks (companion ARX report `05` §1.6) are beginning to blur the evaluation/generation line slightly**, in that a designer can now pose "search over a family of related round-function variants (differing in, say, which words feed which additions) and report which variant gives the best provable bound" as a single automated query — a genuine step toward generation, but still operating over a human-defined *family* of candidates, not inventing a structurally novel family from nothing.

### 2.3 Evolutionary and genetic search (generation, real but shallow)

Real, used, and genuinely capable of *generating* novel rotation schedules, S-box candidates (for SPN-adjacent design), and simple round-function variants by mutating and recombining a population of candidates against a fitness function (typically a fast proxy — diffusion-speed estimation, per this report's `02` §2.1 second elimination pass — rather than full cryptanalytic evaluation, for tractability). **The honest limitation**: evolutionary search's fitness landscape is only as good as the proxy fitness function it's given, and per this report's `03` §3.1 evaluation-ladder discussion, a candidate that scores well on a cheap proxy (diffusion speed) still needs to survive the full, expensive evaluation ladder before it means anything — evolutionary search is a generator of *candidates worth then evaluating properly*, not a substitute for that evaluation, and treating its output as pre-validated is a real, avoidable mistake.

### 2.4 SAT-guided synthesis (evaluation-dominant; generation exists for small, well-specified objects)

For small, precisely-specified objects (a Boolean function meeting stated nonlinearity/differential-uniformity targets, an S-box satisfying a stated property list), SAT solvers can genuinely synthesize — not just check — a satisfying instance, and this is real, established practice for classical S-box design specifically (companion history report `06`'s nonlinearity-metric thread). **For a full round function or permutation**, the search space is currently too large for direct SAT synthesis to generate a complete, novel structure from a bare specification of desired properties — this remains, as of 2026, squarely on the evaluation side of the line for anything beyond component-scale objects.

## 3. What Is Real But Immature, and Genuinely Emerging

### 3.1 Automatic topology search

Directly restating this report's own `02` §3.4 finding, because it's this file's most load-bearing "immature, not absent" example: current tooling can efficiently *evaluate* a human-proposed topology's connectivity/diffusion properties (via the state-graph framing in `02` §3.3), but **generating candidate topologies from scratch, rather than checking human-sketched ones, remains a comparatively underexplored research direction** as of this report's writing, per companion ARX report `09` §3.4's explicit flag. A designer today should expect to invent their own topology by hand, using the column/row/diagonal/butterfly vocabulary (`02` §3.1–3.2) as their working toolkit, and only then hand it to automated evaluation — not the reverse.

### 3.2 Neural-guided search and design

Distinct from differential-neural *cryptanalysis* (a mature, well-established attack technique, companion ARX report `04` §2.10) is the much less mature question of using a trained model to *guide* design-space search — prioritizing which regions of a large combinatorial design space (round-function variants, topology candidates) are worth a human's or a solver's further attention, rather than exhaustively or randomly searching. **What exists**: the general combinatorial-optimization literature has real precedent for neural-guided search accelerating solver performance in adjacent domains (satisfiability solving, circuit design); direct application specifically to novel ARX-permutation *design* (as opposed to cryptanalysis of existing designs) is, per companion ARX report `09` §3.4, thin in the current published literature — real as a plausible near-term direction, not yet an established practice with a track record comparable to §2.1–2.3's tools.

## 4. What Is Currently Aspirational, Stated Honestly

**Fully autonomous, end-to-end primitive generation** — a system that takes `01` §4's requirements list as input and produces a complete, novel permutation specification, already evaluated and margined, with no human involvement in the round-function-invention or topology-invention stages — does not exist as an established, trusted practice as of this report's writing. This is worth stating with the same directness this conversation's companion documents apply to the July 2026 Anthropic HAWK/AES event (companion ARX report `09` §3.3, companion history report `05`): that event demonstrated substantially autonomous AI-conducted *cryptanalysis* — finding a weakness in an already-existing, human-designed candidate — which is a different, and currently more mature, capability than autonomous *design* from a blank requirements sheet. The two capabilities are related (an AI system capable of finding subtle structural weaknesses in existing designs is plausibly also capable, eventually, of avoiding those same weaknesses when generating new ones) but are not the same demonstrated capability, and this report declines to claim more continuity between them than the current evidence actually supports.

## 5. What a Designer Should Actually Delegate Today, and What They Shouldn't

Synthesizing §§2–4 into direct, practical guidance for someone in your stated position (wanting to eventually design their own permutation and PRNG):

| Design-process stage (per `01`–`03`) | Delegate to automated tooling today? |
|---|---|
| Requirements engineering (`01` §4) | No — this requires human judgment about your actual deployment context and goals |
| Architecture choice (`01` §5) | No — requires matching requirements against tradeoffs no current tool reasons about holistically |
| State/word size/lane count (`02` §1) | Partially — automated evaluation can confirm a chosen size's margin, but the initial choice is still a human requirements-matching decision |
| Round function invention (`02` §2) | No — invent by hand using the elimination-pass process (`02` §2.1); evaluate candidates automatically once sketched |
| Topology invention (`02` §3) | No, per §3.1 above — invent by hand, evaluate automatically |
| Rotation-constant search (`03` §1) | **Yes** — this is the one stage where genuine automated generation, not just evaluation, is mature and should be used rather than hand-derived |
| Differential/linear/RX trail search (`03` §3.1, rungs 4–5) | **Yes, mandatory** — companion ARX report `05`'s entire tooling apparatus exists for exactly this, and hand-derivation alone is no longer adequate evidence (companion ARX report `05` §1.7) |
| Diffusion-metric computation (`03` §3.1, rung 2) | Yes — cheap, mechanical, no reason to do by hand |
| Formal security-proof construction (companion ARX report `05` §3) | Partially — proof *assistants* (Coq/Lean/Isabelle) can check a proof once constructed, but constructing the proof's argument is still substantially a human activity for anything beyond routine cases |

Part `08` (`08-checklists-decision-trees-and-bibliography.md`), the final file in this report, consolidates everything from `01`–`07` into the actual, usable design workflow, decision trees, and checklists your original prompt's deliverables list requested.
