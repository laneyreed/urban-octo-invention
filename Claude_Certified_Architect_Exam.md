## Learning Mode: a guided study system for the Claude Certified Architect Exam

**Learning Mode** is a custom **Claude Code** skill that turns Claude into a structured tutor. It is built for studying concepts for their own sake, not building production code, which makes it well suited to exam prep. Every session gets logged to a global learning log, so what you study in one session is remembered the next time you sit down, in any project on your machine.

### Three study modes

|Mode|What it does|When to use it for exam prep|
|---|---|---|
|**Teach Mode**|Walks you through a new concept end to end with explanation, a small runnable example, predictive quizzes, and an integrative quiz at the end.|First-pass learning of a new exam topic, e.g. tool use, prompt caching, agent loops, context management.|
|**Review Mode**|Quizzes you on concepts already in your learning log, defaulting to topics due for review today.|Daily spaced-repetition drilling in the weeks before the exam.|
|**Mental Model Mode**|Gives a concept-only walkthrough with definition, analogy, diagram, and a code sketch, no code written.|Light review on the bus, or grasping a concept before deciding whether to go deep.|

### Quiz features

- **Predictive step quizzes** during Teach Mode that pause before key moments and ask what you think will happen.
- **End-of-session integrative quiz** with 2 to 4 questions that test the whole concept, not single lines. The result is tagged **solid**, **shaky**, or **weak**.
- **Review Mode quizzes** generate 2 to 4 questions per concept from definition, why-it-matters, pattern, and real-world-reference sections of each log entry.
- **Quizzes are opt-in** and toggleable any time with "quiz me" or "stop quizzing".

### Spaced repetition (Leitner-style)

Review Mode is not a one-off quiz tool. Each concept gets a `Next review` date that adjusts based on how you do.

|Outcome|Next review|
|---|---|
|Got most wrong or could not explain the why|1 day|
|Right answer but hesitated or needed correction|3 days|
|Confidently correct|Double previous interval, capped at 90 days. Progression: 3 to 7 to 14 to 30 to 60 to 90|

Mental-model-only entries cap at 30 days, since they were never stress-tested with code.

### Other features worth knowing

- **Level calibration**: Claude asks one targeted question up front and pitches the session at **Beginner**, **Intermediate**, or **Advanced**.
- **Prerequisite check**: surfaces up to 3 prerequisites before starting. You can pivot, embed a refresher, or push through with a logged caveat.
- **Honest code review** with severity tiers (hard-stop, should-know, style-nit) calibrated to your level. Hard-stop issues are never softened.
- **Stretch variants**: if you breeze through the base example, Claude offers one harder variant of the same concept, e.g. thread-safe lazy init after basic lazy init.
- **Persistent learning log** in `~/.claude/learning-log.md` with a structured entry per concept: definition, pattern, real-world reference, exercise built, initial understanding tag, last-reviewed and next-review dates, and a suggested next concept.

### Why it suits exam prep specifically

- **Spaced repetition is built in.** No need to maintain Anki decks; the log is the deck.
- **Conceptual and hands-on review in one tool.** Most exam topics have both a "what is this" layer and a "show me the code" layer. Teach Mode and Mental Model Mode cover both.
- **Every concept gets recorded once.** When you revisit a topic months later, Claude can say "you saw tool use last session, want to build on it with parallel tool calls?"
- **Project-agnostic.** Works the same in any Claude Code project on your machine.

### Setup (one time)

|Step|File|Action|
|---|---|---|
|1|`~/.claude/skills/learning-mode/SKILL.md`|Copy the skill definition.|
|2|`~/.claude/learning-log.md`|Create the log with a `# Learning Log` heading.|
|3|`~/.claude/CLAUDE.md`|Add a `## Learning Log` section pointing to the log.|

### Trigger phrases

- **Teach**: "teach me X", "I want to learn X", "explain X with an example", or `/learning-mode X`
- **Review**: "quiz me on what I've learned", "review mode", "test me on past concepts"
- **Mental Model**: "just give me the mental model for X", "explain X conceptually", "mental model only"

Full system design, including all phase flows and the log entry schema, is in **HOW-IT-WORKS.md** in the repo