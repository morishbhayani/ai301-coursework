# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73

**Verdict output**

All three issues are in the scoped repo, so none are out of field. Here's the grading.

**Repo-level evidence (shared by all three, gathered today 2026-09-21):**

- Last main-branch commit `f89c06f` by Andrew Burke (human), 2026-09-16 — 5 days ago.
- Repo not archived; last push 2026-09-16. No releases ever, but the push branch of the condition passes.
- No `CONTRIBUTING.md` AI clause, no `AI_POLICY.md`, no AI disclosure box in `.github/PULL_REQUEST_TEMPLATE.md` — silence, which passes.
- Only one open PR in the whole repo (#74, against issue #60), so no PR touches any of these three.

## Accepted — ranked by fit (new to the codebase, start small)

**1.** [**#73 — README and** ](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73)[**`.env.example`**](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73)[ **disagree about which LLM API key to set**](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73)

Best fit for a first PR: two text files, a stated 1–2 hour estimate, an empty comment thread, and no xfail marker or test logic to reason about. You learn the branch/commit/CI/review loop with the change itself being near-trivial to verify by eye.

**2.** [**#68 — Keyword search raises** ](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68)[**`ZeroDivisionError`**](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68)[ **when the index is empty**](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68)

The smaller of the two code bugs. The fix is an empty-corpus guard in `index()` because `search()` already early-returns on a falsy `self.bm25` — one branch, and the existing xfail test tells you when you're done.

**3.** [**#69 — Output parser crashes on a top-level JSON array fallback**](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69)

Also bounded, but it ranks last for you: guarding the crash is easy, deciding what a top-level array *should* parse into is a small design call the issue doesn't settle. More judgment than you want on a first PR.

## Rejected

None. Every required check passed on all three.

**One note on the house rule:** #68 and #69 both carry claim comments from classmates (`yulijasso` and `jacho15`, both `author_association: NONE`, i.e. not maintainers). Under the Path Review house rule in `scope.md` those don't block, so "Issue is unclaimed" still passes — but #73 is the only one of the three with nobody visibly on it, which reinforces its top ranking.

svg

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "Maintainer active", "grade": "pass", "evidence": "Commit f89c06f by Andrew Burke (human) on main, 2026-09-16 — 5 days before grading."},
      {"name": "Repo in use", "grade": "pass", "evidence": "isArchived: false; pushedAt 2026-09-16, within 90 days."},
      {"name": "Scope fits newcomer", "grade": "pass", "evidence": "Bounded two-file doc fix: 'Make the two files agree'; labeled good first issue, docs, tier-1; estimated 1-2 hours."},
      {"name": "Issue is unclaimed", "grade": "pass", "evidence": "assignees: []; no linked or cross-referenced PRs; comment thread is empty."},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "No AI clause in docs/CONTRIBUTING.md, no AI policy file, no disclosure box in PULL_REQUEST_TEMPLATE.md — silence passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {"name": "Maintainer active", "grade": "pass", "evidence": "Commit f89c06f by Andrew Burke (human) on main, 2026-09-16 — 5 days before grading."},
      {"name": "Repo in use", "grade": "pass", "evidence": "isArchived: false; pushedAt 2026-09-16, within 90 days."},
      {"name": "Scope fits newcomer", "grade": "pass", "evidence": "Single bounded bug in rag/retriever/keyword_search.py with a covering xfail test (manifest H-01); no design debate in thread."},
      {"name": "Issue is unclaimed", "grade": "pass", "evidence": "assignees: []; no linked or cross-referenced PRs; only claims are from yulijasso (author_association NONE), which the Path Review house rule says do not block."},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "No AI clause in docs/CONTRIBUTING.md, no AI policy file, no disclosure box in PULL_REQUEST_TEMPLATE.md — silence passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "Maintainer active", "grade": "pass", "evidence": "Commit f89c06f by Andrew Burke (human) on main, 2026-09-16 — 5 days before grading."},
      {"name": "Repo in use", "grade": "pass", "evidence": "isArchived: false; pushedAt 2026-09-16, within 90 days."},
      {"name": "Scope fits newcomer", "grade": "pass", "evidence": "Single bounded bug in rag/generator/output_parser.py with a covering xfail test (manifest H-02); not an umbrella or support issue."},
      {"name": "Issue is unclaimed", "grade": "pass", "evidence": "assignees: []; no linked or cross-referenced PRs; only claims are from jacho15 (author_association NONE), which the Path Review house rule says do not block."},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "No AI clause in docs/CONTRIBUTING.md, no AI policy file, no disclosure box in PULL_REQUEST_TEMPLATE.md — silence passes."}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

1. `agreement: 2/3 scored items`
2. `agreement: 0/1 scored items`
3. `agreement: 0/1 scored items`
4. `agreement: 1/1 scored items`
5. `agreement: 18/20 scored items`
6. `agreement: 17/20 scored items`
7. `agreement: 1/1 scored items`
8. `agreement: 1/1 scored items`
9. `agreement: 18/20 scored items`
10. `agreement: 18/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

`issue-01`

My rubric's final verdict was `accept`, and the gold label was also `accept`.

The issue passed because the repository had recent human activity, was in use, the work was bounded, the issue was unclaimed, and AI-assisted contribution was not prohibited.

The run evidence included:

> "Last 5 default-branch commits dated 2026-08-04, one day before the 2026-08-05 capture date"

and:

> "Single docs proposal with no umbrella framing, no support question, no design debate (0 comments), and no core-internals code changes"

My earlier scope rule was too strict because it treated a longer, multi-file documentation issue as unsuitable even when the work still had one clear outcome.

**Check rationale**

Current wording from `rubric.md`:

> `Scope fits newcomer | Issue body and comment thread | Pass unless the issue is explicitly an umbrella or tracking issue, is a pure support/usage question, has unresolved design debate with no maintainer-set direction, or a maintainer states that the work requires major core-internals changes. Multiple files, multiple related edits, or a long issue description do not by themselves cause failure. | required`

I changed this check because `issue-01` showed that the number of files and the length of an issue are not enough by themselves to determine whether it is too large for a newcomer. The revised check focuses on clearer warning signs such as tracking issues, unresolved design questions, support requests, and major core-internals work.

**Trade-offs**

Before revising the scope check, the diagnostic run showed:

> `agreement: 0/1 scored items`

After revising it and re-running `issue-01`, the result became:

> `agreement: 1/1 scored items`

The trade-off is that the rule no longer uses a strict file-count or issue-length limit. Because of that, it may accept an issue that is technically bounded but still takes more time than I personally want to spend on a first contribution.

---

## Selection rationale

**Selection rationale**

1. Issue #73 fits my available time because it is a small documentation and configuration consistency problem with an estimated effort of 1–2 hours. I wanted to begin with a change I could understand completely before taking on a deeper code issue.

2. The verdict correctly identified that the repository is active, the task is bounded, nobody is assigned to it, there is no linked PR, and AI-assisted work is not prohibited. I also considered my own preference for a lower-risk first contribution, which the rubric cannot fully measure.

3. I expect claiming it to be straightforward because there was no assignee, linked PR, or comment from somebody already working on it when I selected it. I will still check the issue again before claiming it in Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.

