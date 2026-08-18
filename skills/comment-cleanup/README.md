# comment-cleanup

A Claude skill for sifting through source code and cleaning up comments that were written for an AI assistant's benefit rather than for human maintainers - reasoning trails, self-directed notes, and comments that over-explain what the next line already says. Worthwhile comments get promoted into concise, idiomatic documentation (docstrings/JSDoc/doc-comments); the rest gets deleted.

## What it does

- Detects language by file extension and follows that language's normal doc-comment convention
- Strips comments addressed to an AI/reviewer instead of a future human maintainer
- Trims over-explaining comments down to the one useful fact, then promotes it to a proper doc comment
- Consolidates documentation to the top-level unit (struct/class/module) when there's a natural anchor point, rather than scattering it across every method
- Leaves terse single-line comments, real TODOs, license headers, and dead code untouched
- Edits in place and gives a short summary of what was changed

## Two categories of comment it fixes

**1. AI-directed notes.** Comments addressed to an assistant or reviewer rather than to a future maintainer - references to "the old version," a conversation, or a spec doc as if the reader was there for it. These get deleted outright.

**2. Over-explaining comments.** Comments that narrate what the code already says or bury a genuinely useful fact (a precondition, a gotcha, an invariant) inside padding. These get trimmed to the useful fact and, where appropriate, promoted to a proper doc comment.

## Usage

Once installed, just ask Claude to clean up comments in a file or directory and it triggers automatically - e.g. "clean up the comments in this file" or "de-AI-ify the comments here."

See [SKILL.md](SKILL.md) for the full instructions.

## Install

See the [repo README](../../README.md) for installation options.
