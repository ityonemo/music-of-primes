# The Music of the Primes — a formalization triage

Machine-checked triage of Alessandro Masullo's draft proof of the **Generalized Riemann
Hypothesis**, released 2026-11-20 and [announced by Ravi
Vakil](https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/)
with an explicit invitation to "formalize, fill any small gaps if they occur".

Written in [2b4m](https://github.com/ityonemo/2b4m), a theorem prover whose `--axioms`
report enumerates exactly what a result depends on — which is the property this work turns
on.

## What this is, and what it is not

**It is** a kernel-checked encoding of the GRH deduction the manuscript outlines in §1.4,
with every assumption named and attributed. Run it and the checker tells you what the claim
rests on.

**It is not** a verification of the mathematics. The encoding is deliberately coarse: zeros,
test functions and L-functions are opaque sorts, and the analytic content lives in
predicates. No measure theory, no complex analysis, no adeles are modelled. That altitude is
right for the question *"does the cited chain reach GRH, and what is missing?"* and wrong for
*"is the analysis correct?"*

**It is not a verdict on the manuscript.** The draft is self-declared unpolished — the
announcement says the plan was a year at Stanford to polish it before announcing — and the
incompletenesses catalogued here include the author's own `Finish!` and `Insert statement`
footnotes. He is marking his own work.

## Files

| file | |
|---|---|
| `grh.b4m` | the GRH chain, kernel-checked, every gap a named hole |
| `DEPENDENCY-MAP.md` | the chain, both hole sets with manuscript locations, documentation issues to raise |
| `ASSESSMENT.md` | what the draft contains: chapter-by-chapter, with page counts |
| `WHAT-IT-WOULD-TAKE.md` | measured scope of a real formalization |
| `the-music-of-the-primes.pdf` | the released draft (940 pp.) |
| `the-music-of-the-primes.txt` | `pdftotext -layout` extraction — the `.md` files cite its line numbers |

## Running it

```sh
2b4m check grh.b4m                          # the transcription backlog
2b4m check --draft --axioms grh.b4m generalizedRiemannHypothesis
```

A **failing** `check` is the normal state. `hole` here is a workspace tool, not a verdict,
and the two kinds split the work by who owes it:

| | means | strict `check` |
|---|---|---|
| bare `hole X` | the manuscript PROVES it — ours to transcribe | **fails**, staying on the backlog |
| `hole X cites "…"` | a true gap in the released text — assumed pending the author | passes, disclosed |

So the checker's output *is* the worklist, and when every bare hole is discharged, what
remains is precisely the list to send the author.

## The headline finding

Chapter 13 — *"Zeros of Hecke L-functions"*, where GRH is deduced — is **two pages**
(pp. 521–522). Every other chapter runs 14–197 pages. Theorem 13.3.1.2, the result that
would assert the zeros lie on the critical line, has for its statement the author's footnote
`Insert statement`; §13.1 and §13.2 are headings with no body.

The machinery beneath it is real and extensive: ~420 stated results, **513 completed
proofs**, and a Trace Formula (Thm 12.5.1.1, p. 519) with a genuine multi-page proof. That is
the work's achievement. GRH is the corollary not yet written where it belongs.

See `ASSESSMENT.md` for the evidence and `DEPENDENCY-MAP.md` for what is owed by whom.
