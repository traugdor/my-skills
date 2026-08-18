# where-was-i

A Claude skill that rereads the actual conversation to find where things derailed - a tangent, an interruption, a session restart, a long unrelated detour - and gives the user a short, evidence-backed catch-up instead of leaving them to scroll back and piece it together.

## What it does

- Rereads the real transcript (not a paraphrase) to find the last concrete, task-directed instruction and the actions taken toward it
- Verifies claims against actual repo/filesystem state (`git status`, re-reading edited files) rather than trusting what the transcript says was done
- Reports: what you were last doing, what state it's actually in right now (with evidence), and where it looked like you were headed
- Never frames the summary as a judgment ("you got distracted") - neutral, short, and oriented toward getting back to work, in the same spirit as [no-gaslighting](../no-gaslighting/README.md)

## Usage

Ask naturally - "where was I," "what was I doing," "catch me up," "remind me where we left off" - or invoke directly:

```
/where-was-i
```

Append a phrase to focus the search when the conversation covered more than one thread, e.g. `/where-was-i the migration script`.

See [SKILL.md](SKILL.md) for the full instructions.

## Install

See the [repo README](../../README.md) for installation options.
