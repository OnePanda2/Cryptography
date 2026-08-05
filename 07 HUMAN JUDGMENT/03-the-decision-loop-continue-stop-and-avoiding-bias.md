# 03 — The Decision Loop: Continue, Stop, and Avoiding Bias

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`02`. Companion design-process report `03` §3.1–3.4 already covers the evidentiary escalation ladder for a single design's evaluation, and companion evaluation-methodology report `01`–`03` covers the underlying epistemic hierarchy and statistical discipline — this file assumes both and focuses on the psychological, not the technical, half of the decision.*

---

## 1. When Evidence Is Sufficient, and When It Isn't — Restated at the Decision-Psychology Level

### 1.1 The technical answer already exists; this file is about why it's hard to act on

Companion evaluation-methodology report `01` §3's five-level hierarchy (measurement, evidence, proof, confidence, security) and `03`'s statistical machinery already specify, precisely, what counts as sufficient evidence for a given claim. **The genuine, additional problem this file addresses**: knowing the correct threshold and actually applying it to your own work, in the moment, under real motivational pressure, are different skills — a researcher can recite companion evaluation-methodology report `03` §5's power-analysis discipline perfectly and still, in practice, stop collecting data the moment a result looks favorable rather than at the pre-planned sample size, precisely because the favorable-looking result is *pleasant* and continuing to collect data risks that pleasantness evaporating.

### 1.2 How much replication is actually expected, restated as a decision rule

Directly restating companion evaluation-methodology report `02` §2 and `03` §8's independent-replication discipline as an operational decision rule: **a result is not "done" — not ready to inform a continue/stop decision — until it has been reproduced by a genuinely independent draw** (a different seed, ideally a different implementation), not merely re-executed. A researcher tempted to treat a single striking result as sufficient grounds for a major program decision (pivoting the whole research direction around it, say) should treat that temptation itself as a signal to slow down, not speed up.

## 2. Confirmation Bias, Named and Defended Against Specifically

### 2.1 What it actually looks like in cryptographic research, concretely

Not a vague general tendency, but a specific, recognizable pattern this report's case studies (`07`) and its companions repeatedly surface: **running many candidate evaluations and reporting only the ones supporting the hypothesis already favored, without disclosing the full set attempted** — directly restating companion cryptanalyst-mindset report `07` §2.4's publication-bias discussion at the level of an individual researcher's own private decision-making, not just the field's aggregate publication record. **A second, subtler form**: interpreting an ambiguous result (a marginal statistical finding, per companion evaluation-methodology report `03` §6's re-run-on-flag discipline) in whichever direction the researcher already expected, rather than treating genuine ambiguity as genuine ambiguity requiring further, unbiased investigation.

### 2.2 The specific defense this report recommends

**Pre-registration, informally applied**: write down, before running an evaluation, what result would confirm the hypothesis and what result would disconfirm it (directly restating `02` §1.1's specificity test) — and, critically, **commit to reporting the actual result against that pre-written prediction, regardless of outcome**, in the same research log `02` §3 describes. This is not a formal, externally-audited pre-registration process (though such processes exist in other fields and are worth knowing about); it's a private discipline whose entire value comes from making it harder for the researcher's own later self to quietly reinterpret an inconvenient result as having actually been expected all along.

### 2.3 Negative and positive controls as a structural defense, restated from the technical to the psychological register

Companion evaluation-methodology report `02` §2's control-methodology discussion exists, technically, to prevent a broken methodology from producing false results. It has a second, equally important psychological function this report adds explicitly: **a methodology that has been checked against known positive and negative controls gives the researcher an external, pre-established anchor to check a surprising result against**, rather than relying entirely on their own in-the-moment judgment about whether the surprising result "feels" trustworthy — a genuinely useful defense against confirmation bias precisely because it doesn't depend on the researcher successfully catching their own bias in real time.

## 3. Sunk-Cost Fallacy, Named and Defended Against Specifically

### 3.1 Why it's especially dangerous in multi-year cryptographic research specifically

The sunk-cost fallacy — continuing an investment because of what's already been spent, rather than because of the investment's actual remaining expected value — is especially acute in this domain because cryptographic research programs genuinely do take years, and the psychological weight of "I've spent three years on this" is real and substantial, independent of whether three more months of honest evaluation would actually change the program's prospects. **The specific, recurring shape this takes**: a researcher whose pre-committed kill criterion (`02` §1.4) has technically been met finds a reason the criterion "doesn't quite apply this time" — a genuinely difficult moment to navigate honestly, and precisely the moment kill criteria's advance-commitment value (§1.4 of `02`) matters most.

### 3.2 The specific defense

**Treat a kill criterion as binding, not advisory, and treat any impulse to reinterpret it in the moment as itself diagnostic** — restating `02` §1.4's core point with the added, explicit instruction: if you find yourself arguing that a pre-committed kill criterion shouldn't apply "this time," the argument itself, not the criterion, is what deserves the most skepticism, precisely because it's arising under exactly the motivational conditions (large sunk investment, emotional attachment to a specific direction) this section names as unreliable.

## 4. Becoming Attached to Designs — a Related but Distinct Failure

### 4.1 Distinguishing this from sunk cost specifically

Sunk cost is about past investment; **attachment** is about a design or hypothesis having become part of the researcher's own identity or self-conception ("I am the person who works on X"), which can drive continued investment even absent large *past* costs — a genuinely distinct psychological mechanism worth naming separately, since the two often co-occur but the defenses differ. **The specific, recognizable symptom**: a researcher who responds to legitimate technical criticism of their own design with defensiveness rather than curiosity is showing attachment, not merely difficulty processing sunk cost.

### 4.2 The specific defense, restated from companion cryptanalyst-mindset report `01` §3

Directly restating that report's "unfamiliar versus suspicious" distinction, applied here to a researcher's relationship with their *own* work rather than someone else's: treat critical scrutiny of your own design exactly as you would treat scrutiny of anyone else's — as the field's normal, expected, valuable process (companion evaluation-methodology report `01` §1's entire justification for why evaluation exists at all), not as a personal challenge. A researcher who has internalized companion design-process report `08` §3.5's finding — that the field's actual reward structure favors a genuinely defensible contribution over an unchallenged one — has a direct, self-interested reason to welcome scrutiny rather than resist it, independent of any purely dispositional discipline.

## 5. Scientific Thinking: Belief Updating, Contradictory Evidence, and Uncertainty

### 5.1 How beliefs should update, stated as a discipline rather than a formal procedure

A researcher need not run literal Bayesian updating on every result, but the underlying discipline is directly borrowable: **the size of a belief update should be proportional to the strength of the evidence**, per companion evaluation-methodology report `03` §3's effect-size-versus-significance distinction — a single, unreplicated, marginally-significant finding should shift confidence only slightly; a result independently replicated across multiple seeds and implementations, with a large, security-relevant effect size, warrants a much larger shift. Treating every new result as equally update-worthy, regardless of its actual evidentiary strength, is a subtler, less-discussed cousin of confirmation bias.

### 5.2 Handling contradictory evidence specifically

When two results genuinely conflict (a positive finding from one evaluation stage, a null finding from an independent replication), the correct first move — restating companion evaluation-methodology report `02` §2's control discipline — is not to decide which result to believe, but to check whether the two used genuinely comparable methodology (same sample size, same seed-independence, same statistical correction), since a large fraction of apparent contradictions in practice trace to a methodological difference between the two attempts, not a genuine underlying disagreement about the world.

### 5.3 Separating speculation from evidence, and communicating the difference honestly

Directly restating this entire conversation's established practice (companion general-cryptography report's "never hide uncertainty" commitment) as a decision-making discipline: a researcher's own internal notes, and eventual publications, should mark speculation as speculation explicitly — "I suspect X, though I have not yet tested it" is a legitimate, useful thing to write down, and its value depends entirely on being clearly distinguished from "I have shown X," a distinction companion evaluation-methodology report `01` §4 shows collapsing is the single most common way a field's overall confidence gets miscalibrated over time.

Part `04` (`04-research-risk-and-negative-results.md`) now develops the full risk taxonomy this decision loop operates under, and covers when a negative result is itself a genuine, publishable contribution.
