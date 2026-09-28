# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives**

In eval mode, read the issue context for the reporter's stated environment, the repo-facts block for environment fields requested by the project's bug-report process, and the candidate repro report for the environment actually tested.

In live mode, read the issue body and relevant thread comments for the target environment. Read the student's draft repro comment for what they actually tested. Repository documentation may be used to identify setup details when the issue depends on them.

**What good looks like**

The report records the environment details that can materially change the result and calls out important differences from the reporter's setup. For executable bugs this may include application/tool version, OS, runtime, installation method, or repository revision. For a static documentation or configuration mismatch, the checked repository revision and exact files inspected are enough.

## Steps

**Where it lives**

In eval mode, use the preparation, commands, inputs, and actions in the candidate repro report and compare them with the trigger described by the issue.

In live mode, use the draft repro comment itself. Repository instructions may establish a starting state, but material commands, inputs, or actions needed to reproduce the result must be represented in the posted report.

**What good looks like**

A stranger can move from the stated starting condition to the tested trigger without guessing a material input or action. The report does not need a particular number of steps or headings. A cannot-reproduce report can still have complete steps.

## Behavior shown

**Where it lives**

In eval mode, use concrete artifacts in the repro report: terminal output, logs, screenshots described in the bundle, file contents, configuration excerpts, test results, or other directly observable evidence. Compare them with the exact behavior in the issue context.

In live mode, grade only evidence that will actually appear in the posted draft. Do not rely on an unstated local file or an observation that the draft does not contain.

**What good looks like**

The artifact shows the issue's actual trigger and resulting behavior. A static inconsistency can be proven with accurate excerpts from the conflicting files. For a cannot-reproduce result, evidence should show that the same trigger instead completes normally or produces the expected behavior. An adjacent error or different input is not evidence of the reported bug.

## Honesty

**Where it lives**

Compare the report's expected behavior, actual behavior, analysis, and conclusion with its artifacts and with the issue description. Also check factual claims in the claim comment against what is known at the time that comment is supposed to be posted.

**What good looks like**

The wording does not go beyond the evidence. Suspected causes are labeled as hypotheses unless the artifacts establish them. Limitations and meaningful environment differences are stated. An evidenced cannot-reproduce is a valid result; pretending that a different failure confirms the issue is not.

## Comms

**Where it lives**

In eval mode, read the candidate claim and repro report against the repo-facts block, including bug-report expectations, contribution policy, and any AI-use or disclosure requirement.

In live mode, read the issue thread, the repository's contribution documentation and templates, any explicit AI policy, the skill's scope house rules, and the student's draft comments.

**What good looks like**

The claim names the issue-specific behavior and says what the contributor will investigate or reproduce next. It does not guarantee a fix or a date. The repro comment contains the contributor's own evidence rather than piggybacking on another person's reproduction. Any explicit disclosure or contribution requirement that applies is followed; if no AI disclosure policy exists, no disclosure is required.
