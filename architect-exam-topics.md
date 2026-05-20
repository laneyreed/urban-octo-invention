# Claude Certified Architect Exam: Suggested Topic Checklist

A starting curriculum for using **Learning Mode** to prepare for the **Claude Certified Architect Exam**. Topics are grouped by area and ordered roughly from foundations to advanced. Tick each one off as you complete a Teach Mode session on it.

> **Note**: This list is a suggested study map, not the official exam blueprint. Adjust it as you learn more about what the exam emphasises. Use it as a menu to pick from, not a sequence to march through.

## How to use this file

1. Pick a topic from any section below.
2. In Claude Code, run `/learning-mode <topic name>` or say "teach me <topic name>".
3. After the session, Claude appends a new entry to `learning-log.md`.
4. Come back and tick the box.
5. Once you have 5 or more entries, start running **Review Mode** ("quiz me on what I've learned") daily.

---

## 1. Claude API fundamentals

- [ ] Messages API: request shape, roles, content blocks
- [ ] Model selection: Opus vs Sonnet vs Haiku, when to pick each
- [ ] System prompts: structure, placement, common pitfalls
- [ ] Core parameters: `max_tokens`, `temperature`, `top_p`, `stop_sequences`
- [ ] Token counting and pricing basics

## 2. Tool use

- [ ] Tool definitions: JSON Schema, descriptions, parameter design
- [ ] Single tool call loop: request, `tool_use`, `tool_result`, follow-up
- [ ] Parallel tool calls: when Claude emits multiple, how to handle them
- [ ] `tool_choice`: auto, any, specific tool, none
- [ ] Multi-turn agent loops with tools
- [ ] Common footguns: hallucinated tools, malformed args, infinite loops

## 3. Prompt caching

- [ ] Cache breakpoints: where to place them, why placement matters
- [ ] 5-minute vs 1-hour TTL: cost and use-case tradeoffs
- [ ] What is cacheable: system, tools, messages
- [ ] Reading cache metrics: `cache_creation_input_tokens`, `cache_read_input_tokens`
- [ ] Cost impact of cache hits vs misses
- [ ] Designing prompts for cache stability

## 4. Extended thinking

- [ ] Thinking budget: how to size it
- [ ] When extended thinking helps vs when it does not
- [ ] Interleaving thinking with tool use
- [ ] Cost and latency tradeoffs

## 5. Context management

- [ ] Context windows by model
- [ ] Token budgeting across system, tools, messages, output
- [ ] Compaction patterns: summarising old turns
- [ ] Memory tools vs in-context memory
- [ ] When to start a new conversation vs continue

## 6. Agent patterns and the Agent SDK

- [ ] The agent loop: model, tools, environment, stopping conditions
- [ ] Subagents: when to delegate, cost of cold starts
- [ ] Handoffs between agents
- [ ] State and persistence across turns
- [ ] Background tasks and async patterns
- [ ] Claude Agent SDK overview

## 7. Model Context Protocol (MCP)

- [ ] MCP server anatomy: tools, resources, prompts
- [ ] Connecting an MCP server to Claude
- [ ] Writing a minimal MCP server
- [ ] Security considerations: untrusted servers, scopes
- [ ] When to use MCP vs native tool definitions

## 8. Batch API

- [ ] Request format and submission flow
- [ ] When batch wins over real-time, cost and latency
- [ ] Polling and result retrieval
- [ ] Error handling per-request in a batch

## 9. Files, citations, and vision

- [ ] File uploads: supported types, size limits
- [ ] Citations: enabling, parsing, presenting to users
- [ ] Vision inputs: image formats, multi-image prompts
- [ ] PDFs: when to use file API vs convert to images

## 10. Streaming

- [ ] SSE event types: `message_start`, `content_block_delta`, etc.
- [ ] Streaming with tool use
- [ ] Handling partial JSON in streaming tool args
- [ ] UX patterns for streamed responses

## 11. Safety, evaluation, and production

- [ ] Prompt injection: detection, mitigation
- [ ] Building evals: golden sets, judge prompts, regression detection
- [ ] Guardrails: input filtering, output checking
- [ ] Rate limits, retries, exponential backoff
- [ ] Error categories: `overloaded_error`, `rate_limit_error`, etc.
- [ ] Logging and observability for LLM apps

## 12. Cost optimisation playbook

- [ ] Model tiering: route easy work to Haiku
- [ ] Prompt caching wins
- [ ] Batch API wins
- [ ] Reducing output tokens with structured outputs

---

## Suggested study sequence

If you are starting from zero and have around four weeks before the exam, a workable order:

| Week | Focus | Sections |
|---|---|---|
| **1** | Foundations | 1, 2 |
| **2** | Performance and patterns | 3, 4, 5 |
| **3** | Agents and integrations | 6, 7, 8 |
| **4** | Production polish and review | 9, 10, 11, 12, plus daily Review Mode |

Daily Review Mode from week 2 onwards keeps earlier topics fresh while you cover new ones.
