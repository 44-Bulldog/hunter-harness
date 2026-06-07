---
name: writing-plans
description: Use when an approach is chosen and you need to turn it into a concrete, file-specific task plan that can be executed autonomously.
---

# Writing Plans

Use after `planning` has chosen an approach and scoped the minimal testable slice. This turns that slice into a plan detailed enough that execution is just writing code, testing, and fixing — with little further input.

Write the plan assuming the implementer has little context for this codebase: which files to touch, what each is responsible for, the actual code/changes, and how to check each step.

**Announce at start:** "Using writing-plans to write the implementation plan."

**Save to:** `plans/YYYY-MM-DD-<feature>.md` at the repo root, unless the user keeps plans elsewhere (their preference overrides this).

## File structure first

Before listing tasks, map which files get created or modified and what each is responsible for — this locks in the decomposition.
- One clear responsibility per file; prefer smaller, focused files over large ones.
- Files that change together live together; split by responsibility, not technical layer.
- In an existing codebase, follow its established patterns; don't unilaterally restructure.

## Task granularity

Break the work into concrete steps, each producing a self-contained change that stands on its own. A step names exact files, shows the actual code/change (no placeholders), and states how to verify it — running the thing, or a test where the logic warrants one (see the `test-driven-development` skill for when that's worth it). Keep the minimal slice first; sequence later steps so each is independently testable.

## Task structure

```markdown
### Task N: [Component]

**Files:**
- Create: `exact/path.ts`
- Modify: `exact/path.ts:120-140`

- [ ] Step: <action> — show the actual code/change, plus how to verify (command + expected result).
```

Use `- [ ]` checkboxes so progress is trackable.

## No placeholders

Every step contains the real content needed. These are plan failures — never write them:
- "TBD", "TODO", "implement later", "add appropriate error handling", "handle edge cases"
- "Write tests for the above" without the actual approach
- References to types, functions, or methods not defined in any task

## Self-review

After writing the plan, check it against the spec with fresh eyes:
1. **Coverage** — can you point to a task for each requirement? List gaps and add tasks.
2. **Placeholder scan** — remove any of the red flags above.
3. **Consistency** — do type / method / property names in later tasks match earlier ones?

Fix issues inline.

## Execution

When the plan is ready, execute it directly: work the tasks in order, run/test each step as you go, and keep it dead-simple (MVP-first). Commit at meaningful checkpoints, not on a fixed cadence. Surface anything the plan left genuinely uncertain; otherwise keep moving.
