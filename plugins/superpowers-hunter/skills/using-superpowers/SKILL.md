---
name: using-superpowers
description: Use when starting a conversation — establishes how and when to use skills.
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

You have a set of skills available via the **Skill** tool. They encode tested workflows — reach for them when they apply.

## When to check for a skill

Before a **non-trivial task**, check whether a skill applies and invoke it. You do not need a skill check before a simple question, a quick lookup, or routine chat — use judgment. The goal is leverage, not ceremony. When a skill clearly fits what you're doing, use it instead of improvising.

## Priority of instructions

1. **The user's explicit instructions** — CLAUDE.md, direct requests, and saved preferences/memory — always win.
2. **Skills** — override default behavior where they apply.
3. **Default behavior** — lowest priority.

If the user (or their memory/preferences) wants something a skill would do differently, follow the user.

## Routing

- "Let's build / add / design X" → **planning** first (approaches + tradeoffs, then scope to the minimal testable version), then implementation.
- "Fix this bug / test failure / unexpected behavior" → **systematic-debugging** first.
- About to claim something is done / fixed / passing → **verification-before-completion**.
- Have a *large* chosen slice to break into a concrete task doc → **writing-plans** (skip for small slices — just build them).
- Deciding to defer / park / backlog something instead of building it now → **backlog**.
- After completing a meaningful change (feature, fix, refactor), or asked to log one → **changelog** (keep `CHANGELOG.md` current with what + why).
- Writing or editing a skill → **writing-skills**.

## How to use a skill

Invoke it with the Skill tool — its content loads and you follow it. Don't Read skill files directly. Briefly announce which skill you're using and why. Skills marked rigid (e.g. systematic-debugging) should be followed closely; flexible ones adapt to context.
