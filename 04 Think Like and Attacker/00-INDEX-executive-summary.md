# How World-Class Cryptanalysts Discover Weaknesses

**Query:** "How do world-class cryptanalysts discover weaknesses in modern cryptographic designs? Not how attacks work — how experts discover the attacks in the first place, decide where to look, and know which assumptions to question."
**Report date:** August 4, 2026

---

## Read this before the rest — how this report relates to its companions

You now have four other reports in this conversation's file history: a general cryptography survey, an ARX-specific deep dive (whose Part `04` is a full, technically complete attack-family catalog), a chronological intellectual history, and a design-process report (whose Part `04` covers reviewer expectations and mistakes from the designer's side). Between them, the *mechanics* of every attack family — what differential cryptanalysis is, how MILP models addition, what a reviewer's checklist contains — are already documented in full technical depth. **This report does not re-derive those mechanics.** It answers a different, narrower, and genuinely harder question: at the moment a cryptanalyst first opens a new specification, *before* any of that machinery has been invoked, what makes them reach for one technique over another? What single detail in a round function makes an experienced reviewer's attention snag? This is the tacit, pattern-matched judgment that sits *upstream* of every technique in the companion catalog, and it's the part no reference document can capture by listing techniques — you have to reconstruct it from how real discoveries actually happened, which is what this report spends its length doing.

This turn's research added genuinely new grounding beyond what the companion reports already cover: fresh case-study material on **TinyJambu's differential-forgery discovery** (where the designers' own MILP analysis carried an unstated independence assumption between AND gates that turned out false — a textbook "which assumption to question" story), **Rocca's multi-round arms race** (a statistical weakness prompting a full-AES-round countermeasure, which itself then failed to a *different* attack vector, whose fix then proved insufficient when reused in an unrelated later cipher, HiAE), and **Keccak's zero-sum-distinguisher and cube-attack lineage** (a rare, instructive case where a real, published distinguisher exists against the *full* 24-round permutation without threatening the actual hash function at all — the sharpest available lesson in why finding a distinguisher and finding a break are not the same event).

---

## How this report is organized

| File | Answers |
|---|---|
| `00-INDEX-executive-summary.md` | This file |
| `01-the-cryptanalyst-mindset-first-contact-with-a-spec.md` | What a professional actually notices in the first five minutes with a new specification, and why |
| `02-the-reconstructed-workflow.md` | The full funnel from first read to experimental verification, with the actual decision points at each stage |
| `03-attack-surface-discovery-warning-signs.md` | The specific structural properties that trigger suspicion, and what each one is actually a proxy for |
| `04-historical-case-studies-how-weaknesses-were-found.md` | Reconstructed discovery narratives — not attack mechanics — for DES, RC4, MD5/SHA-1, Keccak, ChaCha/Salsa, Ascon, Rocca, Simon/Speck, and TinyJambu |
| `05-attack-taxonomy-through-a-discovery-lens.md` | Every major attack family, reframed around "what would make you reach for this technique first" rather than "how it works" |
| `06-automation-and-the-modern-toolkit.md` | How and when an expert actually reaches for MILP versus SAT versus a trained model, and the tool landscape |
| `07-reviewer-perspective-failed-research-and-misconceptions.md` | What convinces and what alarms a reviewer, and a catalog of cryptanalytic conclusions that didn't survive |
| `08-learning-path-top-100-papers-and-bibliography.md` | How working cryptanalysts actually built their intuition, and a curated attack-paper reading roadmap |

---

## Executive Summary — what the mindset actually is, compressed

The central finding, reconstructed across every case study this report examines: **expert cryptanalysis is not a search through attack techniques for one that happens to work. It is a search for the single place in a design where a stated or implied claim of uniformity, independence, or randomness is actually false, followed by a search for the cheapest technique that exploits exactly that falseness.** Every attack this report traces back to its discovery — TinyJambu's forgery attack, Rocca's key-recovery, Keccak's zero-sum distinguishers, RC4's statistical biases, MD5 and SHA-1's collisions — is, at its origin, a cryptanalyst noticing that a specific independence or uniformity assumption the design (implicitly or explicitly) relies on doesn't actually hold, then building the attack technique *around* that specific falseness, not applying a generic technique and hoping something breaks. TinyJambu's designers' own security proof assumed active AND gates behave independently; they don't, and that gap, not any new mathematical technique, is the entire attack. This reframes "how do you find attacks" as "how do you find false independence claims" — a genuinely different, more specific, and more teachable question than the attack-technique catalog alone can answer.

The second finding: **the mindset is fundamentally comparative and pattern-matched, not first-principles.** A cryptanalyst who has read hundreds of specifications develops the ability to recognize, almost instantly, "this round function's diffusion layer resembles X, and X was broken via technique Y" — not because they re-derive the vulnerability from scratch each time, but because they've built a mental library of structural signatures paired with the techniques that exploit them (`01`, `03`). This is why experience compounds in this field in a way that's hard to shortcut: the actual skill being built, turn after turn of reading new designs, is this pattern library, and no amount of studying attack *mechanics* in isolation substitutes for having personally worked through enough real specifications to have that library actually populated.

The third, and possibly the most practically important finding for your stated goal: **a real distinguisher against a design is not the same event as a break, and confusing the two is one of the most common errors on both sides of the field — both among newer researchers who overclaim a distinguisher's significance, and among newer designers who dismiss a real distinguisher as irrelevant without doing the work to show why.** Keccak's zero-sum distinguishers exist against the *full* 24-round permutation and have existed since close to the SHA-3 competition's early rounds — and Keccak remains completely trusted, because the distinguisher's specific structure never translates into an attack against the sponge construction's actual security claims. Learning to calibrate exactly when a structural finding matters and when it doesn't — a judgment call, not a mechanical rule — is, per this report's synthesis, the single hardest-to-teach and most valuable skill separating a working cryptanalyst from someone who has merely memorized the attack catalog.

---

## A note on confidence and sourcing

As with this conversation's design-process report, reconstructing *how a discovery actually happened* — as opposed to *what the resulting attack technically is* — relies more heavily on designers' and cryptanalysts' own retrospective framing (paper introductions, the specific "we noticed that..." moments preserved in published discovery narratives) than pure fact-reporting does, and retrospective accounts smooth over false starts the way any retrospective account does. Where this report reconstructs a discovery sequence from a paper's own stated motivation and methodology section, confidence is high. Where it infers the likely discovery process from the *pattern* of what was eventually published, absent explicit first-person narration, that inference is flagged rather than presented with equal certainty.

Part `01` begins with the actual starting point: what a cryptanalyst notices in the first five minutes.
