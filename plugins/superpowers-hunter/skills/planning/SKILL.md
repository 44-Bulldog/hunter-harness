---
name: planning
description: Use before any non-trivial build, feature, or design work, and whenever the user says "help me think through" something. Lays out approaches with tradeoffs, then scopes down to the minimal testable version before any code.
---

# Planning

Use this before building anything non-trivial, and whenever the user wants to think through a decision. Not Socratic interrogation — lay out the real options, weigh them honestly, then ship the smallest testable slice.

## The loop

1. **Understand the goal and constraints.** Get enough context that execution could run mostly autonomously afterward. Read the relevant code/repo first — don't ask what you can find. Ask only the questions that actually block progress, not a checklist.

2. **Lay out 2–3 approaches with tradeoffs.** Tie each tradeoff to *this* plan, *this* repo, and existing dependencies — not generic pros/cons. A recommendation is welcome, but always show the alternatives and why. Even when the user has already proposed an approach, walk through its tradeoffs against the alternatives before committing — they usually still pick theirs, but want to reason through it first.

3. **Converge, then scope DOWN to the minimal testable version.** Once the approach is chosen, cut it to the smallest slice that can actually be run and tested. Sequence the rest so each step is independently testable. Favor dead-simple over production-ready: discuss production concerns (scale, error handling, edge cases) so they're on the table, but don't build them yet.

4. **Park the spillover.** Everything deferred during scope-down goes to `BACKLOG.md` at the repo root (Obsidian Kanban format). Groom while you're there: merge near-duplicates, drop exact dupes, flag contradictions. Don't just append.

5. **Hand off to execution.** With the approach and the minimal slice defined: for a small slice, just build it — implementation is writing code + testing + fixing, iterating testably. For a large or multi-file slice, use the `writing-plans` skill first to turn it into a concrete task doc, then execute. Check back in only where the plan was genuinely uncertain.

**Roadmap.** If the plan sets or shifts the project's overall direction, reflect it in `BUILD_PLAN.md` — a high-level living roadmap (near-term detailed, further-out directional, kept lean). Ongoing progress on it is reconciled by `/lint-docs`, not maintained here.

## Red flags — stop if you catch yourself

| Thought | Reality |
|---|---|
| "I'll just start coding, the approach is obvious" | Lay out the tradeoffs first. Obvious-to-you ≠ agreed. |
| "Let me build the robust/production version" | Build the minimal testable version. Production is a later iteration. |
| "I should ask 8 clarifying questions" | Ask only what blocks. Find the rest in the code. |
| "The user gave their approach, so just do it" | Still surface the tradeoffs against alternatives first. |
| "I'll keep these extra ideas in the plan" | Park them in BACKLOG.md; don't bloat the current slice. |

## Output

Present the plan in sections the user can react to: goal/constraints → approaches + tradeoffs (with a recommendation) → the minimal testable slice → what's getting backlogged. Concise, concrete, no marketing language.
