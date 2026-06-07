---
name: changelog
description: Use after completing any meaningful change (feature, fix, refactor), and whenever the user asks to update the changelog. Records what changed and why in CHANGELOG.md, dated so history is easy to scan.
---

# Changelog

Keep `CHANGELOG.md` current as work lands. Fire this when a meaningful unit of work is done — a feature, fix, or refactor — not after every tiny edit. Also fire when the user explicitly asks.

## What to do

1. Figure out what actually changed. The working tree is the source of truth: run `git status --short` and `git diff`, and read untracked files the diff won't show. The conversation usually has the *why*.
2. Read `CHANGELOG.md` at the repo root. Create it with a [Keep a Changelog](https://keepachangelog.com) header if absent; otherwise respect the repo's existing structure.
3. Add entries under `## [Unreleased]`, grouped by `### Added` / `### Changed` / `### Fixed` / `### Removed`. Include the date (today's date, from context) — either a dated group heading or a date on each entry — so it's easy to see when things happened.
4. Write each entry as **what — why**, grouped by behavior, not file-by-file.

## Rules

- Skip noise: formatting churn, lockfiles, generated output, the changelog itself.
- Establish the *why* from the conversation; if it's genuinely unclear, ask one short question rather than inventing it.
- Concise, concrete, no marketing language.
- Only edit `CHANGELOG.md`.
