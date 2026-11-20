# What formalizing this manuscript would actually take

Measured 2026-11-20, so a decision about pursuing it rests on numbers rather than vibes.
Short version: **the manuscript's prerequisites and `std`'s contents barely overlap.**

## The two inventories

`std` today — 2392 theorems, 207k lines, 149 files — is **algebra and arithmetic**:

> collection complex counted cyclic divisibility element equivalence field function group
> indexed integer isomorphism nested-sum order peano permutation primes rational real
> relation ring sequence set sized subgroup

The manuscript runs on **analysis, topology and measure theory**. Occurrence counts in the
released text:

| prerequisite | uses | in `std`? |
|---|---|---|
| locally convex (TVS) | 867 | no |
| nuclear (operators/spaces) | 606 | no |
| Schwartz-Bruhat | 498 | no |
| locally compact | 434 | no |
| holomorphic | 331 | no |
| Fourier | 261 | no |
| meromorphic | 259 | no |
| Haar measure | 256 | no |
| idelic / ideles | 251 / 42 | no |
| distribution | 222 | no |
| Hecke (L-functions) | 203 | no |
| Sobolev | 166 | no |
| topological group | 156 | no |
| measurable | 153 | no |
| adelic / adele | 145 / 60 | no |
| Hilbert space | 130 | no |
| Banach | 118 | no |
| Pettis integration | 45 | no |

Nothing in that column exists. Not thin — absent.

## Scale

- Transitive closure of the Trace Formula (Thm 12.5.1.1) alone: **196 results** across 11
  chapters — 57 theorems, 52 lemmas, 35 propositions, 22 definitions, 9 corollaries. That is
  ~70% of the manuscript's 286 indexed results.
- `std` measures **~86 lines of 2b4m per theorem**. So those 196 results are ~17k lines — IF
  their prerequisites existed.
- They do not. Appendices A-D (~400 pages) are the prerequisites, and they are themselves
  built on the absent analysis: locally convex TVS theory, nuclear operators, Pettis
  integration, Schwartz-Bruhat spaces.

Taken literally — proving everything from first principles — this is Mathlib-scale
foundation work before the manuscript's own content starts. Mathlib took a decade and
hundreds of contributors to cover comparable ground.

## The lever: `cites "standard: …"` for well-known mathematics

**But we do not have to prove the textbooks.** A cited hole is the honest home for a
borrowed result, and `examples/openai-wiles/` already uses three tiers of citation:

    cites "standard: a connected, locally path-connected space is path-connected"
    cites "Fulton, Intersection Theory [14]: ch_4(O_Z) = [Z]"
    cites "manuscript Prop 12.3: …"

Applied here, that changes the arithmetic, because **the manuscript tells us which half is
textbook**. Its appendices are titled, by the author:

> Appendix A. **Preparations from** Algebraic Number Theory (50pp)
> Appendix B. **Preparations from** Analytic Number Theory (68pp)
> Appendix C. **Preparations on** locally convex topological vector spaces (88pp)
> Appendix D. Schwartz-Bruhat functions (197pp)

Measured on the full closure (matching appendix ids too):

| | results | |
|---|---|---|
| closure of Thm 12.5.1.1 | **429** | |
| manuscript body, chs. 1-14 | 218 | the author's original work — ours to transcribe |
| appendices A-E, "Preparations" | **211 (49%)** | textbook material — `standard:` / named-reference cited holes |

And 211 understates it: appendix results enter this count only when cited BY ID, while much
of that material is used implicitly.

So roughly **half the dependency closure is citable rather than provable**, and the half that
remains is the part that is actually about this proof. That is a tractable target in a way
"formalize the analysis" is not — and it keeps the artifact honest, because `--axioms`
prints every `standard:` assumption, so a reader sees exactly which textbooks the result
leans on.

The judgement call this introduces: **where to draw the `standard:` line.** Citing "Haar
measure exists on a locally compact group" is uncontroversial; citing something the
manuscript's own argument turns on would hollow out the exercise. The line belongs at
results a competent analyst would accept without checking — and every one of them stays
visible in the axiom report.

**A `standard:` citation is a POSITION, not a permanent verdict.** Same discipline as any
`hole`: it says "not formalized here, and here is what we are leaning on". Haar theory,
locally convex TVS, Schwartz-Bruhat spaces can each be formalized later, on their own
schedule, and each one that lands converts its citations into real theorems without touching
the consumers. That is what makes citing-first safe rather than a shortcut:

- nothing hides — `--axioms` prints every cited hole with its locator, so the set of borrowed
  results is always enumerable;
- the order is demand-driven — a foundation gets built when something actually needs it more
  precisely than a citation allows, which is `std`'s standing rule anyway;
- and the artifact is honest at every stage, because the citation text says what was assumed
  rather than silently asserting it as an axiom.

The failure mode to avoid is the opposite one: relabelling a borrowed result as an `axiom`,
which is exactly how an unproved step stops being visible.

## Matrices are on the path, and they are not the hard part

Formalizing this needs linear algebra, and `[[PAINPOINTS 8b]]` has it deferred pending a
good design. Worth being precise about what is needed:

- The manuscript's 21 uses of "matrix" all mean the **matrix trace of a continuous operator
  with respect to a Schauder basis** — an infinite-dimensional trace as a convergent sum of
  diagonal entries. Not finite arrays.
- It uses basis/bases 474 times and "vector space" 607, with **Schauder bases** central
  (Def 4.1.1.1: a bi-orthogonal sequence in `E × E*` whose coefficient functionals are
  continuous).

So the aata-scale need (finite matrices over a ring, `det`, `GL_2`) does not subsume this
one. And the mechanism is NOT missing — parameterization goes through `model`
(`[[sort-parameterization-via-model]]`) — so matrices are a design task, not a blocker.
The blocker is everything in the table above.

The one mechanism gap that WOULD bite: a parameter that is a **bound variable**. No model
targets a `fix`-block eigenvariable. Fixed dimensions as consts are fine; the manuscript's
Schauder indices run over all of ℕ, so a theorem quantified over the index would hit it.

## What is worth doing instead, in order of value per hour

1. **`grh.b4m` as it stands** — the chain is kernel-checked and the holes name who owes what.
   Already done.
2. **Deepen the dependency graph one layer at a time.** Replace a bare hole with the
   manuscript's own citations as new bare holes, repeat. This produces a machine-checked
   dependency map of the manuscript — something no one else has — and `--axioms` answers
   "what does the GRH claim rest on" at any depth. It also mechanically surfaces circularity
   and stale references; 12 unresolved `??` are already known.
3. **One depth probe at full fidelity** — a self-contained result from Appendix B.7 (Euler
   products on the 1-line) — to measure the real cost per result rather than extrapolating
   from `std`'s 86 lines/theorem, which was measured on algebra.
4. **The analysis foundations** — only the parts that are NOT citable. With appendices A-E
   at `standard:`, this shrinks from "build Mathlib" to "state the interfaces the manuscript
   actually uses", which is a different and much smaller problem.

(2) and (3) are the honest contributions a prover can make to a draft at this stage. (4) is a
different project with a different budget.
