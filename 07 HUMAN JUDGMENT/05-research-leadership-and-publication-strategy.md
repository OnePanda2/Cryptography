# 05 — Research Leadership and Publication Strategy

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`04`. The leadership material in Section 1 is genuinely new relative to every companion report in this conversation.*

---

## 1. Research Leadership

### 1.1 How principal investigators review student and junior-researcher work

Restating and extending companion design-process report `04` §1 and companion cryptanalyst-mindset report `07` §1's reviewer-anticipation material — written there for a submission's *content* — to the specific relationship between a PI and a researcher they're supervising: the review's function is different in kind from peer review, because a PI's job is not merely to accept or reject a piece of work but to **actively shape which of `01`–`04`'s decisions the junior researcher is making, and to teach the decision-making process itself, not just correct its outputs.** A PI who only ever tells a student "this result is wrong, fix it" is optimizing for the current result; a PI who asks "what evidence would have told you this before you spent three weeks on it" is optimizing for the student's next decade of independent judgment — directly restating `03`'s entire kill-criteria discipline as a teaching tool, not just a personal practice.

### 1.2 How projects get assigned

The pattern reconstructable across successful research groups, restating `01`'s problem-selection sequence at the group-management level: a good project assignment sits at the intersection of the group's actual strategic priorities (restating `01` §1.3's load-bearing-assumption test, applied at the group's research-program level rather than one researcher's individual choice) and the specific researcher's current skill level and genuine interest — assigning a junior researcher a project purely because it's important to the group's funding, with no attention to whether it's tractable at their current skill level, produces exactly the technical-risk-category-1.2 failure `04` §1.2 names, at unnecessary cost to the researcher's development.

### 1.3 How projects get terminated

Directly restating `02` §1.4's kill-criteria discipline as an interpersonal, not just technical, act: terminating a project a researcher (especially a junior one) has invested real time and identity in is exactly where `03` §3–4's sunk-cost and attachment dynamics are most acute, and a PI's specific leadership responsibility here is providing the external, less-emotionally-invested judgment the researcher themselves is structurally poorly positioned to provide in the moment — restating companion evaluation-methodology report `01` §1's core justification for why evaluation exists (a design's originator is structurally the worst-positioned party to certify it) at the level of project termination specifically, with the PI serving the role external evaluation serves for a technical claim.

### 1.4 How promising directions get recognized

Restating `01` §2's value-estimation framework as a pattern-recognition skill built through repeated practice, directly analogous to companion cryptanalyst-mindset report `01` §4's finding that cryptanalytic intuition is a library of paired structural-signature-to-technique associations built one real case at a time: an experienced PI's ability to recognize a promising direction early, often before the researcher pursuing it can fully articulate why it's promising, is this same kind of compounded pattern-library, not a mysterious intuitive gift — and it's built the same way, through sustained, direct engagement with many real research programs' actual outcomes, not through reading about research strategy alone (an honest acknowledgment this report, like its companions, applies to itself: no report substitutes for the practice it describes).

### 1.5 Mentoring independent researchers specifically

Distinct from supervising a student embedded in a lab: mentoring an independent researcher (directly relevant to `08`'s capstone chapter) means providing the external accountability and evidentiary-standard-calibration a lab's peer environment would otherwise provide informally — restating companion cryptanalyst-mindset report `07` §1.3's "breadth of scrutiny" point: an independent researcher's single biggest structural disadvantage relative to a lab-embedded one is the comparative absence of this informal, ambient calibration, and a good mentor's specific value is substituting for some of it deliberately, through scheduled, honest check-ins against the researcher's own stated kill criteria and milestones, rather than the accidental, constant hallway-conversation calibration a physical lab provides for free.

---

## 2. Publication Strategy

### 2.1 The venue landscape, mapped to what each is actually for

| Venue type | What belongs here |
|---|---|
| **Conference (CRYPTO/EUROCRYPT/ASIACRYPT/FSE-ToSC/CHES)** | A complete, independently-defensible contribution meeting that specific venue's scope, per companion design-process report `08` §3.1's full venue-scope breakdown | 
| **Journal (Journal of Cryptology, ToSC as a continuous journal)** | Work benefiting from a longer, more thorough review cycle than a conference deadline allows, or a substantially extended version of prior conference work |
| **Preprint (IACR ePrint)** | Anything ready for the field's scrutiny now, ahead of or independent of formal peer review — companion design-process report `08` §3.1's point that essentially every eventually-published paper appears here first applies directly, and a preprint is the correct venue for establishing priority and inviting early feedback, not a substitute for eventual formal review |
| **Technical report** | Internal or institutional documentation not intended for the broader field's peer-review cycle — appropriate for a research-platform's own internal methodology documentation (companion research-platform report `05` §3), not for a claimed cryptanalytic result |
| **Blog / informal write-up** | Exposition, tutorial content, or genuinely preliminary observations explicitly marked as such — restating `03` §5.3's speculation-versus-evidence discipline: a blog post claiming a real result without the rigor a preprint or paper would require is a direct violation of that discipline, not a lower-stakes alternative to it |
| **Open-source release** | The reference implementation and evaluation code themselves, per companion evaluation-methodology report `06` §1.2's artifact-evaluation discipline — increasingly expected *alongside* a paper, not a substitute for one |

### 2.2 What should remain private until verified

Directly restating companion cryptanalyst-mindset report `08` §1.1's responsible-disclosure discipline: a genuine cryptanalytic finding against a live, deployed, or actively-standardizing system should remain private, communicated first to affected parties, until the verification and coordinated-disclosure process that report documents (using the July 2026 HAWK event as its concrete template) is complete — this is not merely a courtesy but, per that report, now a well-established field norm with real institutional infrastructure (NIST's pqc-forum, vendor security-disclosure channels) built specifically to support it correctly.

### 2.3 How researchers respond to reviewer criticism

Restating companion design-process report `08` §3.2's what-convinces-reviewers material from the receiving end: legitimate reviewer criticism, per `03` §4's attachment discussion, should be treated as the field's normal, expected scrutiny process working correctly, not a personal setback — and the specific, practical discipline this section adds is **responding to reviewer criticism with the same evidentiary standard the criticism itself demands**, addressing the actual substance (a missing control, an unexamined assumption, per companion cryptanalyst-mindset report `04`'s recurring pattern) rather than either capitulating wholesale without genuine reconsideration or defending the original claim without engaging the specific critique — both of which are, in their own way, avoidance of the actual epistemic work the criticism is asking for.

Part `06` (`06-research-ethics-and-anti-patterns.md`) now covers research integrity as its own dedicated topic, and catalogs the specific, named failure patterns most common in independent research.
