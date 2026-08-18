---
name: where-was-i
description: Reread the actual conversation history to find the last point of real, on-task progress before things derailed - a tangent, an interruption, a session restart, a long unrelated detour - then give the user a short, evidence-backed catch-up covering what they were last doing, what state it's actually in right now, and where it looked like they were headed. Use whenever the user says something like "where was I," "what was I doing," "catch me up," "remind me where we left off," or invokes /where-was-i directly.
---

# Where Was I

People lose the thread of their own work all the time - a long tangent, an interruption, a session that got cut and resumed, a context switch to something else and back. This skill reconstructs "what was I actually doing" from the real conversation and real repo/filesystem state, and hands the user a short, accurate catch-up instead of leaving them to scroll back and piece it together themselves.

## What "derailed" means here

Not an accusation and not a diagnosis of what went wrong - just the point where the thread of on-task work stopped being the thing happening in the conversation. That could be:

- The user went off on an unrelated question or tangent and never came back to the original task.
- A session boundary (restart, usage limit, dropped connection) cut in mid-task.
- The conversation got pulled into a side discussion (debugging something unrelated, answering a question) that ran long enough to bury the original thread.
- The agent itself wandered - kept "helping" past the point the user actually asked for.

Finding this point is about locating *where to resume*, not about assigning fault. Never frame the summary as "you got distracted" or "we went off track because you..." - just report what happened and where things stand now, the same neutral, non-argumentative stance as [no-gaslighting](../no-gaslighting/SKILL.md).

## Step 1: Reread the transcript for real, don't reconstruct from vibes

Walk back through the actual conversation history - not a summary of it, not a guess at "the gist." Find:

- The last message where the user gave a concrete, task-directed instruction.
- The last few tool calls/actions the agent actually took in service of that instruction (edits, commands, commits, files touched).
- The point after that where the conversation's subject visibly changed - a new question, a different task, a gap, a restart.

If the transcript has been summarized/compacted and older turns aren't directly visible, work from whatever record is available (summaries, prior tool results, file state) rather than inventing detail that isn't there.

## Step 2: Verify against real state, not just what the transcript claims

The transcript records what was *attempted* - it doesn't guarantee what actually *landed*. Before reporting anything as done, check it:

- `git status` / `git diff` / `git log` for repo work - was the described change actually committed? Pushed? Still sitting uncommitted?
- Re-`Read` files that were supposedly edited to confirm the edit is really there.
- Re-check the result of any command that was run rather than trusting a remembered summary of its output.

Where the transcript and reality disagree (something the agent said it did, but the repo shows otherwise), report the real state, plainly, without making it a bigger deal than it is.

## Step 3: Summarize, short and concrete

Give the user:

1. **What you were last doing** - one or two sentences, concrete (which file, which feature, which task), not "working on the project."
2. **Where it actually stands right now** - backed by something checkable (a file path, a git status line, a test result), not a bare assurance.
3. **Where it looked like you were headed** - the next step that was implied or stated before the derail, if there was one. Say so plainly if it's not obvious ("no clear next step was stated before this - want to pick a direction?").

Keep it short. This is a catch-up, not a full replay of the conversation. Skip anything that isn't necessary to reorient the user.

## Step 4: No argument, no over-explaining the detour

Don't relitigate why the derail happened, don't apologize at length, don't editorialize about the tangent's content. If the user wants to know why something happened, answer plainly once and stop - the same standard as [no-gaslighting](../no-gaslighting/SKILL.md). The goal is to get them oriented and moving again, not to narrate the last N messages back at them.

## When invoked as `/where-was-i`

Treat it the same as the natural-language triggers - no argument is required. If the user appends a phrase (e.g. `/where-was-i the migration script`), use it to focus the search on that specific thread if the conversation covered more than one.
