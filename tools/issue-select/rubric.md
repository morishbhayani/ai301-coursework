# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer active | Repo facts: last 5 default-branch commits, maintainer first-response sample, and maintainer comments in the issue thread | Pass if at least one default-branch commit by a human appears within the last 90 days, or an Owner/Member/Collaborator responded or participated within the last 90 days. Bot-only commits do not count unless they merge a human contribution. | required |
| Repo in use | Repo facts: archived flag, last push to any branch, latest release | Pass if the repo is not archived and has had a push within the last 90 days or a release within the last 180 days. | required |
| Scope fits newcomer | Issue body and comment thread | Pass unless the issue is explicitly an umbrella or tracking issue, is a pure support/usage question, has unresolved design debate with no maintainer-set direction, or a maintainer states that the work requires major core-internals changes. Multiple files, multiple related edits, or a long issue description do not by themselves cause failure. | required |
| Issue is unclaimed | Repo facts: assignees and linked PRs; issue comment thread for claim comments or mentioned PRs | Pass only if there is no assignee, no open PR already addressing the issue, and no recent comment showing someone is actively working on it. | required |
| AI contribution allowed | Repo facts contribution-policy line and any linked AI contribution policy | Pass if the repository does not explicitly ban AI-assisted contributions. Conditions such as disclosure, testing, or understanding the generated code still pass. If the repo says nothing about AI use, pass. | required |

## Verdict rule

Accept only if every required check passes. If any required check fails, reject the issue. If there is not enough evidence to decide a required check, treat it as unclear and reject.
