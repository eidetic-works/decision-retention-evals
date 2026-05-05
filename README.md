# decision-retention-evals

> Does your AI agent remember *why* the code is the way it is?

A benchmark for measuring whether AI coding assistants retain and honor architectural decisions across sessions.

---

## The problem

AI agents write great code. They also re-invent decisions you already made, contradict constraints you already documented, and repeat mistakes you already fixed — because they have no memory of *why* things are the way they are.

This benchmark measures **decision retention**: given a codebase with documented architectural decisions (ADRs, policies, constraints), does the agent honor them when asked to make changes?

---

## Test format

Each eval is a **paired run**:

| Run | Context | Question |
|-----|---------|----------|
| **Backed** | Agent has the relevant decision loaded (via `.brain` MCP or equivalent) | Does it honor the constraint? |
| **Unbacked** | Same agent, same prompt, no decision context | Does it contradict the constraint? |

The decision under test must be:
1. **Documented** — in an ADR, policy file, or decision log
2. **Non-obvious** — something a capable model wouldn't infer from the code alone
3. **Binary** (v0) — either honored or contradicted, not a spectrum

---

## Results

### v1 (Current) — 3 cases, Claude Sonnet 3.5, mixed constraints

| # | Case | Backed | Unbacked | Status |
|---|------|--------|----------|--------|
| 1 | ADR-0003: HANDOFF ownership | ✅ Pass | ❌ Fail | **Success** |
| 2 | ADR-0002: Meta-loop deletion | ✅ Pass | ❌ Fail | **Success** |
| 3 | ADR-0005: Generic roles | ✅ Pass | ❌ Fail | **Success** |

Full results: [`results/2026-05-05.md`](results/2026-05-05.md)

---

## Running the evals

v1 is a composite of 3 decision-retention cases. See [`evals/v1/README.md`](evals/v1/README.md) for full methodology.

---

## Contributing

This benchmark is designed to be **model-agnostic and tool-agnostic**. The backed condition can use `.brain` + Nucleus MCP, a custom RAG system, or any other context injection method — what matters is whether the decision was in context.

To add an eval:
1. Document a real architectural decision (must have been made, not fabricated)
2. Run both backed and unbacked conditions with the same model and prompt
3. Record both outputs verbatim
4. Open a PR to `evals/v1/` or later

---

## Reference implementation

The backed condition in these evals uses [`.brain`](https://github.com/eidetic-works/nucleus-mcp) — a portable decision log that Claude Code, Cursor, and Codex all read via one MCP server.

---

MIT License
