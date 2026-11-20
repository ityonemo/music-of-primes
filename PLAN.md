# How to organize this formalization as it grows

Written 2026-11-20, when the repo is one flat `grh.b4m` with 7 holes. That is fine today and
will not survive the first deepening pass, because each bare hole expands into its own
dependency fan. This is the layout to grow into, and the conventions that keep it honest.

## The shape

```
music-of-primes/
├── README.md                     what this is / is not; the hole convention
├── PLAN.md                       this file
├── ASSESSMENT.md                 what the draft contains (page counts, author's TODOs)
├── WHAT-IT-WOULD-TAKE.md         measured scope: the 429-result closure, 49% citable
├── DEPENDENCY-MAP.md             the current frontier: who owes what
│
├── grh.b4m                       TOP: the §1.4 chain. The entry point.
│
├── trace-formula/                Thm 12.5.1.1 and its four direct dependencies
│   ├── trace-formula.b4m         Thm 12.5.1.1 itself
│   ├── geometric-side.b4m        Thm 12.4.1.1
│   ├── orbital-traces.b4m        Thm 11.3.2.1
│   └── spectral-side.b4m         Thm 10.4.2.8, Def 10.4.3.7
│
├── explicit-formulas/            Appendix B.4 — B.4.4.7 and below
├── one-line/                     Appendix B.7 — Euler products, non-vanishing
│
└── standard/                     the `standard:` tier, grouped by subject
    ├── haar.b4m                  Haar measure on a locally compact group
    ├── tvs.b4m                   locally convex spaces, nuclearity (App. C)
    ├── schwartz-bruhat.b4m       App. D
    └── complex-analysis.b4m      Montel, Paley-Wiener, residues
│
└── the-music-of-the-primes.{pdf,txt}
```

## Five conventions

**1. Bare `.b4m` is fine** (user, 2026-11-20). `openai-wiles` went literate because its job
was to walk a reader through a wrong argument beside a right one; here the job is a dependency
map, and comments in the source carry the manuscript locators perfectly well. Use literate
`.md` only where prose genuinely earns its place — a layer whose point is explaining WHY the
manuscript's step works, not just that it is cited.

**2. One directory per ARGUMENT COMPONENT, not per manuscript chapter.** `trace-formula/`
rather than `ch12/`. Chapters are an artifact of how the author chose to write; the component
is what a reader is auditing. A component directory gets a `README.md` saying what it is and
what it still owes.

**3. The two hole kinds stay as they are** (`README.md` has the table). Bare = ours to
transcribe, so strict `check` fails and the output is the worklist. `cites` = assumed,
disclosed. **Do not let a bare hole become `cites` to make the check pass** — that is the one
move that would quietly hollow out the artifact.

**4. `standard/` is a separate tier, and its citations name a SOURCE.** Not "standard: Haar
measure exists" but "standard: Weil, Basic Number Theory §II.9 — Haar measure on a locally
compact group". A reader has to be able to check it. These may become real theorems later, on
their own schedule; nothing downstream changes when they do, which is what makes citing-first
reversible rather than a shortcut.

**5. Every manuscript reference carries a locator.** A theorem number, or a `.txt` line
number, or both. The `.txt` extraction is committed precisely so those citations stay
verifiable from a clone alone.

## Growing it: one layer at a time

The deepening move, repeated:

1. pick a bare hole (start with `theTraceFormula`);
2. read its manuscript proof and list what it cites — Thm 12.5.1.1 cites 10.4.2.8, 11.3.2.1,
   12.4.1.1 and Def 10.4.3.7;
3. replace the hole with a theorem whose proof cites those four as NEW bare holes;
4. `2b4m check --draft --axioms` to confirm the frontier moved and nothing circular appeared;
5. move anything that is textbook into `standard/` with a real citation.

Stop when the frontier is all `standard/` plus the author's four gaps. That is the deliverable:
a machine-checked dependency map in which every assumption is either a named textbook result
or a named thing the author owes.

## Two things to watch

**Altitude creep.** The encoding is coarse ON PURPOSE — opaque sorts, analytic content in
predicates. Deepening tempts you to model the analysis "just a little", and the honest
options are coarse-everywhere or genuinely-formalized, not a blurred middle. If a layer
cannot be stated coarsely without becoming a lie, that layer needs real foundations and the
plan should say so rather than fudge it.

**A gate that hides holes.** `--library` on a directory containing one hole forces the whole
sweep into `--draft`, hiding every other file's problems (`GUIDE.md`'s own warning). This repo
is hole-bearing by design, so never check it with `--library`; per-file `check` is the gate,
and a FAILING check is the expected state.
