# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where it lives.** Bundle: "Plan context" (the plan's changes, files, out-of-scope line, and any deviation note) read against "Diff" and the "Description". Live: `plan.md` (including `## Deviations`) read against `git diff main...HEAD` and `pr_draft.md`.

**What good looks like.** Every hunk maps to a plan item or a stated deviation, every planned item is in the diff or disclosed as left out, and the description's claims ("exactly as planned", "docs updated") are true of the diff. Drift is an unplanned option, flag, rename pass, or rewrite, or a planned piece silently missing.

## Test evidence (harness category: not-tested)

**Where it lives.** Bundle: "Test evidence" and the description's testing section, read against the plan's test plan and the issue's repro. Live: `test_evidence.md` and the Testing section of `pr_draft.md`, read against `plan.md`'s test plan and the Unit 2 repro.

**What good looks like.** The plan's own repro trigger run before and after, with the observable flipping (output, exit code, error), every named repro covered, and the repo's check commands shown with their result lines. Not good: "tested locally", "tests pass" with no output, or evidence from a path that never broke.

## Diff quality (harness category: unreviewable)

**Where it lives.** Bundle: the "Diff" hunks and "Commits". Live: `git diff main...HEAD` and `git log main..HEAD`.

**What good looks like.** Only hunks that implement the fix or its tests. Debris: debug prints, commented-out code or first attempts, dead functions, added TODOs, formatting/indent churn, import reshuffles, reprinted unchanged lines.

## Standards and comms (harness category: standards-wall)

**Where it lives.** Bundle: "Repo facts" (pull requests / PR template line and contribution policy, including AI policy) read against the title, description, and diff. Live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, the course's AI-disclosure ask, read against `pr_draft.md` and the diff.

**What good looks like.** Every stated template section or checklist item is present and filled, the issue is linked when asked ("Closes #N"), required files such as a changelog entry are in the diff, and where the policy requires it the description discloses AI use with tool and extent. No stated template or AI policy asks nothing.
