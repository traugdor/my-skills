# my-skills

A collection of Claude skills.

## Skills

- [comment-cleanup](skills/comment-cleanup/README.md) - Sifts through source code and cleans up comments that were written for an AI assistant's benefit rather than for human maintainers, promoting what's worth keeping into concise, idiomatic documentation.
- [remove-coauthors](skills/remove-coauthors/README.md) - Destructively and permanently strips AI co-authoring attribution from source code and commit history via history rewrite - no backup, no record file, no auto-push.
- [no-gaslighting](skills/no-gaslighting/README.md) - Stops the agent from arguing with the user about the user's own lived experience of the session instead of just acknowledging it and moving on.
- [where-was-i](skills/where-was-i/README.md) - Rereads the conversation to find where things derailed and gives a short, evidence-backed catch-up on what you were doing and where you were headed.

## Install

Requires [`npx`](https://docs.npmjs.com/cli/v10/commands/npx) (ships with Node.js). All commands below use `-g` for a global (user-level) install, available to every project on your machine - drop `-g` to install into just the current project instead.

**Everything, every agent (simplest option)**

```bash
npx skills add traugdor/my-skills -g -y
```

Installs all skills in this repo and auto-detects which agents you have on your machine (Claude Code, Cursor, etc.) - no prompts, no picking anything by hand.

**Just one or two specific skills**

```bash
npx skills add traugdor/my-skills -g -s remove-coauthors no-gaslighting
```

Use `-s`/`--skill` with one or more skill names from the list above. Omit `-y` if you'd like to confirm agent detection interactively.

**Just specific agents**

```bash
npx skills add traugdor/my-skills -g -a claude-code cursor
```

Use `-a`/`--agent` to target only the agents you name, instead of every agent detected on your system. `-s` and `-a` combine freely, e.g. `-s remove-coauthors -a claude-code`.

**Keeping skills up to date**

```bash
npx skills update -g
```

Run any time to pull the latest version of every installed skill from this repo.

**Manual install (no npx)**

Copy the relevant folder from `skills/` into your agent's skills directory (e.g. `~/.claude/skills/` for Claude Code), or upload it via the skill picker in Claude.ai/Cowork. Once installed, just ask Claude to use it and it'll trigger automatically.

If a skill doesn't get picked up right away after an `npx` install, check whether it landed in `~/.agents/skills/` instead of `~/.claude/skills/` - some installers use the former, and the two don't always sync automatically.

See each skill's README (linked above) for what it does in detail.

## License

MIT - see [LICENSE](LICENSE).
