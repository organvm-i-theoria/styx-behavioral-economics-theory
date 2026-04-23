# Taxis Audit Orchestration

This document formalizes how ORGAN-IV (Taxis) may participate in the Styx system once the downstream product supports simulated stakes and agent spawning. The key constraint is architectural: Taxis may orchestrate, route, and supervise an audit process, but it does not become the source of truth for settlement. Truth still resolves downstream through the audit and ledger mechanisms governed by ORGAN-III.

## The Orchestration Role of Taxis

Taxis exists to order processes, not to redefine the incentive model. In the Styx stack, this means Taxis may receive a **simulated stake signal** that represents urgency, seriousness, or queue priority for an auditable event. That signal can trigger the creation of an audit agent, but the signal itself is not a monetary settlement instruction.

The simulated stake therefore serves three functions:

1. **Priority encoding** -- higher simulated stake implies higher routing urgency and stronger demand for rapid review.
2. **Attention commitment** -- the caller exposes something scarce, even if test-denominated, before consuming orchestration capacity.
3. **Audit provenance** -- the stake payload becomes part of the event record that explains why an audit agent was spawned.

Taxis may hold this signal in a dispatch ledger, but only as an orchestration artifact. It cannot slash, release, or redistribute real value on its own authority.

## Minimal Event Flow

The allowed flow is:

1. A downstream Styx surface emits an auditable event plus simulated stake metadata.
2. Taxis records the event in its orchestration ledger with a unique dispatch identifier.
3. Taxis spawns an audit agent with bounded scope: inspect the claim, gather relevant proof context, and return a structured recommendation.
4. The audit agent hands its output back to the governed audit path -- for example Fury consensus, honeypot validation, or a ledger-attested review service.
5. ORGAN-III performs the actual consequence update, if any, and writes the durable truth record.

This preserves the unidirectional flow:

`Theory -> Art -> Product -> Orchestration support -> Product settlement`

Taxis may accelerate a verdict. It may not replace one.

## The Audit Agent Contract

An audit agent spawned from Taxis must operate under a narrow contract:

- It receives the simulated stake amount, the triggering event, and the proof pointer.
- It may inspect evidence, request additional context, and score confidence.
- It must return a typed recommendation such as `PASS`, `FAIL`, `ESCALATE`, or `INSUFFICIENT_SIGNAL`.
- It cannot directly mutate integrity, balances, or contract status.

This constraint matters because the spawned agent is an epistemic instrument, not a sovereign judge. If agent outputs directly changed stake state, the orchestration layer would collapse into an unreviewable authority.

## Why Simulated Stake Belongs Here

Simulated stake is especially appropriate for Taxis because it preserves the behavioral shape of commitment without requiring that the orchestration organ become a financial custodian. In early or internal flows, a test-denominated stake can still encode:

- queue seriousness
- escalation threshold
- audit budget
- retry friction

That is enough to prevent free spam and to align routing effort with perceived consequence.

The behavioral logic is consistent with the broader Styx theory: even simulated loss changes behavior when the user or calling subsystem believes the signal has procedural consequence.

## Safety Constraints

Four constraints keep this model coherent:

1. **Simulation boundary**: Taxis may only accept simulated or test-denominated stake signals unless an explicit downstream settlement authority is referenced.
2. **No unilateral slashing**: Taxis never finalizes forfeiture or payout.
3. **Bounded spawn authority**: each spawned audit agent must cite a parent dispatch identifier and a single audit objective.
4. **Durable handoff**: every recommendation must return to a downstream truth system that owns final state mutation.

Without these constraints, orchestration can become shadow governance.

## Theoretical Payoff

This model gives Styx a scalable intermediate layer between raw user events and final platform consequences. Simulated stake lets the system express urgency before money moves. Agent spawning lets the system scale inspection without pretending that every audit must be manually initiated. Taxis therefore becomes a force multiplier for audit throughput while leaving truth, integrity, and settlement in their proper downstream home.
