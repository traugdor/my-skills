# did-i-stutter

A Claude skill that stops the agent from substituting its own guess about what the user "probably wants" for what the user actually said - and from bending an instruction to fit its existing plan instead of the other way around.

## What it does

- Treats the user as the sole authority on their own intent - a mismatch between an instruction and the agent's expectation means the agent's expectation is wrong
- Stops the agent from quietly reinterpreting, narrowing, or "improving on" an instruction so it fits a plan already in progress
- Requires a genuine reset (reread and rebuild understanding from what was actually said) instead of patching an assumption that's already leaking into the work
- Directs the agent to ask a specific clarifying question when a choice is genuinely ambiguous and materially affects the outcome, rather than guessing
- Keeps the bar for asking honest - trivial implementation details still just get decided, no interrogating the user over plumbing
- Carries out the clarified instruction in full once it's clear, even when that means rewriting or discarding a substantial amount of the agent's own prior work - sunk cost is never a reason to talk the user into a smaller change

## Usage

This triggers automatically whenever the agent notices it's filling a gap with an assumption, working around an instruction rather than following it, or proceeding on a guess instead of asking.

If the user has to invoke it directly (`/did-i-stutter`), that means the agent's own judgment missed the drift and the user had to catch it instead - a real mistake, not a routine correction. In that case the agent apologizes genuinely and specifically first, as its own moment, before making any further changes.

If the user has to invoke it a second time in the same piece of work, that's a sign the agent's read of the task is unreliable, not just a one-off slip. At that point the agent apologizes again, asks the user directly and open-endedly what they actually want, and switches to small, checked steps - proposing one concrete change at a time and confirming it's right before moving to the next - until the user's intent is clearly established.

See [SKILL.md](SKILL.md) for the full instructions.

## Install

See the [repo README](../../README.md) for installation options.
