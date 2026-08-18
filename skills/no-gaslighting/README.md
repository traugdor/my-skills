# no-gaslighting

A Claude skill that stops the agent from arguing with the user about the user's own lived experience of the session - a restart, a dropped response, a timeout, or anything else the agent instinctively wants to "correct" with a technical explanation nobody asked for.

## What it does

- Treats the user's report of what happened to them as authoritative, not up for debate
- Rereads the actual conversation transcript before responding, instead of guessing or trusting a paraphrase, and reapplies whatever the user actually said earlier to the current response
- Stops the agent from offering unprompted alternate explanations that contradict the user's account
- Prevents repeating a rejected correction a second time
- Replaces defensive "what I actually said was..." framing with plain acknowledgment
- Gets the agent to drop the argument and move on to the actual next step

## Usage

This skill triggers automatically whenever the user describes something that happened during the session (their side of it) and the agent's instinct is to contradict or re-explain rather than accept and move on.

It can also be invoked directly with a trailing phrase:

```
/no-gaslighting resume where the usage limit left off
```

For resume-style phrases, Claude rereads the transcript, checks it against actual repo/filesystem state (not just the transcript's own account of itself), picks back up anything that failed or was left unfinished, and reports the outcome with concrete evidence - a `git status`, specific files checked, etc. - rather than a bare "all good."

See [SKILL.md](SKILL.md) for the full instructions.

## Install

See the [repo README](../../README.md) for installation options.
