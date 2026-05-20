# Learning Mode

A guided, hands-on coding learning system for **Claude Code**. Pairs a custom skill with a global learning log so every concept you study is captured once and remembered across every future session, in every project.

## Three modes

| Mode | Purpose | How to trigger |
|---|---|---|
| **Teach Mode** | Learn a new concept with explanation, a runnable example, and an end-of-session integrative quiz. | "teach me X", "explain X with an example" |
| **Review Mode** | Quiz yourself on concepts from your learning log. Spaced-repetition aware. | "quiz me on what I've learned", "review mode" |
| **Mental Model Mode** | Concept-only walkthrough with analogy and diagram, no code written. | "just give me the mental model for X", "mental model only" |

## Quick start

1. Copy `SKILL.md` to `C:\Users\<username>\.claude\skills\learning-mode\SKILL.md` (or `~/.claude/skills/learning-mode/SKILL.md` on macOS / Linux).
2. Copy `learning-log.md` to `C:\Users\<username>\.claude\learning-log.md` (or `~/.claude/learning-log.md`) and clear the example entry.
3. Append the snippet from `global-claude-md-snippet.md` to your global `CLAUDE.md`.
4. Open any Claude Code project and try one of the trigger phrases above.

## Files in this repo

| File | Purpose |
|---|---|
| `SKILL.md` | The skill definition. Copy to your `.claude/skills/learning-mode/` directory. |
| `learning-log.md` | Starter log template with one example entry. Copy to your `.claude/` directory. |
| `global-claude-md-snippet.md` | The block to paste into your global `CLAUDE.md`. |
| `HOW-IT-WORKS.md` | Full system design: all three mode flows, log entry schema, spaced-repetition schedule, and the file-relationship diagram. |

## Learn more

For the full system design (mode flows, schedule logic, log schema, setup details), see **[HOW-IT-WORKS.md](HOW-IT-WORKS.md)**.
