# Hunter's Claude Code Harness

A personal Claude Code plugin that shapes *how* a coding agent works with you — plan first, build the smallest testable thing, weigh options instead of dictating, and keep the repo's docs alive as durable context. A small, curated set of *skills* (workflows the agent triggers automatically) plus one *command*, installed once and used identically on every machine.

Plugin: `superpowers-hunter` · Marketplace: `hunter-harness`

## Philosophy

This harness encodes one person's way of working with a coding agent. The bets it makes:

- **Plan deeply, then build the smallest testable thing.** Most of the value is in thinking a problem through until execution is obvious — *then* you ship a minimal slice, test it, and iterate, rather than building the full production version up front. Scoping down is a first-class step, and what gets cut is parked, not lost.
- **A thinking partner, not an order-taker.** When you're deciding something, the agent lays out real options with tradeoffs instead of asserting one answer — even when you've already picked one, so you can reason through it. Your instructions and saved preferences always outrank any skill.
- **The repo is the context.** Four living docs — changelog, backlog, build plan, README — stay current as you work, so any session (you or an agent) starts oriented instead of re-deriving state. The *why* behind a change is perishable, so it's captured in the moment; the *state* of the docs is reconstructable, so it's reconciled on demand.
- **Discipline where it pays, not as ceremony.** TDD when logic is worth protecting, skipped for spikes and glue. Root-cause debugging before patching. Verify before claiming done. But no mandatory test-first-on-everything, and no forced subagent / worktree / review pipeline that overscopes solo, fast-moving work.
- **Lean and portable.** One repo, one install, same behavior everywhere.

## Who it's for

**A good fit if you:**
- plan heavily and want the agent to think *with* you, not railroad you
- ship MVPs and iterate, and resent over-engineering
- want your repo's docs to stay current automatically, as context for the next session
- work mostly solo and fast, and find heavy process (mandatory TDD, review gates) gets in the way

**Probably not for you if you:**
- want enforced TDD and process rigor across a team
- prefer to just write code with zero planning ceremony
- are on a large codebase that benefits from a full subagent / worktree / review pipeline — consider upstream [superpowers](https://github.com/obra/superpowers) instead
- don't want an agent editing your changelog / README / plan automatically

## How it works

- **A SessionStart hook** injects a short bootstrap each session, telling the agent which skills exist and when to reach for them.
- **Skills** auto-trigger from their descriptions — you just talk ("let's build X", "backlog that", "update the changelog") and the right one fires.
- **One command**, `/lint-docs`, for the explicit, on-demand reconcile of the repo's living docs.

Skills are advisory, not absolute — your explicit instructions and saved memory always outrank them.

## Living docs

Four documents the harness keeps current in every repo:

- **CHANGELOG.md** — what changed and why (`changelog` skill, on each meaningful change)
- **BACKLOG.md** — deferred work, as an **Obsidian Kanban board** committed in the repo (`backlog` skill, whenever something's parked)
- **BUILD_PLAN.md** — the high-level roadmap; near-term detailed, further-out directional (seeded by `planning`, maintained by lint)
- **README.md** — what the project is and how to use it (refreshed by `changelog` when a change affects it)

The split: the *why* is perishable, so skills capture it in the moment; the *state* is reconcilable, so `/lint-docs` audits all four against the repo and fixes drift on demand.

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
| `using-superpowers` | Session start (this is the bootstrap) | Tells the agent what skills exist, the routing, and the priority rules. |

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
/plugin marketplace add 44-Bulldog/Bulldog
/plugin install superpowers-hunter@hunter-harness
```

(The marketplace's internal name is still `hunter-harness` — that comes from `marketplace.json`, not the repo name.)

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

- **Change behavior:** edit a skill's `SKILL.md` — the frontmatter `description` controls *when* it fires; the body controls *what* the agent does. Re-run `/reload-plugins` or start a fresh session.
- **Add a skill:** new folder `plugins/superpowers-hunter/skills/<name>/SKILL.md`. The `writing-skills` skill covers the conventions.
- **Add a command:** new `plugins/superpowers-hunter/commands/<name>.md` for an explicit `/slash` trigger (used here only for `/lint-docs` — auto-firing behaviors are skills).

## Credits

Derived from [obra/superpowers](https://github.com/obra/superpowers) (MIT) — the original skill content and the SessionStart hook plumbing are upstream's. The curation, the softened bootstrap and TDD, the `planning` / `backlog` / `changelog` skills, and the `/lint-docs` command are local.
