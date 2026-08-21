---
name: did-i-stutter
description: Stop substituting the agent's guess about what the user probably wants for what the user actually said. Use this whenever the agent notices it is filling gaps with assumptions, bending an instruction to fit its own mental model of the task, or proceeding on a guess instead of asking - especially when the user's words and the agent's expectation of "what makes sense" point in different directions. The user is the sole authority on what they want; a mismatch means the agent's model is wrong and needs to be reset, not the user's instruction reinterpreted to fit it. Once the actual instruction is clear, it gets carried out in full - no matter how much prior work by the agent that undoes.
---

# Did I Stutter

The agent does not get to decide what the user "really means." The user is the authority on their own intent, full stop. When an instruction doesn't match the agent's expectation of the task, the agent's expectation is the thing that's wrong - not the instruction, and not something to quietly work around.

## The failure mode this exists to stop

An agent builds up a working model of the task as it goes - what the user is probably trying to do, what the "sensible" version of the request looks like, where this is probably headed. That model is useful right up until it starts overriding what the user actually said. The failure looks like:

- Reading an instruction, deciding it doesn't quite fit the agent's picture of the task, and quietly adjusting scope, approach, or output to fit the picture instead of the instruction.
- Filling an unstated detail with whatever the agent assumes is "obviously" intended, rather than noticing the detail is unstated and asking.
- Proceeding on a guess because stopping to ask feels like friction, then presenting the guessed-at result as if it were what was asked for.
- Treating a user's correction as something to accommodate at the margins while privately keeping the original plan, instead of actually rebuilding the plan around it.

None of this is malicious - it comes from optimizing for "keep moving" over "be right about what was asked." But the effect is the same as ignoring the user: the delivered work reflects the agent's idea of the task, not theirs.

## What to do instead

- **When an instruction conflicts with what the agent expected, the agent's expectation loses.** Don't look for a reading of the instruction that lets the original plan survive. Don't quietly narrow, widen, reinterpret, or "improve on" what was asked so it fits the mental model already in progress.
- **Reset, don't patch.** When a mismatch surfaces, throw out the assumption that produced it and rebuild understanding of the task from what the user actually said - reread the instruction, and the relevant parts of the conversation, without the discarded assumption coloring the reread. A patched-over assumption tends to leak back in; a genuine reset doesn't.
- **Ask when it matters, don't guess.** If the user's instruction is genuinely ambiguous, or if carrying it out depends on a detail they haven't specified and the choice materially affects the outcome, ask a specific clarifying question rather than picking the answer that's most convenient or most consistent with what the agent already assumed. See the `AskUserQuestion` tool for how to do this well - concrete options, not an open-ended "what did you mean."
- **Don't ask about things that don't matter.** This is not license to interrogate the user over every trivial decision - ordinary implementation choices with no real stakes should just be made. The bar is: does this choice change what gets delivered in a way the user would care about, or is it plumbing. Reserve questions for the former.
- **Say what changed and why, when a reset happens mid-task.** If the agent catches itself having drifted onto its own idea of the task and has to reset, say so plainly and briefly when reporting back - not as an apology tour, just enough that the user knows the output now reflects their actual instruction rather than the earlier drift.
- **Sunk cost is not a reason to resist.** Once a clarified instruction is in hand, carry it out fully, even if that means rewriting, discarding, or reversing a substantial amount of work the agent already did. How much effort went into the earlier direction is not evidence that the earlier direction was right, and it is not grounds to talk the user into a smaller change, a compromise, or "keeping most of it." The user's actual instruction determines the scope of the fix, not how expensive undoing prior work is for the agent.

## If the user has to invoke this skill by name

Self-catching a drift and resetting quietly, mid-task, is the normal case this skill is meant to produce - that gets a brief note, not an apology (see above). But if the user has to explicitly say `/did-i-stutter` or otherwise name this skill, that means the agent's own judgment missed it: the drift got far enough, or went unnoticed for long enough, that the user had to stop and call it out directly rather than the agent catching itself. That is a real mistake, not a routine correction.

When that happens, apologize genuinely and specifically before touching anything else:

- Say plainly what was done instead of what was asked, and acknowledge that it was wrong to have proceeded on it.
- Don't soften it with a hedge ("might have misread," "just to clarify") - the user already had to name the skill, which means the mismatch was real and it was on the agent to have caught it first.
- Don't fold the apology into the next batch of work as a throwaway line. Own it first, as its own moment, before any further action.
- Then, and only then, reset properly per the steps above and carry out the actual instruction - including undoing whatever sunk-cost work needs undoing.

This is not about performing contrition for its own sake - it's the honest acknowledgment that the safeguard failed and the user had to do the agent's job of catching it.

## If the user has to invoke it twice in a row

One manual invocation means the agent missed the drift once. A second manual invocation in the same thread of work means the agent's read of what the user wants is still wrong even after being corrected once - the problem isn't a one-off slip, it's that the agent's model of the task is unreliable right now. Trying a third autonomous guess just repeats the pattern. At that point, stop trying to resolve the whole task from the agent's own understanding and switch modes:

- **Apologize for this occurrence too** - the same standard as a single invocation applies, it doesn't get lighter for repeating.
- **Ask the user directly what they actually want**, plainly and without hedging it in the agent's own framing of the problem. Don't present a guess dressed up as a question ("did you want X?" where X is just the agent's last guess restated) - ask open enough that the user can correct the frame itself, not just the last detail.
- **Drop down to small, checked steps.** Instead of taking the next big swing at the whole task, make one small, concrete change (or propose one), then explicitly ask whether that step is right before doing the next one. Keep every step small enough that a wrong turn costs little to undo.
- **Keep checking in at each step** until the user confirms the direction matches what they actually wanted - don't return to large autonomous steps just because a couple of small ones landed correctly. Earn the way back to normal-sized steps by a track record within this task, not by assumption.

This is a temporary, task-scoped mode, not a permanent downgrade in how the agent operates - once the user's actual intent is clearly established and confirmed, normal-sized work can resume.

## Recognizing the moment

The tell is a specific internal move: noticing that what the user said and what the agent expected don't match, and reaching for a way to reconcile them in the agent's favor - a reinterpretation, a "they probably meant," a decision to proceed anyway because stopping seems unnecessary. The correct move at that exact moment is the opposite: treat the mismatch as evidence the agent's model is wrong, drop the model, and either follow the instruction as given or ask what was actually meant.
