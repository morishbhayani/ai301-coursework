# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`.

---

## Your identity upstream

**GitHub username**

morishbhayani

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5878182380

I'm a newer contributor working through this repo, and I'd like to take this one.

Restating the mismatch in my own words so it's clear what I'm checking, as of `f89c06f` on `main`: the Quick Start block in `README.md` tells you to add `OPENROUTER_API_KEY` to `.env` and then copy `.env.example` across, but `.env.example` never defines that variable — the only provider key it ships is `OPENAI_API_KEY`, and its `LLM_PROVIDER` comment offers just `mock` and `openai`. `core/config.py` declares fields for both providers. So whichever of those two files a new contributor reads first decides which key they go looking for.

My next step is to walk the setup path on a clean checkout rather than only diffing the two files: copy `.env.example` to `.env` exactly as the README instructs, load the project's own settings object, and record what the OpenRouter key actually resolves to afterwards. I also want to check whether any other setup document points in the same direction as the README, since that would bear on which side should be treated as authoritative.

I'll follow up with a reproduction report giving the revision, environment, steps, and what I observe. I'm not promising a fix or a date at this point — I'd rather have the evidence in front of me first.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5878185754

I reproduced this. Following the Quick Start literally leaves you with a `.env` that has no `OPENROUTER_API_KEY` in it, and the application's own settings object resolves that key to an empty string.

## Environment

- OS: macOS 15.2 (build 24C101), Apple Silicon
- Python: 3.14.7
- git: 2.52.0
- Repository state: my fork of `codepath/pathreview-ai301-fa26-s1`, commit `f89c06fc3ff292df2a04a39ac51319d32a76b779` (`f89c06f`) on `main`, clean working tree
- Dependencies: a throwaway venv with only what `core/config.py` imports — `pydantic` 2.13.5 and `pydantic-settings`. I did **not** run `make setup` or start the Docker services; neither is needed to observe this, and skipping them keeps the repro cheap to re-run.

## Steps to reproduce

```bash
git clone https://github.com/<your-fork>/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1
git checkout f89c06fc3ff292df2a04a39ac51319d32a76b779

python3 -m venv .venv
.venv/bin/pip install "pydantic[email]>=2.5.0" "pydantic-settings>=2.1.0"

# This is the README's own Quick Start step, verbatim:
cp .env.example .env

# 1. Does the file the README told us to create contain the key it named?
grep -n OPENROUTER .env || echo "(no match)"

# 2. What does the application actually resolve?
.venv/bin/python -c "
from core.config import Settings
s = Settings()
print('llm_provider       =', repr(s.llm_provider))
print('openai_api_key     =', repr(s.openai_api_key))
print('openrouter_api_key =', repr(s.openrouter_api_key))
"
```

## Expected behavior

`README.md` instructs the reader to add `OPENROUTER_API_KEY` to `.env` and to create that `.env` by copying `.env.example`. A reader who follows both instructions should end up with a template that has somewhere to put that key.

## Actual behavior

The template never mentions it, so the key the README names is silently absent and the setting resolves empty.

Output of step 1:

```
(no match)
```

Output of step 2:

```
llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = ''
```

## Supporting file excerpts

All at `f89c06f`.

`README.md`, lines 24–25 — the instruction:

```bash
# Configure environment (add your OPENROUTER_API_KEY to .env)
cp .env.example .env
```

`.env.example`, lines 16–19 — the provider block, complete:

```dotenv
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
```

`grep -rn "OPENROUTER" .env.example` returns nothing; the file is 27 lines and the block above is all it says about providers.

`core/config.py`, lines 18–22 — both providers declared:

```python
    llm_provider: str = Field(default="mock")
    openai_api_key: str = Field(default="")
    openrouter_api_key: str = Field(default="")
    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```

## One thing I found that the issue does not mention

The README is not the only document pointing at OpenRouter. `docs/SETUP.md`, line 47, in its own setup block:

```bash
# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
```

So it is two setup documents naming `OPENROUTER_API_KEY` against one template that omits it. That seems worth folding into the scope of "make the two files agree" — otherwise fixing `README.md` alone leaves `docs/SETUP.md` pointing the same wrong way.

## What I am not claiming

- I did not make a real API call to either provider; this is about what configuration the documented setup path produces, not about runtime provider behavior downstream.
- At this revision `grep -rn "llm_provider\|openrouter_api_key" --include="*.py"` matches only the declarations in `core/config.py` — no other module reads them yet. So my evidence shows the documentation disagrees and that the key resolves empty, but it does **not** establish which side is authoritative. Whether `.env.example` should gain `OPENROUTER_API_KEY` and an `openrouter` provider option, or whether the README and `docs/SETUP.md` should be corrected toward `openai`, is a maintainer call I would want direction on before opening a PR.
- Tested on one machine and one OS. I would not expect the result to be platform-dependent, since nothing here is platform-specific, but I have only run it on the environment recorded above.

## Eval iterations

**Run history**

1. `agreement: 3/3 scored items` — a three-package warm-up (`pkg-01`, `pkg-02`, `pkg-03`) to check that my revised wording parsed and produced verdicts before spending a full run.
2. `agreement: 19/20 scored items  (bar: 18/20: PASS)` — first full run. Already over the bar, but with one disagreement: `pkg-09`, which my rubric rejected and the gold label accepts.
3. `agreement: 2/2 scored items` — partial `--only pkg-09,pkg-02,calib-03` after revising the evidence check. `pkg-09` flipped to `accept` and now agrees; `pkg-02` and `calib-03` were canaries, not targets, and both held at `reject`.
4. `agreement: 20/20 scored items  (bar: 18/20: PASS)` — confirming full run, saved with `--save-run eval-run.txt`. This is the run committed in this directory.

Only run 4 was written by the harness with `--save-run`. The scores for runs 1-3 above are
recomputed from the per-package grading records those runs left behind, checked against
`gold-labels.json`; run 4's score is the agreement line in the committed `eval-run.txt`.

**Package analysis**

`pkg-09` (source `sharkdp/fd#2033`, category `clear-accept`).

On my first full run my rubric returned `reject`. The gold label is `accept`.

The package is an honest cannot-reproduce. The contributor made a real attempt at the
issue's trigger — `--exec-batch` command reordering when the argument list exceeds
`ARG_MAX` — ran it five times, and reported that the bug did not appear:

> "I never observed a TWO marker overtaking a ONE marker."

They then named exactly which condition they could not recreate:

> "What differed from the report's conditions: the report states the reordering needs one
> command to hit the limit before the other, and my padding approach may not achieve that,
> since fd appears to flush both command buffers at the same file-count boundary on this
> input (uniform name lengths). A distribution where argument lengths differ per file, or a
> much lower forced limit than my 2 MiB ARG_MAX, may be required."

My rubric read it that way because my evidence check at the time asked whether the
artifacts *demonstrated the behavior the issue describes*, full stop. `pkg-09`'s artifacts
demonstrate the opposite — markers in the correct order — so the check graded `fail`, and
under my verdict rule a single failed required check forces `reject`.

That was the rubric being literally right and substantively wrong. The check had no concept
of a negative result. It could only recognise proof that a bug exists, so a competent,
well-evidenced "I tried this properly and it did not happen" was indistinguishable from
having no evidence at all. The assignment is explicit that an honest, evidenced
cannot-reproduce is a full-credit outcome, and my check could not express that.

**Check rationale**

The check as it now reads in the `rubric.md` I uploaded to `tools/repro-check/`:

> | Evidence matches the reported behavior | Output excerpts, logs, screenshots, file excerpts, or other artifacts in the repro report read directly against the behavior described by the issue | Pass if the evidence demonstrates the same behavior the issue describes. For a cannot-reproduce result, pass if the report makes a concrete attempt at the reported trigger, shows the observed non-failing or different outcome, and explicitly names any material condition it could not recreate. A materially different input or adjacent failure presented as confirmation does not count as reproducing the issue. | required |

It reads that way in three deliberate parts.

The first sentence is the original check, unchanged: the ordinary case is still that the
artifacts show the issue's behavior.

The second sentence is the addition, and it is what `pkg-09` forced. Rather than weakening
the check to something like "pass if the report is thorough" — which would have let sloppy
packages through on presentation — I gave the cannot-reproduce path three conditions it
must meet together: a concrete attempt at *the reported trigger*, a shown observation of
the non-failing or different outcome, and an explicit statement of any material condition
the attempt could not recreate. `pkg-09` satisfies all three; a report that merely says "I
couldn't get it to happen" satisfies none of them.

The third sentence is the guardrail I kept precisely because the second sentence loosens
the check. Allowing a different-looking outcome to pass creates an obvious hole: a package
that runs the wrong thing and calls the resulting error a confirmation. Naming that
explicitly — a materially different input or an adjacent failure presented as confirmation
does not count — keeps the wrong-target family rejecting for the same reason as before.

I rejected the simpler alternative of demoting the check from `required` to `preferred`.
That would have fixed `pkg-09` in one edit, but it would have stopped the rubric failing
anything on evidence grounds at all, which is the single thing this tool exists to judge.

**Trade-offs**

The revision loosened a required check, so before spending the confirming full run I
re-ran two canaries alongside `pkg-09` with
`--only pkg-09,pkg-02,calib-03`.

I picked those two because they fail in exactly the way the new sentence could have
started excusing. `pkg-02` (`sharkdp/bat#3845`, `wrong-target`, the only scored package in
its category at risk here) ran a prefix range instead of the issue's offset-from-end
syntax and narrated a graceful exit-1 argument error as the reported exit-101 crash.
`calib-03` (`mikefarah/yq#2795`) is the worksheet's operator-swap trap: long, confident and
well formatted, but its artifact is an HCL syntax error rather than the issue's panic.
Both are "different outcome, presented as the issue" — the shape my new cannot-reproduce
clause had to not cover. Both held at `reject`, which is what told me the third sentence
was doing its job; the full run then confirmed it at 20/20.

What the check now gives up: it accepts a cannot-reproduce whose named missing condition is
wrong. `pkg-09` guesses that a non-uniform argument-length distribution or a smaller
`ARG_MAX` is what the trigger needs. My check asks that such a condition be stated, not that
it be correct — grading correctness would mean reproducing the bug myself, which is the one
thing the grader cannot do. So a contributor who attempts the right trigger, shows a clean
run, and offers a confident but mistaken explanation of why it did not fire will pass this
check. I accept that: the alternative is rejecting every honest negative result, which is
the failure I started from.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
