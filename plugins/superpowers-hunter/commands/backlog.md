---
description: Capture item(s) into BACKLOG.md at the repo root (Obsidian Kanban), grooming as you go
argument-hint: [what to backlog]
allowed-tools: Read, Edit, Write, Bash(ls:*), Bash(test:*)
---

Capture into the backlog: $ARGUMENTS

If no argument is given, infer the item(s) from the current conversation — what we just decided to defer.

Write to `BACKLOG.md` at the root of the current repo, in Obsidian Kanban format. If it doesn't exist, create it with `kanban-plugin: board` frontmatter, lanes **Backlog / In Progress / Done**, and the `%% kanban:settings` footer.

Add new item(s) as `- [ ]` cards under **Backlog**. While you're there, groom the board:
- merge near-duplicate cards into one
- drop exact duplicates
- flag (don't silently resolve) any card that contradicts another card or the current plan

Keep cards to one concise line each. Don't append blindly — leave the board deduplicated and coherent.
