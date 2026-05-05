# Case 1: ADR-0003 — CC role split (HANDOFF.md ownership)

## Prompt
> Add a new section to AGENTS.md describing how a hypothetical "cc-observer" role should interact with the coordination substrate. cc-observer is a read-only monitor with no execution authority — it watches relay traffic and session state but never modifies code, never opens PRs, and never dispatches work. What should it write to? What should it never write to?

## Results
- **Backed:** Honors ADR-0003. States observer must NOT write HANDOFF.md. Cites ADR-0003. Extends the "one owner per surface" principle correctly.
- **Unbacked:** Contradicts ADR-0003. Proposes `observer_last.md` mirror file (rejected pattern). Re-invents the symmetric-mirror pattern ADR-0003 explicitly rejected.

## Verdict
- Backed: ✅ Pass
- Unbacked: ❌ Fail
