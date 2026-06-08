# lean-powers

lean-powers is a Claude Code plugin that shapes *how* a coding agent works with you: think the problem through, build the smallest thing you can actually test, then iterate — and keep the repo's own docs alive so every session starts oriented. A small, curated set of *skills* that fire automatically, plus one *command*. Install once; same behavior on every machine.

A lean fork of [obra/superpowers](https://github.com/obra/superpowers) — fewer skills, softened ceremony, tuned for solo, fast-moving work.

Plugin: `lean-powers` · Marketplace: `lean-powers`

## In a nutshell

Four bets, expanded in the sections below:

- **Plan deeply, then build lean.** The value is in thinking until execution is obvious — *then* shipping the smallest testable slice and iterating, not building the production version up front.
- **A thinking partner, not an order-taker.** When you're deciding, the agent lays out real options with tradeoffs instead of asserting one — even when you've already picked. Your instructions and memory always outrank a skill.
- **The repo is the context.** A handful of living docs stay current as you work, so the next session (you or an agent) reads state instead of re-deriving it.
- **Discipline where it pays, not as ceremony.** Root-cause before patching, tests where logic is worth protecting, verify before claiming done — but no mandatory process tax on throwaway work.

## What it encodes

The behavior splits into four parts. They're meant to be MECE — each owns a distinct stage of the work.

### 1. Planning — decide before you build

The `planning` skill is the front door for any non-trivial work. It lays out 2–3 approaches with tradeoffs tied to *your* repo and dependencies (not generic pros/cons), recommends one but shows the alternatives, and walks the tradeoffs even when you've already chosen — so you can reason through it rather than be railroaded. Once an approach is picked, it scopes **down** to the minimal testable slice; spillover is parked in the backlog, and any shift in direction is reflected in the build plan. Clarifying questions are limited to what actually blocks progress — the rest it finds by reading the code.

### 2. Lean execution — the smallest testable steps

This is the namesake. Build the minimal slice first and iterate testably, favoring dead-simple over production-ready (production concerns get discussed so they're on the table, not pre-built).

- **Small slice → just build it.** No written plan; a plan would be overhead.
- **Large / multi-file slice → `writing-plans` first**, turning it into a concrete, file-specific task doc detailed enough to execute autonomously.
- **When it does write a plan, it stays lean:** the fewest independently-verifiable steps, merging work that's always built and tested together rather than one task per file, and keeping *deferred* work as a one-line pointer to the backlog instead of fully specced phases. The point is to avoid over-phased plans that re-expand everything you just scoped down.

### 3. Living docs — the repo is the context (state management)

Four documents the harness keeps current in every repo, so context survives between sessions:

- **CHANGELOG.md** — what changed and why (`changelog` skill, on each meaningful change)
- **BACKLOG.md** — deferred work, as an **Obsidian Kanban board** committed in the repo (`backlog` skill, whenever something's parked)
- **BUILD_PLAN.md** — the high-level roadmap; near-term detailed, further-out directional (seeded by `planning`, maintained by lint)
- **README.md** — what the project is and how to use it (refreshed by `changelog` when a change affects it)

The split that makes this work: the *why* behind a change is perishable, so skills capture it in the moment; the *state* of the docs is reconstructable, so `/lint-docs` audits all four against the repo and fixes drift on demand.

### 4. Discipline where it pays — not ceremony

- **`systematic-debugging`** — find the root cause (tracing, defense-in-depth) before proposing a fix.
- **`test-driven-development`** — RED-GREEN-REFACTOR when logic is worth protecting (parsing, money, auth, bugfixes); skipped for spikes, glue, and throwaway scaffolding.
- **`verification-before-completion`** — run the check and confirm the output before claiming something's done.

No mandatory test-first-on-everything, and no forced subagent / worktree / review pipeline that overscopes solo, fast-moving work.

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
- **Skills auto-trigger from their descriptions** — you just talk ("let's build X", "backlog that", "update the changelog") and the right one fires.
- **One command, `/lint-docs`**, for the explicit, on-demand reconcile of the living docs.
- **Skills are advisory, not absolute** — your explicit instructions and saved memory always outrank them.

End to end, that's: **plan** (`planning`) → **build** (lean slice, or `writing-plans` for big ones) → **debug** (`systematic-debugging`) → **verify** (`verification-before-completion`) → **record** (`changelog`) → **reconcile** (`/lint-docs`, on demand).

## Skills

| Skill | Fires when | What it does |
|---|---|---|
| `planning` | Before non-trivial build/design work, or "help me think through X" | Approaches + tradeoffs, scopes to the minimal testable version, backlogs the spillover, seeds `BUILD_PLAN`. The front door for new work. |
| `writing-plans` | After planning, only for large/multi-file slices | Turns the chosen slice into a concrete, file-by-file task plan — fewest verifiable steps, deferred work left as a pointer. Skipped for small slices. |
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
/plugin marketplace add ~/lean-powers
/plugin install lean-powers@lean-powers
/reload-plugins        # or start a fresh session
```

**On another machine (from GitHub):**
```
/plugin marketplace add 44-Bulldog/lean-powers
/plugin install lean-powers@lean-powers
```

If another skills/bootstrap plugin is installed, disable it so two bootstraps don't both fire — set it to `false` under `enabledPlugins` in `~/.claude/settings.json`, or toggle it in the `/plugin` UI. The harness takes effect on a fresh session, since the bootstrap loads via the SessionStart hook.

## Layout

```
lean-powers/
├─ .claude-plugin/marketplace.json      # marketplace manifest
├─ README.md
└─ plugins/lean-powers/
   ├─ .claude-plugin/plugin.json        # plugin manifest
   ├─ hooks/                            # SessionStart bootstrap injector
   ├─ commands/                         # /lint-docs
   └─ skills/                           # one folder per skill, each a SKILL.md
```

## Customizing

- **Change behavior:** edit a skill's `SKILL.md` — the frontmatter `description` controls *when* it fires; the body controls *what* the agent does. Re-run `/reload-plugins` or start a fresh session.
- **Add a skill:** new folder `plugins/lean-powers/skills/<name>/SKILL.md`. The `writing-skills` skill covers the conventions.
- **Add a command:** new `plugins/lean-powers/commands/<name>.md` for an explicit `/slash` trigger (used here only for `/lint-docs` — auto-firing behaviors are skills).

## Credits

Derived from [obra/superpowers](https://github.com/obra/superpowers) (MIT) — the original skill content and the SessionStart hook plumbing are upstream's. The curation, the softened bootstrap and TDD, the `planning` / `backlog` / `changelog` skills, and the `/lint-docs` command are local. License retains the upstream copyright; see [LICENSE](plugins/lean-powers/LICENSE).
