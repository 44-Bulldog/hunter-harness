---

kanban-plugin: board

---

## Backlog

- [ ] Set up on the work machine: `/plugin marketplace add 44-Bulldog/hunter-harness` → `/plugin install superpowers-hunter@hunter-harness` → disable upstream there
- [ ] Smoke-test in a fresh session: `planning` fires on "let's build X"; "backlog that" and "update the changelog" trigger their skills
- [ ] Discuss git worktrees + better git/commit discipline, then likely add a harness skill for it _(next conversation)_
- [ ] Automate `/lint-docs` via a guarded, detached Stop hook running headless `claude -p` — phase 2 of doc-linting
- [ ] (Directional) Path 2 refactor: lean from-scratch plugin if upstream cruft bothers you

## In Progress



## Done

- [x] Push harness to GitHub (`44-Bulldog/hunter-harness`) + install locally; relative marketplace `source` confirmed
- [x] Disable upstream `superpowers@claude-plugins-official` on personal (verified in settings.json)
- [x] changelog / backlog / plan as auto-firing skills (planning covers plan; `/plan` command removed)
- [x] Doc-upkeep system: README folded into `changelog`, `BUILD_PLAN` seed in `planning`, `/lint-docs` autofix command
- [x] Clean orphan support files after the skill rewrites
- [x] Sharpen the `planning` / `writing-plans` boundary (Option A)

%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false]}
```
%%
