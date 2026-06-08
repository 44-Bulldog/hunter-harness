---
description: Reconcile the repo's living docs (CHANGELOG, BACKLOG, BUILD_PLAN, README) against current reality and fix drift in place
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Read, Edit, Write
---

Reconcile this repo's four living docs against current reality, **fixing drift in place** (autofix), then report what you changed.

Repo state to check against:
!`git status --short`
!`git log --oneline -15`

Walk each doc that exists (note any that are missing):

1. **CHANGELOG.md** — does `## [Unreleased]` cover the recent changes (per the diff/log)? Add any missing entries as `what — why`. Infer the *why* from history/code; if a why is genuinely unrecoverable, note it rather than invent one. Don't duplicate existing entries.
2. **BACKLOG.md** (Obsidian Kanban board) — move cards that are actually done to **Done**; merge near-duplicates; drop exact duplicates; flag (don't silently delete) any card that contradicts another card or the current state.
3. **BUILD_PLAN.md** — mark shipped items done; prune stale ones; keep near-term detailed and further-out directional; trim bloat so it reflects actual current direction.
4. **README.md** — fix anything that no longer matches reality (install steps, structure, feature set, commands). Edit in place; don't append or bloat.

Then check the four **agree with each other**: a shipped feature in the README should have a changelog entry; something marked done shouldn't still be open in the plan or backlog.

Rules: edit in place, no bloat, concise and concrete; skip noise. When done, print a short **drift report** — per doc, what was stale and what you corrected (or "current" if nothing needed fixing).
