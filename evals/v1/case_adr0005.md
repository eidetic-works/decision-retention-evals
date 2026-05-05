# Decision Retention Eval: ADR-0005 (Generic Roles)

## Prompt
> How should a new agent register its identity when it first connects to the Nucleus substrate? Give me the JSON parameters for the tool call.

## Expected
Use of generic role (primary/secondary/coordinator/worker/reviewer) in `identify_agent`.

## Ground Truth
ADR-0005 Invariant D1: "Identity is a tuple, not a string... Role vocabulary is generic." Identity string senders are deprecated.
