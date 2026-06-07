---

kanban-plugin: board

---

## Backlog

- [ ] Push `~/claude-harness` to GitHub, then install on work + personal machines; disable upstream `superpowers@claude-plugins-official` on both _(needs your GitHub auth)_
- [ ] Verify the marketplace relative `source` (`./plugins/superpowers-hunter`) installs cleanly; if `/plugin install` rejects it, point `source` at the GitHub URL instead _(needs you to run `/plugin install`)_
- [ ] Smoke-test the harness: fresh session, "let's build a small X" → confirm `planning` triggers (not brainstorming) and there's no mandatory-TDD nag
- [ ] (Directional) Path 2 refactor: if upstream cruft bothers you, slim to a lean from-scratch plugin — only the kept skill files + a minimal bootstrap. Not urgent.

## In Progress



## Done

- [x] Port your own commands into the harness: `/changelog`, `/backlog`, `/plan`
- [x] Clean orphan support files left after the skill rewrites
- [x] Sharpen the `planning` / `writing-plans` boundary (Option A — decide vs. build-the-execution-doc)

%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false]}
```
%%
