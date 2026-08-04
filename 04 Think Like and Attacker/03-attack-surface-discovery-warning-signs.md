# 03 — Attack Surface Discovery: The Warning-Sign Catalog

*Part of a multi-file report — see `00-INDEX-executive-summary.md` for the map. Assumes `01`–`02`. Each entry below states what the warning sign looks like on first inspection, what deeper problem it's a proxy for, and how it's cheaply confirmed or dismissed before committing to expensive analysis — this is the field guide `02`'s Stages 3–4 actually run against.*

---

## Weak diffusion

**What it looks like**: a state-dependency graph (companion design-process report `02` §3.3) with a large diameter relative to the design's stated round count — some pair of input/output bits provably or empirically doesn't influence each other until many more rounds than the design claims are needed. **What it's a proxy for**: localized structure surviving long enough into the cipher for a differential or linear characteristic to exploit it directly, without needing any subtle mathematics — the crudest, most direct route to a break. **Cheap confirmation**: a dependency-matrix heatmap (companion ARX report `03` §2.1) computed over a handful of rounds; a genuinely weak-diffusion design shows visibly incomplete mixing well before the design's full round count.

## Weak nonlinearity

**What it looks like**: for SPN-family designs, an S-box with poor differential-uniformity or nonlinearity figures relative to the theoretical optimum for its size (companion history report `01`'s AES-S-box discussion gives the positive baseline to compare against); for ARX designs, a round function with too few additions, or additions whose carry chains get reset too early by aggressive rotation placement (companion design-process report `02` §2.3). **What it's a proxy for**: the confusion half of Shannon's confusion/diffusion pair being under-delivered, leaving the cipher closer to a linear (or near-linear) transformation than its round count would suggest. **Cheap confirmation**: for SPN, compute the S-box's differential distribution table and Walsh spectrum directly (small, exact, fast for any realistic S-box size); for ARX, estimate nonlinearity accumulation per round via the Lipmaa-Moriai formula (companion ARX report `01` §2.5) applied to the specific addition placements.

## Poor mixing (cross-lane, not just per-bit)

**What it looks like**: a topology (companion design-process report `02` §3.1) where some grouping of state words never appears together in the same operation across the full round count — a structural, not merely statistical, gap. **What it's a proxy for**: an entire class of meet-in-the-middle or divide-and-conquer attacks (companion ARX report `04` §2.6) that specifically exploit a state's decomposability into independently-analyzable parts. **Cheap confirmation**: trace, by hand or via a small script, which lane-groups interact across the stated round count — this is often visible directly from the specification's diagram without needing any computation at all.

## Equivalent states

**What it looks like**: two distinct (key, state) or (key, nonce) pairs that produce identical subsequent behavior — a collision not in the cryptographic-hash sense, but in the "the cipher can no longer distinguish these two configurations" sense. **What it's a proxy for**: an effective key or state space smaller than the nominal one, silently reducing the real security level below the claimed one. **Cheap confirmation**: check whether the key/nonce-injection schedule (companion design-process report `02` §2.4) has any structural redundancy — e.g., does XORing key material at two different points ever let two different keys produce the same effective round-key sequence?

## Symmetry

**What it looks like**: identical or simply-related structure across rounds — the primary target of rotational and RX-cryptanalysis (companion ARX report `04` §2.4) and slide attacks. **What it's a proxy for**: an attacker's ability to relate the cipher's behavior on one input to its behavior on a *transformed* version of that input (rotated, sliced) with much higher probability than a generic, asymmetric design would allow. **Cheap confirmation**: check whether round constants (companion design-process report `02` §2.4) actually break rotational invariance — apply the design's own stated rotation to a round constant and check it doesn't map to another round constant already in use.

## Fixed points

**What it looks like**: an input to the round function (or the full permutation) that maps to itself. **What it's a proxy for**: on their own, fixed points aren't automatically dangerous (every permutation on a large enough domain has some), but an unusually *large number* of fixed points, or fixed points reachable via a simply-describable input class, is a proxy for the permutation deviating measurably from "behaves like a uniformly random permutation" — precisely the property an ideal-permutation-model security proof (companion ARX report `05` §3.2) depends on holding. **Cheap confirmation**: for small enough state sizes, exhaustive or sampled search for fixed points and comparison against the count a truly random permutation of the same size would be expected to have.

## Invariant subspaces

**What it looks like**: a nonlinear (or affine) subspace of the state space that the round function maps back into itself, regardless of key — the specific, historically damaging pattern named explicitly in companion ARX report `04` §2.8. **What it's a proxy for**: an entire class of weak keys or a full-blown distinguisher/key-recovery route that survives arbitrarily many rounds, because the round function's action on the invariant subspace never actually "mixes" it back into the general state space. **Cheap confirmation**: this is genuinely one of the harder warning signs to check cheaply — it typically requires either an automated search (companion ARX report `05`) or a specific, informed guess about where an invariant subspace might live, usually motivated by the round function's algebraic structure (a round function built from operations that individually preserve some algebraic property is the natural place to look first).

## Low algebraic degree

**What it looks like**: the round function, expressed as a Boolean polynomial in its input bits, has surprisingly low total degree relative to the number of rounds — checked via the same higher-order-derivative framing (Lai, 1994, companion history report `09`) that underlies both integral attacks and cube attacks. **What it's a proxy for**: cube attacks and integral/higher-order-differential attacks specifically (companion ARX report `04` §2.9) — a low-enough algebraic degree after R rounds means the output, summed over a structured input subset of the right size, is provably zero (or otherwise predictable), independent of any statistical argument. **Cheap confirmation**: for each round, track how quickly the algebraic degree of the round function composed with itself grows (many designs state this explicitly in their own security rationale, precisely because it's a standard, expected check — Keccak's own designers tracked and published this figure, per `04`'s case study, and third-party researchers' zero-sum and cube-attack work directly built on exactly this tracked quantity).

## Weak constants

**What it looks like**: round constants generated by a process that isn't fully specified, isn't reproducible, or turns out (on inspection) to share unexpected structure — small Hamming weight across all constants, a shared low-order-bit pattern, or values close to what a "trivial" (all-zero, sequential-counter) choice would produce. **What it's a proxy for**: both the rotational-symmetry concern above, and, separately, the provenance-trust concern this conversation's companion history report traces from Dual_EC_DRBG through 2013 (`03`) — a constant sequence that isn't obviously "nothing up my sleeve" invites suspicion regardless of whether an actual weakness is ever found, because the suspicion itself has a real, if softer, cost to the design's institutional trust. **Cheap confirmation**: check the constant-generation procedure is fully, independently reproducible from the specification alone (companion design-process report `02` §2.4), and run the symmetry check above against the actual constant values.

## Weak topology

**What it looks like**: covered from the design side in companion design-process report `02` §3, the cryptanalyst-side mirror is checking whether the *specific* grouping choices (which lanes interact in which operations) leave any subset of lanes under-connected relative to the rest — often visible directly from a state-dependency-graph sketch (`02` §3.3 of the companion report) without needing computation.

## Predictable carries

**ARX-specific**: covered mathematically in companion ARX report `01` §2.2 — a carry chain whose behavior can be predicted or biased with non-trivial probability, given partial knowledge of the inputs, is the literal mechanism differential cryptanalysis against ARX exploits. **What raises this specific flag on first read**: additions where both operands come from earlier, already-partially-known state (rather than fresh, unpredictable material), especially early in a design's initialization/key-schedule phase where an attacker often has the most control or knowledge over inputs.

## Poor extraction

**Relevant to PRNG and sponge-squeeze design specifically (companion ARX report `06` §1.4)**: an output-extraction step that doesn't fully "consume" the internal state's entropy before releasing it — checked by asking whether the extraction function is a proper randomness extractor (companion general-cryptography report `04` §2.2's leftover-hash-lemma framing) or a naive, structure-preserving operation (a raw truncation, a simple linear combination) that might leak exploitable structure from the internal state directly into the output.

## Weak key schedules

**What it looks like**: a key schedule that's simpler, more linear, or more structurally regular than the round function it feeds — companion ARX report `04` §2.7's related-key-attack discussion is the direct consequence. **Cheap confirmation**: check whether the key schedule alone (ignoring the round function entirely) has any of this file's other warning signs — weak diffusion, symmetry, low algebraic degree — since a key schedule is, structurally, just another small cipher, and the same checklist applies to it recursively.

## Weak permutations (as a keyed vs. unkeyed distinction)

**The specific, easily-missed warning sign this report's Keccak case study (`04`) exists to teach**: a permutation whose security case was made and validated for one use context (unkeyed, as a hash function's building block) doesn't automatically carry over to a different use context (keyed, as a stream cipher or MAC's building block) — per `01` §2's fourth bullet, this is a standing, specific thing to check whenever a specification repurposes an existing, trusted permutation into a new mode.

## Weak state layouts

Covered from the design side in companion design-process report `02` §1; the cryptanalyst-side check is whether the *chosen* state layout leaves any structurally-privileged subset of bits (a specific word, a specific lane) that receives systematically less mixing than the rest across the stated round count — often the direct, visible consequence of the "weak topology" and "weak diffusion" entries above, rather than an independent phenomenon.

## Incomplete avalanche

The direct empirical symptom of "weak diffusion" above, measured via SAC/BIC (companion ARX report `03` §2.1–2.2) — listed separately here only because it's the specific, cheap, mechanical *test* a cryptanalyst actually runs, as distinct from "weak diffusion" being the underlying structural property the test is checking for.

## Insufficient rounds

**What it looks like**: a design whose stated round count leaves uncomfortably little margin above the best attack the design's *own* analysis found — the direct target of `02` Stage 5–6's escalating differential/linear/RX search, and the single most common, least subtle warning sign in this entire catalog, precisely because it's the one every serious specification is required to address explicitly (companion design-process report `04` §1.2's reviewer-checklist item on stated margin) — meaning a cryptanalyst's job here is less about *discovering* the concern (the design itself states its own margin) and more about independently *verifying* the design's own margin claim wasn't optimistic, exactly the funnel Stages 5–6 in `02` describe.

## Automatic warning signs — a compact triage list

For a cryptanalyst triaging many specifications with limited time (a competition round with dozens of candidates, say — precisely the situation every major NIST competition this conversation's companion history report traces creates), the fastest, cheapest checks, roughly in the order an experienced reviewer runs them: (1) does the round-constant generation reproduce independently and break rotational symmetry; (2) does the stated active-component-count argument secretly assume independence; (3) does a quick dependency-graph sketch show full connectivity well within the stated round count; (4) is the algebraic-degree-growth-per-round figure stated, and does it look plausible; (5) does the specification's prose security claim match what its own proof section actually establishes. A design that clears all five cheaply earns the right to expensive, Stage 5–9-level scrutiny on its own merits, rather than being deprioritized relative to competitors that failed an earlier, cheaper check.

Part `04` (`04-historical-case-studies-how-weaknesses-were-found.md`) now applies this entire catalog against real, historical designs — reconstructing, case by case, which specific warning sign was the actual entry point for each major discovery.
