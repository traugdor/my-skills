# comment-cleanup

A Claude skill for sifting through source code and cleaning up comments that were written for an AI assistant's benefit rather than for human maintainers - reasoning trails, self-directed notes, and comments that over-explain what the next line already says. Worthwhile comments get promoted into concise, idiomatic documentation (docstrings/JSDoc/doc-comments); the rest gets deleted.

## Usage

**Option A: npx skills**

```bash
npx skills add traugdor/comment-cleanup/skills/comment-cleanup
```

This installs the skill into your agent's skills directory automatically. If Claude Code doesn't pick it up right away, check whether it landed in `~/.agents/skills/` instead of `~/.claude/skills/` - some installers use the former, and the two don't always sync automatically.

**Option B: manual install**

Copy the `skills/comment-cleanup/` folder into your skills directory (e.g. `~/.claude/skills/` for Claude Code, or upload via the skill picker in Claude.ai/Cowork). Once installed, just ask Claude to clean up comments in a file or directory and it'll trigger automatically.

## What it does

- Detects language by file extension and follows that language's normal doc-comment convention
- Strips comments addressed to an AI/reviewer instead of a future human maintainer
- Trims over-explaining comments down to the one useful fact, then promotes it to a proper doc comment
- Consolidates documentation to the top-level unit (struct/class/module) when there's a natural anchor point, rather than scattering it across every method
- Leaves terse single-line comments, real TODOs, license headers, and dead code untouched
- Edits in place and gives a short summary of what was changed

See `skills/comment-cleanup/SKILL.md` for the full instructions.

## License

MIT - see [LICENSE](LICENSE).
