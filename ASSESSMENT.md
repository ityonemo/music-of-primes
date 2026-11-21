# "The Music of the Primes" (Masullo) — what the draft contains, for formalization triage

Written 2026-11-20 against the release PDF (940 pp., pdfTeX, dated 2026-11-20), announced by
Ravi Vakil at <https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/>
with the explicit invitation to "formalize, fill any small gaps if they occur".

Every claim below is a direct quotation or a page/line count from the released file, so each
is checkable. Line numbers refer to `the-music-of-the-primes.txt` (`pdftotext -layout`).

**Read this as a triage note on a self-declared DRAFT, not as a referee's verdict.** The
announcement is explicit that the manuscript is unpolished and was released ahead of plan:
the intention was "to bring Alessandro to Stanford for a year to polish the writing and then
announce it after running it by enough people," and Brian Conrad's months-long feedback is
still being incorporated. The incompletenesses catalogued below are therefore expected
features of a draft at this stage — the author's own `Finish!` and `Insert statement`
footnotes are him marking his own work, not something uncovered. What follows is only an
inventory of WHERE the writing stops, because that is what determines what can be formalized
today.

## The bottom line for a formalizer

**The chapter that derives GRH is a two-page stub.** The deduction exists only as a prose
OUTLINE in the introduction (§1.4). So there is no Chapter-13 argument to formalize: a
formalization would be of §1.4's sketch, with its own gaps, not of a written proof.

That is a statement about the DRAFT's completeness, not about whether the mathematics works.
The analytic machinery underneath is extensive and largely written out.

## Chapter lengths — the shape of the gap

| chapter | pages | |
|---|---|---|
| 1 Introduction | 46 | contains the only GRH derivation, as an outline (§1.4) |
| 2-12 (the machinery) | 14-70 each | adelic θ-functions … Trace Formula, Geometric Side |
| **13 Zeros of Hecke L-functions** | **2** | **the GRH chapter. No proof.** |
| 14 Complements | 14 | |
| A-D appendices | 50-197 each | |

Chapter 13 runs pp. 521-522; p. 523 is Chapter 14. Its three sections are all listed at
p. 522 in its own table of contents — i.e. §13.1 and §13.2 are headings with no body.

## What Chapter 13 actually says

It restates the Trace Formula (as Theorem 13.3.1.1) and then:

    Theorem 13.3.1.2 (Divisor of L(s, χ)).
      2
      3

    1 Correct statement.
    2 Insert statement.
    3 State and prove the Main Theorems, also explaining how to deduce them by
      approximation φ → e^{-s|log(t)|} as Re(s) > 1.

(lines 31645-31650.) Footnotes 1-3 are the author's own. Theorem 13.3.1.2 — the result that
would assert the zeros lie on the critical line — has no statement and no proof. Footnote 1
says even the restated Trace Formula needs correcting.

## The machinery IS there

Not a stub: **241 titled results** (113 Theorem, 67 Lemma, 45 Proposition, 16 Corollary) plus
56 titled Definitions, and **513 completed proofs** (`□`).

Counting method, so the figures are reproducible: a titled result is a line beginning
`Theorem 1.2.3 (Title)` etc. in `the-music-of-the-primes.txt`; proofs are `□` marks.
Counting every `Theorem 1.2.3` mention instead gives 184, but that includes in-text
citations, so it overstates declarations — an earlier "~420" figure in this file came from a
method of that kind and was not reproducible. The declared main theorem, the **Trace
Formula (Theorem 12.5.1.1, p. 519)**, is fully stated and carries a real multi-page proof.

So the work's achievement is the Trace Formula. GRH is claimed as a consequence, and it is
that consequence which is unwritten.

## The intended derivation (§1.4), and its gaps

§1.4 sketches the deduction in ~5 pages, and it is a genuine argument, not hand-waving:

1. §1.4.1 specialise the Trace Formula character-by-character;
2. §1.4.2 approximate the holomorphic family `φ_z(t) = t^{-z}` (t>1, 0 otherwise) by smooth
   compactly supported test functions satisfying `φ(C_F^1) = 0`;
3. §1.4.3 **Montel's theorem** to extract a subsequence along which convergence is uniform,
   so the limit is holomorphic in z;
4. §1.4.4 the non-Archimedean sum tends to `L'_f(s+1/2, χ) / L_f(s+1/2, χ)`;
5. §1.4.5 conclude that `L'_f/L_f` is holomorphic on `C_{>0} - {1/2}`, hence `L(s,χ)` has no
   zeros on `C_{>1/2}`, and by the functional equation none on `C_{<1/2}`.

Gaps visible in that outline itself:

- **Two unfinished footnotes land in §1.4** — `5Finish!` and `6Finish!` (lines 1255-1256),
  attached to the Trace Formula statement that the whole deduction runs on.
- **§1.4.5 cites "§5.1"** for the step that assembles the conclusion, but §5.1 is "The
  topological vector spaces of adelic θ-functions and Idelic Z-functions". Apparently a stale
  reference; the actual justification is not located.
- The interchange of limits in steps 2-4 (mollifier `ε → 0`, bump `h → 1`, and the infinite
  sum over non-Archimedean places) is where the difficulty is, and the outline says so:
  "they are the source of all the difficulties". This is exactly the part Chapter 13 was
  meant to write out.

## A documentation inconsistency worth flagging to the author

The draft contradicts itself about its own central mechanism:

- §1.4.6 and §1.7.2: "**Weil's Positivity Criterion is not used.**" With a reason —
  positivity needs `φ = ψ1 * ψ2`, and the only such φ with `φ(C_F^1) = 0` is identically zero.
- §14.1.2 (line 31925) and line 32160: Theorem 13.3.1.2 is "a consequence of the Trace
  Formula combined with the Positivity Criterion (in its form from **Theorem ??**)".

The `??` is an unresolved LaTeX reference — that theorem is not in the document. Most likely
Chapter 14 is older text written when positivity WAS the intended route, left unupdated. It
matters because a reader auditing the GRH step finds Ch. 14 pointing at a mechanism the
introduction explicitly disowns.

## Unresolved cross-references (12 total)

`??` appears 12 times, meaning LaTeX could not resolve the label. Three sit in the GRH
argument (lines 31643, 31925, 32160 — all the Positivity Criterion). The rest: Caveat (1592),
Lemma (3577, 4666), Definition (9835), equation (13129), Chapter (37800, 46144), §43303,
equations (52640, 53205, 53217).

## What formalization could usefully do here

1. **Formalize the Trace Formula's STATEMENT** (Thm 12.5.1.1) and the §1.4 derivation as an
   explicit chain, with the unwritten steps as declared holes. That converts "is this a
   proof?" into a mechanical question and localises precisely what is owed.
2. **Do not** attempt to formalize Chapter 13: there is nothing there to check.
3. Ask the author to resolve the positivity contradiction and the §5.1 reference — both are
   cheap to fix and both currently obstruct a reader following the main argument.

The honest summary for an evaluator: **a large, serious body of analytic work, released
mid-draft, whose headline corollary is outlined in the introduction but not yet written out
where it is meant to live.** Whether the outline closes is a mathematical question this note
does not attempt to answer — it cannot be answered from the released text, because the step
in question is the one not written.
