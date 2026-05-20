# Studying for the Claude Certified Architect Exam with Learning Mode

A practical guide for using the **Learning Mode** system in this repo as your primary study tool for the **Claude Certified Architect Exam**. If you have never seen Learning Mode before, skim the [README](README.md) first for the high-level overview, then come back here.

## Who this is for

- You have **Claude Code** installed.
- You are preparing for the **Claude Certified Architect Exam**.
- You want a study system that handles spaced repetition, hands-on practice, and conceptual review without leaving the editor.

## Why use Learning Mode for this exam specifically

| Exam-prep need | How Learning Mode covers it |
|---|---|
| Cover a wide topic surface | The 12-section curriculum in [architect-exam-topics.md](architect-exam-topics.md) gives you a menu, no guessing what to study next. |
| Retain what you learn | Each session writes a structured entry to `learning-log.md` with a `Next review` date, so old topics keep resurfacing automatically. |
| Practice both concept and code | Teach Mode builds runnable examples, Mental Model Mode covers conceptual-only topics. The exam tests both layers. |
| Avoid burnout | Three modes let you match effort to energy. Tired? Mental Model Mode. Focused? Teach Mode. Maintenance day? Review Mode. |
| Honest gap detection | Integrative quizzes tag each concept **solid**, **shaky**, or **weak**, and weak concepts come back for review sooner. |

## One-time setup

| Step | File | Action |
|---|---|---|
| 1 | `~/.claude/skills/learning-mode/SKILL.md` | Copy `SKILL.md` from this repo. |
| 2 | `~/.claude/learning-log.md` | Copy `learning-log.md` and delete the example entries so you start clean. |
| 3 | `~/.claude/CLAUDE.md` | Append the block from `global-claude-md-snippet.md`. |
| 4 | This repo, kept open | Use [architect-exam-topics.md](architect-exam-topics.md) as your topic menu. |

After setup, open any Claude Code project. The system is global, so it works the same in every project on your machine.

## The 4-week study plan

The full topic breakdown lives in [architect-exam-topics.md](architect-exam-topics.md). The condensed plan:

| Week | Focus | Sections from the topic checklist |
|---|---|---|
| **1** | Foundations | 1. API fundamentals, 2. Tool use |
| **2** | Performance and patterns | 3. Prompt caching, 4. Extended thinking, 5. Context management |
| **3** | Agents and integrations | 6. Agent patterns, 7. MCP, 8. Batch API |
| **4** | Production polish and review | 9. Files/citations/vision, 10. Streaming, 11. Safety, 12. Cost. Plus daily Review Mode. |

Daily Review Mode from **week 2 onwards** is the lever that makes the rest of the plan stick. Skip it and earlier topics fade.

## A typical study day

| Time block | What to do | How |
|---|---|---|
| **Start of session, 5 min** | Run Review Mode to clear today's due concepts. | "review what I've learned" |
| **Main block, 30 to 60 min** | Pick one topic from [architect-exam-topics.md](architect-exam-topics.md) and run Teach Mode. | "teach me <topic>" or `/learning-mode <topic>` |
| **End of session, 2 min** | Tick the topic off the checklist. Note what to do next. | Edit `architect-exam-topics.md` directly. |

This gives you one new concept per day, plus rolling review of everything older, which is enough to cover all 12 sections in four weeks with margin.

## Which mode for which situation

| Situation | Mode | Trigger |
|---|---|---|
| First time encountering a topic | **Teach Mode** | "teach me X" |
| Topic where running code would not help, e.g. exam policy, model selection criteria | **Mental Model Mode** | "just give me the mental model for X" |
| Daily maintenance of past topics | **Review Mode** | "review what I've learned" |
| You skimmed a topic and want to test if it stuck | **Review Mode** scoped to that concept | "quiz me on prompt caching" |
| You learned a concept but it feels shaky | Re-run **Teach Mode** with a different example | "teach me X again with a different example" |

## Reading your learning log

The `learning-log.md` is your study record. Two example entries in the file show the full schema. Fields worth knowing for exam prep:

- **Initial understanding**: `solid`, `shaky`, or `weak`. Weak entries come back for review tomorrow, solid in 3 days.
- **Next review**: when Claude will surface this concept in Review Mode by default.
- **Stretch variant**: only set if you pushed past the base example. Signals you can be quizzed at the harder depth.
- **Prerequisite caveat**: only set if you pushed through a flagged knowledge gap. Worth revisiting before the exam.
- **Next suggested concept**: the natural follow-on. Use this when you finish a session and do not know what to study next.

Open the log periodically and scan for entries tagged **weak** or with **prerequisite caveats**. Those are the topics most likely to hurt you on the exam.

## Spaced repetition cheat sheet

Review Mode picks what to quiz you on. The intervals:

| Outcome on a concept | Next review |
|---|---|
| Got most wrong, or could not explain the why | 1 day |
| Right answer but hesitated | 3 days |
| Confidently correct | Double the previous interval. Progression: 3, 7, 14, 30, 60, 90 days |

Mental-model-only entries cap at 30 days. In the final week before the exam, override the schedule and run "quiz me on all entries" daily.

## Tips for using this system well

- **Pick the example that produces visible output.** When Teach Mode offers 2 or 3 example projects, pick the one that runs and prints something. You will remember it better.
- **Accept the stretch variant when offered.** Phase 4.75 is where exam-level depth gets built in.
- **Be honest in quizzes.** If you guessed, say so. The spaced-repetition schedule is only useful if your `Initial understanding` tag reflects reality.
- **Use Mental Model Mode for the dry topics.** Things like rate-limit error categories or model pricing tiers do not need runnable code.
- **Keep `architect-exam-topics.md` open in a second tab** while you study so you always have your next topic ready.
- **In the final week**, switch to Review-Mode-only and stop introducing new topics. Consolidate.

## What this system does not do

Worth being clear about, so you can plug the gaps:

- It is not a **mock exam tool**. Real exam questions are timed and multiple-choice. After you have 20+ log entries, supplement with actual practice questions if you can get them.
- It does not **track your overall progress** as a chart or dashboard. The topic checklist file is the closest thing. Tick boxes as you go.
- It does not **know the official exam blueprint**. The topic list in this repo is a best-effort study map, not an Anthropic-published syllabus. Adjust as you learn what the exam emphasises.

## Getting started

1. Do the setup above.
2. Open [architect-exam-topics.md](architect-exam-topics.md) and pick your first topic from section 1.
3. In Claude Code, type: `teach me the messages API: request shape, roles, content blocks`.
4. After the session, check `learning-log.md` to see your first entry.
5. Tomorrow, start with `review what I've learned`, then pick your next topic.

That is the full loop. Repeat for four weeks.
