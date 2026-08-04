# The Intellectual History of Modern Cryptography, 1993–2026

**Query:** "How did modern cryptographic design evolve from approximately 1993 through 2026, and what sequence of discoveries led to today's best portable cryptographic primitives?"
**Organizing principle:** chronological, not topical — a reconstruction of the field's actual sequence of discovery, motivation, and response, not a reference map.
**Report date:** August 2, 2026

---

## A note on method, read before the rest

This report draws on two things: (1) well-established, stable historical record for 1993–roughly 2020, which does not require re-verification by search — these are settled facts (publication years, competition dates, who broke what) the way "the AES competition ended in 2000" doesn't change based on when you ask; and (2) extensive live search grounding for 2020–2026 specifically, carried out over the course of this same conversation while producing two companion reports (a general cryptography survey and an ARX-cryptography-specific deep dive, both in your file history) — the current-state findings from that search work (NIST PQC standardization status, the July 2026 AI-cryptanalysis event, ChaCha's creeping cryptanalytic record, Ascon's finalization, the compressing quantum-hardware timeline, MILP/SAT tooling advances) are carried into this report's later chapters rather than re-fetched, since nothing about those facts has changed in the interim and re-searching would be pure redundancy.

**On scope**: a true year-by-year account with full paper-level detail for 34 years would be several hundred pages. This report resolves that the way a real historian resolves it — by identifying the actual *inflection points* (years where something genuinely changed the field's trajectory) and giving those real depth, while treating quieter years as the connective tissue between inflections rather than pretending every year contributed equally. Nothing important is skipped; the *density* of treatment tracks the actual density of consequential events, which is itself a historically honest choice, not a shortcut.

---

## How this report is organized

Ten files, in reading order:

| File | Covers |
|---|---|
| `00-INDEX-executive-summary.md` | This file |
| `01-timeline-1993-2001-classical-foundations-to-aes.md` | From Matsui's linear cryptanalysis and the random oracle model through the AES competition and its selection |
| `02-timeline-2002-2008-hash-crisis-arx-emerges.md` | MD5/SHA-1's collapse, the provable-security ROM critique, eSTREAM, and ARX's arrival via Salsa20/ChaCha |
| `03-timeline-2009-2015-sha3-curve25519-snowden.md` | The SHA-3 competition and sponge-construction theory, Curve25519/Ed25519, the Dual_EC_DRBG backdoor and Snowden, CAESAR |
| `04-timeline-2016-2020-pqc-opens-neural-cryptanalysis-begins.md` | NIST opens PQC standardization, TLS 1.3, differential-neural cryptanalysis's birth, Sparkle/Alzette and the long-trail strategy |
| `05-timeline-2021-2026-standardization-and-ai-cryptanalysis.md` | PQC finalization, SIKE's fall, Ascon's win, the compressing quantum timeline, and the July 2026 AI-cryptanalysis event |
| `06-evolution-of-ideas-by-concept.md` | Cross-cutting: how ARX, SPN, sponges, AEAD, diffusion metrics, randomness testing, MILP/SAT, and formal-proof frameworks each evolved as continuous threads across every era above |
| `07-dead-ends-and-five-year-trends.md` | What looked promising and failed, and what each five-year window was actually excited about |
| `08-research-culture-and-major-researchers.md` | How the field's institutions and norms changed (competitions, peer review, disclosure, benchmarking), and profiles of the researchers this history traces |
| `09-reading-roadmap-top100-top50-bibliography-future.md` | A chronological reading order that doubles as the top 100 papers, a text-form paper dependency graph, the top 50 milestones, and future directions |

---

## Executive Summary — the shape of the whole story

If the 33 years since 1993 have a single throughline, it's this: **cryptography moved from a craft practiced by a small number of trusted designers working mostly alone or in small closed teams, toward an open, adversarial, competition-driven, formally-scrutinized discipline where a design earns trust only by surviving the entire world trying to break it — and every major institutional and technical change in this period is, in one way or another, a consequence of that shift.**

The mechanism driving the shift was **repeated, painful demonstration that closed or lightly-scrutinized design fails**: DES's NSA-influenced but eventually-vindicated S-boxes (Part `01`) showed classified expertise *could* exceed open knowledge, which briefly cut the other way — but the pattern that actually stuck, and recurred at every subsequent decision point, was the reverse: MD5 and SHA-1's slow-motion collapse through the 2000s (Part `02`) happened to hash functions designed by small teams without an open competition process; the response — the SHA-3 competition (Part `03`) — was explicitly modeled on AES's open, multi-round, public-cryptanalysis process specifically because that process had *worked*. The Dual_EC_DRBG episode (Part `03`) and the 2013 Snowden disclosures turned "was this designed transparently, with auditable, reproducible parameter generation" from a nice-to-have into a first-order trust criterion the field has never relaxed since — it's the direct reason Curve25519 displaced NIST curves for new design, the direct reason NIST's own PQC process (Part `04`–`05`) was run as an open, multi-round competition with public cryptanalysis invited at every stage, and the direct reason ISO rejected NSA-designed Speck/Simon in 2018 (Part `04`) despite no cryptanalytic weakness ever being found in them — provenance and process became, functionally, part of the security model, not just the mathematics.

The second throughline is **the automation of cryptanalysis itself**. Through the 1990s and 2000s, differential and linear cryptanalysis (Part `01`) were craft skills — a strong cryptanalyst hand-constructed trails through careful, laborious characteristic-chaining. Mouha, Wang, Gu, Preneel's 2011 insight that this search could be posed as a Mixed-Integer Linear Programming problem (Part `04`) — followed by Sun et al.'s 2014 extension to ARX's addition operator specifically — turned trail-finding from a craft into a mechanically-verifiable computation, and this one methodological shift is arguably as consequential to *how the field actually works today* as any single new cipher: it's why a 2026-era security evaluation without MILP/SAT trail-search evidence is now considered incomplete, not merely weaker, and it set up the later, even more striking development — the field's first genuinely AI-conducted cryptanalytic discovery, against HAWK, in July 2026 (Part `05`), which reads, in the long view this report takes, less like a sudden rupture and more like the logical next step after three decades of steadily automating what a human cryptanalyst used to do by hand.

The third throughline, and the one your prompt specifically asked to be traced with care: **portable ARX cryptography's rise is a direct, traceable response to a specific, dated vulnerability class** — table-lookup-based software AES's cache-timing weakness, understood and published by the mid-2000s (Part `02`), years before AES-NI hardware (2010) made the problem moot for anyone with new-enough silicon. Bernstein's Salsa20 (2005) and ChaCha (2008) exist specifically because, at that moment, there was no good answer for anyone running AES in software on hardware without dedicated acceleration — and the reason ARX *remained* competitive for the next twenty years, through BLAKE2/3, through SipHash, through the entire NIST Lightweight Cryptography competition (Part `04`–`05`), is that the underlying condition never fully went away: mobile chipsets, embedded microcontrollers, and IoT silicon without AES-NI-equivalent acceleration remain common in 2026, and general-purpose SIMD width (the thing ARX actually rides on for its performance) kept growing on exactly the timeline needed to keep ARX designs' throughput competitive without anyone ever needing to build ARX-specific hardware.

---

## Evolution Map — the field's major conceptual transitions, at a glance

| From | To | Roughly when | Why |
|---|---|---|---|
| Hand-derived differential/linear cryptanalysis | MILP/SAT-automated trail search | 2011–2016 | Manual search couldn't scale to modern word sizes/round counts; automation made trail-finding mechanically verifiable |
| Closed/small-team hash function design | Open, multi-round, publicly-cryptanalyzed competitions (AES → SHA-3 → PQC → LWC) | 1997 → 2016, four iterations | AES's process visibly worked; MD5/SHA-1's closed-process failures visibly didn't |
| Trust-by-authority curve/parameter generation | Rigid, auditable, "nothing up my sleeve" generation (Curve25519) | 2006–2013, crystallized by Dual_EC/Snowden | A demonstrated real backdoor made unauditable provenance an active liability, not a neutral unknown |
| Table-lookup software ciphers | ARX / constant-time-by-construction design | 2005 onward | Cache-timing attacks on software AES were real, published, and pre-dated hardware acceleration by years |
| Random-oracle-model-only security proofs | Standard-model proofs preferred where affordable, ROM proofs explicitly flagged as heuristic | 2004 onward (Canetti-Goldreich-Halevi) | A proof of the ROM's *theoretical* insufficiency, even though practical ROM constructions kept working |
| Ad hoc, informal security margin ("more rounds than anyone's broken") | Provable margin via wide-trail (SPN) and long-trail (ARX) strategies | 2001 (AES) → 2020 (Sparkle/Alzette) | Designers wanted a *proof* no attack below a stated bound exists, not just an absence-of-known-attacks claim |
| Classical-only cryptanalysis | Classical + differential-neural (ML-based) cryptanalysis | 2019 (Gohr, CRYPTO) onward | Neural networks empirically matched or exceeded hand-derived distinguishers on real benchmark ciphers |
| Human-only cryptanalytic research | Human-directed, substantially autonomous AI cryptanalytic research | July 2026 (Anthropic/HAWK) | The first verified, standards-altering result from a largely autonomous AI research effort |
| RSA/ECC as the presumed permanent public-key foundation | Post-quantum migration as an active, funded, deadline-driven engineering program | 1994 (Shor) → 2016 (NIST PQC opens) → 2024 (FIPS finalized) | A 22-year gap between the theoretical threat and the first finalized standard, driven by the field needing that long to develop, compete, and scrutinize replacement mathematics it trusted as much as RSA/ECC |

Part `01` (`01-timeline-1993-2001-classical-foundations-to-aes.md`) begins the chronological account.
