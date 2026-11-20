# The Music of the Primes — a formalization triage

Machine-checked triage of Alessandro Masullo's draft proof of the **Generalized Riemann
Hypothesis**, released 2026-11-20 and [announced by Ravi
Vakil](https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/)
with an explicit invitation to *"formalize, fill any small gaps if they occur"*.

## The goal

Answer one question mechanically: **what does the GRH claim actually rest on?**

Not "is the proof correct" — that is a mathematician's judgement on 940 pages of analysis.
The narrower question is answerable by a machine, and worth answering: encode the deduction
as explicit steps, name every assumption, and let the checker enumerate the dependencies. The
output is a list in which each entry is either a named textbook result, a result proved
elsewhere in the manuscript, or something the author still owes.

For a draft under community review, that list is the useful artifact. It tells a reader where
to look, and it tells the author what to send next.

## The strategy

**Start at the top and deepen.** The GRH deduction is encoded first, with every supporting
result a `hole` — a stated-but-unproved assumption. Then each hole is replaced by the
manuscript's own proof of it, whose citations become new holes. Repeat. The frontier moves
downward and `--axioms` reports it at every stage.

**Two kinds of hole, splitting the work by who owes it.** This is the core of the method:

| | means | strict `check` |
|---|---|---|
| bare `hole X` | the manuscript PROVES this — **ours** to transcribe | **fails**, keeping it on the worklist |
| `hole X cites "manuscript …"` | a true gap in the released text — **the author's**, assumed pending his work | passes, disclosed |
| `hole X cites "assertion: …"` | the author ASSERTS it rather than proving it — recorded as such | passes, disclosed |
| `hole X cites "standard: …"` | classical, with a textbook source | passes, disclosed |

An **`assertion:`** citation quotes the author's own words — "we further assume", "where we
assume", "for example, take…" — so a reader can see exactly what was claimed and judge it.
It is a statement of STATUS, not a verdict: Vakil's announcement invites readers to "fill any
small gaps if they occur", so an assertion is a candidate to pick up and close. Several are
marked CANDIDATE TO CLOSE where the step looks routine.

So a **failing check is the normal state**, and its output *is* the transcription backlog.
When every bare hole is discharged, what remains is precisely the list to send the author.

**Cite the textbooks, prove the manuscript.** Roughly half the dependency closure (211 of 429
results) sits in appendices the author himself titles *"Preparations from…"* — collected
textbook material, not his contribution. Those are cited with a real source rather than
reproved, which is what makes the job finite. Each can be formalized later on its own
schedule; nothing downstream changes when it is, so citing-first is reversible rather than a
shortcut. Every citation stays visible in `--axioms`, so nothing hides.

**Stay coarse on purpose.** Zeros, test functions and L-functions are opaque sorts; the
analytic content lives in predicates. No measure theory, no complex analysis, no adeles are
modelled. That altitude is right for *"does the chain reach GRH and what is missing?"* and
wrong for *"is the analysis correct?"* — and the files say so rather than implying more.

## What this is not

- **Not a verification of the mathematics.** See "stay coarse", above.
- **Not a verdict on the manuscript.** The draft is self-declared unpolished — the
  announcement says the plan was a year at Stanford to polish it before announcing — and the
  incompletenesses catalogued here include the author's own `Finish!` and `Insert statement`
  footnotes. He is marking his own work, and this repo is reading his markings.

## About 2b4m

[**2b4m**](https://github.com/ityonemo/2b4m) — "too big for margin", after Fermat — is a
proof checker built on an unusual premise: proofs are *not* expected to be written by humans,
but they are expected to be **extremely easy for humans to check**. Which is exactly the
situation an audit is in.

Three properties matter here:

- **Every step is explicit.** A step states its formula in full and names the rule and the
  earlier steps justifying it. No proof search, no hidden context, no tactic that quietly
  does three things. What is written is what was checked.
- **Assumptions are disclosed, never silent.** A `hole` is tracked; a theorem resting on one
  cannot pass a default check without saying so. `--axioms` prints the full dependency list
  with file and line. This is the property the whole approach is built on.
- **Citation is first-class.** `hole X cites "source"` records a borrowed result as borrowed
  — passing the check while printing what it leans on. The alternative, relabelling it an
  `axiom`, is how an unproved step stops being visible.

Repository: **https://github.com/ityonemo/2b4m**

## Files

| file | |
|---|---|
| `grh.b4m` | the GRH chain, kernel-checked, every gap a named hole |
| `PLAN.md` | how this grows: directory layout and conventions |
| `DEPENDENCY-MAP.md` | the current frontier — who owes what, with manuscript locations |
| `ASSESSMENT.md` | what the draft contains, chapter by chapter with page counts |
| `WHAT-IT-WOULD-TAKE.md` | measured scope of a real formalization |
| `the-music-of-the-primes.pdf` | the released draft (940 pp.) |
| `the-music-of-the-primes.txt` | `pdftotext -layout` extraction — the `.md` files cite its line numbers, so it is committed to keep them verifiable |

## Running it

```sh
2b4m check grh.b4m                          # the transcription backlog
2b4m check --draft --axioms grh.b4m generalizedRiemannHypothesis
```

## The headline finding

Chapter 13 — *"Zeros of Hecke L-functions"*, where GRH is deduced — is **two pages**
(pp. 521–522). Every other chapter runs 14–197 pages. Theorem 13.3.1.2, the result that
would assert the zeros lie on the critical line, has for its statement the author's footnote
`Insert statement`; §13.1 and §13.2 are headings with no body. The deduction exists only as a
prose outline in §1.4 of the introduction.

The machinery beneath it is real and extensive: ~420 stated results, **513 completed
proofs**, and a Trace Formula (Thm 12.5.1.1, p. 519) with a genuine multi-page proof. That is
the work's achievement. GRH is the corollary not yet written where it belongs.

See `ASSESSMENT.md` for the evidence and `DEPENDENCY-MAP.md` for what is owed by whom.
