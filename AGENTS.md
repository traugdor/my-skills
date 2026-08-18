# AGENTS.md

Instructions for agents working in this repository.

## Using the comment-cleanup skill

When asked to clean up, declutter, or "de-AI-ify" comments in a file or directory, use the `comment-cleanup` skill (see [skills/comment-cleanup/SKILL.md](skills/comment-cleanup/SKILL.md)). It also triggers on requests to make code comments more human-readable or professional before committing or shipping.

Follow the skill's rules exactly:

- Delete AI-directed notes (comments addressed to an assistant/reviewer rather than a future maintainer).
- Trim over-explaining comments to the one useful fact, and promote genuinely useful facts to proper doc comments (docstrings/JSDoc/etc.) at the appropriate anchor point.
- Leave terse single-line comments, real TODOs, license headers, and dead code untouched.
- Edit in place and give a short summary of what changed.

## Using the remove-coauthors skill

When asked to remove AI co-author tags or "de-coauthor" the repository, use the `remove-coauthors` skill (see [skills/remove-coauthors/SKILL.md](skills/remove-coauthors/SKILL.md)). This rewrites commit history, is destructive, and is permanent - it creates no backup and no record file, by design, so it never undermines its own purpose. Always follow the skill's confirmation and no-auto-push requirements exactly; a request matching this skill's trigger is not itself consent to skip those steps. Once it has run against a repo, that repo is treated as having never had AI involvement - the agent must never again add AI/assistant attribution there, in this session or any future one.

## Using the no-gaslighting skill

The `no-gaslighting` skill (see [skills/no-gaslighting/SKILL.md](skills/no-gaslighting/SKILL.md)) always applies, not just when working in this repo's skill files: if the user reports something that happened during the session - a restart, a dropped response, something the agent said - accept it and move on. Never argue that it couldn't have happened, never repeat a rejected explanation, never reframe the user's account as a misunderstanding.

## Using the where-was-i skill

When the user asks "where was I," "what was I doing," "catch me up," or invokes `/where-was-i` (see [skills/where-was-i/SKILL.md](skills/where-was-i/SKILL.md)), reread the actual conversation and verify against real repo/filesystem state before summarizing - don't reconstruct from a vague sense of "the gist." Report what was last happening, what state it's actually in now (with evidence), and where it looked like things were headed. No editorializing about why the derail happened, per [no-gaslighting](skills/no-gaslighting/SKILL.md).

## Commits

Never add a `Co-Authored-By` line or any other AI/assistant co-author tag to commit messages produced in this repository, including when running the comment-cleanup skill or any other automated workflow.
