# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

morishbhayani

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5986284101

Here's my plan for this, built on my reproduction above at `f89c06f`.

**Diagnosis.** In my repro, following the Quick Start literally (`cp .env.example .env`) left a `.env` with no OpenRouter key, and `Settings()` resolved `openrouter_api_key = ''`. Both `README.md:24` ("add your OPENROUTER_API_KEY to .env") and `docs/SETUP.md:47` ("set your OPENROUTER_API_KEY (required for AI features)") name that key, and `core/config.py:20-22` declares the `openrouter_*` settings. As far as I can tell, `.env.example` is the odd one out: its provider block has just `LLM_PROVIDER=mock` and `OPENAI_API_KEY=sk-your-key-here`.

**Which side I'll change.** In my repro I said this was a maintainer call, and nobody has weighed in since. I plan to fix the template, not the guides, for three reasons: two setup docs and the declared settings agree, the issue lists `.env.example` as a relevant file, and the change is additive.

**Scope: one file, `.env.example`.**
- I'll add `OPENROUTER_API_KEY=sk-or-your-key-here` to the `# LLM provider` block, using the same placeholder style as the existing OpenAI line.
- I'll add a short comment on each key saying what it's for. `OPENAI_API_KEY` is still read by the OpenAI embedding provider, so it stays.

Not in scope:
- `README.md` and `docs/SETUP.md`, which should match the template once it has the key.
- Any code change: no wiring of the `openrouter_*` settings into review generation, and no change to provider selection.
- Adding `"openrouter"` to the `LLM_PROVIDER` options comment, because nothing in the code accepts that value today.

**Test plan.** I'll re-run my repro steps unchanged on the branch. I expect `grep -n OPENROUTER .env` to print the new `OPENROUTER_API_KEY=sk-or-your-key-here` line instead of `(no match)`, and `Settings()` to give `openrouter_api_key = 'sk-or-your-key-here'` instead of `''`. `llm_provider` (`'mock'`) and `openai_api_key` should stay the same. I'll also run `make lint` and `make test-unit`.

**Unknowns.** I'm inferring which provider is canonical. If you'd rather standardise on OpenAI, tell me and I'll correct the README and SETUP lines instead of changing the template. I haven't made a real OpenRouter call, and getting the key actually used at runtime is a separate change.

Branch will be `docs/73-env-example-openrouter-key` on my fork.

I used Claude (Claude Code) to help draft this plan and make the edit. I've reviewed the plan and the diff, and I ran the repro commands myself.

---

## Your branch

**Branch**

docs/73-env-example-openrouter-key

**Evidence**

The before is my Unit 2 repro, re-run at the base commit `f89c06f` in a clean checkout. The after is the same repro steps, unchanged, re-run on the branch in a clean checkout. Each run used a fresh `cp .env.example .env`.

Before (`main` at `f89c06f`):

```
$ git rev-parse --short HEAD
f89c06f
$ cp .env.example .env
$ grep -n OPENROUTER .env || echo "(no match)"
(no match)
$ .venv/bin/python -c "
from core.config import Settings
s = Settings()
print('llm_provider       =', repr(s.llm_provider))
print('openai_api_key     =', repr(s.openai_api_key))
print('openrouter_api_key =', repr(s.openrouter_api_key))
"
llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = ''
```

After (`docs/73-env-example-openrouter-key` at `1a2542e`):

```
$ git rev-parse --short HEAD
1a2542e
$ git diff --stat main
 .env.example | 3 +++
 1 file changed, 3 insertions(+)
$ cp .env.example .env
$ grep -n OPENROUTER .env || echo "(no match)"
20:OPENROUTER_API_KEY=sk-or-your-key-here
$ .venv/bin/python -c "
from core.config import Settings
s = Settings()
print('llm_provider       =', repr(s.llm_provider))
print('openai_api_key     =', repr(s.openai_api_key))
print('openrouter_api_key =', repr(s.openrouter_api_key))
"
llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = 'sk-or-your-key-here'
$ make lint
.venv/bin/ruff check .
All checks passed!
$ make test-unit
================= 375 passed, 53 xfailed, 2 warnings in 10.22s =================
```

The `grep` went from `(no match)` to the new line 20. `openrouter_api_key` went from `''` to the template's placeholder. `llm_provider` and `openai_api_key` stayed the same.

## Eval iterations

**Run history**

1. `agreement: 19/20 scored items  (bar: 18/20: PASS)`. This was the first full run. It already met the bar with every category matched (`clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`), but it disagreed on `pkg-14`.
2. `agreement: 4/4 scored items`. A partial run, `--only pkg-14,pkg-10,pkg-17,pkg-18`, after I loosened the "A stranger could start building" check. `pkg-14` flipped to `accept`. The three `unbuildable` canaries stayed at `reject`.
3. `agreement: 20/20 scored items  (bar: 18/20: PASS)`. The confirming full run, saved with `--save-run eval-run.txt`. This is the file committed in this directory.

**Package analysis**

`pkg-14` (source `zellij-org/zellij#5174`, category `clear-accept`). On my first full run, my rubric decided `reject`. The gold label is `accept`.

The plan is a tightly scoped fix. It drains pending OSC color-query responses in the Unix reattach handshake, before pane input is wired. It is grounded in a clean regression window (0.44.1 clean, 0.44.2 leaking), and it explicitly defers the Windows variant because the author cannot test it. My rubric failed it on "A stranger could start building". The grader quoted this line from the plan's Files section:

> "exact functions to be pinned in the PR after tracing the query issuance with debug logs"

and graded it `fail` with the evidence "Only crate-level areas across zellij-server and zellij-client ... defers the location decision to build time."

My rubric read it that way because the check, as I first wrote it, asked the plan to name "the specific file(s), function(s), or code location(s) to change". `pkg-14` names a code path ("the client attach path in `zellij-server`'s client connection handling") but not a function. So the grader treated "functions to be pinned later" the same way it treated `pkg-18`'s "recover() somewhere". The two are different. `pkg-14` has already decided what to do (drain OSC responses, bounded to OSC patterns rather than a time window) and where it happens (the reattach handshake). Only the exact function name is left, and the plan says how it will find it: by tracing with debug logs it already has working. `pkg-18` hasn't decided where or how. My check couldn't tell those cases apart. The preferred "Unknowns" check also failed this package, but preferred checks never change the verdict, so the executability check was the one that decided it.

**Check rationale**

The check as it now reads in the `rubric.md` I uploaded to `tools/plan-check/`:

> | A stranger could start building | The plan's files / areas list and its approach steps | Pass if the plan names where the change lands (specific file(s), function(s), or a named component and code path such as "the reattach path in `zellij-server`'s client connection handling") AND commits to one chosen approach for the change, so someone else could begin the edit without asking the author anything. Pinning exact function names during the build is fine when the code path and the approach are already decided. Fail if the location is "somewhere", a list of candidate layers or libraries still undecided, "profile first then decide", or if the core decision is deferred to build time ("whichever is easier"). | required |

It reads that way because of `pkg-14`. The first version required a specific file, function, or code location. For a seasoned contributor that is too literal: you often know the code path and the approach before you've opened the exact function. I rewrote the "where" half so that "a named component and code path" counts. I added an example drawn from `pkg-14`, so the grader can see what level of precision is enough. I also added one sentence saying that pinning function names during the build is fine *when the code path and the approach are already decided*.

I kept the second half, "commits to one chosen approach", unchanged on purpose, and I kept the list of failure shapes ("somewhere", undecided candidate layers, "profile first then decide", "whichever is easier"). That half is what keeps the `unbuildable` category rejecting. Every unbuildable package leaves the core decision open, not just a function name. I chose not to drop the location requirement entirely, which would have been the one-word fix. Without it, a plan that names a chosen approach but no place in the code ("add caching") would pass, and a stranger couldn't start from that.

**Trade-offs**

This revision loosened a required check, so before spending the confirming full run I re-ran `pkg-14` alongside all three `unbuildable` packages as canaries: `--only pkg-14,pkg-10,pkg-17,pkg-18`. Those are the packages most likely to slip through a looser executability check:
- `pkg-10`: "profile and optimize", with no files.
- `pkg-17`: "gocui? tcell? not sure".
- `pkg-18`: "recover() somewhere", "whichever is easier".

All three held at `reject`, and the confirming full run then came back 20/20.

What the check now gives up: it trusts the plan's claim that the code path is decided. A plan that names a plausible-sounding component ("the reattach path in the server") and a chosen approach will pass, even if the component turns out to be the wrong place once someone opens the code. The grader can't verify that from the package, because it would mean reading the real codebase. I accept that miss. The alternative was rejecting every plan that hadn't pinned a function name before building, and that rejected a plan the gold label calls ready.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
