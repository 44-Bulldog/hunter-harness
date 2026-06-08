# Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com).

## [Unreleased]

### Changed

- `writing-plans`: added counter-pressure to the *Task granularity* section to curb over-phased build plans — use the fewest independently-verifiable steps (merge work that's always built/tested together; ~1–3 tasks per 2–4 file change, not one per file), and keep deferred work as a one-line pointer to `BACKLOG.md` rather than a full task block unless detail is explicitly requested. The skill previously only pushed toward finer splitting, with no pull back. (2026-06-07)
