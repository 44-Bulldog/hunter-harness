---
name: backlog
description: Use the moment something is deferred or parked rather than built now — the user says "backlog that", "park it", "not now", or you jointly decide an idea is out of scope for the current slice. Captures it into BACKLOG.md at the repo root so it isn't lost.
---

# Backlog

Fire this whenever a task or idea is deferred instead of built — don't wait to be asked with a slash command. If the user says "backlog that" / "park it" / "later", or scoping a plan down leaves spillover, capture it immediately.

## What to do

1. Locate the board first, don't blindly create one. Resolve the repo root with `git rev-parse --show-toplevel`; the board is `<root>/BACKLOG.md`, in Obsidian Kanban format. Append/groom into it if it exists there; create it from the skeleton below **only** if it's absent at the root — never spawn a second board from a subdirectory.
2. Add the item(s) as `- [ ]` cards under **Backlog**, one concise line each. Several things deferred in one breath = several cards.
3. Groom while you're there — don't just append:
   - merge near-duplicate cards into one
   - drop exact duplicates
   - flag (don't silently resolve) any card that contradicts another card or the current plan

Capture the essence, not a paragraph.

## Fresh-board skeleton

The Kanban footer below uses a triple-backtick block; the whole skeleton is wrapped in a four-backtick fence so it shows literally. Write the file with normal triple backticks.

````markdown
---

kanban-plugin: board

---

## Backlog

- [ ] First item

## In Progress



## Done



%% kanban:settings
```
{"kanban-plugin":"board","list-collapse":[false,false,false]}
```
%%
````
