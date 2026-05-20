# Learning Log

A running record of concepts covered in `/learning-mode` sessions across all projects.
Each entry is dated and tagged with the project it came from.

> **Note**: This file is a starter template. The entry below is an **example** showing the expected format. Delete or replace it once you have completed your first learning-mode session.

---

## YYYY-MM-DD, Example Concept (example-project)

**Session type**: teach
**Concept**: One-line definition of the concept covered in the session.

**Why it matters**:
- Practical motivation, the bug it solves, or the property it gives you.
- Add as many bullets as needed.

**The pattern**:
```language
Minimal code shape worth memorizing.
```

**Real-world reference**: Where this pattern shows up in actual code you have written or read.

**Hands-on exercise built**: The mini-project or files you built during the session. Omit this line for mental-model-only sessions.

**Stretch variant**: Optional. Only include this line if Phase 4.75 ran during the session. One-line description of the harder variant covered, e.g. "Added thread safety with `threading.Lock`".

**Initial understanding**: solid | shaky | weak

**Prerequisite caveat**: Optional. Only include if the user pushed through a flagged prerequisite gap. Example: "User proceeded without solid grasp of closures. Consider teaching this first if Review Mode flags weakness."

**Last reviewed**: YYYY-MM-DD

**Next review**: YYYY-MM-DD

**Next suggested concept**: A natural follow-on topic for the next learning session.

---

## 2099-01-05, Prompt caching (claude-architect-prep)  *(example entry, fake date)*

**Session type**: teach
**Concept**: A request-level mechanism that lets the Claude API skip re-processing of stable prefix content (system prompts, tools, long context) by reusing a cached version, charged at a discounted rate.

**Why it matters**:
- Cuts input cost on cached tokens by up to 90 percent and reduces latency on long prompts.
- Critical for agent loops where the same system prompt and tool definitions repeat every turn.
- Cache placement is a design decision, not an afterthought. Bad placement gives you cache misses on every turn.

**The pattern**:
```python
client.messages.create(
    model="claude-sonnet-4-6",
    system=[
        {"type": "text", "text": LARGE_STABLE_PROMPT, "cache_control": {"type": "ephemeral"}},
    ],
    messages=[{"role": "user", "content": user_turn}],
)
```

**Real-world reference**: Used in the `claude-code-learning-mode` skill's `SKILL.md` itself, which is large and stable. Any agent built on top of it benefits from caching the skill body.

**Hands-on exercise built**: A minimal Python script that sends two identical requests with a 3000-token system prompt and a `cache_control` breakpoint, then prints `cache_creation_input_tokens` from the first response and `cache_read_input_tokens` from the second to verify the cache hit.

**Stretch variant**: Added a third request after a deliberate 6-minute sleep to confirm the 5-minute ephemeral TTL had expired and the cache was rebuilt.

**Initial understanding**: solid

**Last reviewed**: 2099-01-05

**Next review**: 2099-01-08

**Next suggested concept**: 1-hour cache TTL and when its cost premium pays off vs the default 5-minute ephemeral cache.

---

## 2099-01-01, Tool use loop (claude-architect-prep)  *(example entry, fake date)*

**Session type**: teach
**Concept**: The request-response cycle where Claude emits a `tool_use` block, the client executes the tool, sends the result back as a `tool_result` block, and Claude continues until it produces a final assistant message with no further tool calls.

**Why it matters**:
- This is the foundation of every agent. Misunderstanding the loop leads to dropped tool results, infinite loops, or premature termination.
- The model decides when to stop calling tools, the client decides when to stop the loop. Both decisions must agree.

**The pattern**:
```python
while True:
    response = client.messages.create(model=model, tools=tools, messages=messages)
    messages.append({"role": "assistant", "content": response.content})
    if response.stop_reason != "tool_use":
        break
    tool_results = [run_tool(block) for block in response.content if block.type == "tool_use"]
    messages.append({"role": "user", "content": tool_results})
```

**Real-world reference**: The same shape powers Claude Code's internal tool execution and any Agent SDK loop.

**Hands-on exercise built**: A calculator agent with `add`, `multiply`, and `divide` tools. Tested it against "what is (3 + 4) times 5" to confirm sequential tool calls, then against "compute 12 / 0" to see how the model handles a tool error returned as `tool_result`.

**Initial understanding**: shaky

**Prerequisite caveat**: User proceeded without solid grasp of JSON Schema. Consider teaching this first if Review Mode flags weakness on tool parameter design.

**Last reviewed**: 2099-01-01

**Next review**: 2099-01-03

**Next suggested concept**: Parallel tool calls and how to execute them concurrently on the client side.
