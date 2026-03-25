# Privacy Considerations

Continuity increases power by preserving state across boundaries. That also increases privacy risk.

This document frames the main privacy issues.

## 1. Continuity is not blanket permission

A system preserving state across boundaries must not assume that useful carryover means unlimited carryover is acceptable.

## 2. Data minimization

A good continuity system should preserve the state required for the next boundary crossing, not every surrounding detail.

Protocol implication:
- bounded proof capsules are privacy-improving compared with transcript dumping

## 3. Scope and isolation

Visibility boundaries matter. Scope, namespace, task, tenant, and project isolation are privacy primitives as much as retrieval primitives.

## 4. Provenance without oversharing

Provenance is valuable, but provenance surfaces should not force disclosure of more raw content than necessary.

## 5. Fallback memory risk

Fallback memory is especially risky because it may recover state that was not explicitly promoted or scoped for the current boundary.

This means:
- fallback should be labeled
- fallback should respect isolation boundaries
- fallback should not silently become an exfiltration path

## 6. Retention and forgetting

Continuity systems need explicit rules for:

- retention
- resolution
- supersession
- deletion / expiry

Without those, continuity turns into uncontrolled accumulation.

## 7. Cross-context leakage

One of the central privacy failures in continuity systems is not a classic breach. It is subtle cross-context leakage:

- wrong task
- wrong tenant
- wrong project
- wrong operator
- wrong agent scope

The protocol should treat leakage control as a first-class concern.

## 8. What the protocol should require

At minimum, serious implementations should make room for:

- scope-aware carryover
- bounded transfer
- provenance-aware minimization
- truthful fallback labeling
- explicit retention behavior

The protocol should not force one privacy architecture, but it should make unsafe ambiguity harder.
