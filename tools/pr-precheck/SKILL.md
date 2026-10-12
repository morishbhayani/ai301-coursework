---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: **is this pull request ready to submit?** A PR package is a candidate pull request (its title, description, commits, diff, and test evidence), read against the plan it claims to implement (including that plan's Deviations notes) and the issue the plan belongs to, plus the repo's stated PR template and contribution policy. Grade only that package. Do not grade the plan's quality, the issue's merit, or any other PR, and never answer from gut feel: execute `procedure.md`, which applies `rubric.md`.

## Inputs and modes

- **Live mode** (the student's own PR, before it is opened). Read these inputs:
  - `plan.md` in the current working directory, including its `## Deviations` section.
  - The branch's diff: run `git diff main...HEAD` (three dots) and `git diff main...HEAD --stat` from the working copy. Only committed changes count. Also read `git log main..HEAD --oneline` for the commit messages.
  - The draft PR: `pr_draft.md` in the working directory. Its first line is the PR title; the rest is the description.
  - The test evidence: `test_evidence.md` in the working directory, plus whatever the description pastes.
  - The issue: fetch the issue the student names with `gh issue view <URL> --comments`.
  - The repo's standards: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the working copy.
  - A house-chain student reads the house plan and house repro pack instead of their own plan and repro; the same checks grade the same things.
  - Never stage, commit, or edit files. `plan.md`, `comment.md`, `pr_draft.md`, and `test_evidence.md` are drafts and must not appear in the diff.
- **Eval mode** (a package bundle). The bundle text is the whole world: issue, thread highlights, repo facts, plan context, candidate PR title, description, commits, diff, and test evidence. Fetch nothing and read nothing else. Always grade every check with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. If the `Repo:` line still carries a bracketed placeholder such as `<ORG>/<PATH-REVIEW-REPO>`, stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. If the issue URL is not in the scoped repo, refuse to grade and say why. Apply the house rules scope.md lists (one PR per issue per student; branch on the student's own fork with a type prefix, issue number, and slug; a classmate's PR does not block this one). In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and hold the outgoing PR text (the title and the description) against each rule. In the summary, list every rule the draft breaks, quoting the rule and the offending line. The voice guide never changes the verdict on its own, because voice is the student's own standard and no rubric check reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks (name, evidence, pass condition, weight) and the verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives in a bundle and in a live PR, and what good looks like.
- `procedure.md` is the operating procedure. Execute it as written, step by step. Where it is silent on something you need, note the gap in the summary instead of improvising a step.
- If `rubric.md` has no checks or `procedure.md` has no steps (only template comments), refuse to grade and say which file is empty. Do not invent checks.

## Verdict and output

The verdict is binary: `accept` means ready to submit; `reject` means hold. There is no third verdict; reservations go in check evidence lines. You may print a short per-check summary first (in live mode, add the voice-guide notes and any procedure gaps). Then end the reply with this fenced JSON block, valid, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the quote or fact that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready, and a long confident description can hide drift. Read the diff itself against the plan, the evidence itself against the test plan.
- The rubric decides: if a check passes by its stated condition but feels wrong, it still passes; note the tension in the summary.
- The procedure decides how: follow `procedure.md` and report its gaps rather than inventing steps.
- `unclear` follows the rubric's verdict rule; where the rule is silent, `unclear` counts as `fail`, because a PR you cannot verify from the package is not ready to submit.
