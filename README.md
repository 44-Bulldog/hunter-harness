# Hunter's Claude Code Harness

A personal Claude Code plugin that shapes how Claude approaches work: **plan first, build the smallest testable thing, weigh options instead of dictating, and keep the project's docs current.** It's a curated set of *skills* — workflows Claude triggers automatically when the moment matches — installed once and used identically on every machine.

Plugin: `superpowers-hunter` · Marketplace: `hunter-harness`

## How it works

- **A SessionStart hook** injects a short bootstrap at the start of every session, telling Claude which skills exist and when to reach for them.
- **Skills** auto-trigger: each has a one-line description Claude sees at all times, and when your message or the task matches, Claude loads the full skill and follows it — no command to remember. You just talk ("let's build X", "backlog that", "update the changelog") and the right skill fires.

The skill check is advisory, not absolute: Claude reaches for a skill when it fits and otherwise just answers. Your explicit instructions and saved memory always outrank a skill.

## The workflow it encodes

The default loop for any non-trivial build:

1. **Plan** (`planning`) — understand the goal, lay out 2–3 approaches with tradeoffs, then scope down to the *minimal testable slice*. Spillover gets parked via `backlog`.
2. **Build** — for a small slice, just write it and iterate testably. For a large or multi-file slice, write a detailed task plan first (`writing-plans`).
3. **Debug** (`systematic-debugging`) — on any bug or test failure, find the root cause before patching.
4. **Verify** (`verification-before-completion`) — run the thing and show evidence before claiming it works.
5. **Record** (`changelog`) — log what changed and why as work lands; deferred ideas go to the backlog.

## Skills

| Skill | Fires when | What it does |
|---|---|---|
| `planning` | Before non-trivial build/design work, or "help me think through X" | Lays out approaches + tradeoffs, scopes to the minimal testable version, backlogs the spillover. The front door for new work. |
| `writing-plans` | After planning, only for large/multi-file slices | Turns the chosen slice into a concrete, file-by-file task plan detailed enough to execute autonomously. Skipped for small slices. |
| `systematic-debugging` | Any bug, test failure, or unexpected behavior | Structured root-cause analysis (tracing, defense-in-depth) before proposing a fix. |
| `test-driven-development` | Logic worth protecting — parsing, money, auth, bugfixes | RED-GREEN-REFACTOR, when it earns its keep. Skipped for spikes, glue, and throwaway MVP scaffolding. |
| `verification-before-completion` | About to claim something is done / fixed / passing | Run verification and confirm the output before asserting success. |
| `backlog` | You defer or park something instead of building it now ("backlog that") | Captures it into `BACKLOG.md` (Obsidian Kanban) at the repo root, merging duplicates and flagging contradictions. |
| `changelog` | After a meaningful change, or "update the changelog" | Records what changed + why in `CHANGELOG.md`, dated, from the working-tree diff plus the conversation. |
| `writing-skills` | Creating or editing a skill in this harness | Conventions for authoring and testing new skills. |
| `using-superpowers` | Session start (this is the bootstrap) | Tells Claude what skills exist, the routing, and the priority rules. |

## Install

**Locally (this machine, dev loop):**
```
/plugin marketplace add ~/claude-harness
/plugin install superpowers-hunter@hunter-harness
/reload-plugins        # or start a fresh session
```

**On another machine (from GitHub):**
```
/plugin marketplace add 44-Bulldog/hunter-harness
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
   ├─ hooks/                            # SessionStart bootstrap injector
   └─ skills/                           # one folder per skill, each a SKILL.md
```

## Customizing

- **Change behavior:** edit a skill's `SKILL.md` — the frontmatter `description` controls *when* it fires; the body controls *what* Claude does. Re-run `/reload-plugins` or start a fresh session.
- **Add a skill:** new folder `plugins/superpowers-hunter/skills/<name>/SKILL.md`. The `writing-skills` skill covers the conventions.
- **Want an explicit trigger?** A skill can be paired with a `plugins/superpowers-hunter/commands/<name>.md` slash command, but this harness deliberately uses skills so behavior is automatic rather than something to remember.

## Credits

Derived from [obra/superpowers](https://github.com/obra/superpowers) (MIT) — the original skill content and the SessionStart hook plumbing are upstream's. The curation, the softened bootstrap and TDD, and the `planning` / `backlog` / `changelog` skills are local.
