---
description: Summarize working-tree changes + rationale into CHANGELOG.md
allowed-tools: Bash(git status:*), Bash(git diff:*), Read, Edit
---

Update `CHANGELOG.md` for this repo. The working tree is the source of truth, not git history.

Changed files:
!`git status --short`

Tracked edits:
!`git diff --stat`

Full diff:
!`git diff`

Optional focus: $ARGUMENTS

Steps:
1. Read the change signals above. For untracked files (`??` in status), `git diff` won't show contents — Read them if relevant.
2. Establish the rationale for each meaningful change: from this conversation if the changes were made here, else inferred from the code. If the *why* is genuinely unclear, ask one short question rather than inventing it.
3. Read `CHANGELOG.md` (create it with a Keep a Changelog header if absent) and merge entries into `## [Unreleased]`, grouped under `### Added` / `### Changed` / `### Fixed` / `### Removed`.
4. Write each entry as **what — why**. Group by behavior, not file-by-file.

Rules: skip noise (formatting churn, lockfiles, generated output, the changelog itself). Concise, concrete, no marketing language. Only edit `CHANGELOG.md`. If there are no substantive changes, say so and change nothing.
