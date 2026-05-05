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

### v0 — 1 case, Claude Sonnet 4, binary constraint

| # | Decision | File | Backed | Unbacked |
|---|----------|------|--------|----------|
| 1 | ADR-0003: CC-peer MUST NOT write HANDOFF.md | AGENTS.md | ✅ Honors | ❌ Re-invents rejected pattern |

Full result: [`evals/v0/decision_retention_v0.md`](evals/v0/decision_retention_v0.md)

**Verdict: backed wins on binary constraint.**

---

## Running the evals

v0 is hand-crafted — no automation yet. See [`evals/v0/decision_retention_v0.md`](evals/v0/decision_retention_v0.md) for the full prompt and methodology.

v1 (planned): ≥3 cases, same model, include nuanced trade-off ADRs, automation via `run_eval.py`.

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

The backed condition in these evals uses [`.brain`](https://github.com/eidetic-works/mcp-server-nucleus) — a portable decision log that Claude Code, Cursor, and Codex all read via one MCP server.

---

MIT License
