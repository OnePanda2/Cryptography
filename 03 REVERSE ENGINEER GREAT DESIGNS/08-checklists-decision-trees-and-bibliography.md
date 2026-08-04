# 08 — Checklists, Decision Trees & Bibliography

*Final file of this report — see `00-INDEX-executive-summary.md` for the map. This file consolidates `01`–`07` into a working reference you can actually use at your desk; it does not re-argue anything already argued in the earlier files, only restates conclusions in checklist/tree form.*

---

## 1. The Design Workflow, End to End

```
1. IDENTIFY THE UNSTATED NEED (`01` §1)
   → State it as a specific, dated, external pressure, not an ambition.
   → If you can't name the pressure in one sentence, stop here.

2. FORM A FALSIFIABLE HYPOTHESIS (`01` §2)
   → Name the mechanism, the property it buys, and the cost you're testing against.
   → If you can't state what evidence would make you abandon this direction, it's not falsifiable yet.

3. REQUIREMENTS ENGINEERING (`01` §4)
   → Security goal (number + adversary model)
   → Performance goal (relative to a named comparison point)
   → Threat model, INCLUDING what's explicitly out of scope
   → Target platform(s), named concretely
   → Portability / SIMD-width requirements
   → Memory/footprint limits, in concrete units
   → Auditability/implementation-simplicity requirement
   → Future-proofing, stated as a margin number, not a promise
   → All eight answered in writing before step 4.

4. ARCHITECTURE CHOICE (`01` §5)
   → Run the requirements from step 3 against the ARX / AND-RX / SPN / sponge / counter-based decision procedure.
   → Verify: does the architecture follow from the requirements, or did you pick a favorite first? (`04` §2.1 self-check)

5. STATE DESIGN (`02` §1)
   → Negotiate state size against security headroom, performance cost, platform fit — don't resolve to one number if multiple targets warrant multiple variants.
   → Word size = narrowest native register width across target platforms, unless a stated reason argues wider.
   → Lane count divisible into common SIMD register widths — check this explicitly, don't discover it later.

6. ROUND FUNCTION INVENTION (`02` §2)
   → Generate several structurally-different candidates.
   → Elimination pass 1: structural fit to chosen architecture (cheap, fast)
   → Elimination pass 2: rough diffusion-speed estimate (cheap)
   → Elimination pass 3: rough performance estimate vs. step 3's goal (cheap)
   → Elimination pass 4: full MILP/SAT trail search — survivors only (expensive)
   → Confusion (nonlinearity) and diffusion should be delivered by structurally distinct steps, not entangled.
   → Decide constant-injection and key-injection points; generate constants via a reproducible, auditable procedure.

7. TOPOLOGY DESIGN (`02` §3)
   → Sketch groupings (columns/rows/diagonals, or butterfly/shuffle for wider states) as a state-dependency graph.
   → Minimize graph diameter for your complexity budget.
   → Invent by hand; evaluate automatically once sketched (no mature generation tooling exists yet, `07` §3.1).

8. ROTATION-CONSTANT SEARCH (`03` §1)
   → Rank objectives: diffusion speed first, rotational/RX-resistance as a filtering constraint.
   → Prune the naive search space using equivalence-class awareness before searching.
   → Escalate: hand-reasoning → quick script → MILP/SAT-assisted → automated RX-check, in that order.
   → This IS a stage you should delegate to automated tooling (`07` §5 table).

9. PERFORMANCE ENGINEERING (`03` §2) — CONTINUOUS, not a final step
   → Sketch dependency chains before writing code.
   → Check register pressure and carry-chain length jointly.
   → Structure operations for auto-vectorization from the earliest candidate stage.

10. SECURITY EVALUATION LADDER (`03` §3.1) — run against every surviving candidate, cheapest first
    → Rung 1: structural/architectural fit
    → Rung 2: diffusion metrics (SAC/BIC/avalanche) — necessary, NOT sufficient
    → Rung 3: rough hand-derived differential/linear estimate
    → Rung 4: full MILP/SAT automated trail search (mandatory for publication-grade work)
    → Rung 5: RX-cryptanalysis, differential-linear/PNB combined attacks, differential-neural evaluation where relevant
    → At every rung: is the margin comfortable, or barely sufficient? Barely-sufficient survivors should worry you.

11. MATURITY CHECK (`03` §3.4)
    → Have you cleared every rung with stated, comfortable margin?
    → Does your reference implementation independently verify the spec's own claims?
    → Is the only remaining scrutiny external (peer review) rather than internal (more of your own testing)?
    → If yes to all three: you're ready to write it up. If no: you know exactly which step to revisit.

12. WRITE-UP AND SUBMISSION (`04` §1)
    → Identify your independently-defensible contribution beyond "I designed something" — ideally decided back at step 3, not now.
    → Run the reviewer-anticipation checklist (§4 below) BEFORE submitting, not after a rejection.
```

---

## 2. Decision Tree: Choosing an Architecture

```
Does the target platform mix lack dedicated symmetric-cipher hardware acceleration
(mobile / embedded / IoT), AND is constant-time-by-construction a priority?
├── YES → Is raw hardware gate-count/footprint the dominant constraint (not general
│         software throughput)?
│         ├── YES → AND-RX (Simon/Simeck-style)
│         └── NO  → ARX (Salsa20/ChaCha/Speck/LEA-style)
└── NO  → Do you have (or can build) expertise to design/characterize a nonlinear
          component with near-optimal, jointly-tuned properties?
          ├── YES → Do you need one primitive to flexibly serve multiple roles
          │         (hash + MAC + AEAD + PRNG from one permutation), or is
          │         Merkle-Damgård's length-extension specifically unacceptable?
          │         ├── YES → Sponge/duplex construction (SPN-style permutation, Keccak/Ascon-style)
          │         └── NO  → Classical SPN (AES-style)
          └── NO  → Default to ARX unless a specific, stated reason argues otherwise
                    (ARX's "derive nonlinearity from addition" avoids needing
                    S-box-design expertise, at the cost of less classically-clean bounds)

[Separate branch, orthogonal to the above:]
Does the actual requirement include embarrassing parallelism / trivial seek-ahead
(any output computable independently, in any order)?
├── YES → Counter-based construction (Threefry/Philox-style) — this REQUIREMENT
│         categorically rules out sequential stream-cipher-derived designs,
│         regardless of what the tree above concluded for the underlying primitive.
└── NO  → Proceed with whatever the tree above concluded.
```

---

## 3. Comparison Tables

### 3.1 Architecture family comparison

| Family | Nonlinearity source | Best-fit platform | Provable-bound maturity | Canonical case study (`05`) |
|---|---|---|---|---|
| ARX | Modular addition carry chain | Software, no dedicated hardware | Mature since 2020 (long-trail strategy) | ChaCha, Salsa20 |
| AND-RX | Bitwise AND | Hardware-constrained | Shares ARX's general maturity level | Simon, Xoodoo |
| SPN (designed S-box) | Fixed nonlinear substitution | General-purpose, hardware-accelerable | Mature since 2001 (wide-trail strategy) | AES |
| SPN-sponge hybrid | Fixed S-box within sponge/duplex | Lightweight, flexible-mode needs | Mature (indifferentiability framework) | Ascon |
| Counter-based | Varies (addition for Threefry, multiplication for Philox) | Massively parallel/GPU | Comparatively thin adversarial scrutiny (`05`, open problem) | Threefry, Philox |

### 3.2 Topology comparison

| Topology | Diffusion speed | Analytical tractability | Hardware/wiring cost |
|---|---|---|---|
| Column-then-row | Moderate | High (easy to hand-reason) | Low |
| Column-then-diagonal | Faster than column-then-row | High | Low |
| Butterfly network | Fast, provably log-depth | Lower (harder to hand-reason) | Moderate |
| Shuffle layer | Design-dependent | Lower | Often hardware-friendly |

### 3.3 State-size tradeoff comparison

| State size class | Generic-attack headroom | Per-round cost | Typical target |
|---|---|---|---|
| ~256-bit | Adequate for 128-bit-class security with less margin | Lowest | Lightweight/embedded |
| ~384-bit | More margin | Moderate | Mid-range lightweight |
| ~512-bit | Comfortable margin | Higher | General-purpose |
| ~1024-bit+ | Large margin, sponge-scale | Highest | Sponge permutations targeting high security levels across multiple modes |

### 3.4 Performance-vs-security tradeoff, stated as a single governing question

Every performance/security tradeoff this report has traced reduces to one question, worth keeping visible at every stage: **"does this change buy a performance gain large enough, and measured concretely enough, to justify the security-margin cost, also measured concretely, that it introduces — and has both sides of that ledger actually been measured, not estimated by feel?"** ChaCha's column/diagonal choice (`03` §2.4) passes this test with an explicit, documented answer; a design that can't produce both sides of this ledger on demand hasn't actually made the tradeoff deliberately, whatever its author believes.

---

## 4. The Reviewer-Anticipation Checklist

Restated in full from companion ARX report `08` §3.4, reframed here as a pre-submission self-audit rather than a reviewer's tool — run this against your own design before anyone else sees it:

- [ ] Full specification with reference implementation
- [ ] Diffusion-metric evaluation (SAC, BIC, avalanche, branch number)
- [ ] MILP or SAT-derived active-operation lower bounds and optimal-trail search, with stated margin
- [ ] Explicit differential-linear/PNB-style combined-attack evaluation
- [ ] Explicit rotational-XOR (RX) cryptanalysis evaluation
- [ ] Explicit consideration of boomerang/rectangle, impossible-differential, meet-in-the-middle attacks
- [ ] Explicit statement of the Markov-cipher-assumption's applicability and any sanity-check performed
- [ ] Differential-neural distinguisher evaluation, or explicit acknowledgment of the attack family's relevance
- [ ] Formal reduction to the ideal-permutation/indifferentiability framework for any wrapped mode, with a concrete-security bound
- [ ] Performance benchmarking with disclosed methodology (platform, compiler, optimization level, variance)
- [ ] Hardware footprint and energy figures, if lightweight/embedded-targeted
- [ ] For PRNG designs: forward-secrecy, backward-secrecy, and fork-safety treatment, plus separate internal-permutation cryptanalysis and output-stream statistical testing
- [ ] Published, reproducible artifacts (models, code, training pipelines if applicable)
- [ ] Explicit, honest discussion of open questions and unresolved margin
- [ ] Your independently-defensible contribution beyond the design itself, stated explicitly (`04` §1.3)

---

## 5. Design Anti-Patterns — Condensed

Restating this report's `04` §2 table in single-line form for quick reference:

1. Trusting diffusion metrics as security evidence (they're rung 2 of 5, not the finish line).
2. Comparing your optimized implementation against someone else's unoptimized one.
3. Testing against an unfalsifiable ambition rather than a scoped hypothesis.
4. Leaving your threat model implicit, especially what's out of scope.
5. Claiming novelty without checking equivalence-class collapse against prior art.
6. Treating randomness-test-suite passage as adversarial-cryptanalysis-grade evidence.
7. Accepting barely-sufficient margin at any evaluation rung instead of treating it as a warning.
8. Citing last decade's literature in a sub-field that's moved substantially since.
9. Asserting a provable bound by analogy instead of doing the bound-establishment work.
10. Choosing the architecture first and writing requirements to justify it afterward.

---

## 6. Annotated Bibliography — Design-Process-Specific Additions

This report's factual bibliography is almost entirely inherited from its companions — the companion ARX-specific report's `10` (a ~100-paper roadmap) and the companion history report's `09` (chronological reading order, top 50 milestones) together cover essentially every primary source relevant to this report's case studies and technical claims, and are not re-listed here. This section adds only what's specifically about *design process and philosophy*, not already centered in either companion document:

- **Daemen & Rijmen, *The Design of Rijndael*** — read here specifically for its methodology exposition, not its AES-specific results (companion history report `09`, entry #13, covers the latter).
- **Bernstein's public design-rationale notes for Salsa20 and ChaCha** (available directly via his own site) — the closest thing to primary-source testimony about real-time design reasoning this report relies on; read these directly rather than via any secondary paraphrase, including this report's own.
- **Aumasson, *Serious Cryptography*** (companion general-cryptography report `06` §4) — read here specifically for its expository-philosophy choices (what it includes, what it deliberately simplifies), as a window into Aumasson's teaching/communication priorities distinct from his design work.
- **Rogaway, "The Moral Character of Cryptographic Work"** — the primary source for `06`'s discussion of ethical requirements-engineering; read directly, as this report's paraphrase deliberately doesn't attempt to compress its actual argument.
- **The Sparkle/Alzette CRYPTO 2020 paper's methodology sections specifically** (as distinct from its results sections, already covered in companion ARX report `02` §2.12) — read here for the explicit, rare instance of a paper narrating its own design-and-proof-methodology co-development process in enough detail to reconstruct `03` §1.4's finding directly from the primary source.
- **Pornin's public technical writing on constant-time implementation and elliptic-curve edge cases** — no single canonical paper; read his sustained public technical-forum output as a corpus, the way this report's `06` treats it.

---

## 7. Suggested Software and Reference Implementations

Inherited in full from companion ARX report `09` §2 — CLAASP, CryptoSMT, the RustCrypto implementations, the Sparkle suite's own repositories, and SUPERCOP/eBASH for benchmarking are all directly relevant to the design workflow in §1 above, and are not re-listed here to avoid duplicating a reference you already have.

---

## 8. Open Problems, Specifically at the Design-Process Layer

Beyond companion ARX report `09` §1's ranked technical open problems (rotation-constant search above 64 bits, formal PRNG security proofs, generic differential-neural cryptanalysis for sponge-based permutations, and others), this report surfaces two open problems specifically about the *process* itself, not about any specific technical gap:

1. **Automatic topology *generation*** (`02` §3.4, `07` §3.1) remains the most significant capability gap between "what automated tooling can evaluate" and "what automated tooling can generate" in the entire workflow this report describes — closing it would be a genuine, high-value contribution, and is realistically tractable for a researcher with both graph-theoretic and cryptanalytic-tooling fluency, per the tractability framing your other companion report applies to comparable problems.
2. **A formal, general theory of "how much margin is enough"** — this report's `03` §3.2–3.3 describes the *practice* of margin-tracking (comfortable versus barely-sufficient survival at each evaluation rung) as it's actually done, but a rigorous, general, quantitative framework for exactly how much margin a given design's stated threat model and deployment lifetime actually warrant — as opposed to the current, largely convention-and-judgment-driven practice of "more rounds than the best known attack, by a comfortable amount" — remains, to this report's knowledge, unformalized, and would be a genuinely novel, defensible contribution for a designer more interested in the methodology layer (in the Daemen/Rijmen/Rogaway tradition profiled in `06`) than in a specific new permutation.

---

This closes the eight-file report on how expert cryptographers actually design new primitives. Return to `00-INDEX-executive-summary.md` for the map, or to your companion ARX-specific and chronological-history reports for the factual and historical depth this report has consistently pointed to rather than repeated.
