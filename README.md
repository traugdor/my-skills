# my-skills

A collection of Claude skills.

## Skills

- [comment-cleanup](skills/comment-cleanup/README.md) - Sifts through source code and cleans up comments that were written for an AI assistant's benefit rather than for human maintainers, promoting what's worth keeping into concise, idiomatic documentation.
- [remove-coauthors](skills/remove-coauthors/README.md) - Destructively strips AI co-authoring attribution from source code and commit history (history rewrite, with a backup tag, a permanent `HISTORY-REWRITES.md` record, and no auto-push).
- [no-gaslighting](skills/no-gaslighting/README.md) - Stops the agent from arguing with the user about the user's own lived experience of the session instead of just acknowledging it and moving on.

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
