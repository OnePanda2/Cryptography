# 05 — Case Studies, Reverse-Engineered

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`04`'s process framework — this file applies it directly rather than re-explaining it. Factual grounding (dates, exact structural details, cryptanalytic status) is drawn from your companion ARX-specific report's `02`; this file's distinct contribution is the reconstructed *decision sequence and rejected alternatives*, not the facts themselves.*

---

## Salsa20 (Bernstein, 2005)

**The unstated need** (`01` §1): software AES's cache-timing leakage, with no acceptable software-only fix and years before AES-NI hardware would exist.

**The hypothesis** (`01` §2): a cipher built entirely from addition, rotation, and XOR eliminates the leakage by construction, without sacrificing throughput relative to table-lookup AES on the same hardware.

**Architecture choice** (`01` §5): ARX, chosen specifically because the requirement (no secret-dependent memory access, ever) rules out any S-box-table-based SPN design *categorically*, not as a preference among viable options — this is one of this report's cleanest cases of the architecture choice being logically forced by a single requirement, rather than negotiated among several live options.

**State/word size** (`02` §1): 32-bit words, chosen as the smallest universally-native register width (`02` §1.2); a 16-word (512-bit) state arranged as a 4×4 grid, sized to hold constants, key, counter, and nonce material with room for the diffusion topology below to work over.

**Round function invention, rejected alternatives**: the quarter-round's specific add/rotate/XOR sequence is the survivor of exactly the elimination-pass logic in `02` §2.1 — Bernstein's own public design commentary is explicit that earlier internal variants with different operation orderings were tried and discarded for slower diffusion, consistent with diffusion-speed screening (`02` §2.1's second pass) running before expensive cryptanalytic modeling (a luxury this early ARX design didn't yet have access to in the same automated-tooling form later designs would, since MILP-based ARX modeling didn't exist until 2014 — companion history report `03`–`04` — meaning Salsa20's own design-time evaluation ladder necessarily relied more heavily on hand-derived differential reasoning than any design this report examines from 2014 onward).

**Topology**: column round then row round (`02` §3.1) — a working, adequate choice, but one Bernstein himself would revisit and improve on three years later.

**Compromise accepted**: Salsa20's diffusion rate, while adequate, is measurably slower per round than what ChaCha would later achieve with a different topology — an accepted cost at the time, since the alternative (inventing the diagonal-round structure immediately) apparently wasn't yet part of the design vocabulary, or wasn't judged worth the extra round-count-versus-diffusion-speed tradeoff analysis at that specific moment.

**What later research validated or challenged**: full Salsa20/20 remains unbroken (companion ARX report `02` §2.1); the explicit, deliberately-published reduced-round variants (Salsa20/8, Salsa20/12) — a genuine design-process innovation in their own right, offering the cryptanalytic community named, weaker targets to practice on — became a template every subsequent major ARX design (ChaCha, Speck, Simon) would reuse.

---

## ChaCha (Bernstein, 2008)

**The unstated need**: not a new external pressure — Salsa20 had none of the vulnerabilities that later force redesigns. This is this report's clearest case of **voluntary, analysis-driven refinement** rather than crisis response (`02` §3.1's closing point in the companion ARX report).

**The hypothesis**: a different topology (column-then-diagonal instead of column-then-row) achieves faster full-state diffusion per round at equal or better performance.

**Round function invention**: the quarter-round's *internal* operation sequence changed slightly from Salsa20's (different rotation amounts, different operation ordering within the quarter-round itself), but the deeper, more consequential decision was topological (`02` §3.1) — a case where the round-function-invention and topology-invention processes (`02` §2 and §3) are revisited *together*, because changing one without reconsidering the other would have left an unexploited opportunity.

**Rotation constant re-search**: ChaCha's schedule (16, 12, 8, 7) is not Salsa20's schedule reused — a fresh application of `03` §1's process, evaluated against the new topology specifically, since a rotation schedule tuned for one topology has no guarantee of being optimal for a different one.

**Compromise accepted**: none stated as a cost in the historical record — this is the rare case in this report's examined history where a redesign appears to be a clean, dominant improvement (better diffusion, comparable-or-better performance) rather than a tradeoff, which likely explains why ChaCha displaced Salsa20 as the field's default rather than the two coexisting as equally-favored siblings the way, say, Speck and Simon do for their genuinely distinct hardware/software niches.

---

## SipHash (Aumasson, Bernstein, 2012)

**The unstated need**: algorithmic-complexity denial-of-service attacks against hash tables using non-keyed hash functions — an application-security problem, not a classical confidentiality/integrity problem, and a genuinely different kind of requirement than any other case study in this file starts from.

**Requirements engineering, distinctively narrow**: the target platform (hash-table keys, typically short) and the security goal (keyed-PRF security specifically, not collision resistance in the general-purpose-hash-function sense) are both far more tightly scoped than a general-purpose primitive's requirements would be — companion ARX report `02` §2.4's framing of SipHash as "narrowly-scoped and rigorously analyzed for its specific goal" is, from this report's process angle, a direct consequence of requirements engineering (`01` §4) being done unusually precisely and restrictively from the outset, rather than left broad "just in case."

**Architecture and state size**: ARX, inherited directly from ChaCha's quarter-round lineage rather than independently reinvented (`03` §1.4's BLAKE2-schedule-reuse pattern, recurring here) — a compact, four-64-bit-word state, sized specifically for short-input speed rather than the wide-state generic-attack-margin considerations that dominate general-purpose hash/permutation state-sizing decisions (`02` §1.1).

**Round-count tuning as a distinct decision**: SipHash's "2-4" naming (2 compression rounds, 4 finalization rounds) reflects a decision this report's process framework specifically predicts — once the round function and topology are fixed (inherited from ChaCha), the remaining design freedom is almost entirely in round-count tuning against the narrow, specific security goal, and SipHash's own published variants (SipHash-1-3 as a faster, less-conservative alternative) mirror the "publish the weaker variant explicitly" pattern from Salsa20's design.

---

## BLAKE2 (Aumasson, Neves, Wilcox-O'Hearn, Winnerlein, 2013)

**The unstated need**: a hash function combining MD5's raw software speed with SHA-3-level security margin — an unusually explicit *dual* requirement, stated as a comparison against two named prior artifacts simultaneously (`01` §4's "state your performance goal relative to a named comparison point" pattern, doubled).

**Architecture choice**: not a fresh architecture decision at all — BLAKE2 inherits its predecessor BLAKE's overall Merkle–Damgård-with-HAIFA-mode-influence structure and, more consequentially for this report's process focus, its **G function is reused directly from ChaCha's quarter-round lineage** rather than reinvented, a second clean instance (after SipHash) of `03` §1.4's "reuse an already-validated rotation schedule as trusted infrastructure" pattern, applied here at the level of an entire nonlinear-mixing function, not just its rotation constants.

**Where genuine new design work happened**: not in the core G function, but in the surrounding parameter block, tree-hashing support, and performance-tuning specifically for modern (2012-era) CPU pipelines — this is a case study where this report's framework correctly predicts that reused-infrastructure decisions (the G function) free up design effort to concentrate elsewhere, and the elsewhere BLAKE2's team chose (surrounding structure and performance tuning) is exactly where their explicit dual requirement (MD5-speed, SHA-3-margin) actually needed new work.

---

## BLAKE3 (O'Connor, Aumasson, Neves, Winnerlein, 2020)

**The unstated need**: BLAKE2's sequential Merkle–Damgård-style chaining structurally caps how much multi-core/SIMD parallelism any implementation can extract, regardless of how well-optimized the underlying G function is — a performance ceiling not fixable by tuning, only by restructuring (`02` §1.3's alignment point, escalated to the whole-hash-function-architecture level rather than just the round function).

**The hypothesis**: replacing sequential chaining with a Merkle-tree internal structure removes the serial dependency entirely, letting independent tree nodes hash concurrently.

**What this reveals about the process, distinctively**: BLAKE3 is this report's clearest instance of a design team recognizing that the bottleneck they needed to solve lived at the **architecture level** (`01` §5), not the round-function or rotation-constant level (`02`–`03`) — and specifically *not* re-litigating the already-validated G function (reused from BLAKE2, itself reused from ChaCha) in the process. This is a genuinely important, generalizable lesson this case study demonstrates cleanly: **correctly diagnosing which process layer (`01` through `03`) a given performance or security problem actually lives at, and revisiting only that layer, is itself a design skill** — a less experienced team facing BLAKE2's parallelism ceiling might have mistakenly gone looking for a faster G function, when the actual fix required going back to `01`'s architecture-choice layer instead.

---

## Threefry and Philox (Salmon, Moraes, Dror, Shaw, 2011)

**The unstated need**: GPU/parallel scientific computing needs a PRNG whose outputs are computable independently and in any order — a requirement that, examined against `01` §5's architecture-decision procedure directly, **categorically rules out any stream-cipher-derived sequential-state design**, exactly the same kind of hard, requirement-forced elimination Salsa20's cache-timing requirement produced for SPN designs.

**Architecture choice, forced rather than preferred**: counter-based construction — compute output(counter, key) directly and statelessly — chosen not because it's more elegant than a stream-cipher-style generator but because it's the *only* structure satisfying the parallelism requirement, a clean illustration of `01` §5's closing point that architecture choice should follow from requirements, not the reverse.

**Threefry versus Philox as parallel, not sequential, design efforts**: the two designs made genuinely different choices at the round-function-invention stage (`02` §2) for the *same* architecture and requirements — Threefry adapting an existing ARX block cipher (Threefish) into counter mode, Philox instead choosing multiplication as its primary nonlinear operation (making it, notably, not strictly ARX at all, companion ARX report `02` §2.5) — worth reading as evidence that requirements engineering and architecture choice (`01`) can converge on the same broad structural family while still leaving genuine, legitimate design-space freedom at the round-function level, with two capable teams reasonably resolving that freedom differently.

**A gap this report's process framework surfaces explicitly**: per companion ARX report `06` §1.7 and `09` §1's open-problems list, neither design received the same depth of adversarial cryptanalytic evaluation (the `03` §3.1 ladder's rungs 4–5 specifically) that mainstream CSPRNG-adjacent designs receive — a real, acknowledged asymmetry between how thoroughly the *statistical*-quality tiers of evaluation were run (extensively, and successfully) versus the *adversarial*-cryptanalysis tiers (comparatively thin), worth flagging here as a case where the evaluation ladder from `03` §3.1 was not run to completion, for reasons of differing intended use case (statistical simulation quality, not adversarial-security-critical key generation) rather than oversight — a legitimate, stated scoping decision, but one worth being explicit about rather than assuming full parity with a mainstream CSPRNG's evaluation depth.

---

## Xoodoo and Xoodyak (Daemen, Hoffert, Peeters, Van Assche, Van Keer)

**The unstated need**: bring Keccak-style sponge/duplex construction benefits (length-extension immunity, flexible mode construction) to a genuinely lightweight hardware/software footprint — the same Keccak-team lineage (`01` §1's origin-point list) applied to a new, more constrained target platform than SHA-3's original brief.

**Architecture choice, revisited from prior work rather than chosen fresh**: sponge/duplex, inherited directly from the team's own earlier Keccak work — a case, like BLAKE2/3's G-function reuse, of architecture-level decisions carrying forward across a design team's projects once validated, rather than being re-derived from scratch each time.

**Round-function invention, a genuine departure**: Xoodoo's round function uses AND, not addition, as its nonlinear operation (companion ARX report `02` §2.6) — a deliberate choice distinguishing it from pure ARX, made specifically because AND gates are cheaper in hardware than a full adder's carry-chain circuitry, directly serving the lightweight-hardware-footprint requirement in a way pure ARX would not have.

**Xoodyak's "Cyclist" mode as a requirements-engineering artifact**: the explicit design goal of flexible protocol construction (not just a fixed AEAD mode) shaped the *surrounding* mode design as much as the underlying Xoodoo permutation itself — a reminder that requirements engineering (`01` §4) covers not just the permutation's own properties but how flexibly it needs to be *wrapped*, and that this wrapping-flexibility requirement is a genuine, separate design axis from the permutation's cryptanalytic margin.

**Compromise accepted, per the competition outcome**: Xoodyak reached NIST's Lightweight Cryptography competition's later rounds but was not selected — per companion history report `07` §1.2's dead-end discussion (applied there to isogeny cryptography, but structurally the same lesson here), this is not evidence of a design flaw; it's evidence that the field's actual selection criteria (companion ARX report `08` §3.5) weigh a fuller package (footprint, API flexibility, implementation maturity, and — as Ascon's win demonstrates — sometimes less-formally-provable-but-more-thoroughly-evaluated margin) rather than any single axis alone.

---

## Ascon (Dobraunig, Eichlseder, Mendel, Schläffer)

**The unstated need**: the same NIST Lightweight Cryptography competition forcing function driving Sparkle and Xoodyak, met with a structurally different architecture choice.

**Architecture choice, the case study's most instructive decision**: SPN-sponge hybrid, *not* ARX or AND-RX — a deliberate choice, per `01` §5's decision procedure, that a fixed 5-bit S-box's differential/linear properties, tuned specifically for the bitsliced software/hardware implementation profile the team wanted, better served their specific combination of requirements (side-channel posture, implementation maturity, hardware *and* software footprint simultaneously) than either pure-ARX's carry-chain nonlinearity or AND-RX's AND-gate nonlinearity would have.

**Why this case study matters most for this report's central thesis**: Ascon's eventual competition win over the more provably-elegant Sparkle (companion ARX report `02` §2.7, `08`'s process discussion) is this report's sharpest available evidence that **the design process's later stages (implementation maturity, footprint across multiple target profiles, API/mode flexibility) can outweigh an earlier-stage methodological achievement (Sparkle's long-trail-strategy provable bound) in the field's actual, final evaluation** — meaning a designer optimizing purely for `03`'s evaluation-ladder rigor, while necessary, is not by itself sufficient for adoption success, a genuinely humbling, and genuinely important, lesson this report's process framework would otherwise risk implying is the whole story.

---

## Sparkle and Alzette (Beierle, Biryukov, Cardoso dos Santos, Großschädl, Perrin, Udovenko, Velichkov, Wang, 2020)

**The unstated need**: ARX design, despite two decades of real-world trust, lacked AES's specific kind of assurance — a provable bound, not an absence of found attacks (`01` §1).

**The hypothesis, exceptionally explicit and well-documented in the primary source**: the wide-trail strategy's active-component-counting argument can be adapted to ARX's addition operator, given a separately-established per-ARX-box probability bound — this report's clearest case where the falsifiable hypothesis from `01` §2 is stated almost exactly as this report frames it, in the paper's own language.

**Round-function and rotation-constant co-design**: per `03` §1.4, Alzette's specific add/rotate/XOR/constant sequence was chosen jointly with the proof-methodology development, not independently — the clearest documented instance in this entire case-study set of `03` §3.4's "when provable security is the explicit goal, constant search is partly steered by what will be provable" pattern.

**State-size decision, explicitly multi-variant**: three named Sparkle sizes (256/384/512), a direct, undisguised instance of `02` §1.1's "ship multiple points on the tradeoff curve rather than resolving the negotiation once" pattern.

**What later research validated**: the long-trail strategy itself, independent of Sparkle's specific competition outcome — per this file's Ascon entry above, the methodology's citation and influence (companion ARX report `10`'s reading roadmap treats it as required primary-source reading) has outlasted and outweighed the specific permutation's non-selection, exactly the "novelty concentrated at one carefully-chosen point" pattern `01` §3 identifies as the field's actual reward structure.

---

## NORX (Aumasson, Jovanovic, Neves, 2014)

**The unstated need**: CAESAR-era authenticated encryption needing genuine, first-class parallelism support, not a bolted-on afterthought — the requirements-engineering step (`01` §4) here explicitly names parallelism as a primary design goal in a way most contemporary AEAD proposals didn't.

**Architecture and topology**: ARX-based sponge/duplex, with an explicit "NORX-P" parallel mode as a first-class alternative to the sequential baseline — a case where the topology-design stage (`02` §3) produced *two* supported topologies (sequential and parallel) for the same underlying permutation, rather than one team choosing between them, directly serving the stated dual deployment context (single-core and multi-core targets both needing first-class support).

**What makes this case study distinctive for this report's purposes**: NORX's eventual fate — reaching CAESAR's third round, then being **withdrawn by its own designers**, not defeated by cryptanalysis (companion ARX report `02` §2.8) — is this report's only case study where the design process's normal endpoint (publication, competition evaluation, adoption or non-selection) was interrupted by a decision external to every process stage this report has described. **The lesson worth drawing carefully**: none of `01`–`04`'s process framework predicts or explains this outcome, because it wasn't a design-process failure at all — it's a reminder, worth stating honestly rather than forcing NORX's story to fit this report's framework artificially, that a design's fate is not fully determined by how well its design process was executed; institutional and political factors external to the design itself (echoing companion history report `07` §1.4–1.5's Speck/Simon and NIST-curve-provenance discussions) can end a design's trajectory regardless of how rigorously `01`–`03`'s process was followed.

---

Part `06` (`06-design-philosophies-of-major-designers.md`) steps back from individual designs to compare how the designers behind them — Bernstein, Daemen, Rijmen, Rogaway, Aumasson, Pornin — each actually think, agree, and disagree.
