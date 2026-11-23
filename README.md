# The Music of the Primes — a formalization triage

Machine-checked triage of Alessandro Masullo's draft proof of the **Generalized Riemann
Hypothesis**, released 2026-11-20 and [announced by Ravi
Vakil](https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/)
with an explicit invitation to *"formalize, fill any small gaps if they occur"*.

**Status:** two steps of the GRH deduction are refuted; the Trace Formula underneath it is
not, and none of this disproves GRH. Details below.

## Findings

### 1. Two steps are refuted

Jenny Nielsen's *A Failure of Uniform Boundedness* (Nov 2026, `NIEIOM.pdf`) exhibits
counterexamples to two assertions the deduction relies on. Her argument is formalized in
`nielsen.b4m`, which **passes a strict check on ten citations, every one classical**.

**Montel's theorem does not apply (§1.4.3, repeated §1.8.4.3).** The deduction needs the
Archimedean integrals locally uniformly bounded. For the trivial character over ℚ the
positive-real part contains `∫₁^{1+δ} φₙ(t)·t^{−1/2}/(t−1) dt`, whose liminf is `+∞` by
Fatou — the pointwise limit has a logarithmic singularity at `t = 1`. Each individual `φₙ`
vanishes near 1 so its own integral is finite, but the vanishing neighbourhood shrinks, and
no *uniform* bound survives. This is the step Masullo himself calls *"the source of all the
difficulties"*.

**A norm-preimage identification is false (p. 493, §11.3.1.1).** Two descriptions of a
stratum are asserted equal; in `F = ℚ(i)` the prime 5 splits into two places both `≅ ℚ₅`, so
local norms are the identity and `g = (5, 5⁻¹, 1, …)` has unit norm everywhere by
cancellation while having no unit components.

*Granted, by explicit decision:* Fatou's lemma, the divergence of `∫dt/(t−1)`, the splitting
of 5 in ℚ(i), and the standard meaning of *mollifier* — a term Masullo uses three times in
940 pages and never defines. *Checked:* that those facts compose into the conclusions,
including the inference from divergence to no-uniform-bound, which Nielsen's paper joins in
the two words "In particular" and which is where a quantifier error would hide.

**This repo flagged both sentences independently, before the counterexample was published** —
as unargued assertions, quoting them. That is the hole-first discipline doing its job: it
could not prove those steps, declined to assume them quietly, and the steps it could not
prove turned out to be false.

### 2. Chapter 13 is two pages

*"Zeros of Hecke L-functions"*, where GRH is deduced, runs pp. 521–522. Every other chapter
is 14–197 pages. Theorem 13.3.1.2 — the result that would assert the zeros lie on the
critical line — has for its statement the author's footnote `Insert statement`; §13.1 and
§13.2 are headings with no body. The deduction exists only as the §1.4 prose outline.

The machinery beneath it is real and extensive: **241 titled results and 513 completed
proofs** (`□` marks) across 940 pages, and a Trace Formula (Thm 12.5.1.1, p. 519) with a
genuine multi-page proof, which `trace-formula/` reconstructs from his own skeleton. That is
the work's achievement, and the main objection does not touch it.

### 3. An incompleteness the author flagged himself

Theorem 12.4.1.1's proof — which the Trace Formula depends on — carries his footnotes
*"Include section."* and *"Extend Definition 9.3.3.1 … Return here and change the choice of
smooth (λ,ε)-cutoff … Then study the convergence of the integral in (12.4.1.23)."* So the
cutoff definition that proof needs is, by his own note, not yet the one he has. Unlike the
Chapter 13 gaps, this sits inside a result presented as complete.

### 4. Documentation issues

§1.4.6 and §1.7.2 state Weil's Positivity Criterion is *not* used, while §14.1.2 derives
Theorem 13.3.1.2 from it via an unresolved `Theorem ??` — Chapter 14 reads as older text from
when positivity was the intended route. §1.4.5 attributes its assembly to "§5.1", which is
about topological vector spaces.

Unresolved `??` cross-references appear on **14 lines**, six of them in labelled form
(3 × `Theorem ??`, 2 × `Lemma ??`, 1 × `Definition ??`).

`LOGICAL-FINDINGS.md` has the structural pass in full: the positivity contradiction, the
footnote-flagged incompleteness inside Thm 12.4.1.1, the stale §5.1 reference, and the false
set identification — each with the quotations and line numbers to check it. Also what the pass
did NOT find, which bounds the rest: no circularity, and the Trace Formula's own derivation is
sound as structure.

See `DEPENDENCY-MAP.md` for the full frontier and `ASSESSMENT.md` for the chapter-by-chapter
evidence.

## The goal

Answer one question mechanically: **what does the GRH claim actually rest on?**

Not "is the proof correct" — that is a mathematician's judgement on 940 pages of analysis.
The narrower question is answerable by a machine, and worth answering: encode the deduction
as explicit steps, name every assumption, and let the checker enumerate the dependencies. The
output is a list in which each entry is either a named textbook result, a result proved
elsewhere in the manuscript, or something the author still owes.

For a draft under community review, that list is the useful artifact. It tells a reader where
to look, and it tells the author what to send next.

**It also turned out to find errors.** Two sentences this repo flagged as unargued
assertions — quoting them, months-of-pagination deep in a 940-page draft — were later shown
false by a published counterexample. That objection is now itself formalized here
(`nielsen.b4m`) and checks. Neither outcome was the goal; both fell out of refusing to assume
a step quietly.

## The strategy

**Start at the top and deepen.** The GRH deduction is encoded first, with every supporting
result a `hole` — a stated-but-unproved assumption. Then each hole is replaced by the
manuscript's own proof of it, whose citations become new holes. Repeat. The frontier moves
downward and `--axioms` reports it at every stage.

**Holes split the work by WHO OWES IT.** This is the core of the method, and the taxonomy
earned every tier the hard way:

| | means | strict `check` |
|---|---|---|
| bare `hole X` | the manuscript PROVES this — **ours** to transcribe | **fails**, keeping it on the worklist |
| `cites "refuted: …"` | shown FALSE as written, by an argument checked here | passes, disclosed |
| `cites "gap: …"` | absent from the released text — **the author's** | passes, disclosed |
| `cites "assertion: …"` | the author ASSERTS it rather than proving it | passes, disclosed |
| `cites "standard: …"` | classical, with a textbook source | passes, disclosed |

An **`assertion:`** citation quotes the author's own words — "we further assume", "where we
assume", "for example, take…" — so a reader can see exactly what was claimed and judge it.
Vakil's announcement invites readers to "fill any small gaps if they occur", so an assertion
is a candidate to pick up and close.

**Two tiers were tried and retired, and the reasons generalize:**

- `manuscript-proved:` — for a result the text proves but we have not transcribed. Wrong,
  because it let *our own backlog* pass a strict check. A result the manuscript proves is
  ours to do, so it is a BARE hole; the reference lives in a `// TRANSCRIBE:` comment.
- `refuted:` *on a reading alone* — wrong for the same reason in a different costume: it put
  a credence into the axiom record as though it were established. The tier is legitimate only
  once the refuting argument is itself formalized and the facts it grants are named.

The common failure both represent: **borrowing authority the artifact has not earned.** The
tier system exists to make that impossible to do quietly.

A **failing check was the normal state** during transcription, and its output *was* the
backlog. That backlog is now empty: every bare hole has been discharged, so what remains is
precisely the list to send the author.

**How the bare holes were closed, since "closed" can mean relabelling and here does not.**
Each bottomed out in one of the manuscript's own results, whose proof rests on classical
analysis this encoding does not build. The move was to SHIM the classical foundation —
`standard/haar-integration.b4m` states six named theorems (quotient-measure / Weil's formula,
Grothendieck's kernel theorem, Lidskii, trace under subspace restriction, periodization
sections, the Schwartz kernel theorem) with textbook sources, and each consumer then DERIVES
from them. What remains at each site is the author's reading of his own setting, recorded as
`assertion:`.

The signature that this was not relabelling: **the entry count ROSE, 54 → 60, while bare
holes fell to zero.** One bundled transcription task becomes a classical citation plus one or
two located readings. Three of the manuscript's opaque implication chains were removed
outright in the process, and the dead-step lint caught one step that had become vacuous.

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

- **Not a verification of the mathematics.** See "stay coarse", above. What the kernel checks
  is that stated facts COMPOSE into stated conclusions. Where a classical fact is granted
  rather than proved, the citation says so under "WHAT IS GRANTED".
- **Not a disproof of GRH.** The two refuted steps are in the passage FROM the Trace Formula
  TO GRH. The Trace Formula itself is untouched by the main objection.
- **Not a verdict on the manuscript's value.** The draft is self-declared unpolished — the
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

Two top-level chains, each with its dependencies in a subdirectory:

| file | |
|---|---|
| `grh.b4m` | **Masullo's chain** — the §1.4 GRH deduction, every gap a named hole |
| `nielsen.b4m` | **Nielsen's objection** — her two counterexamples, kernel-checked |
| `trace-formula/` | Thm 12.5.1.1, proved from the manuscript's own skeleton |
| `nielsen-objection/` | the two propositions behind `nielsen.b4m` |
| `standard/` | the classical tier: analysis, algebraic number theory, operator theory |
| `shared/vocabulary.b4m` | sorts and predicates both chains use — declared ONCE, since identity in 2b4m is by origin, not spelling |

| document | |
|---|---|
| `LOGICAL-FINDINGS.md` | **the structural pass** — four defects in the argument's logic, each verified with line references |
| `DEPENDENCY-MAP.md` | the current frontier — who owes what, with manuscript locations |
| `ASSESSMENT.md` | what the draft contains, chapter by chapter with page counts |
| `WHAT-IT-WOULD-TAKE.md` | measured scope of a real formalization |
| `PLAN.md` | directory layout and conventions |
| `the-music-of-the-primes.pdf` | the released draft (940 pp.) |
| `the-music-of-the-primes.txt` | `pdftotext -layout` extraction — citations reference its line numbers, so it is committed to keep them verifiable |
| `NIEIOM.pdf` | Nielsen, *A Failure of Uniform Boundedness in Masullo's Proposed Proof of the GRH* (Nov 2026) |

## Running it

```sh
2b4m check .                                 # everything — passes
2b4m check --axioms grh.b4m generalizedRiemannHypothesis   # the dependency list
```

**Both chains now pass strictly.** Every assumption is either classical, the author's own
assertion, or refuted — none is ours. `grep` the tiers to see which:

Grep the tiers to get any one list:

```sh
grep -rn 'cites "refuted:'   .               # steps shown false
grep -rn 'cites "gap:'       .               # absent from the released text
grep -rn 'cites "assertion:' .               # candidates a reader could close
grep -rn 'TRANSCRIBE:'       .               # our own backlog
```
