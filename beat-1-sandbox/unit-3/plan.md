# Plan for #73: README and `.env.example` disagree about which LLM API key to set

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73
Base: `f89c06f` on `main` (still the tip of upstream `main` when this plan was written)
My reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5878185754

## Diagnosis

What my repro showed, at `f89c06f`. Following the README's Quick Start literally (`cp .env.example .env`) leaves a `.env` with no OpenRouter key in it, and the app's settings object resolves that key to an empty string:

```
$ grep -n OPENROUTER .env || echo "(no match)"
(no match)

llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = ''
```

The two setup documents both name the key. `README.md:24`:

```bash
# Configure environment (add your OPENROUTER_API_KEY to .env)
```

and `docs/SETUP.md:47`:

```bash
# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
```

The template they tell you to copy, `.env.example:16-19`, has no slot for it:

```dotenv
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
```

`core/config.py:20-22` does declare `openrouter_api_key`, `openrouter_base_url` and `openrouter_model`, so the setting exists in the app. It is the template that falls behind.

So the cause is narrow: `.env.example` is missing the `OPENROUTER_API_KEY` entry that both setup guides and `core/config.py` expect. My reading is that the README and SETUP have the right key, and the template just never got the line. That is an inference; see Risks.

Choosing which side to fix. In my repro I said this was a maintainer call, and no maintainer has commented on the thread since. Three things make the template the side to change:
- two setup docs agree with each other and with the declared `openrouter_*` settings;
- the issue lists `.env.example` as a relevant file and asks to "make the two files agree";
- changing the template is additive and does not alter any existing variable.

The other option would be rewriting both guides toward `openai` and dropping the OpenRouter settings. That removes config the app declares, so it is a bigger change and I am not proposing it.

## Scope

In scope, one file: `.env.example`.
- Add an `OPENROUTER_API_KEY` line to the `# LLM provider` block, with a placeholder value in the same style as the existing `OPENAI_API_KEY=sk-your-key-here`.
- Reword the block's comments so they say what each key is for: the OpenRouter key is the one the setup guides ask for, and `OPENAI_API_KEY` is still read by the OpenAI embedding provider (`ingestion/embeddings/provider.py:67`).

Not in scope:
- `README.md` and `docs/SETUP.md`. Once the template has the key, they should match it as written.
- `core/config.py`, `rag/generator/review_generator.py` and `ingestion/embeddings/provider.py`. No change to how a provider is chosen, and no wiring of the `openrouter_*` settings into `ReviewConfig`. My repro found nothing that reads those settings yet, and wiring them would be a feature, not this docs fix.
- Adding `"openrouter"` as an `LLM_PROVIDER` option in the comment. Nothing in the code accepts that value today (the embedding factory takes only `mock` and `openai`), so listing it would add a new inaccuracy.
- `OPENROUTER_BASE_URL` / `OPENROUTER_MODEL`. They have working defaults in `core/config.py`, and neither guide asks the reader to set them.

## Files I'll touch

- `.env.example`: the `# LLM provider` block, lines 16-19. Nothing else.

## Approach

1. Branch from `main` as `docs/73-env-example-openrouter-key`.
2. Edit the `# LLM provider` block in `.env.example` to:
   ```dotenv
   # LLM provider
   # Options: "mock" (default, no API key needed), "openai"
   LLM_PROVIDER=mock
   # OpenRouter key for AI review generation (README Quick Start / docs/SETUP.md)
   OPENROUTER_API_KEY=sk-or-your-key-here
   # OpenAI key, used by the "openai" embedding provider
   OPENAI_API_KEY=sk-your-key-here
   ```
3. Run the test plan below. Then run `make lint` and `make test-unit` to confirm nothing else moved (the change is to a dotenv template only, so I expect no change there).
4. Commit as a Conventional Commit, `docs: add OPENROUTER_API_KEY to .env.example`, with `Fixes #73` in the body. `plan.md` and `comment.md` stay out of the commit.

## Test plan

Re-run my Unit 2 repro steps unchanged, on the branch, in a clean checkout with a fresh `cp .env.example .env`:

```bash
cp .env.example .env
grep -n OPENROUTER .env || echo "(no match)"
.venv/bin/python -c "
from core.config import Settings
s = Settings()
print('llm_provider       =', repr(s.llm_provider))
print('openai_api_key     =', repr(s.openai_api_key))
print('openrouter_api_key =', repr(s.openrouter_api_key))
"
```

Before (from my repro, re-run at `f89c06f`): step 1 prints `(no match)` and `openrouter_api_key = ''`.

Expected after the fix:
- Step 1 prints the new line, `OPENROUTER_API_KEY=sk-or-your-key-here`, instead of `(no match)`.
- Step 2 prints `openrouter_api_key = 'sk-or-your-key-here'` instead of `''`. This shows the template's key is now the one the settings object loads.
- `llm_provider` stays `'mock'` and `openai_api_key` stays `'sk-your-key-here'`, so the existing defaults are unchanged.

## Risks and unknowns

- **Authority is inferred, not confirmed.** No maintainer has said which provider is canonical. I'm going with the side that two docs and the declared settings agree on. If a maintainer prefers OpenAI, I would rewrite the README and SETUP lines instead and drop this change.
- **Placeholder value.** A non-empty placeholder resolves to a non-empty key, the same way `OPENAI_API_KEY=sk-your-key-here` already does today. I'm following the file's existing convention and not changing it. Nothing reads `openrouter_api_key` at this revision, so the placeholder cannot trigger a real API call.
- **Runtime behaviour is untested.** I have not made a real OpenRouter call. Making the key actually used for review generation is out of scope (see above).
- **Classmates' overlapping plans.** Other students have posted plans on #73. Under the course house rules that does not block this one, and the plan above comes from my own repro.

## Deviations

Nothing changed: the plan held. The build is the one commit planned (`1a2542e`, `docs: add OPENROUTER_API_KEY to .env.example`), on `docs/73-env-example-openrouter-key`. It touches only the `# LLM provider` block of `.env.example`, and the three lines added are exactly the ones in Approach step 2.

The test plan came out as predicted. After a fresh `cp .env.example .env`, `grep` now prints `20:OPENROUTER_API_KEY=sk-or-your-key-here` instead of `(no match)`. `Settings()` now gives `openrouter_api_key = 'sk-or-your-key-here'` instead of `''`, and `llm_provider` (`'mock'`) and `openai_api_key` (`'sk-your-key-here'`) are unchanged.

One small note on how I ran the checks, not on what was built. My Unit 2 venv only had pydantic in it, so before `make lint` and `make test-unit` could run I had to install the project's dev dependencies (`pip install -e ".[dev]"`). After that, `ruff check .` reported "All checks passed!" and the unit suite gave `375 passed, 53 xfailed`. A `grep` also showed that no code, test, or CI workflow reads `.env.example`, so I didn't expect either check to move.

Before posting the comment I softened two sentences that stated "README and SETUP are correct" as fact, because that is an inference (my plan-check run flagged it against my voice guide). The planned change itself did not move.
