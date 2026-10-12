# Voice guide: how I talk upstream

## Who I am in threads

I am a newer open-source contributor working through the issue carefully rather than presenting myself as an expert on the codebase.
I want my comments to be useful to maintainers: specific about what I checked, clear about what I have not proved, and focused on evidence.

## Rules I write by

### Rule: Be specific instead of generic

Name the actual behavior or files I am investigating instead of posting a generic claim.

- Wrong: "I'll take this issue and look into it."
- Right: "I'd like to investigate the mismatch between the README setup instructions and `.env.example` and report what I can reproduce."

### Rule: Promise investigation, not a fix or deadline

I can commit to checking and reporting evidence, but I should not promise that I can fix the issue or finish by a particular date before I understand it.

- Wrong: "I'll fix this by tomorrow."
- Right: "I'll reproduce the issue first and post what I observe."

### Rule: Separate evidence from guesses

Do not state a suspected root cause as fact until the evidence actually establishes it.

- Wrong: "This is definitely caused by `core/config.py`."
- Right: "`core/config.py` defines both key fields; I'll compare that configuration with the README and `.env.example` before drawing a conclusion."

### Rule: State the limits of what I tested

Do not turn one environment or one observation into a universal claim.

- Wrong: "This is broken for everyone."
- Right: "I reproduced this in the environment described below; I have not tested other environments."

### Rule: Name the direction I'm choosing, and what I'm leaving alone

A plan comment commits to an approach in front of maintainers. Say which option I picked, why, and what is out of scope, so a maintainer can redirect me before I build. If a maintainer has already suggested a direction, follow it or say plainly why not.

- Wrong: "I'll fix the docs so everything agrees."
- Right: "I plan to add `OPENROUTER_API_KEY` to `.env.example` and leave the README and `docs/SETUP.md` as they are; I am not touching provider wiring in code. If you'd rather standardise on OpenAI instead, I'll switch before opening a PR."

### Rule: Disclose AI help where the repo or course asks for it

If AI tools helped draft the plan or the change, say which tool and how it was used, in one plain sentence.

- Wrong: (no mention, when the policy requires it)
- Right: "I used Claude (Claude Code) to help draft this plan and make the edit; I reviewed every line and ran the checks myself."

### Rule: A PR description claims only what the diff does

Every sentence about what changed must be checkable against the diff, and anything I planned but left out gets said.

- Wrong: "Implements the plan exactly; also tidied some docs."
- Right: "Adds `OPENROUTER_API_KEY` to `.env.example` (3 lines). README and SETUP are unchanged, as planned."

## Things I never post

- A guaranteed fix before I have investigated the issue.
- A promised completion date I cannot guarantee.
- "Same as above" instead of my own reproduction evidence.
- A root-cause claim that the evidence does not establish.
- Exaggerated claims about severity, priority, or how many users are affected.
- Secrets, API keys, tokens, or other credentials.
- A plan comment that just says "same approach as above" instead of my own plan.
