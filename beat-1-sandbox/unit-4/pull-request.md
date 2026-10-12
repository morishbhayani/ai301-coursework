# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/149

**Branch**

docs/73-env-example-openrouter-key

## Eval iterations

**Run history**

1. `agreement: 15/20 scored items  (bar: 18/20: below the bar)`. This was the first full run. The categories line was `clear-accept 2/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. Every reject category was fully matched, but five clear accepts (`pkg-05`, `pkg-08`, `pkg-13`, `pkg-16`, `pkg-19`) failed on "Evidence proves the fix".
2. `agreement: 9/9 scored items`. A partial run after I loosened that check: `--only pkg-05,pkg-08,pkg-13,pkg-16,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`. All five targets flipped to `accept`. All four `not-tested` canaries and `calib-04` stayed at `reject`.
3. `agreement: 20/20 scored items  (bar: 18/20: PASS)`. The confirming full run, saved with `--save-run eval-run.txt`, with categories `clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the file committed in this directory.

**Package analysis**

`pkg-16` (source `kubernetes/minikube#23471`, category `clear-accept`). On my first full run my rubric decided `reject`. The gold label is `accept`.

The PR makes `minikube image load` fail loudly on a corrupt tarball, and its test evidence re-runs the plan's repro on the branch:

```
$ minikube image load ./notanimage.tar -p exitcode
X Failed to load image ./notanimage.tar: docker load: invalid tar header
$ echo $?
1
```

The before/after flip (silent `OK` to a printed error and exit 1) and the valid-tarball control are both shown. My rubric failed it on "Evidence proves the fix", with the grader's evidence line:

> "Not-a-tar before/after (OK -> exit 1) and valid-tar control are shown, but the repo check is only claimed: "`go test ./pkg/minikube/machine/... ./cmd/...` passes" with no output."

My rubric read it that way because the check, as I first wrote it, required "the repo's own test/check command was run with its result shown", and the procedure said "\"Tests pass\" with no output shown fails." The grader took "shown" to mean pasted terminal output. `pkg-16` names the exact command and its result in one line, but doesn't paste the run. That one line was enough to hold a PR whose actual proof (the repro flipping from silent success to a loud exit 1) is the strongest part of the package. Four other clear accepts failed the same way, over a stated `go test ./... passes`, `make functionaltest passes`, or a control reported as "unchanged".

**Check rationale**

The check as it now reads in the `rubric.md` I uploaded to `tools/pr-precheck/`:

> | Evidence proves the fix | The test evidence and the description's testing section read against the plan's test plan and the issue's repro | Pass if (1) the plan's main repro trigger (the same input/path that failed) is shown run before and after the change, with the observable outcome flipping from the failing result to the expected one; (2) every other failure mode or repro the plan's test plan names is addressed with an observed result (a one-line stated outcome for a control or secondary case is enough); and (3) the repo's own test/check command is named with its result (a stated "`go test ./...` passes" counts; pasted output is not required). Fail if the main repro has no before/after (only "tested locally", "works now", "verified on my machine"), if the evidence exercises a different or unchanged path (a control that never broke) instead of the reported trigger, if a failure mode the plan names is never mentioned, or if no repo test/check command is named at all. A check that fails or cannot run, reported honestly, does not fail this check. | required |

It reads that way because of run 1. I split the evidence into three parts, and each part has its own bar:

- **(1) The main repro must show a before and after.** I didn't loosen this at all. It is the thing the `not-tested` category is built around: `pkg-04`'s "tested locally, colors work now" and `pkg-10`'s "verified working on my machine for a full day" both fail here.
- **(2) Other failure modes and controls only need an observed result.** A one-line stated outcome is enough, but a failure mode the plan names and the PR never mentions still fails. I kept that clause on purpose for `calib-04`, whose evidence re-runs one of the plan's two repros and is silent on the second.
- **(3) The repo's check needs a named command and its result.** I added a literal example, "a stated `go test ./...` passes counts", so the grader stops treating "shown" as "pasted". A bare "tests pass" that names no command still fails.

I rejected the simpler fix of demoting the repo-check requirement to a preferred check. That would have dropped the "repo checks never run" part of the not-tested category entirely.

**Trade-offs**

Loosening a required check can let not-tested PRs through, so my partial run added canaries: all four scored `not-tested` packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) plus `calib-04`, the worksheet borderline that skips a second repro. All five stayed at `reject`. `pkg-07` and `pkg-14` still fail because their evidence runs the path that never broke: the single-file control, and a GET with header casing. Clause (1) catches those, not the repo-check clause.

What the check now gives up: it trusts a one-line claim that the repo's suite passed. A PR author who writes "`go test ./...` passes" without running it will pass part (3). The grader can't verify that from the package without re-running the suite. I accept that because the decisive proof sits in part (1), which still requires the before/after artifact itself. The alternative was holding every terse but honest PR, and the gold set calls those ready.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
