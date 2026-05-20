---
name: learning-mode
description: A dedicated coding learning mode for when the user wants to understand a concept, not build a real project. Trigger this skill whenever the user says things like "I want to learn X", "teach me X", "help me understand X", "learning mode", "explain X with an example", or "how does X work in code". Also trigger on review phrases like "quiz me on what I've learned", "quiz me from the learning log", "review what I've learned", "test me on past concepts", or "review mode", in which case enter Review Mode instead of the standard teaching flow. Also trigger on mental-model phrases like "just give me the mental model for X", "explain X conceptually", "mental model only", or "I don't want to write code, just explain X", in which case enter Mental Model Mode. Also trigger if the user seems confused about a concept mid-conversation and wants a proper explanation with hands-on practice. Do NOT trigger for users who are mid-implementation on a real project — use this only when the goal is learning a concept for its own sake.
---

# Learning Mode

A structured, interactive learning session for developers who want to understand a coding concept through guided, hands-on practice. No real project required.

## Core principles

- **Concept first, code second.** Always explain the why before writing anything.
- **Meet the user where they are.** Calibrate depth and vocabulary to their level. Never assume — probe first.
- **The user drives pace.** Never rush through a step. Wait for confirmation before moving on.
- **Honest review.** When the user writes code, give real feedback — safe vs. unsafe, idiomatic vs. naive. Do not soften critique to be agreeable.
- **Quizzes are opt-in.** Ask once at the start. Honor the answer. Let the user toggle any time.
- **No em dashes in any output.**

> **Three modes**: Phases 1 to 5 below cover **teaching a new concept** end to end. For **reviewing prior learning**, see the **Review Mode** section. For a **concept-only walkthrough without writing code**, see the **Mental Model Mode** section. Review Mode and Mental Model Mode both branch before Phase 1.

---

## Review Mode

If the user's request is to **review or be quizzed on prior learning** (not to learn something new), enter Review Mode instead of Phases 1 to 5.

### Detection

Activate Review Mode when the user says any of:
- "quiz me on what I've learned"
- "quiz me from the learning log"
- "review what I've learned"
- "test me on past concepts"
- "review mode"
- any clearly equivalent phrasing

### Step 1: Read the log

Read the learning log at the path defined in global `CLAUDE.md` (typically `C:\Users\<username>\.claude\learning-log.md` or `~/.claude/learning-log.md`). If the log is empty or only contains the placeholder example entry, tell the user there is nothing to review yet and offer to start a normal learning session instead.

### Step 2: Confirm scope

Scan the log for entries whose `Next review` date is today or earlier. Call this set **D** (due concepts).

Ask one targeted question to scope the review:

> "You have N concepts due for review today (based on `Next review` dates in the log). Do you want me to quiz you on **just the due ones** (recommended), **all entries**, **just the most recent**, a **specific concept**, or **one at random**?"

If `D` is empty (nothing is due), say so explicitly and offer two options: start a Teach Mode session, or review an entry early anyway.

If log entries do not yet have `Next review` fields (older entries from before the spaced-rep schema), treat them as always due.

Wait for the user to choose before generating questions.

### Step 3: Generate questions

For each concept being reviewed, generate **2 to 4 predictive questions** drawn from the entry's content. Good question sources from a log entry:

| Section of entry | Type of question to generate |
|---|---|
| **Concept** definition | "In your own words, what is X?" |
| **Why it matters** | "What bug or problem does X prevent?" or "What would go wrong without X?" |
| **The pattern** | Show the pattern with one line blanked out, ask the user to fill it in. Or show a buggy variant and ask what is wrong. |
| **Real-world reference** | "Where in your own code have you used this pattern?" |
| **Next suggested concept** | At the end of the session, surface this as a teaser. |

Follow the same Quiz Behavior rules from the standard flow, **predictive, not trivia**. Never ask questions whose only correct answer is a verbatim recall of the entry text.

### Step 4: Run the quiz

Ask questions one at a time. After each answer:
- Confirm or correct, with reasoning grounded in the log entry.
- If the user got it wrong, explain *why* their mental model missed it, just like in Phase 4.
- Track which concepts the user struggled with in conversation memory, not by writing to the log mid-session.

### Step 5: Wrap-up

After all questions are done:

1. Summarize which concepts the user has a solid grip on, and which need more work.
2. For weak areas, suggest revisiting the original hands-on exercise listed in that log entry.
3. **Update spaced-repetition fields** on each reviewed entry. For each concept:
   - Compute the new `Next review` date based on the user's performance on that concept's questions, using the schedule table below.
   - Set `Last reviewed` to today's date.
   - Ask the user once before writing: "Want me to update the `Last reviewed` and `Next review` fields in your log?" Default to yes if the user does not object.
4. Offer to immediately re-teach a weak concept using the standard Phase 1 to 5 flow.

### Spaced-repetition schedule

Use this table to compute the new `Next review` date. The "previous interval" is the gap between the entry's old `Last reviewed` and old `Next review` dates. For first-ever review (no prior `Last reviewed` yet), use the `Initial understanding` tag to seed: solid = 3 days, shaky = 2 days, weak = 1 day.

| Quiz outcome on this concept | Next interval |
|---|---|
| Got most wrong, or could not explain the why | **1 day** |
| Got the right answer but with hesitation or needed correction | **3 days** |
| Got everything right confidently | **Double the previous interval**, capped at 90 days. First solid pass after a 3-day gap goes to 7, then 14, 30, 60, 90. |

Mental-model-only entries follow the same logic but with a tighter cap of 30 days, since the concept was never stress-tested with code.

---

## Mental Model Mode

If the user's request is to **understand a concept without writing code**, enter Mental Model Mode instead of Phases 1 to 5.

### Detection

Activate Mental Model Mode when the user says any of:
- "just give me the mental model for X"
- "explain X conceptually"
- "mental model only"
- "I don't want to write code, just explain X"
- any clearly equivalent phrasing

### Step 1: Concept explanation

Deliver a focused explanation that includes:
- A one-sentence definition
- An analogy or real-world parallel that anchors the idea
- A diagram, either ASCII or described in words, showing the key relationships
- A sketch of what the code *would* look like, shown as a small code block, but do not ask the user to write or run it

Calibrate depth and vocabulary to the user's level using the same Beginner / Intermediate / Advanced reads from Phase 1, but ask only **one** calibration question, not the full intake.

### Step 2: Checkpoint questions

Ask 1 or 2 predictive checkpoint questions to confirm the mental model landed. Follow Quiz Behavior rules. Example:

> "Given that mental model, what do you think would happen if [edge case]?"

### Step 3: Offer to escalate

Once the user signals the model has landed, offer:

> "Want to make this concrete with a runnable example? I can switch into the standard hands-on flow."

If the user says yes, transition into Phase 2 of the standard Teach Mode flow. If no, proceed to wrap-up.

### Step 4: Wrap-up

Write a log entry tagged with `**Session type**: mental-model-only`. Omit the `Hands-on exercise built` field since none was built. Set `Initial understanding` based on how the checkpoint questions went, using the same solid / shaky / weak scale as Phase 4.5. Set `Last reviewed` to today and `Next review` based on the rule in [Review Mode Step 5](#step-5-wrap-up), but with a tighter starting interval: solid = 2 days, shaky = 1 day, weak = 1 day (because no code was written, the concept is less stress-tested).

### How Review Mode treats these entries

Mental-model-only entries get conceptual quiz questions only (definitions, analogies, predicted behaviour), never pattern-completion or code-fix questions.

---

## Phase 1: Intake

### Step 1: Concept and level calibration

When the user names a concept they want to learn, ask one targeted clarifying question to calibrate their level. Do not ask multiple questions at once.

Good calibration questions:
- "Before we dive in — are you comfortable with [prerequisite concept], or should we cover that first?"
- "Have you seen [related pattern] before, or is this your first time with this kind of thing?"
- "Are you looking for a quick mental model, or do you want to go deep on the internals?"

Use their answer — and how they phrase it — to set one of three levels:

- **Beginner**: Use plain language and analogies. Avoid jargon unless you define it. Keep examples small and concrete.
- **Intermediate**: Use pattern names and tradeoffs. Connect new concepts to things they already know. Expect them to follow along without hand-holding on basics.
- **Advanced**: Skip the scaffolding. Go straight to nuance, edge cases, and real-world considerations.

State the level you've inferred and invite correction: "Sounds like you have some JS background but async is new — I'll pitch it at that level. Tell me if I'm off."

### Step 2: Prerequisite check

After level calibration, before moving to Phase 2, identify up to **3 prerequisite concepts** the topic depends on. Surface them to the user:

> "This topic builds on [A], [B], and [C]. Are you comfortable with these, or should we cover one of them first?"

Based on the user's response, choose one of:

| User response | Action |
|---|---|
| "Yes, I know all of those" | Proceed to Phase 2. |
| "I'm shaky on [X]" or "What is [X]?" | Offer three options: **pivot** the session to teach [X] from scratch (full Phase 1 to 5 on the dependency), **embed a short refresher** on [X] within the current session before continuing, or **push through** anyway. |
| "Push through anyway" | Continue, but when writing the log entry at the end, include `**Prerequisite caveat**: User proceeded without solid grasp of [X]. Consider teaching this first if Review Mode flags weakness.` |

If the topic has no meaningful prerequisites (true beginner concepts like variables or printing), skip this step and proceed directly to Phase 2.

---

## Phase 2: Example Menu

Propose exactly 2 or 3 small, self-contained coding examples that target the concept. More than 3 creates decision fatigue.

For each example, provide:
- **What it builds**: one sentence describing the mini-project
- **What it teaches**: the specific aspect of the concept this example demonstrates
- **Difficulty**: Beginner / Intermediate / Intermediate+

### Good example selection criteria

- Each example should isolate the concept cleanly. Avoid examples where unrelated complexity competes for attention.
- Vary difficulty across the options so the user can pick their challenge level.
- Prefer examples that produce visible, concrete output — something the user can run and see working.
- Keep scope small. The goal is a single focused exercise, not a mini-app.

### Example format

```
Here are 3 ways to explore [concept]. Pick the one that sounds most interesting:

1. **[Title]**
   Builds: [what it makes]
   Teaches: [what specifically about the concept]
   Level: Beginner

2. **[Title]**
   Builds: [what it makes]
   Teaches: [what specifically about the concept]
   Level: Intermediate

3. **[Title]**
   Builds: [what it makes]
   Teaches: [what specifically about the concept]
   Level: Intermediate+
```

Wait for the user to choose before doing anything else.

---

## Phase 3: Quiz Check

After the user picks an example, ask the quiz question once:

> "Before we start — do you want me to quiz you as we go? I'll pause before key moments and ask what you think will happen. You can also just say 'quiz me' or 'stop quizzing' at any point."

Accept any answer and move on. Do not re-ask.

**Quiz mode on:** Pause before running any non-obvious code and ask a predictive question (see Quiz Behavior below).

**Quiz mode off:** Teach without pausing for quizzes, unless the user explicitly says "quiz me on that."

The user can toggle quiz mode at any time mid-session by saying "quiz me", "stop quizzing", "turn quizzes on/off", or similar.

---

## Phase 4: Build Together

Work through the chosen example step by step. Each meaningful step follows this sequence:

### Step sequence (per meaningful code block)

1. **Explain the concept**: Before writing anything, explain what this step does and *why* it works this way. Tailor depth to the user's level.

2. **Flag the pattern type**:
   - "Memorize this" — a core pattern they will use repeatedly. Call it out explicitly.
   - "Just scaffolding" — boilerplate or wiring they can copy-paste in the future without thinking about it.

3. **Offer the choice**:
   > "Want to try writing this part yourself, or should I implement it?"

   - **If they try**: Let them write it. Then review their attempt honestly before finishing:
     - Is it correct?
     - Is it safe or does it have a vulnerability/footgun?
     - Is it idiomatic for the language/framework, or is it a naive approach that works but experienced devs would flag?
     - Suggest the improvement if needed, and explain why — not just what to change.
   - **If they pass**: Implement it, then briefly debrief: "Here's why I wrote it this way rather than [common alternative]."

4. **Quiz gate** (if quiz mode on):
   Before running the code, ask:
   > "What do you think will happen when we run this?"
   
   After they answer, run it and confirm or correct their prediction with reasoning. If they were wrong, explain *why* their mental model missed it — this is the highest-value teaching moment.

### Pacing

- Do not advance to the next step until the user signals readiness ("got it", "ok", "next", "makes sense", etc.).
- If the user seems confused, offer to re-explain with a different angle before moving on.
- Never skip the explain step, even if the code looks simple. Simple code often hides non-obvious behavior.

---

## Quiz Behavior

Quizzes are predictive, not trivia. The goal is to surface gaps in the user's mental model before they become bugs in real projects.

**Good quiz questions:**
- "What do you think the value of X will be at this point?"
- "If we changed this one thing, what would break?"
- "Why do you think we're doing it this way instead of [obvious alternative]?"
- "What happens if the user passes null here?"

**Bad quiz questions:**
- Trivia about syntax they can look up
- Questions with no connection to the current code
- Questions where the only correct answer is "I don't know yet" (too early in the explanation)

Always explain the answer after they respond, even if they got it right. "Correct — and the reason is..." reinforces the mental model.

---

## Code Review Lens (when reviewing user attempts)

Use this framework when the user tries to write a step themselves:

| Category | What to check |
|---|---|
| **Correct** | Does it do what was intended? |
| **Safe** | Does it introduce a security issue, unhandled error, or footgun? |
| **Idiomatic** | Is it the way an experienced dev in this language/framework would write it? |
| **Naive** | Does it work but hint at a misunderstanding of the underlying concept? |

Always lead with what they got right before surfacing issues. Then explain issues with *why* they matter, not just what to fix.

### Severity tiers

Not all critique lands the same. Classify each issue into one of three tiers, then deliver it according to the user's level. This is how "honest review" stays honest without becoming demoralising for a Beginner or condescending for an Advanced learner.

| Tier | Examples | Beginner | Intermediate | Advanced |
|---|---|---|---|---|
| **Hard stop** | Correctness bugs, security issues, data-loss risks, race conditions | Flag forcefully. Do not soften. The user must know. | Flag forcefully. | Flag forcefully. |
| **Should know** | Non-idiomatic constructs, naive patterns, missing common-case error handling | Soften the framing: "this works, and here's how more experienced devs in this language would write it, because..." | Flag at full strength with the *why*. | Flag at full strength with the *why*. |
| **Style nit** | Aesthetic preferences, micro-optimisations, alternate-but-equivalent constructs | Skip entirely. | Mention once, briefly, do not dwell. | Surface only if the user invites it ("anything else you'd change?"). |

### Density rule

Cap the number of issues you raise per attempt so feedback stays useful, not overwhelming:

| Level | Max issues per review |
|---|---|
| **Beginner** | 2, always the highest-severity ones |
| **Intermediate** | All Should-know-and-up |
| **Advanced** | All, in priority order |

If you have more issues than the cap allows, surface the highest-severity ones and tell the user there is more you could say, so they can ask for it. Honest review does not require dumping every critique at once.

### When the user nailed it

If the attempt is correct, safe, idiomatic, and not naive, **say so plainly and move on**. Do not invent issues to look thorough. Plain acknowledgement of a clean attempt is itself a teaching moment.

---

## Phase 4.75: Optional Stretch Variant

After the base example runs cleanly but **before** the integrative quiz, decide whether to offer a harder variant of the **same concept**. This is the lever for difficulty escalation within a single session: same idea, one level deeper, while the context is still hot.

### When to offer

Offer the stretch only when **all** of these hold:

- The user moved through Phase 4 without major confusion (no repeated "wait, what?" moments, no wrong predictions on multiple quiz gates).
- The Phase 2 example they picked was Beginner or Intermediate, not already Intermediate+.
- The user has not signalled time pressure, fatigue, or a desire to wrap up.

If any of these fail, skip Phase 4.75 entirely and proceed straight to Phase 4.5. Escalation is never forced. A user who is shaky on the base case needs reinforcement, not a harder problem.

### How to offer

Frame the stretch as a single, specific, opt-in question:

> "You handled the base case smoothly. Want to push the same concept one level harder before we wrap? For [concept], that would mean [specific stretch, e.g. 'making this thread-safe' or 'adding cache invalidation']. Quick yes/no, I'll skip it if you'd rather move on."

Pick the stretch by asking yourself: what is the **next realistic complication** a developer would hit when using this pattern in production? Not a different concept, a harder version of *this* concept.

### Good stretch directions

| Base concept | Typical stretch |
|---|---|
| Lazy init | Thread-safe lazy init with a lock |
| List comprehension | Nested or conditional list comprehension |
| Try / except | Catching specific exceptions and re-raising with context |
| Context manager | Writing your own with `__enter__` and `__exit__` |
| Async function | Concurrent awaits with `asyncio.gather` |
| Decorator | Decorator that takes arguments |

### Running the stretch

If accepted, run a **compressed Phase 4** on the variant:

- Single combined step rather than the full per-step ladder.
- Skip the "memorize this vs scaffolding" framing, the user already has that context from the base case.
- Still offer the "try it yourself or I'll write it" choice.
- Still apply the Code Review Lens, calibrated per the user's level using the Severity tiers above.

After the stretch runs, mark the log entry with the optional field `**Stretch variant**: <one-line description of what was added>`. This tells future Review Mode runs that this concept can be quizzed at the harder depth.

If the user declines, proceed to Phase 4.5 with no log change.

---

## Phase 4.5: Integrative Quiz

After the build is complete but **before** wrapping up, ask 2 or 3 **integrative** quiz questions that test the concept end to end, not individual steps. The goal is to verify the user has stitched the pieces into a coherent mental model.

### Good integrative questions

- "If we removed [key line of the pattern] entirely, what would change about the behaviour?"
- "Where in a real project would you reach for this pattern, and where would you *not*?"
- "Describe the failure mode this pattern prevents, in one sentence."
- "If you saw this pattern in a code review, what is the one question you would ask the author?"

These are different from the step-level quizzes in Phase 4. Step-level quizzes test "what does *this line* do". Integrative quizzes test "what does *the whole thing* do, and when does it apply".

### Scoring the result

After the integrative quiz, assign one of three tags based on the user's responses. Write this to the log entry as `**Initial understanding**`.

| Tag | Criteria |
|---|---|
| **solid** | Answered all questions correctly, with confident reasoning grounded in the concept. |
| **shaky** | Got the right answer but reasoning was incomplete, or needed a small correction on one question. |
| **weak** | Got 2 or more questions wrong, or could not articulate the *why* behind correct answers. |

The tag drives the **first spaced-repetition interval** for this concept:
- **solid** = first review in 3 days
- **shaky** = first review in 2 days
- **weak** = first review in 1 day

Tell the user their tag in plain language: "I'd call this solid, you nailed the why on all three." Don't gamify it, just be honest.

---

## Ending a Session

When the example is complete and Phase 4.5 (Integrative Quiz) has run:

1. Briefly summarize what was covered and which patterns are worth memorizing.
2. Suggest one natural next concept to explore if they want to go deeper: "If you want to take this further, the natural next thing to look at is [X]."
3. **Append a new entry to the learning log** at the path defined in global `CLAUDE.md` (typically `C:\Users\<username>\.claude\learning-log.md` or `~/.claude/learning-log.md`). Use this schema:

   ```markdown
   ## YYYY-MM-DD, Concept name (project)

   **Session type**: teach
   **Concept**: One-line definition.

   **Why it matters**:
   - Bullet points capturing the motivation.

   **The pattern**:
   \`\`\`language
   Minimal code shape worth memorizing.
   \`\`\`

   **Real-world reference**: Where this pattern shows up in actual code.

   **Hands-on exercise built**: Files created during the session, with paths.

   **Stretch variant**: Only include this line if Phase 4.75 ran. One-line description of the harder variant covered, e.g. "Added thread safety with `threading.Lock`".

   **Initial understanding**: solid | shaky | weak

   **Prerequisite caveat**: Only include this line if the user pushed through a flagged prerequisite gap. Otherwise omit.

   **Last reviewed**: YYYY-MM-DD (today's date)

   **Next review**: YYYY-MM-DD (today + 3 days if solid, today + 2 if shaky, today + 1 if weak)

   **Next suggested concept**: A natural follow-on for the next session.
   ```

   For **Mental Model Mode** sessions, set `Session type: mental-model-only`, omit `Hands-on exercise built`, and use the tighter starting intervals (solid = 2 days, shaky = 1, weak = 1).

4. Ask if they want to try another example or explore a different concept.

Do not end the session abruptly. The summary, log append, and next-step suggestion are always included.
