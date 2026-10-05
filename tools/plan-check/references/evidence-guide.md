# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives**

Eval mode: the cause is in the candidate plan's "Diagnosis" or "Cause" line (sometimes the first sentence of the plan). The behavior it must explain is in the `## Repro evidence` block: the numbered steps, the failing artifact, and especially the "Control" runs and any intermediate artifact (e.g. a step showing data before a later processing stage).

Live mode: the cause is in the draft `plan.md`'s diagnosis section; the evidence is the student's posted repro comment on the issue, as quoted in the drafts.

**What good looks like**

The stated cause explains the failing artifact and is consistent with every control. If a control shows the blamed component working on the same input, or the failure is visible before the blamed step runs, the diagnosis is contradicted, however confident or well-sourced (a thread's diagnosis included) it sounds.

## Scope

**Where it lives**

The plan's "Scope", "In / Out", "Not in scope", and "Proposed changes" lists, and the plan comment's sentences about what will be built ("beyond", "while touching", "also", "PR series"). Compare them with the issue's ask.

**What good looks like**

One change aimed at the reported behavior, plus its regression tests. Related work is named and explicitly deferred. A drive-by plan bundles the fix with migrations, dependency swaps, new settings or options, UI rework, or a subsystem rewrite.

## Executability

**Where it lives**

The plan's "Files and areas" / "Change" lines (file paths, function names, call sites) and its approach steps.

**What good looks like**

A named file and code location plus a single chosen approach: someone else could open that file and start. Not good: "somewhere", "gocui? tcell? not sure", "whichever is easier", or "profile then optimize" with no target.

## Test plan

**Where it lives**

The plan's "Test" / "Test plan" section, read against the repro evidence's trigger steps and failing artifact.

**What good looks like**

The repro (or the issue's test case as a regression test) is re-run and a specific observable is named that flips from the failing artifact to the expected result (exit 0 instead of 101, color changes at step 3, test passes instead of panicking). A vague plan only says "run the test suite" or "it should feel faster".

## Honesty

**Where it lives**

The plan's "Risks", "Unknowns", "Open question" lines and any hedged or certain causal claims; in live mode also the `## Deviations` section of `plan.md` after the build.

**What good looks like**

Unverified things are labeled as such (e.g. "I have not measured the per-print cost"), and a build that changed course records what changed and why under Deviations. False confidence states guesses as facts.

## Comms

**Where it lives**

Eval mode: `## Thread highlights` (comments marked OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR carry maintainer direction) and the `## Repo facts` "contribution policy" line, read against `## Candidate plan comment`.

Live mode: the issue thread on GitHub (`gh issue view <url> --comments`), the repo's CONTRIBUTING and any AI policy file, the scope's house rules, and the draft `comment.md`.

**What good looks like**

The comment follows (or explicitly engages and gives reasons against) any maintainer direction: named culprit code, a proposed or rejected approach, a posted patch to test, an existing PR. Where the policy demands AI disclosure for comments or for all AI use, the comment names the tool and the extent of help. Where there is no such demand, silence about AI is fine. Boilerplate that ignores the thread fails even if the plan itself is sound.
