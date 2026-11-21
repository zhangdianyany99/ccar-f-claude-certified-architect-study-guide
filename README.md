# CCAR-F (Claude Certified Architect — Foundations) Study Guide

An open-source, domain-by-domain study guide, weight breakdown, and exam readiness checklist for the **Anthropic Claude Certified Architect — Foundations (CCAR-F)** certification (Dec 2026 blueprint).

## Exam Overview & Official 5-Domain Weighting

| Parameter | Details |
| :--- | :--- |
| **Exam Code** | CCAR-F (also referenced as CCA-F in early community notes) |
| **Exam Fee** | $125 USD |
| **Passing Score** | 720 / 1,000 |
| **Format** | 60 scenario-based multiple-choice questions (120 minutes) |
| **Prerequisites** | Hands-on experience building agentic workflows, Claude Code, MCP servers, and structured outputs |

| Domain | Official Weight | Core Focus Areas |
| :--- | :---: | :--- |
| **1. Agentic Architecture & Orchestration** | **27%** | Multi-agent patterns, subagent context isolation, stop conditions, HITL gates |
| **2. Claude Code Configuration & Workflows** | **20%** | `.claude/CLAUDE.md` hierarchy, slash commands, headless CI (`claude -p`), hooks |
| **3. Prompt Engineering & Structured Output** | **20%** | `tool_use` / JSON Schema enforcement, few-shot calibration, extended thinking, XML isolation |
| **4. Tool Design & MCP Integration** | **18%** | MCP Tools vs. Resources vs. Prompts, `.mcp.json`, `stdio` / Streamable HTTP, `isError` recovery |
| **5. Context Management & Reliability** | **15%** | Prompt caching prefixes, compaction/summarization, eval suites, `429`/`529` backoff |

> **Interactive Timed Mock & Sample Preview**: Try 3 free no-login CCAR-F sample questions or unlock full 5-domain-weighted timed mock exams (one-time $4.99 / $9.99, no subscription) at [AICertMock — CCAR-F Practice Exam](https://aicertmock.com/claude-certified-architect).

---

## Domain Breakdown & Key Concepts

### Domain 1: Agentic Architecture & Orchestration (27%)
The highest-weighted domain focuses on multi-agent patterns, orchestration loops, and state handoffs.
- **Orchestrator-Worker vs. Peer-to-Peer**: Know when to use a central coordinator (deterministic routing, centralized retry) versus decentralized handoffs.
- **Subagent Context Isolation**: Understand why subagents receive scoped context rather than full parent history to prevent context pollution and token bloat.
- **Stop Conditions & Guardrails**: Implement explicit `max_turns`, token budgets, and loop-detection termination criteria.
- **Human-in-the-Loop (HITL) Checkpoints**: Identify high-blast-radius tool calls (deletions, external writes, billing actions) that require confirmation gates.

### Domain 2: Claude Code Configuration & Workflows (20%)
Covers repository-level configuration, memory hierarchy, and CLI automation.
- **CLAUDE.md Hierarchy**: Precedence between enterprise/global rules, project root `CLAUDE.md`, and directory-scoped instructions.
- **Custom Slash Commands & Skills**: Structuring reusable prompts in `.claude/commands/` and skill definitions with clear input/output contracts.
- **Headless / CI Integration**: Running non-interactive mode (`claude -p`), `--allowedTools` scoping, and structured JSON output for PR reviews and automated triage.
- **Hooks**: Pre-tool and post-tool hooks for linting, secret scanning, and deterministic validation.

### Domain 3: Prompt Engineering & Structured Output (20%)
Focuses on reliability, determinism, and schema enforcement in production systems.
- **JSON Schema / Tool-Use Extraction**: Enforcing strict output schemas via tool definitions or constrained decoding instead of fragile regex parsing.
- **Few-Shot Calibration**: Selecting diverse edge-case examples to anchor classification and extraction boundaries.
- **Extended Thinking Trade-offs**: Balancing reasoning token budgets against latency and cost for multi-step architectural decisions.
- **XML Tagging & Role Separation**: Isolating untrusted user/document input from system instructions to mitigate prompt injection.

### Domain 4: Tool Design & MCP Integration (18%)
Covers Model Context Protocol (MCP) architecture and ergonomic tool interfaces.
- **MCP Primitives**: Distinguishing **Tools** (model-controlled actions), **Resources** (application-controlled read-only context), and **Prompts** (user-controlled templates).
- **Tool Schema Design**: Writing unambiguous parameter descriptions, enums, and action-oriented error messages that guide self-correction.
- **Transport & Security**: Local `stdio` vs. remote `Streamable HTTP` / SSE, OAuth scoping, and least-privilege credential isolation.
- **Idempotency & Error Recovery**: Designing retry-safe tools and returning structured error payloads (`isError: true`) rather than crashing the agent loop.

### Domain 5: Context Management & Reliability (15%)
Covers long-horizon execution, caching, and observability.
- **Prompt Caching**: Placing static system prompts, tool definitions, and large reference docs at the prefix to maximize cache hit rates and reduce TTFT/cost.
- **Compaction & Summarization**: Triggering rolling summaries before hitting context limits while preserving critical constraints and file paths.
- **Evaluation & Regression Testing**: Building deterministic eval suites (exact match, schema validation, LLM-as-a-judge) before changing prompts or models.
- **Rate Limits & Fallbacks**: Exponential backoff with jitter for `429`/`529` responses and graceful degradation.

---

## Common Exam Traps

1. **Over-engineering with multi-agent swarms**: If a single prompt with well-defined tools solves the task deterministically, multi-agent orchestration is usually the wrong answer.
2. **Putting dynamic timestamps at the start of the system prompt**: This invalidates prompt cache prefixes on every request.
3. **Treating MCP Resources as Tools**: Read-only contextual data should be exposed as Resources (or fetched deterministically), not forced through unnecessary tool round-trips.
4. **Ignoring `CLAUDE.md` scope**: Project-level rules should stay concise; bloated instruction files dilute attention on critical constraints.

---

## Practice & Diagnostic Resources

- **Official Anthropic Exam Blueprint**: Review the official domain objectives and sample scenarios published by Anthropic (`docs.anthropic.com`).
- **Timed Mock Exams & Free Sample Preview**: [AICertMock — Claude Certified Architect (CCAR-F) Practice Exam](https://aicertmock.com/claude-certified-architect) provides 3 free no-login sample questions for quick preview, plus full-length timed mock exams weighted to the official 5 CCAR-F domains (one-time $4.99 / $9.99 unlock, no subscription).
- **MCP Specification**: Study the official Model Context Protocol docs (`modelcontextprotocol.io`) for Tools, Resources, and Transport security.

## License

MIT
