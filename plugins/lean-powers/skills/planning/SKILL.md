---
name: planning
description: Use whenever the user wants to build, add, implement, refactor, redesign, or "figure out how to approach" something non-trivial — and whenever they say "help me think through" a decision. Lays out approaches with tradeoffs, then scopes down to the minimal testable version before any code.
---

# Planning

Use this before building anything non-trivial, and whenever the user wants to think through a decision. Not Socratic interrogation — lay out the real options, weigh them honestly, then ship the smallest testable slice.

## The loop

1. **Understand the goal and constraints.** Get enough context that execution could run mostly autonomously afterward. Read the relevant code/repo first — don't ask what you can find. Ask only the questions that actually block progress, not a checklist.

2. **Lay out 2–3 approaches with tradeoffs.** Tie each tradeoff to *this* plan, *this* repo, and existing dependencies — not generic pros/cons. A recommendation is welcome, but always show the alternatives and why. Even when the user has already proposed an approach, walk through its tradeoffs against the alternatives before committing — they usually still pick theirs, but want to reason through it first.

3. **Converge, then scope DOWN to the minimal testable version.** Once the approach is chosen, cut it to the smallest slice that can actually be run and tested. Sequence the rest so each step is independently testable — but count acceptance tests, not features: a step earns its own boundary only if its test buys something (de-risks a real unknown, or is a checkpoint you'd actually stop at). Favor dead-simple over production-ready: discuss production concerns (scale, error handling, edge cases) so they're on the table, but don't build them yet.

4. **Park the spillover.** Everything deferred during scope-down goes to the repo's `BACKLOG.md` (Obsidian Kanban format) — at the repo root (`git rev-parse --show-toplevel`), not the current subdirectory. Groom while you're there: merge near-duplicates, drop exact dupes, flag contradictions. Don't just append.

5. **Hand off to execution.** With the approach and the minimal slice defined: for a small slice, just build it — implementation is writing code + testing + fixing, iterating testably. For a large or multi-file slice, use the `writing-plans` skill first to turn it into a concrete task doc, then execute. If the work decomposes into 2+ streams with no shared state or ordering dependency, you may *propose* running them as parallel sub-agents — each isolated in its own git worktree if they touch files — but ask before dispatching; default to serial and in-place. Check back in only where the plan was genuinely uncertain.

**Roadmap.** If the plan sets or shifts the project's overall direction, reflect it in `BUILD_PLAN.md` — a high-level living roadmap (near-term detailed, further-out directional, kept lean). Ongoing progress on it is reconciled by `/lint-docs`, not maintained here.

## Red flags — stop if you catch yourself

| Thought | Reality |
|---|---|
| "I'll just start coding, the approach is obvious" | Lay out the tradeoffs first. Obvious-to-you ≠ agreed. |
| "Let me build the robust/production version" | Build the minimal testable version. Production is a later iteration. |
| "I should ask 8 clarifying questions" | Ask only what blocks. Find the rest in the code. |
| "The user gave their approach, so just do it" | Still surface the tradeoffs against alternatives first. |
| "I'll keep these extra ideas in the plan" | Park them in BACKLOG.md; don't bloat the current slice. |
| "I'll just spin up parallel agents / a worktree for this" | Only if the work is genuinely independent — propose it and wait for approval. Never fan out unilaterally; default serial/in-place. |

## Output

Present the plan in sections the user can react to: goal/constraints → approaches + tradeoffs (with a recommendation) → the minimal testable slice → what's getting backlogged. Concise, concrete, no marketing language.
