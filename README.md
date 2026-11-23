# The Music of the Primes — a formalization triage

Alessandro Masullo [released](https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/)
a 940-page draft proof of the **Generalized Riemann Hypothesis** on 2026-11-20, with an
invitation to *"formalize, fill any small gaps if they occur"*.

This repo takes that invitation and answers one mechanical question: **what does the GRH
claim actually rest on?** The deduction is encoded step by step in
[2b4m](https://github.com/ityonemo/2b4m), every assumption is named, and the checker
enumerates the dependencies. The output is a sorted list of what is classical, what the
author asserts, and what he still owes.

**Status:** `2b4m check .` passes. **Five logical defects in the argument are catalogued,
and one of them breaks the deduction of GRH** — see §3.

---

## 1. Strategy

**Top-down, with holes.** State the GRH deduction first; make every supporting result a
`hole`, a stated-but-unproved assumption. Replace each hole with the manuscript's own proof,
whose citations become new holes. Repeat. Strict `check` rejects a bare hole, so a failing
check *is* the worklist.

**Holes are sorted by who owes the work.** That is the whole method:

| | means | strict `check` |
|---|---|---|
| bare `hole` | the manuscript proves it — **ours** to transcribe | **fails** |
| `cites "standard: …"` | classical, with a textbook source | passes, disclosed |
| `cites "assertion: …"` | the author asserts it rather than proving it | passes, disclosed |
| `cites "gap: …"` | absent from the released text | passes, disclosed |
| `cites "refuted: …"` | shown false, by an argument checked here | passes, disclosed |

**Stay coarse on purpose.** Zeros, test functions and L-functions are opaque sorts; no
measure theory or complex analysis is modelled. That altitude answers *"does the chain reach
GRH, and what is missing?"* — not *"is the analysis correct?"*

**Shim the classical foundations.** Where a step needs analysis this encoding does not build,
`standard/haar-integration.b4m` states the named classical theorem (quotient measure,
Grothendieck's kernel theorem, Lidskii, Schwartz kernels, …) and the consumer *derives* from
it. That makes the **dependence** checked — each step's inputs are theorems a reader can look
up — without pretending the analysis is verified.

## 2. What the claim rests on

`2b4m check --axioms grh.b4m generalizedRiemannHypothesis` reports **60 entries**:

| | n | whose |
|---|---|---|
| bare holes | 0 | — |
| `standard:` | 20 | nobody — classical, cited and closed |
| `assertion:` | 28 | the author's, or a reader's to close |
| `gap:` | 3 | the author's — nothing in the text supports them |
| `refuted:` | 2 | the author's — false as written |
| definitional | 7 | introduce symbols, not assumptions |

Roughly half the dependency closure (211 of 429 results) sits in appendices the author titles
*"Preparations from…"* — textbook material, cited rather than reproved, which is what makes
the job finite.

```sh
2b4m check .                                  # everything — passes
2b4m check --axioms grh.b4m generalizedRiemannHypothesis
grep -rn 'cites "assertion:' .                # the 28 candidates to close
```

**`--axioms` is the deliverable, not the green checkmark.** Read the statements it lists, not
their names.

## 3. The flaws

Five logical defects, in [**`LOGICAL-FINDINGS.md`**](LOGICAL-FINDINGS.md) with the quotations
and line numbers to check each one:

1. **A direct contradiction** — §1.4.6 and §1.7.2 argue Weil's Positivity Criterion *cannot*
   be used; §14.1.2 derives the GRH theorem *from* it. Both cannot hold, and §14.1.2 is the
   only written derivation of that theorem.
2. **An incompleteness inside a proof marked complete** — Thm 12.4.1.1's own footnote says its
   cutoff definition must be extended and a convergence study is outstanding. The Trace
   Formula depends on it.
3. **A stale reference standing in for a missing argument** — §1.4.5 attributes its key step to
   "§5.1", which is about topological vector spaces. The argument is nowhere else either.
4. **A false set identification** — two descriptions of a stratum are asserted equal; they
   differ, by cancellation of local valuations under the norm.
5. **The Montel step has no uniform bound** — the Archimedean integrals diverge, so the
   theorem §1.4.3 invokes does not apply. **This one breaks the deduction**, and it holds even
   granting the Trace Formula.

Findings 4 and 5 are Jenny Nielsen's (`NIEIOM.pdf`), formalized in `nielsen.b4m`. The method
surfaces defects of this kind because encoding a step forces its justification to be
*located*, and a step whose justification cannot be located becomes a hole the checker will
not pass.

**None of this disproves GRH**, and none of it impugns the 513 completed proofs beneath
Chapter 12. The Trace Formula is a real achievement with a real proof; it is the deduction
*from* it that does not hold.

## 4. Next steps

### a. Patch the assertions into implications

The 28 `assertion:` entries are the live surface. Each is a step the author states without
proving, and most are plausibly consequences of results he proves elsewhere.

The pass: for each assertion, assume good faith — that it *is* a consequence of an earlier
theorem — and rewrite the hole as an **implication** from that theorem to the asserted step.
The assertion then stops being a free-standing assumption and becomes a named dependency on
something the manuscript establishes. Where no such theorem exists, that absence is itself the
finding, and the hole becomes a `gap:`.

This needs no new foundations, which is what makes it the next move rather than a later one.

### b. Thread the classical tier down to `std`

The `standard:` citations currently bottom out in prose: a formula plus a textbook reference a
human checks. Each could instead be *proved* from [2b4m's standard
library](https://github.com/ityonemo/2b4m/tree/main/std), turning a citation into a derivation.

The dependency shape is already right for this. A `standard:` hole presents a fixed interface
to its consumers, so replacing its citation with a proof changes nothing downstream — which is
what made citing-first reversible rather than a shortcut. The order to attack it in is cheapest
first: the order and arithmetic facts are reachable from `std/real.b4m` today; Haar measure,
nuclear operators and trace-class theory are a foundations project, measured in
[`WHAT-IT-WOULD-TAKE.md`](WHAT-IT-WOULD-TAKE.md).

Done fully, the artifact would rest on nothing but `std` and the author's own assertions — and
the second list is the one worth sending him.

---

## Files

| | |
|---|---|
| `grh.b4m` | Masullo's chain — the §1.4 GRH deduction |
| `nielsen.b4m` | Nielsen's objection, kernel-checked |
| `trace-formula/` | Thm 12.5.1.1, proved from the manuscript's own skeleton |
| `nielsen-objection/`, `standard/`, `shared/` | dependencies: her two propositions; the classical tier; shared vocabulary |
| [`LOGICAL-FINDINGS.md`](LOGICAL-FINDINGS.md) | the five defects, with line references |
| `DEPENDENCY-MAP.md` | the full frontier, with manuscript locations |
| `ASSESSMENT.md` | what the draft contains, chapter by chapter |
| `WHAT-IT-WOULD-TAKE.md` | measured scope of a real formalization |
| `the-music-of-the-primes.{pdf,txt}` | the draft; the text extraction citations index by line |
| `NIEIOM.pdf` | Nielsen, *A Failure of Uniform Boundedness…* (Nov 2026) |
