# Hunter's Claude Code Harness

A personal Claude Code plugin that shapes how Claude approaches work: **plan first, build the smallest testable thing, weigh options instead of dictating, and keep the repo's docs current.** It's a set of *skills* (workflows Claude triggers automatically) plus one *command* for explicit doc maintenance — installed once, used identically on every machine.

Plugin: `superpowers-hunter` · Marketplace: `hunter-harness`

## How it works

- **A SessionStart hook** injects a short bootstrap each session, telling Claude which skills exist and when to reach for them.
- **Skills** auto-trigger from their descriptions — you just talk ("let's build X", "backlog that", "update the changelog") and the right one fires.
- **One command**, `/lint-docs`, for the explicit, on-demand reconcile of the repo's living docs.

Skills are advisory, not absolute — your explicit instructions and saved memory always outrank them.

## Living docs

The harness keeps four documents current in every repo, so any session starts with clear context:

- **CHANGELOG.md** — what changed and why (`changelog` skill, on each meaningful change)
- **BACKLOG.md** — deferred work, as an **Obsidian Kanban board** committed in the repo (`backlog` skill, whenever something's parked)
- **BUILD_PLAN.md** — the high-level roadmap; near-term detailed, further-out directional (seeded by `planning`, maintained by lint)
- **README.md** — what the project is and how to use it (refreshed by `changelog` when a change affects it)

The split: the *why* behind a change or a deferral is perishable, so skills capture it in the moment; the *state* of the docs is reconcilable, so `/lint-docs` audits all four against the repo and fixes drift on demand.

## The workflow it encodes

1. **Plan** (`planning`) — approaches + tradeoffs, then scope to the minimal testable slice; spillover → `backlog`; direction → `BUILD_PLAN`.
2. **Build** — small slice: just build it and iterate testably. Large/multi-file: write a task plan first (`writing-plans`).
3. **Debug** (`systematic-debugging`) — find the root cause before patching.
4. **Verify** (`verification-before-completion`) — show evidence before claiming done.
5. **Record** (`changelog`) — log what + why as work lands; refresh the README if affected.
6. **Reconcile** (`/lint-docs`, on demand) — fix any drift across the four living docs.

## Skills

| Skill | Fires when | What it does |
|---|---|---|
| `planning` | Before non-trivial build/design work, or "help me think through X" | Lays out approaches + tradeoffs, scopes to the minimal testable version, backlogs the spillover, seeds `BUILD_PLAN`. The front door for new work. |
| `writing-plans` | After planning, only for large/multi-file slices | Turns the chosen slice into a concrete, file-by-file task plan detailed enough to execute autonomously. Skipped for small slices. |
| `systematic-debugging` | Any bug, test failure, or unexpected behavior | Structured root-cause analysis (tracing, defense-in-depth) before proposing a fix. |
| `test-driven-development` | Logic worth protecting — parsing, money, auth, bugfixes | RED-GREEN-REFACTOR, when it earns its keep. Skipped for spikes, glue, and throwaway MVP scaffolding. |
| `verification-before-completion` | About to claim something is done / fixed / passing | Run verification and confirm the output before asserting success. |
| `backlog` | You defer or park something instead of building it now ("backlog that") | Captures it into `BACKLOG.md` (Obsidian Kanban board) at the repo root, merging duplicates and flagging contradictions. |
| `changelog` | After a meaningful change, or "update the changelog" | Records what changed + why in `CHANGELOG.md` (dated); also refreshes `README.md` when a change affects how the project is used or structured. |
| `writing-skills` | Creating or editing a skill in this harness | Conventions for authoring and testing new skills. |
| `using-superpowers` | Session start (this is the bootstrap) | Tells Claude what skills exist, the routing, and the priority rules. |

## Commands

| Command | What it does |
|---|---|
| `/lint-docs` | Reconciles CHANGELOG, BACKLOG, BUILD_PLAN, and README against the repo's current state and **fixes drift in place**, then reports what it corrected. |

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
   ├─ commands/                         # /lint-docs
   └─ skills/                           # one folder per skill, each a SKILL.md
```

## Customizing

- **Change behavior:** edit a skill's `SKILL.md` — the frontmatter `description` controls *when* it fires; the body controls *what* Claude does. Re-run `/reload-plugins` or start a fresh session.
- **Add a skill:** new folder `plugins/superpowers-hunter/skills/<name>/SKILL.md`. The `writing-skills` skill covers the conventions.
- **Add a command:** new `plugins/superpowers-hunter/commands/<name>.md` for an explicit `/slash` trigger (used here only for `/lint-docs` — auto-firing behaviors are skills).

## Credits

Derived from [obra/superpowers](https://github.com/obra/superpowers) (MIT) — the original skill content and the SessionStart hook plumbing are upstream's. The curation, the softened bootstrap and TDD, the `planning` / `backlog` / `changelog` skills, and the `/lint-docs` command are local.
