# GRH in "The Music of the Primes" — the dependency map

`grh.b4m` formalizes the GRH deduction the manuscript outlines in §1.4, with every step
kernel-checked. This file is the index to it.

## How to read the holes

`hole` here is a **workspace tool**, not a verdict on the manuscript. It marks "work to do,"
and the two kinds split that work by WHO owes it:

| kind | means | strict `check` |
|---|---|---|
| **bare `hole X`** | the manuscript PROVES this; it is OURS to transcribe into 2b4m | **FAILS** — stays on the worklist |
| **`hole X cites "…"`** | a true gap in the released text; assumed pending the author | passes, disclosed |

So a failing `2b4m check` is the normal state, and its output is the transcription backlog.
When every bare hole is discharged, what remains is exactly the list to send the author.

    2b4m check examples/music-of-primes/grh.b4m              # the backlog
    2b4m check --draft --axioms examples/music-of-primes/grh.b4m generalizedRiemannHypothesis

## The chain

Each arrow is a kernel-checked step in `grh.b4m`; no analysis is modelled (see "Altitude").

```
  theMollifiedApproximantsExist          [OURS]   §1.4.2 — explicit mollifier + bump
            │
            ▼  unpack φ
  theTraceFormula at φ                   [OURS]   Thm 12.5.1.1, p.519 — proved, multi-page
            │                                     (needs φ(C_F^1)=0, the standing hypothesis)
            ▼
  theLimitInterchangeIsValid             [AUTHOR] §1.4.3-4 — the infinite non-Archimedean
            │                                     sum passing to the limit
            ▼
  theNonArchimedeanSum… computes L'_f/L_f
            │
            ▼
  theLogDerivativeHasOnlyTheCentralPole  [AUTHOR] §1.4.5 — asserted, cites a stale §5.1
            │
            ▼
  theEulerProductsOnTheOneLine           [OURS]   Thms B.7.2.1, B.7.2.4 — proved
            │
            ▼
  noZerosRightOfTheCriticalLine
            │
            ▼
  theFunctionalEquationReflects…         [AUTHOR] §1.4.5, one line
            │
            ▼
  noZerosLeftOfTheCriticalLine
            │
            ▼
  everyZeroOffTheLineIsExcluded          [AUTHOR] IS Theorem 13.3.1.2 — "Insert statement"
            │
            ▼
  generalizedRiemannHypothesis
```

## OURS to transcribe (3) — bare holes, currently failing

| hole | manuscript source | status there |
|---|---|---|
| `theTraceFormula` | **Theorem 12.5.1.1**, p. 519 | stated, multi-page proof. The work's achievement. |
| `theMollifiedApproximantsExist` | **§1.4.2** | explicit construction (mollifier ρ_ε, bump h) |
| `theEulerProductsOnTheOneLine` | **Theorems B.7.2.1, B.7.2.4** | stated and proved, Appendix B.7 |

Also proved in the text and available when the chain needs it: **Theorem B.4.4.7** (Idelic
Explicit Formulas, Appendix B.4, proof written out) — what §13.1 was to recall. Not currently
cited by `grh.b4m`, and an uncited hole is invisible, so it is recorded here rather than
declared.

## The AUTHOR's gaps (4) — cited holes, assumed pending his work

Each is a `cites` string in `grh.b4m` recording what is assumed and why the released text
does not supply it.

1. **`theLimitInterchangeIsValid`** — §1.4.3–1.4.4. Montel's theorem gives uniform
   convergence for the **finitely many Archimedean** integrals. The non-Archimedean sum is
   infinite, and the author names it himself: *"they are the source of all the difficulties:
   their sum approximates L'_f(s+1/2, χ)/L_f(s+1/2, χ)."* That the interchange is valid is
   asserted, not proved. **This is the mathematical heart of the gap.**

2. **`theLogDerivativeHasOnlyTheCentralPole`** — §1.4.5. Asserted, with the justification
   attributed to "§5.1" — but §5.1 is *"The topological vector spaces of adelic θ-functions
   and Idelic Z-functions"*, so the reference is stale and the argument is not located. The
   author's own `Finish!` footnotes (5 and 6) attach to §1.4's Trace Formula statement.

3. **`theFunctionalEquationReflectsTheZeroFreeRegion`** — §1.4.5, a single line: *"so by the
   functional equation the same holds for L(s, χ) over C_{<1/2}."* The functional equation is
   in the manuscript; this deduction from it is not written out. Likely genuinely routine.

4. **`everyZeroOffTheLineIsExcluded`** — **this is Theorem 13.3.1.2**, the result that would
   assert GRH. Its statement in the released draft is the author's footnote
   `Insert statement`, and §13.1/§13.2 are headings with no body (Ch. 13 is pp. 521–522,
   while every other chapter runs 14–197 pages).

## Two documentation issues to raise with the author

Neither is mathematical; both obstruct a reader following the main argument.

- **The positivity contradiction.** §1.4.6 and §1.7.2 state *"Weil's Positivity Criterion is
  **not** used"*, with a reason (positivity needs φ = ψ₁∗ψ₂, and the only such φ with
  φ(C_F^1)=0 is identically zero). But §14.1.2 and p. 32160 say Theorem 13.3.1.2 follows from
  *"the Trace Formula combined with the Positivity Criterion (in its form from **Theorem ??**)"*
  — an unresolved reference. Chapter 14 reads as older text from when positivity was the
  intended route.
- **The stale §5.1 reference** in §1.4.5, above.

Unresolved `??` references total 12; three of them are this Positivity Criterion.

## Altitude

Deliberately coarse. `Zero`, `TestFunction`, `Complex` are opaque sorts; the analytic content
lives in predicates (`theTraceFormulaHoldsAt`, `theNonArchimedeanSumComputesTheLogDerivative`,
…). No measure theory, no complex analysis, no adeles are modelled.

That is the right altitude for the question being asked — *does the cited chain reach GRH,
and what is missing?* — and it keeps every hole's statement readable against the manuscript.
Formalizing the **analysis** is a different and vastly larger undertaking; this file is the
scaffold that says where it would have to go.
