# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment is reproducible | Issue context plus the repro report's environment record; use the repo-facts bug-report requirements when relevant | Pass if the report identifies the versions, OS/runtime, repository revision, or other setup details that materially affect this issue. A material difference from the reporter's environment must be stated. For a static documentation/configuration reproduction, naming the repository revision and files inspected is sufficient. | required |
| Steps can be rerun | Repro report's commands, inputs, preparation, and actions read against the issue's trigger | Pass if a stranger can start from the stated setup and reach the tested behavior using the information provided. Required inputs, commands, filenames, or UI actions must be present or directly derivable. An honest cannot-reproduce may still pass when the attempted steps are complete. | required |
| Evidence matches the reported behavior | Output excerpts, logs, screenshots, file excerpts, or other artifacts in the repro report read directly against the behavior described by the issue | Pass if the evidence demonstrates the same behavior the issue describes. For a cannot-reproduce result, pass if the report makes a concrete attempt at the reported trigger, shows the observed non-failing or different outcome, and explicitly names any material condition it could not recreate. A materially different input or adjacent failure presented as confirmation does not count as reproducing the issue. | required |
| Outcome is honest and bounded | Expected behavior, actual behavior, conclusion, and any causal statements read against the artifacts shown | Pass if the report says only what its evidence supports, distinguishes observation from suspected cause, and accurately reports reproduced or cannot-reproduce outcomes. Unsupported certainty, invented scope, or claiming a different failure as confirmation fails. | required |
| Claim is specific and realistic | Candidate claim comment read against the issue description and thread | Pass if the claim identifies the specific issue being investigated and states a concrete next investigation or reproduction step. It must not promise a guaranteed fix, promise a delivery date, or claim evidence that has not actually been established. | required |
| Repo conventions are respected | Repo-facts contribution policy, bug-report expectations, any AI-use/disclosure policy, and the candidate comments | Pass if the comments comply with explicit repository requirements that apply to the contribution, including any required AI-assistance disclosure. If the repository has no AI disclosure requirement, silence about AI passes. | required |

## Verdict rule

Accept only if every applicable required check passes. In a full reproduction package, any required check graded fail or unclear causes reject. In a claim-only live-mode draft, checks that require the future repro report are not yet applicable and are excluded from the verdict as directed by SKILL.md.
