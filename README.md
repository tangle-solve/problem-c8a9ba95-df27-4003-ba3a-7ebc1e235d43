<!-- solve-problem:c8a9ba95-df27-4003-ba3a-7ebc1e235d43 -->
# Deterministic worst-case polylogarithmic dynamic maximal matching

A [Tangle Solve problem](https://solve.tangle.tools/problems/c8a9ba95-df27-4003-ba3a-7ebc1e235d43).

## What is the problem?

Maintain a maximal matching in a dynamic graph subject to an adversarial sequence of edge insertions and deletions, using a **deterministic** algorithm with **polylog(n)** **worst-case** update time.

Formal setting: an undirected graph G = (V, E), |V| = n, initially empty or given up front. An adversary adaptively chooses updates (insert(e) / delete(e)). After each update, the algorithm must report a matching M ⊆ E that is *maximal* (every edge not in M shares an endpoint with some edge in M). The complexity measure is the worst-case time per update over the whole adversarial sequence. The goal: worst-case update time O(log^c n) for constant c, with no randomization.

The three requirements compound: (1) *maximality* (not approximate maximum — no approximation ratio is required, only maximality); (2) *determinism* (no private randomness; the adversary sees all algorithmic choices); (3) *worst-case* update time (no amortization, no "deamortization debt" to be paid off later — relevant to real-time guarantees).

## Why does it matter?

Maximal matching is the canonical minimal dynamic graph structure and the base case for dynamic edge coloring (Δ+1 colors), vertex cover 2-approximation, and distributed symmetry breaking. Deterministic worst-case polylog updates would supply predictable latency for real-time systems and would likely transfer: the techniques typically yield deterministic distributed algorithms and deterministic dynamic algorithms for these downstream problems. Randomized dynamic algorithms eventually fail with probability 1 under infinite adversarial sequences, so derandomizing is not cosmetic.

## What's already known?

- **Amortized randomized:** the classic baseline is O(log n) amortized expected update time (Baswana, Gupta, Sen 2011-style).
- **Worst-case randomized:** Neiman & Solomon; Bernstein & Stein (SODA 2015/2016) achieved O(log^2 n) worst-case expected time.
- **Deterministic amortized:** Bhattacharya, Henzinger, Nanongkai gave deterministic amortized polylog algorithms for vertex cover / matching (STOC/FOCS ~2016–2018).
- **Deterministic worst-case:** progress via derandomizing the randomized machinery conditionally and unconditionally — e.g., deterministic worst-case updates with time roughly Õ(n^{1/3}) or better for matching/vertex cover, with the exponent improving through a sequence of papers (Bhattacharya–Chakrabarty–Henzinger–Nanongkai line of work; 2022–2024 results reached near-constant or constant approximation in n^{o(1)}-type deterministic worst-case time — (verify exact current-best)). Christiansen (2025) and STOC 2025 results push deterministic dynamic matching further, but none achieves deterministic polylog worst-case for maximal matching.

The gap: known deterministic worst-case bounds are polynomially super-polylog, while polylog is known only with randomness or amortization.

## What would count as solving it?

1. **Upper bound (positive resolution):** a deterministic algorithm maintaining a maximal matching with worst-case update time O(log^c n) for explicit constant c, with a full correctness and worst-case-time proof.
2. **Lower bound (negative resolution):** a proof that any deterministic algorithm with worst-case update time o(n^ε) for some ε > 0 cannot maintain a maximal matching against an adaptive adversary (conditioned or unconditional; even a conditional lower bound from OMv would be significant but does not fully close the problem unless it rules out polylog).
3. **Intermediate milestones:** any deterministic worst-case update time of the form 2^{O(√log n)} or n^{o(1)}, or deterministic worst-case polylog for (2+ε)-approximate maximum matching.

## How to start

1. Read Bernstein & Stein (SODA 2015) "Deterministic partially dynamic single source shortest paths in polynomial time" is the wrong paper — read their worst-case "Fully dynamic matching in polynomial time in worst case" (ICALP 2015) to understand the deamortization of randomized amortized maximal matching and the "lazy" short-augmenting-path structure.
2. Read the Bhattacharya–Henzinger–Nanongkai deterministic dynamic vertex-cover papers to see the current best derandomization toolkit (deterministic density-gradient/degree-class arguments replacing random ranks).
3. Identify the exact obstruction: the randomized algorithms use random ranks/orientations whose conditional expectations must be maintained deterministically; check whether the recent method of *deterministic conditional expectations via local recomputation* (used in 2023–2025 deterministic dynamic results) can be made worst-case rather than amortized.
4. Main obstacle: derandomization typically introduces long "cascade" recomputations; deamortizing these cascades deterministically (without the random shuffle that lets Bernstein–Stein truncate them) is the crux.

## Key references

- **Baswana, Gupta, Sen, "Fully Dynamic Maximal Matching in O(log n) Update Time" (SIAM J. Comput. 2011; arXiv:1103.1902).** Randomized amortized O(log n) algorithm; the baseline structure (levels, random ranks) all later work builds on.
- **Neiman, Solomon, "Simple deterministic algorithms for fully dynamic maximal matching" (STOC 2013).** Deterministic amortized O(√n) algorithm.
- **Bernstein, Stein, "Fully Dynamic Matching in Polynomial Time in Worst Case" (ICALP 2015).** Worst-case (deamortized) randomized O(n^ε / ε^2)... reduces the problem to a "short paths" matching problem.
- **Abraham et al., "A deterministic almost fully dynamic 2+ε approximate matching" / Bernstein–Stein follow-ups** — (verify exact citations) — worst-case randomized O(log^2 n) for maximal matching.
- **Bhattacharya, Henzling, Nanongkai et al., deterministic dynamic vertex cover / fractional matching papers (FOCS 2017 onward).** The deterministic amortized machinery whose worst-case adaptation is the current frontier.
- **Christiansen, "Algorithms for Dynamic Graphs" (PhD thesis / dissertation, 2025).** Consolidates recent deterministic dynamic matching results; the census cites it as the state of the art — read for the exact current gap.

## Warnings

- The problem is about **maximal** matching, not maximum or approximate-maximum; don't conflate. An n^{1−ε}-approximate maximum matching is not a valid answer.
- "Worst-case" excludes amortized results; many strong deterministic results are amortized only, and deamortization here is genuinely hard, not bookkeeping.
- The adversary is adaptive and the algorithm deterministic, so any "with high probability" statement is vacuous; avoid carrying whp machinery over silently.
- The census cites 2025 works the solver should verify directly; I have not confirmed their exact statements.

---
*`ALGO-000124` · unverified*

## Acceptance guidance

1. **Upper bound (positive resolution):** a deterministic algorithm maintaining a maximal matching with worst-case update time O(log^c n) for explicit constant c, with a full correctness and worst-case-time proof.
2. **Lower bound (negative resolution):** a proof that any deterministic algorithm with worst-case update time o(n^ε) for some ε > 0 cannot maintain a maximal matching against an adaptive adversary (conditioned or unconditional; even a conditional lower bound from OMv would be significant but does not fully close the problem unless it rules out polylog).
3. **Intermediate milestones:** any deterministic worst-case update time of the form 2^{O(√log n)} or n^{o(1)}, or deterministic worst-case polylog for (2+ε)-approximate maximum matching.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/c8a9ba95-df27-4003-ba3a-7ebc1e235d43?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000124.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000124)
