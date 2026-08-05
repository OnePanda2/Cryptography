# 06 — Research Ethics and Anti-Patterns

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`05`.*

---

## 1. Research Ethics

### 1.1 Beyond "don't lie" — what research integrity actually requires

Companion design-process report `06`'s treatment of Phillip Rogaway's writing on the moral character of cryptographic work already establishes that requirements-engineering should include an ethical dimension — not just what a design defends against, but what the capability is actually for. This section extends that same instinct to the *conduct* of research itself, across the specific dimensions your outline names.

### 1.2 Responsible disclosure

Covered in full process detail in companion cryptanalyst-mindset report `08` §1.1 and restated in `05` §2.2 of this report; the ethical framing this section adds: responsible disclosure isn't merely a professional courtesy or a way to avoid reputational cost — it's a direct expression of the field's actual purpose (companion evaluation-methodology report `01` §1's argument for why evaluation exists at all) applied to timing specifically: a finding disclosed irresponsibly can cause real, avoidable harm to real deployed systems and their users before the affected parties have any chance to respond, and the ethical weight of that harm exists independent of whether the discovering researcher's own reputation would suffer from mishandling it.

### 1.3 Scientific integrity

Restating `03`'s confirmation-bias and sunk-cost material as an ethical, not merely a methodological, matter: knowingly reporting a result more strongly than the evidence supports, or knowingly omitting a disconfirming result, is a distinct category of failure from an honest methodological mistake — the field's tolerance for the latter (correctable, expected, part of how research actually proceeds) is and should be much higher than for the former, and a researcher's own internal discipline should draw this line as sharply as the field's external judgment does.

### 1.4 Citation ethics

Directly restating companion design-process report `04` §2's "ignoring prior art" mistake and companion cryptanalyst-mindset report `07` §2.4's publication-bias discussion from the ethical rather than purely strategic angle: citing prior work accurately and completely is not merely good practice for avoiding an awkward reviewer comment — it's the mechanism by which the field's collective knowledge stays genuinely cumulative rather than fragmenting into isolated, uncredited rediscoveries, directly serving the same field-wide function companion research-platform report `06` §4.3's knowledge-graph discussion tries to automate.

### 1.5 Reproducibility and data transparency as ethical commitments

Restating companion evaluation-methodology report `06` §1 as an ethical claim, not just a methodological one: withholding the data or code underlying a published claim — even without any dishonesty in the claim itself — denies the field the ability to verify it, which companion evaluation-methodology report `01` §1 identifies as the entire mechanism by which trust in cryptographic research gets manufactured; a researcher genuinely committed to that mechanism working should treat publishing reproducible artifacts as an obligation flowing directly from having made a public claim at all, not an optional courtesy.

### 1.6 Experimental honesty and reporting uncertainty

Directly restating `03` §5.3's speculation-versus-evidence discipline and companion evaluation-methodology report `01` §3's epistemic hierarchy: a researcher should report exactly the confidence level their evidence actually supports — no more, and, just as importantly, no less than genuinely warranted, since understating a well-supported result is its own, subtler failure of honest communication, not merely an excess of caution.

### 1.7 Avoiding exaggerated claims

Restating companion cryptanalyst-mindset report `07` §2.2–2.3's related-key-AES-256-significance and distinguisher-versus-break discussions as the standing template: a technically accurate claim can still be exaggerated in its *framing* — reporting a real but practically-limited-significance result in language that implies broader alarm than the evidence supports is a distinct failure mode from outright fabrication, and one this report's companions have shown recurring often enough across the field's history to deserve its own explicit, named discipline rather than being treated as a lesser concern.

### 1.8 Research responsibility, synthesized

Restating this entire section's throughline: research integrity in this field is not a separate checklist appended to the technical work — it is, per companion evaluation-methodology report `01` §1's founding argument, the actual mechanism that makes the technical work mean anything to anyone beyond the researcher who produced it. A brilliant result, reported dishonestly or irreproducibly, contributes nothing to the field's actual, cumulative confidence — it is, in the terms this entire report series has used throughout, not evidence at all, however technically correct it might privately be.

---

## 2. Research Anti-Patterns

A compact catalog, cross-referencing this report's own earlier files and its companions rather than re-deriving what's already covered, with a small number of genuinely new entries added.

| Anti-pattern | Where it's covered | New content here |
|---|---|---|
| Confirmation bias | `03` §2 | — |
| Sunk-cost fallacy | `03` §3 | — |
| Ignoring prior art | `01` §3.1; companion design-process report `04` §2.4 | — |
| Overclaiming | §1.7 above; companion cryptanalyst-mindset report `07` §2 | — |
| Weak baselines | Companion design-process report `04` §2.1, `08` §5 | — |
| Incomplete evaluation | Companion evaluation-methodology report `02`, `06` §2 | — |
| Poor documentation | `02` §3; companion evaluation-methodology report `06` §1 | — |
| Unclear hypotheses | `02` §1.1 | — |
| **Moving goalposts** | `02` §1.2 | Distinguished explicitly there from legitimate hypothesis refinement — repeated here because it's specifically named in your outline as its own anti-pattern, not merely a hypothesis-formation issue |
| **Metric chasing** | New | Optimizing a candidate specifically against whichever metric is cheapest or most flattering to compute, rather than the metric that actually matters for the stated goal — the research-strategy-level version of companion evaluation-methodology report `03` §3's statistical-significance-versus-effect-size confusion, and companion design-process report `08` §2.1's "confusing diffusion with security" mistake generalized: a researcher who reports SAC compliance because it's easy to compute and impressive-looking, while never running the full evaluation ladder because it's harder, is metric-chasing in exactly this sense |
| **Publication pressure** | New | The specific failure mode where external pressure (a funding deadline, career stakes per `04` §1.8's career-risk category, a competitor's rumored progress) drives premature publication before `02` §1.5's success criteria are genuinely met — the single most common proximate cause, per this report's synthesis of its companions' failure catalogs, of results that later require correction or retraction, and worth naming as a distinct pressure from any purely cognitive bias, since it can corrupt good judgment even in a researcher genuinely free of confirmation bias or sunk-cost attachment |

### 2.1 The synthesized lesson, restated one final time for this file

Every entry in this table, including the two new ones, traces back to the same root this entire report has argued from its first page: **a pre-committed, written kill criterion and success criterion (`02` §1.4–1.5), checked honestly against a research log (`02` §3) rather than memory, is the single most effective defense against essentially every anti-pattern in this list simultaneously** — not because it eliminates the underlying psychological or institutional pressures, but because it gives the researcher a fixed, external reference point to check their own behavior against, precisely when internal judgment is least reliable.

Part `07` (`07-historical-case-studies-of-decision-making.md`) now applies this entire report's framework against the specific historical research programs your outline names — not for their technical content, already covered elsewhere in this conversation, but specifically for their decision points.
