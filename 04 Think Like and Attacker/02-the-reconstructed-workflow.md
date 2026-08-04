# 02 — The Reconstructed Workflow

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`'s mindset framing — this file turns it into an actual staged process with decision points, not just a description of what a technique does.*

---

## Why this workflow is a funnel, not a checklist

The stages below are not run exhaustively and independently against every candidate weakness; they're a **cost-escalating funnel**, directly mirroring the elimination-pass logic your companion design-process report describes from the design side (`02` §2.1 of that report) — cheap, fast checks first, narrowing to a small set of genuinely promising leads before anything expensive is invested. A cryptanalyst who runs full automated MILP search against every vague suspicion, rather than screening suspicions cheaply first, is not being more thorough — they're being inefficient in a way that experienced researchers specifically learn to avoid, because the field's actual constraint is researcher-hours, not compute.

## Stage 1 — Read the specification for its own stated assumptions

Before any analysis begins: read the specification specifically hunting for **every explicit or implicit claim the design's own security argument depends on** — per `01` §1, this is the actual target of the entire subsequent process. This includes the stated security level, the threat model the design claims to defend against (and, as importantly, what it explicitly does *not* claim to defend against — companion design-process report `01` §4's point about stating scope explicitly applies symmetrically to how a cryptanalyst reads that scope statement, treating anything left vague as worth clarifying before proceeding), and every place the specification's own security proof uses a simplifying assumption (independence, uniformity, a specific probabilistic model) to make an argument tractable.

**Decision point**: does the specification's own proof or rationale contain a load-bearing simplifying assumption that isn't independently justified? If yes, that assumption becomes the first, highest-priority candidate target for the rest of this workflow — this is exactly how TinyJambu's forgery attack (`04`) actually originated, per the discoverers' own stated framing.

## Stage 2 — Identify the threat model and scope the effort

Distinct from reading the design's *own* threat model (Stage 1): the cryptanalyst independently decides what threat model *they* are analyzing under, which may be broader than the design's stated claims (checking robustness beyond the claimed guarantee is itself informative, even when it isn't strictly "breaking" the design as specified) or narrower (focusing effort on the single most consequential attack surface — key recovery over a mere distinguisher, say — rather than spreading effort thin across every conceivable angle). **Decision point**: given limited time, is the highest-value target a full break (key recovery, forgery), a distinguisher (weaker, but often a necessary stepping stone and sometimes the actual, honest ceiling of what's achievable, per Keccak's zero-sum lineage, `04`), or a specific implementation/side-channel concern separate from the algorithm's abstract security? This scoping decision, made early and explicitly, shapes everything downstream — a cryptanalyst who doesn't make it explicitly tends to drift across all three without making real progress on any.

## Stage 3 — Symmetry and algebraic-structure analysis (cheap, first-pass)

Before any statistical or automated technique: does the round function or permutation have an exploitable symmetry (rotational, per companion ARX report `04` §2.4; a fixed point or short-cycle structure, per companion design-process report `02` §1.2's cycle-structure discussion; an invariant subspace preserved regardless of key, per companion ARX report `04` §2.8)? This is checked by hand or via a quick script, not automated search — it's cheap, and a real, exploitable symmetry found here can make every subsequent, more expensive stage unnecessary.

**Decision point**: does a symmetry or invariant exist that survives a quick, informal check? If yes, formalize it into a concrete attack (often via the automated tooling in `06`, used here to *confirm and quantify* a lead already found cheaply, not to *discover* it from scratch) before moving further down the funnel. If no obvious symmetry exists, proceed to diffusion analysis.

## Stage 4 — Diffusion analysis (cheap, still first-pass)

SAC/BIC/avalanche testing (companion ARX report `03` §2) — cheap, mechanical, run against the full candidate design. **Decision point, stated precisely because it's a commonly-mishandled one (companion ARX report `03` §2.6, `08` §2.1)**: diffusion metrics can rule a design *into* further, more expensive scrutiny (poor diffusion is a genuine, cheap-to-find red flag worth escalating) but they cannot rule a design *out* of it — passing diffusion metrics is not evidence of security, only evidence that the *cheapest* possible check found nothing, and the funnel continues regardless of a clean diffusion result. A cryptanalyst who stops here because diffusion looks fine has not actually cleared the design; they've simply not yet found anything, which is a different, much weaker claim.

## Stage 5 — Differential and linear analysis (escalating cost)

Hand-derived or quick-script characteristic search first (cheap); full MILP/SAT-automated optimal-trail search (companion ARX report `05`) reserved for candidates that survive the cheaper check — the exact mirror of the design-side elimination-pass escalation (companion design-process report `02` §2.1, `03` §1.3). **Decision point**: does the best-found trail's probability, at the design's full stated round count, come anywhere near a practically or even theoretically consequential complexity? If the gap is large and comfortable, this specific avenue is likely exhausted (though not necessarily the whole analysis — see Stage 7) and effort should redirect; if the gap is uncomfortably small, or if a reduced-round version shows a trend suggesting the full-round gap might be smaller than the design's own analysis claims, this becomes the priority thread to push further.

## Stage 6 — Automated search, broadened (RX-cryptanalysis, differential-neural, and beyond)

Once plain differential/linear search is exhausted, broaden to the fuller current toolkit — rotational-XOR cryptanalysis specifically (companion ARX report `04` §2.4), boomerang/rectangle combinations, impossible-differential search, and, where the target resembles the Speck/Simon/Simeck family specifically, differential-neural distinguisher training (companion ARX report `04` §2.10). **Decision point**: this stage is expensive enough (real researcher-hours to build each model correctly, real compute/training time for neural approaches) that it's reserved for targets where Stages 3–5 found *something* worth pursuing further, or where the target's stakes (a live NIST standardization candidate, a widely-deployed cipher) justify the investment even absent an early lead — the July 2026 HAWK event (companion ARX report `04` §2.10, `09` §3.3; companion history report `05`) is a genuine instance of this stage being pushed hard specifically *because* the target's standardization stakes were high enough to justify sustained, expensive effort even without Stages 3–5 having flagged an obvious early lead.

## Stage 7 — Side-channel and implementation review

Run largely in parallel with, not strictly after, Stages 3–6, because it's evaluating a genuinely different attack surface (companion ARX report `07` §1's constant-time and microarchitectural material) — but decision-theoretically, it's still funnel-shaped: check for the cheap, obvious implementation red flags first (floating-point/variable-time operations, per `01` §2's final bullet; secret-dependent branches or memory access visible directly in a reference implementation) before investing in genuine microarchitectural analysis (cache-timing measurement, speculative-execution attack surface assessment, companion ARX report `04` §1.5's SLAP/FLOP-class analysis).

## Stage 8 — Security-claims cross-check

Explicitly compare the design's *stated* security claims against what Stages 3–7 actually established, looking specifically for the gap `01` §2's fifth warning sign names — prose claims that outrun the formal proof's actual scope. Rocca's original security-claim gap (`04`) was found exactly this way: not by a novel attack technique, but by careful, close reading of precisely what the design's own proof did and didn't cover, compared against what its accompanying prose implied.

## Stage 9 — Experimental verification

**The stage every serious published result passes through, and the one independent/amateur analysis most commonly skips or shortcuts (companion ARX report `08` §1.2–1.3, `09` §1's positive/negative-control discussion)**: any finding from Stages 3–8 gets independently, experimentally confirmed — not just computed once and trusted, but replicated, checked against a positive control (does the same methodology correctly reproduce a known, already-published result on a related design?) and, where feasible, checked against a negative control (does the methodology correctly *fail* to find a weakness in a deliberately-strengthened variant?). TinyJambu's and Rocca's discovery papers (`04`) both include this kind of verification explicitly — provided key/nonce pairs producing identical tags, in TinyJambu's case, as concrete, checkable proof the forgery actually works, not merely a probability estimate on paper.

---

## The funnel, visualized as a single decision flow

```
Read spec → identify load-bearing assumptions (Stage 1)
  ↓
Scope the effort: break vs. distinguisher vs. implementation? (Stage 2)
  ↓
Cheap check: symmetry / invariant subspace? ──found something?──→ formalize with automated tooling → verify (Stage 9)
  │ (nothing obvious)
  ↓
Cheap check: diffusion metrics ──poor diffusion?──→ escalate priority, continue funnel anyway
  │ (looks clean — NOT evidence of security, just "nothing found yet")
  ↓
Escalating check: hand-derived differential/linear ──promising lead?──→ full MILP/SAT (Stage 5) → verify (Stage 9)
  │ (gap looks comfortable)
  ↓
Broaden: RX-cryptanalysis, boomerang, differential-neural, if stakes justify the cost (Stage 6)
  │ (in parallel throughout:)
  ↓                                                    Side-channel / implementation review (Stage 7)
Cross-check stated claims against what was actually established (Stage 8)
  ↓
Every genuine finding: independently verify before publishing (Stage 9)
```

Part `03` (`03-attack-surface-discovery-warning-signs.md`) now catalogs, in full, the specific structural properties this workflow's early cheap-check stages are actually looking for, and what each one is a proxy for.
