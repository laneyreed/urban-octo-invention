# Snippet to add to global CLAUDE.md

This is the block to append to your global Claude Code instructions file. The file path depends on your OS.

| OS | Path to global `CLAUDE.md` |
|---|---|
| Windows | `C:\Users\<username>\.claude\CLAUDE.md` |
| macOS / Linux | `~/.claude/CLAUDE.md` |

It tells Claude, in every project on this machine, where the learning log lives and what to do with it.

## What to copy

Paste the block below at the bottom of your global `CLAUDE.md`. Replace `<username>` with your actual Windows username, or swap the whole path for `~/.claude/learning-log.md` if you are on macOS or Linux.

```markdown
## Learning Log
- A running log of `/learning-mode` sessions across all projects is at `C:\Users\<username>\.claude\learning-log.md`.
- At the start of any `/learning-mode` session, read it to see what has already been covered so you can build on prior sessions instead of repeating them.
- After completing a learning-mode example, append a new dated entry summarizing the concept, the pattern, the hands-on exercise built, and a suggested next concept.
```

## Why each line matters

| Line | Purpose |
|---|---|
| **Pointer to the log file** | Without an explicit path, Claude has no way to find the log. The path makes it machine-readable and unambiguous. |
| **Read at session start** | Forces Claude to load prior context *before* proposing examples, so it can avoid repeating concepts and can suggest natural follow-ons. |
| **Append after each session** | Closes the loop, every session enriches the log so future sessions get smarter. |

## How to verify it is working

1. Open Claude Code in any project.
2. Run `/learning-mode` followed by a concept name.
3. Claude should read `learning-log.md` *before* proposing examples. If you have prior entries, Claude should reference them. If the log is empty, Claude should proceed normally.
4. After the session ends, open `C:\Users\<username>\.claude\learning-log.md` (or `~/.claude/learning-log.md`) and confirm a new dated entry was appended.

## If you move machines

Recreate the file at the equivalent path on the new machine and adjust the path in the snippet to match. On macOS or Linux this would typically be `~/.claude/learning-log.md`.
