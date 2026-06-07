---

kanban-plugin: board

---

## Backlog

- [ ] Disable upstream `superpowers@claude-plugins-official` on this machine so only one bootstrap fires (the fork is installed but upstream wasn't disabled yet)
- [ ] Set up on the work machine: `/plugin marketplace add 44-Bulldog/hunter-harness` → `/plugin install superpowers-hunter@hunter-harness` → disable upstream there
- [ ] Smoke-test the harness: fresh session, "let's build a small X" → confirm `planning` triggers (not brainstorming) and there's no mandatory-TDD nag
- [ ] (Directional) Path 2 refactor: if upstream cruft bothers you, slim to a lean from-scratch plugin — only the kept skill files + a minimal bootstrap. Not urgent.

## In Progress



## Done

- [x] Push harness to GitHub (`44-Bulldog/hunter-harness`) and install locally; relative marketplace `source` confirmed working
- [x] Port your own commands into the harness: `/changelog`, `/backlog`, `/plan`
- [x] Clean orphan support files left after the skill rewrites
- [x] Sharpen the `planning` / `writing-plans` boundary (Option A — decide vs. build-the-execution-doc)

%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false]}
```
%%
