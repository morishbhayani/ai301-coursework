# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` (stop on a placeholder `Repo:` line or an out-of-scope issue), then `voice-guide.md`. Eval mode: skip this step.
2. Read `rubric.md` and `references/evidence-guide.md`. List the check names, their weights, and the verdict rule.
3. Read the issue and thread highlights. Note in one line the reported behavior and any maintainer direction.
4. Read the repo facts (live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`). Write down every stated PR requirement: template sections or checklist items, required issue link, required files (changelog/whatsnew), and the exact AI-policy wording.
5. Read the plan (bundle "Plan context"; live: `plan.md`) before the PR. Write down: (a) the list of changes it commits to, (b) the files it names, (c) what it scopes out, (d) every repro or failure mode its test plan names, (e) every Deviations note. The plan is read first so the PR is judged against what was promised.
6. Read the diff (live: `git diff main...HEAD`) hunk by hunk, then the commits, then the description, then the test evidence.

## Evidence gathering

1. **Diff matches the plan**: make a two-column list. Left: each diff hunk (file + what it does). Right: the plan item or deviation note that covers it, or "none". Then list each plan commitment from step 5(a) and mark it "in diff", "disclosed as left out", or "missing silently". Finally quote every description sentence that claims fidelity or a deliverable ("exactly as planned", "no other changes", "docs now ...") and mark whether the diff bears it out.
2. **Evidence proves the fix**: for each repro/failure mode from step 5(d), find the before output and the after output in the evidence and quote the observable that changed. Record whether the input/path matches the plan's repro or is a different/unchanged path. Record the repo check command(s) shown and their result lines.
3. **Diff is clean and reviewable**: scan every hunk for debug prints/logging, commented-out code, dead functions, TODOs added, formatting/indent churn, import reshuffles, reprinted identical lines. Quote each one found.
4. **Repo standards are met**: take each requirement from step 4 and find it in the description or diff. Quote it or record "absent". For AI policy, record whether a disclosure naming the tool and extent is present.
5. **Description explains the change**: quote the title and the summary sentence.

## Check execution

1. Run the checks in rubric order, using only the evidence recorded above; re-read the package only if a needed item was not recorded.
2. Apply each pass condition literally and write `pass`, `fail`, or `unclear` with one line naming the deciding quote or fact.
3. Consistency rules:
   - Drift: one diff hunk with no plan item or deviation note, or one false fidelity/deliverable claim, is enough to fail. A shortfall disclosed in the plan's notes or the description passes.
   - Evidence: the main repro needs a shown before and after. Controls and secondary cases need only a stated observed result. The repo check needs a named command and its result; a one-line "`<command>` passes" is enough. Fail when the main repro has no before/after, the evidence runs a path that never broke, a failure mode the plan names is never mentioned, or "tests pass" names no command. A failing or not-runnable check shown honestly does not fail.
   - Clean: one debris hunk is enough to fail. Do not fail on commit-message wording alone.
   - Standards: fail only on a requirement the repo actually states. No stated template or AI policy means nothing is required.
4. Grade `unclear` only when the needed evidence is genuinely absent from the package (e.g. no diff shown), not when it is brief.
5. Grade the preferred check last.

## Verdict assembly

1. Collect the grades of the four required checks.
2. All four `pass` gives `accept`; anything else gives `reject`. `unclear` counts as fail. The preferred check is never consulted.
3. In the summary, name each failing required check and quote its deciding evidence.
4. Live mode: then list broken voice-guide rules and any procedure gaps; these never change the verdict.
5. End with the JSON block from `SKILL.md`, every check listed in rubric order, nothing after it.
