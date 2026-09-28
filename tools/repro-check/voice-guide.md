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

## Things I never post

- A guaranteed fix before I have investigated the issue.
- A promised completion date I cannot guarantee.
- "Same as above" instead of my own reproduction evidence.
- A root-cause claim that the evidence does not establish.
- Exaggerated claims about severity, priority, or how many users are affected.
- Secrets, API keys, tokens, or other credentials.
