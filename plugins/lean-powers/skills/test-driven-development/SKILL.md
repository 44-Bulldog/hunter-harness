---
name: test-driven-development
description: Use when building logic worth protecting — features with real failure modes, bugfixes you want to stay fixed, anything you'll iterate on. Skip it for throwaway spikes and trivial glue.
---

# Test-Driven Development

TDD here is a tool, not a mandate. Use it where it earns its keep; skip it where it's ceremony.

## When to use it

- Logic with real failure modes (parsing, calculations, state machines, money, auth).
- Bugfixes — write the failing test that reproduces the bug first, so it stays fixed.
- Code you'll iterate on, where a suite keeps later changes honest.

## When to skip it (MVP-first)

- Throwaway spikes and exploratory scaffolding you're likely to rewrite.
- Trivial glue, wiring, config, and one-off scripts.
- A minimal testable slice where running the thing *is* the test — add tests as the slice proves out.

Default to the lighter choice. When unsure, ship the minimal version, see if it survives, then add tests as it solidifies. Treat coverage as a tradeoff to discuss, not a reflex.

## The cycle (when you do use it)

1. **RED** — write one failing test for the next small behavior. Run it; watch it fail for the right reason (feature missing, not a typo).
2. **GREEN** — write the minimal code to pass. No more than the test demands.
3. **REFACTOR** — clean up with the test green. Repeat.

Keep tests fast and behavior-focused: one behavior per test, real code over mocks where practical, no testing of framework internals or things that can't fail.
