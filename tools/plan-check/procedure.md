# Procedure: how this skill grades a plan package

## Read order

1. Live mode only: read `scope.md` first. Confirm the issue URL is in the scoped repo; if it is not, or the `Repo:` line is still a placeholder, stop and say so. Note the house rules (a classmate's plan does not block this one; "same approach as above" is never a plan). Then read `voice-guide.md`. Eval mode: skip this step.
2. Read `rubric.md` and `references/evidence-guide.md`. Write down the list of check names, which are `required`, and the verdict rule.
3. Read the **issue** (title and body). Note in one line what behavior is reported and what the issue asks to be done.
4. Read the **repro evidence** next, before the plan. Note: (a) the trigger steps, (b) the failing artifact (output, exit code, color, panic), (c) every control run and what it rules in or out, (d) any intermediate artifact that shows where in the pipeline the behavior first goes wrong. The evidence is read before the plan so that the plan's diagnosis is judged against what was observed, not the other way round.
5. Read the **thread highlights** (live: the issue thread). For each comment by an OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR maintainer, note any explicit direction: a named culprit location, a proposed or rejected approach, a posted patch or test build, a pointer to an existing PR.
6. Read the **repo facts** block (live: CONTRIBUTING and any AI policy). Note the exact AI-policy wording, and whether it requires disclosure in comments, in PRs only, or not at all.
7. Read the **candidate plan** in full, then the **candidate plan comment** in full. Only now start grading.

## Evidence gathering

For each check, pull exactly this and record it as a short quote:

1. **Diagnosis follows the repro evidence**: quote the plan's cause sentence. From your step-4 notes, list each control run or intermediate artifact and write whether it is consistent with that cause or rules it out.
2. **Change is one bounded fix**: list every change the plan and the comment commit to building (numbered changes, "while touching", "also", "beyond", PR-series plans). Mark each as *needed for the reported behavior* or *not asked for by the issue*. Note any explicit deferrals ("not in scope", "follow-up").
3. **A stranger could start building**: quote the named files / functions / locations, and the approach step(s). Note any hedges ("somewhere", "not sure", "whichever is easier", "profile first", "?").
4. **Test plan proves the fix observably**: quote the test plan. Write down which repro trigger it re-runs, and the observable outcome it names; compare that outcome to the failing artifact from step 4.
5. **Comment engages the thread's direction**: take each maintainer direction from step 5 and find whether the plan or comment follows it, or explicitly mentions it and gives a reason to depart. Quote the matching line or record "not mentioned".
6. **Comment complies with the repo's stated policy**: quote the policy line from step 6 and search the plan comment for an AI disclosure (a tool name plus extent of help). Record "disclosure present" or "absent".
7. **Unknowns are stated, not hidden**: quote the risks / unknowns section, or record that there is none.

Live mode: the drafts are the candidate side; gather issue-side evidence with `gh issue view <url> --comments` and the repo's docs, and take repro evidence only from what the drafts quote or the student's posted repro comment.

## Check execution

1. Run the checks in rubric-table order, one at a time, using only the evidence recorded for that check above. Do not re-read the whole package for a check unless its recorded evidence is missing.
2. Apply the check's pass condition literally. Write the grade (`pass`, `fail`, `unclear`) and one line naming the quote or fact that decided it.
3. Grade `unclear` only when the evidence the check needs is genuinely absent from the package (e.g. no test plan section at all), not when it is merely brief. A terse but complete plan passes; length and polish never decide a grade.
4. Grading rules that keep two executors consistent:
   - Diagnosis: one control run that shows the blamed component working, or the failure already present before it, is enough to fail.
   - Scope: one committed change that the issue did not ask for is enough to fail, even if the core fix is right. Deferred or "out of scope" items do not count against scope.
   - Executability: an undecided core choice fails even if files are named.
   - Thread: a maintainer direction that the comment never mentions fails, even if the plan is otherwise excellent.
   - Policy: missing disclosure fails only when the policy requires disclosure for comments or for all AI use.
5. Grade the `preferred` check last; it is reported but not used in the verdict.

## Verdict assembly

1. Collect the grades of the six `required` checks.
2. If all six are `pass`, the verdict is `accept`.
3. Otherwise the verdict is `reject`; `unclear` counts as `fail`. The `preferred` check is never consulted.
4. In the summary, name the deciding check(s) for a reject and quote the line of evidence that failed it.
5. Live mode: after the verdict, list any voice-guide rule the draft comment breaks, quoting the rule. This never changes the verdict.
6. End with the JSON block from `SKILL.md`, listing every check (including the preferred one) with its grade and one-line evidence, and nothing after it.
