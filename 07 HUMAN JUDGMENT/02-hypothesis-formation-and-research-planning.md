# 02 — Hypothesis Formation and Research Planning

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`. Companion design-process report `01` §2 already covers the basic falsifiable-hypothesis discipline for a single design; this file extends it to the multi-year-program scale your outline actually asks about.*

---

## 1. Hypothesis Formation

### 1.1 How specific a hypothesis should be, restated with a concrete calibration

Companion design-process report `01` §2's Sparkle/Alzette example ("a design built entirely from addition, rotation, and XOR can eliminate the cache-timing attack surface... while matching or exceeding throughput") is this report's calibration reference: specific enough to name the exact mechanism and the exact tradeoff being tested, general enough to survive the inevitable refinement a real research program applies as it learns. **The test this file adds**: a hypothesis is specific enough when two different researchers, reading only the hypothesis statement (not the surrounding prose), would independently agree on what result would confirm it and what result would disconfirm it — if two competent readers could reasonably disagree about which outcome counts as success, the hypothesis needs tightening before planning proceeds.

### 1.2 Refining a hypothesis without quietly moving the goalposts

A hypothesis legitimately gets refined as early evidence comes in — this is normal, expected scientific practice, not a failure. The distinction this report insists on, restated from `06`'s anti-patterns treatment in advance because it belongs here structurally: **legitimate refinement narrows or sharpens the original claim in a way that's traceable and documented; illegitimate goalpost-moving quietly redefines success after the fact to match whatever result was actually obtained.** The concrete discipline separating the two: maintain a dated log of every hypothesis revision, with the specific evidence that motivated each change — a practice that costs almost nothing to maintain and makes the difference between the two categories externally auditable rather than a matter of the researcher's own self-report.

### 1.3 Documenting assumptions explicitly

Directly restating companion cryptanalyst-mindset report `01` §1's core finding at the hypothesis-formation stage, before any cryptanalysis begins: every hypothesis rests on assumptions (about a threat model, about which prior results remain valid, about a tool's correctness), and companion cryptanalyst-mindset report `04`'s entire TinyJambu case study exists because a design team's central, load-bearing assumption went undocumented as an assumption at all — it was simply built into their methodology invisibly. **The concrete practice this file recommends**: a short, explicit, separately-maintained assumptions list alongside every hypothesis, written *before* work begins, specifically because assumptions are far easier to state honestly before you've invested months believing them than after.

### 1.4 Kill criteria — the single most important discipline in this file

Restating and generalizing this report's Executive Summary: a **kill criterion** is a pre-committed, specific condition that, if met, ends the research direction regardless of how much has already been invested or how close success might feel. **Why this must be written before starting, not decided in the moment**: the moment a kill criterion would actually apply is precisely the moment a researcher is least equipped to decide fairly whether it applies — `03` develops the specific psychological mechanisms (sunk cost, motivated reasoning) that make in-the-moment kill decisions unreliable, and a pre-committed criterion is a structural defense against exactly those mechanisms, not merely a planning nicety. **What a good kill criterion looks like, concretely**: "if the best-found trail after full MILP evaluation is within a factor of 4 of the target security level with no further round-count budget available, abandon this specific round-function candidate" — specific, measurable, and stated in terms available *before* the evaluation runs, not defined retroactively once results are in hand.

### 1.5 Measurable success criteria, as kill criteria's positive mirror

Every kill criterion needs a positive counterpart: what result, specifically, constitutes success clear enough to move to the next stage (publication, in `05`'s framing) — companion evaluation-methodology report `08`'s "what evidence would be sufficient to publish" section is the direct model for what this looks like at the level of one design's evaluation; at the level of a whole research program, the equivalent is a dated, written statement of what the program's own final deliverable needs to demonstrate before the researcher considers the *program*, not just one candidate within it, complete.

## 2. Research Planning

### 2.1 How multi-year programs actually get planned

Restating companion research-platform report `07` §3's year-by-year roadmap as the general template this section extends: a multi-year program should be decomposed into phases where **each phase has its own kill criteria and success criteria** (§1.4–1.5), not one criterion applied to the whole multi-year effort — a program that only checks in with itself at the three-year mark has given up the ability to course-correct early, cheaply, exactly the funnel-cost-escalation logic companion cryptanalyst-mindset report `02` and companion design-process report `02` §2.1 both apply at the technical-evaluation level, generalized here to the level of program-years rather than evaluation-stages.

### 2.2 Defining milestones

A good milestone, per this report's synthesis of its case studies (`07`), is **specific, dated, and tied to a concrete artifact** — not "make progress on the diffusion problem" but "have a working MILP model of the candidate round function, tested against at least one positive control, by [date]." Vague milestones fail silently: a researcher can convince themselves they're "making progress" indefinitely without a concrete artifact forcing an honest check.

### 2.3 Adjusting priorities as the program unfolds

Directly extending `01` §3.2's revisit-feasibility-at-every-milestone discipline: a milestone review is exactly the moment to re-run `01`'s entire problem-selection sequence against the *remaining* work, not just check whether the original plan is on schedule — new information genuinely changes what's worth prioritizing next, and treating the original plan as fixed once written is the planning-level version of the same rigidity `03` catalogs at the individual-decision level.

### 2.4 Managing dependencies

Restating companion research-platform report `07` §3's explicit sequencing logic (mature-tooling integration before attempting genuinely novel generation methods) as a general planning principle: order a program's phases so that **later phases depend on earlier phases having produced trustworthy infrastructure**, not on earlier phases having produced a specific *result* — a phase-2 plan that only works if phase 1 finds a particular answer is fragile in a way a phase-2 plan built on phase 1's *methodology* (regardless of what specific answer it found) is not.

### 2.5 Anticipating dead ends explicitly, before they happen

Directly restating companion history report `07` §1's dead-end catalog (XSL, SIDH/SIKE, Rainbow) as a planning discipline rather than a retrospective one: a mature multi-year plan names, in advance, which of its own assumptions are most likely to turn out wrong, and pre-plans what the program pivots to if each specific one does — not a vague "we'll figure it out if something goes wrong," but a specific, if necessarily provisional, fallback direction attached to each named risk, directly connecting to `04`'s full risk-taxonomy treatment.

## 3. How Successful Laboratories Organize This in Practice

Restating companion research-platform report `05` §3's experimental-infrastructure discipline at the human-organizational level rather than the software level: successful labs maintain the equivalent of that report's Experiment Database as a **human practice** — a maintained, dated research log recording every hypothesis, every kill criterion, every milestone outcome, and every revision, serving the identical function companion evaluation-methodology report `06` §1 argues reproducibility infrastructure serves for a single result: making the program's actual history externally auditable, including to the researcher's own future self, rather than relying on memory, which — per `03`'s upcoming treatment of confirmation bias — is precisely the thing least trustworthy exactly when a hard, sunk-cost-laden decision needs to be made fairly.

Part `03` (`03-the-decision-loop-continue-stop-and-avoiding-bias.md`) now develops, in full, the specific psychological failure modes this file's kill-criteria discipline exists to defend against.
