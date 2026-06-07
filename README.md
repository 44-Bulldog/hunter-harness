# Hunter's Claude Code Harness

A personal Claude Code plugin that shapes how Claude approaches work: **plan first, build the smallest testable thing, weigh options instead of dictating, and keep the project's docs current.** It's a curated set of *skills* (workflows Claude triggers automatically when relevant) plus a few *commands* (slash commands you run on demand) — installed once and used identically on every machine.

Plugin: `superpowers-hunter` · Marketplace: `hunter-harness`

## How it works

Claude Code loads the harness through three mechanisms:

- **A SessionStart hook** injects a short bootstrap at the start of every session, telling Claude which skills exist and when to reach for them.
- **Skills** auto-trigger: each has a one-line description Claude sees at all times, and when a task matches, Claude loads the full skill and follows it — no command needed.
- **Commands** are manual `/` slash commands you invoke yourself.

The skill check is advisory, not absolute: Claude reaches for a skill on non-trivial tasks and otherwise just answers. Your explicit instructions and saved memory always outrank a skill.

## The workflow it encodes

The default loop for any non-trivial build:

1. **Plan** (`planning`) — understand the goal, lay out 2–3 approaches with tradeoffs, then scope down to the *minimal testable slice*. Deferred ideas get parked in the backlog.
2. **Build** — for a small slice, just write it and iterate testably. For a large or multi-file slice, write a detailed task plan first (`writing-plans`).
3. **Debug** (`systematic-debugging`) — on any bug or test failure, find the root cause before patching.
4. **Verify** (`verification-before-completion`) — run the thing and show evidence before claiming it works.
5. **Record** — log notable changes to the changelog; keep the README and build plan current.

## Skills

| Skill | Fires when | What it does |
|---|---|---|
| `planning` | Before non-trivial build/design work, or "help me think through X" | Lays out approaches + tradeoffs, scopes to the minimal testable version, backlogs the spillover. The front door for new work. |
| `writing-plans` | After planning, only for large/multi-file slices | Turns the chosen slice into a concrete, file-by-file task plan detailed enough to execute autonomously. Skipped for small slices. |
| `systematic-debugging` | Any bug, test failure, or unexpected behavior | Structured root-cause analysis (tracing, defense-in-depth) before proposing a fix. |
| `test-driven-development` | Logic worth protecting — parsing, money, auth, bugfixes | RED-GREEN-REFACTOR, when it earns its keep. Skipped for spikes, glue, and throwaway MVP scaffolding. |
| `verification-before-completion` | About to claim something is done / fixed / passing | Run verification and confirm the output before asserting success. |
| `writing-skills` | Creating or editing a skill in this harness | Conventions for authoring and testing new skills. |
| `using-superpowers` | Session start (this is the bootstrap) | Tells Claude what skills exist, the routing, and the priority rules. |

## Commands

| Command | What it does |
|---|---|
| `/plan [task]` | Manually run the planning loop on something. |
| `/backlog [item]` | Capture item(s) into `BACKLOG.md` at the repo root (Obsidian Kanban format), merging duplicates and flagging contradictions. Infers the item from the conversation if none is given. |
| `/changelog [focus]` | Read the working-tree diff plus the conversation's rationale and write `what — why` entries into `CHANGELOG.md`. The working tree is the source of truth, not git history. |

## Install

**Locally (this machine, dev loop):**
```
/plugin marketplace add ~/claude-harness
/plugin install superpowers-hunter@hunter-harness
/reload-plugins        # or start a fresh session
```

**On another machine (after pushing this repo to GitHub):**
```
/plugin marketplace add <your-github-user>/claude-harness
/plugin install superpowers-hunter@hunter-harness
```

If another skills/bootstrap plugin is installed, disable it so two bootstraps don't both fire — set it to `false` under `enabledPlugins` in `~/.claude/settings.json`, or toggle it in the `/plugin` UI. The harness takes effect on a fresh session, since the bootstrap loads via the SessionStart hook.

## Layout

```
claude-harness/
├─ .claude-plugin/marketplace.json      # marketplace manifest
├─ README.md
├─ BACKLOG.md                           # this repo's own backlog
└─ plugins/superpowers-hunter/
   ├─ .claude-plugin/plugin.json        # plugin manifest
   ├─ commands/                         # /changelog, /backlog, /plan
   ├─ hooks/                            # SessionStart bootstrap injector
   └─ skills/                           # one folder per skill, each a SKILL.md
```

## Customizing

- **Change behavior:** edit a skill's `SKILL.md` — the frontmatter `description` controls *when* it fires; the body controls *what* Claude does. Re-run `/reload-plugins` or start a fresh session.
- **Add a skill:** new folder `plugins/superpowers-hunter/skills/<name>/SKILL.md`. The `writing-skills` skill covers the conventions.
- **Add a command:** new `plugins/superpowers-hunter/commands/<name>.md`.

## Credits

Derived from [obra/superpowers](https://github.com/obra/superpowers) (MIT) — the original skill content and the SessionStart hook plumbing are upstream's. The curation, the softened bootstrap and TDD, the `planning` skill, and the commands are local.
