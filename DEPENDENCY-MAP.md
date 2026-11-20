# GRH in "The Music of the Primes" — the dependency map

`grh.b4m` formalizes the GRH deduction the manuscript outlines in §1.4, with every step
kernel-checked. This file is the index to it.

## The current frontier, exactly

`2b4m check --draft --axioms grh.b4m generalizedRiemannHypothesis` reports **13 entries**,
and they are three different kinds — worth separating, because only the first is work we owe:

| kind | n | |
|---|---|---|
| bare `hole` — **ours to transcribe** | **7** | 2 in `grh.b4m`, 5 in `trace-formula/` |
| `hole … cites` — assumed | 5 | 4 the author's gaps + `addingNothingChangesNothing` (standard: x+0=x) |
| DEFINITION clause | 1 | `theTraceFormulaHoldsAt`, defined as the equality of the two sides |

A definition is not an assumption — it introduces a symbol rather than constraining a
primitive, which is why `--axioms` labels it separately. Count it apart when quoting totals.

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
  theTraceFormula at φ                   [CLOSED] proved in trace-formula/, which now owes
            │                                     6 located holes of its own
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

## OURS to transcribe (8) — bare holes, currently failing

**`theTraceFormula` is CLOSED** (2026-11-20): Theorem 12.5.1.1 is now proved in
`trace-formula/trace-formula.b4m` from the manuscript's own skeleton, and `grh.b4m` cites it.
Its six dependencies became the new frontier — which is the deepening move working as
intended: one opaque assumption replaced by six located ones.

In `grh.b4m`:

| hole | manuscript source | status there |
|---|---|---|
| `theMollifiedApproximantsExist` | **§1.4.2** | explicit construction (mollifier ρ_ε, bump h) |
| `theEulerProductsOnTheOneLine` | **Theorems B.7.2.1, B.7.2.4** | stated and proved, Appendix B.7 |

In `trace-formula/trace-formula.b4m` (the Trace Formula's own proof, txt 31539-31567):

| hole | manuscript source | status there |
|---|---|---|
| `anAdmissibleSetOfPlacesExists` | proof ¶1 | by construction; rests on Appendix A (S-units, class group) |
| `theGeometricKernelDecomposition` | **eq (11.3.1.4)** | the kernel splits along the diagonal |
| `theOrbitalTracesVanishOffS` | **Theorem 11.3.2.1**, txt 29947 | proved |
| `theSLocalTraceCalculation` | **Theorem 12.4.1.1**, txt 31123 | proved |
| `theSpectralSideIsTheKernelIntegral` | **Def 10.4.3.7** + **Thm 10.4.2.8**, txt 28655 / 27947 | both proved |
| `theTwoSidesCombine` | proof's last line | assembly only |

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

## Where the deepening stops being structural

`theTraceFormula` decomposed cleanly because its proof is BOOKKEEPING: choose S, decompose,
observe a vanishing, identify two sides, substitute. Each step is a citation or an equation,
so the kernel can check the assembly and the frontier moves down.

Its dependencies are not like that. **Theorem 11.3.2.1** (Orbital Traces on the Paley-Wiener
Levels, txt 29947) is the analysis itself: indicator functions of shrinking idele
neighbourhoods, smooth (ε, Λ)-cutoffs, and convergence in the Hardy-Sobolev space
`W_I^s(C_F)`. Its proof cites eleven results, of which three are classical —

| cited | what it is |
|---|---|
| Thm A.5.4.1 | Lebesgue Differentiation over a Hausdorff locally compact abelian group |
| Thm A.5.3.7 | Hardy-Littlewood Global Maximal Theorem |
| Prop B.4.1.2 | Poisson Summation Formula (jump singularities) |

— and the rest are the author's own nuclearity machinery (Thms 7.4.2.1, 10.3.2.1, 11.1.2.1,
11.2.2.1, 12.2.2.2, Defs 9.3.3.1, 12.1.1.1).

**Decomposing it further would be dishonest at this altitude.** Splitting an analytic
argument into opaque predicates named after its steps does not check anything — it only moves
the assumption and makes the artifact look deeper than it is. The three classical inputs are
captured in `standard/analysis.b4m` with real sources, which is useful; the analytic core
stays one bare hole until someone has the foundations to state it properly.

That is the honest boundary of this approach, and it is worth knowing where it falls: the
STRUCTURE of the Trace Formula's derivation is now machine-checked; the ANALYSIS underneath
it is not, and cannot be without the measure theory and functional analysis
`WHAT-IT-WOULD-TAKE.md` measures.

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
