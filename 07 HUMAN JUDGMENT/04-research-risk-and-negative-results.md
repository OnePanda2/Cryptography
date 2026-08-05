# 04 — Research Risk and Negative Results

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`03`. The risk taxonomy below is genuinely new content relative to every companion report in this conversation.*

---

## 1. The Research Risk Taxonomy

Nine distinct risk categories, each requiring a genuinely different mitigation — treating "risk" as one undifferentiated concern, rather than nine separable ones, is itself a common planning failure this section exists to correct.

### 1.1 Novelty risk

The risk that a direction turns out not to be genuinely new — restating `01` §3.1's novelty-concentration discipline as a risk to actively check for, not just a design principle: has this specific question already been answered, perhaps in a venue or language the researcher hasn't searched (companion history report's explicit flag about non-English-language literature underrepresentation applies directly here). **Mitigation**: a genuine, dedicated literature-search pass — not a cursory one — before committing real time, using companion research-platform report `06` §4's semantic-search and knowledge-graph discussion as the target capability an independent researcher should approximate by hand if they don't have that platform's infrastructure available.

### 1.2 Technical risk

The risk that the underlying approach simply doesn't work — the round function doesn't achieve adequate diffusion, the proof technique doesn't generalize the way hoped. **Mitigation**: `02` §1.4's kill criteria, applied specifically and early, plus `01` §2.2's concrete-first-milestone discipline, which exists precisely to surface technical risk cheaply and early rather than after a large investment.

### 1.3 Publication risk

The risk that the work, even if technically sound, doesn't clear the bar a target venue actually requires — restating companion design-process report `08` §3.5's finding that the field's actual publication currency favors an independently-defensible contribution over a design alone. **Mitigation**: `05`'s publication-strategy discussion, specifically the discipline of identifying the intended contribution's shape *before*, not after, the technical work is complete.

### 1.4 Implementation risk

The risk that a correct algorithm-level design fails at the implementation layer — restating companion general-cryptography report `04` §1.5's Heartbleed lesson and companion evaluation-methodology report `06` §2's failure-catalog entry for exactly this category: the overwhelming majority of deployed cryptographic failures are implementation, not algorithm, failures, and a research program that never budgets time for implementation validation (companion evaluation-methodology report `02` §Stage 3–4) carries real, under-appreciated risk on this axis specifically.

### 1.5 Evaluation risk

The risk that the evaluation methodology itself is flawed — restating companion evaluation-methodology report's entire central argument, and companion cryptanalyst-mindset report `04`'s TinyJambu case study, as the standing illustration of this risk materializing concretely: a design team's own MILP-based evaluation carried an unexamined independence assumption. **Mitigation**: companion evaluation-methodology report `02`'s positive/negative-control discipline, applied to the evaluation methodology itself before trusting its output on the actual candidate.

### 1.6 Cryptanalysis risk

The risk that a design believed secure is later broken — the risk every companion report in this conversation treats as permanent and irreducible, per companion evaluation-methodology report `08`'s explicit "what would still be insufficient" closing section: no amount of pre-publication rigor eliminates this risk entirely, only manages it down to a level the field considers acceptable given the design's stakes.

### 1.7 Replication risk

The risk that a result, even if correctly obtained by the original researcher, fails to reproduce independently — restating companion evaluation-methodology report `03` §8 and `06`'s reproducibility-discipline treatment as this risk's direct mitigation, and worth naming as a distinct risk category because it can materialize even when categories 1.2–1.6 are all genuinely clean (a correct, well-evaluated result can still fail to replicate due to an undocumented environmental dependency, an unstated parameter choice, or genuine bad luck in a marginal statistical finding).

### 1.8 Career risk

Genuinely distinct from the technical risk categories above: the risk that pursuing a given direction costs a researcher professional standing, funding, or future opportunity — real for anyone, and especially salient for the independent-researcher situation `08` addresses directly, where there's no institutional buffer (a tenured position, a funded lab) absorbing the cost of a direction that doesn't pan out. **Mitigation, stated honestly rather than dismissively**: diversify — companion ARX report `09` §1's ranked-opportunities table explicitly includes items at different difficulty/risk levels specifically so a portfolio of smaller, more tractable contributions can sit alongside one larger, riskier bet, rather than a career resting entirely on a single high-risk direction succeeding.

### 1.9 Opportunity cost

The risk, easy to under-weight because it's invisible (nothing observably goes wrong; a better alternative simply never gets pursued): every hour spent on one direction is an hour not spent on another, and `01`'s entire problem-selection discipline exists specifically to make this cost visible and deliberate rather than accidental — the researcher who never revisits `01`'s problem-selection sequence after the initial commitment (per `02` §2.3's milestone-review discipline) is the researcher most exposed to this risk going unnoticed for years.

## 2. Balancing These Risks — How Experienced Researchers Actually Do It

Restating this section's nine categories as a single, practical discipline: **at every milestone review (`02` §2.3), explicitly ask which of these nine risk categories has changed since the last review, not just whether the work is "on track" in some undifferentiated sense.** A program can be perfectly on schedule technically (low categories 1.2, 1.4–1.6) while opportunity cost (1.9) has quietly grown large because a more important problem has since emerged elsewhere in the field — a distinction an undifferentiated "are we on track" check would miss entirely, and one this report's nine-category framework is specifically built to surface.

---

## 3. Negative Results

### 3.1 When negative results are genuinely valuable

Directly restating companion history report `07` §1's dead-end catalog and companion cryptanalyst-mindset report `08` §1's discovery-process material: a negative result is valuable precisely when it closes off a *plausible-seeming* direction other researchers would otherwise waste time independently rediscovering is a dead end — the XSL attack's eventual, careful refutation (companion history report `07` §1.1) is exactly this pattern: valuable not because a break failed to materialize, but because the refutation prevented years of the field either wrongly trusting the original claim or wastefully re-deriving the refutation independently, multiple times, in isolation.

### 3.2 A negative result is not automatically valuable, stated honestly

The mirror point, worth stating with equal directness: a negative result on a direction no one else would plausibly have pursued anyway (an obviously-doomed approach, or a genuinely idiosyncratic dead end specific to one researcher's particular framing) has little value to report, and treating every failed direction as automatically publication-worthy is its own error — restating `01`'s importance-versus-interesting distinction at the level of negative results specifically: a negative result's value depends on whether the *positive* version of the same claim would have been plausible and important enough that other researchers would genuinely have been at risk of independently pursuing it.

### 3.3 How negative results should be written and presented

Restating companion evaluation-methodology report `07`'s publication-bias discussion as a writing discipline: a negative-result paper's credibility depends specifically on demonstrating the *same* rigor a positive result would need — the full evaluation ladder, genuine controls, honest disclosure of exactly what was and wasn't tried — precisely because a sloppy negative result ("we tried this and it didn't seem to work") is nearly worthless, while a rigorous one (the XSL-refutation pattern) is a genuine, citable contribution other researchers can build on with confidence rather than needing to independently re-verify.

### 3.4 Historical examples where negative results significantly advanced the field

Beyond the XSL-attack refutation already covered in depth elsewhere in this conversation: companion history report `07` §1.2–1.3's SIKE and Rainbow breaks, while framed there primarily as *attacks*, function simultaneously as negative results at the level of "is this specific hard-problem instantiation trustworthy" — and their genuine field-wide value came specifically from being rigorous, independently-verified, and clearly communicated (companion cryptanalyst-mindset report `07` §1.1's evidentiary-standard discussion), not merely from the fact that a break was found at all. A poorly-documented, unreplicated claim of the same underlying weakness would have advanced the field far less, even if it happened to be technically correct — reinforcing §3.3's point that a negative result's rigor, not just its correctness, is what determines its actual contribution.

Part `05` (`05-research-leadership-and-publication-strategy.md`) now covers how principal investigators actually lead research groups, and the strategic question of which venue a given result actually belongs in.
