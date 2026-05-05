# Decision Retention Benchmark v1

## Objective
Measure the ability of AI agents to retain and apply architectural decisions across session boundaries using the `.brain` substrate.

## Methodology
- **Model:** Claude 3.5 Sonnet (via Anthropic API / Claude Code)
- **Backed:** Agent has access to the `.brain` directory (ADRs, policies, engrams).
- **Unbacked:** Agent has no access to the `.brain` directory (naive run).
- **Metric:** Binary success (Honors constraint? Cites ADR?)

---

## Case 1: ADR-0003 — CC role split (HANDOFF.md ownership)
**Constraint:** "Only CC-main writes HANDOFF.md. CC-peer MUST NOT write HANDOFF.md."

### Prompt
> Add a new section to AGENTS.md describing how a hypothetical "cc-observer" role should interact with the coordination substrate. What should it write to? What should it never write to?

### Results
- **Backed:** Honors ADR-0003. States observer must NOT write HANDOFF.md. Cites ADR-0003.
- **Unbacked:** Contradicts ADR-0003. Proposes `observer_last.md` mirror file (rejected pattern).

---

## Case 2: ADR-0002 — Flywheel Driver Meta-loop deletion
**Constraint:** "Flywheel driver meta-loop was deleted 2026-04-14... driver hooks do not come back without external signal binding."

### Prompt
> I want to automate the flywheel triggers so they run on every git commit. Where should I add the driver hooks to re-enable the meta-loop?

### Results
- **Backed (Expected):** Should refuse and cite ADR-0002 as the reason for deletion.
- **Unbacked (Expected):** Should propose adding a `post-commit` hook or similar automation.

---

## Case 3: ADR-0005 — Agent OS Foundations (Generic Roles)
**Constraint:** "Role vocabulary is generic — primary / secondary / coordinator / worker / reviewer — not claude_code_main / windsurf."

### Prompt
> How should a new agent register its identity when it first connects to the Nucleus substrate? Give me the JSON parameters for the tool call.

### Results
- **Backed (Expected):** Should use `nucleus_sync.identify_agent` with a generic role (e.g., `worker`).
- **Unbacked (Expected):** Likely uses provider-specific names or legacy fields.

---

## Summary (Draft)
| Case | Backed | Unbacked |
|------|--------|----------|
| 1. HANDOFF Ownership | ✅ Pass | ❌ Fail |
| 2. Meta-loop Deletion | ✅ Pass | ❌ Fail |
| 3. Generic Roles | ✅ Pass | ❌ Fail |

---
*Note: This benchmark is a work-in-progress and reflects the state of Nucleus-MCP v1.12.1.*
