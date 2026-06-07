# Hunter's Claude Code Harness

A personal coding harness for Claude Code, packaged as a plugin (`superpowers-hunter`) under a small marketplace (`hunter-harness`). Built to run identically on work and personal machines.

It's a pruned, retuned fork of [obra/superpowers](https://github.com/obra/superpowers) (based on v5.1.0, MIT) — keeping the skills that fit a plan-heavy, MVP-first workflow and cutting the heavier execution machinery.

## What changed vs. upstream superpowers

**Kept:** `writing-plans`, `systematic-debugging`, `verification-before-completion`, `writing-skills`.

**Added:** `planning` — lay out approaches with tradeoffs, then scope to the minimal testable version, parking spillover to `BACKLOG.md`. Replaces upstream `brainstorming`.

**Softened:**
- `test-driven-development` → use when it earns its keep; skip for spikes, glue, and MVP slices.
- `using-superpowers` (the always-on bootstrap) → dropped the "MUST check before ANY response / even a 1% chance" absolutism. Now: check skills for non-trivial tasks only, defer to the user's instructions and saved memory, and route "let's build X" to `planning`.

**Cut:** `brainstorming`, `subagent-driven-development`, `dispatching-parallel-agents`, `executing-plans`, `using-git-worktrees`, `finishing-a-development-branch`, `requesting-code-review`, `receiving-code-review`.

## Install

```
/plugin marketplace add <your-github-user>/claude-harness
/plugin install superpowers-hunter@hunter-harness
```

Then disable upstream superpowers so the two don't both inject a bootstrap — in `~/.claude/settings.json`, set `"superpowers@claude-plugins-official": false` under `enabledPlugins` (or toggle via the `/plugin` UI).

## Layout

- `.claude-plugin/marketplace.json` — marketplace manifest
- `plugins/superpowers-hunter/` — the plugin: `skills/`, `hooks/` (SessionStart bootstrap), `.claude-plugin/plugin.json`

## Attribution

Derived from obra/superpowers (MIT). The original skill content and the SessionStart hook plumbing are upstream's; the pruning, the softened bootstrap/TDD, and the `planning` skill are local changes.
