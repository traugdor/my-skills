# my-skills

A collection of Claude skills.

## Skills

- [comment-cleanup](skills/comment-cleanup/README.md) - Sifts through source code and cleans up comments that were written for an AI assistant's benefit rather than for human maintainers, promoting what's worth keeping into concise, idiomatic documentation.
- [remove-coauthors](skills/remove-coauthors/README.md) - Destructively and permanently strips AI co-authoring attribution from source code and commit history via history rewrite - no backup, no record file, no auto-push.
- [no-gaslighting](skills/no-gaslighting/README.md) - Stops the agent from arguing with the user about the user's own lived experience of the session instead of just acknowledging it and moving on.
- [where-was-i](skills/where-was-i/README.md) - Rereads the conversation to find where things derailed and gives a short, evidence-backed catch-up on what you were doing and where you were headed.

## Usage

**Option A: npx skills**

```bash
npx skills add traugdor/my-skills
```

This installs skills into your agent's skills directory automatically. If Claude Code doesn't pick them up right away, check whether they landed in `~/.agents/skills/` instead of `~/.claude/skills/` - some installers use the former, and the two don't always sync automatically.

**Option B: manual install**

Copy the relevant folder from `skills/` into your skills directory (e.g. `~/.claude/skills/` for Claude Code, or upload via the skill picker in Claude.ai/Cowork). Once installed, just ask Claude to use it and it'll trigger automatically.

See each skill's README (linked above) for what it does in detail.

## License

MIT - see [LICENSE](LICENSE).
