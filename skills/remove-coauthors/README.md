# remove-coauthors

A Claude skill for destructively stripping AI/assistant co-authoring attribution from a repository - both `Co-Authored-By:` trailers in commit messages (via history rewrite) and matching attribution comments in source files.

**This is a destructive operation.** Rewriting commit history changes commit hashes for every rewritten commit and everything downstream of it. The skill always creates a backup tag before rewriting, always records the rewrite in a committed `HISTORY-REWRITES.md` file so the removal itself is never silent, and never pushes or force-pushes on its own - those remain separate, explicitly-confirmed actions.

## What it does

- Creates a `pre-decoauthor-backup` tag before touching history
- Commits a `HISTORY-REWRITES.md` entry (date, branch, commit range, backup tag) *before* rewriting, so there's a permanent, plain-text record that AI co-authoring attribution existed and was removed - the rewrite can never erase the fact that it happened
- Scrubs co-authoring comments/headers out of source files as an ordinary, reviewable commit
- Rewrites commit messages to remove `Co-Authored-By:` (and similar) trailers, using `git filter-repo` (or `git filter-branch` as a fallback)
- Verifies no attribution trailers remain, confirms the record survived the rewrite, and reports what changed
- Never force-pushes or deletes the backup automatically

## Usage

Ask Claude to remove AI co-author tags / de-coauthor the repository. Claude will confirm scope (which branches/range, whether to touch source too, backup, and push intentions) before rewriting anything.

See [SKILL.md](SKILL.md) for the full instructions.

## Install

See the [repo README](../../README.md) for installation options.
