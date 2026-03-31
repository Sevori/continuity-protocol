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

## 9. Prompt injection via continuity-in-the-loop

When continuity state is fed back into agent prompts — as context packs, extraction hints, or metacognitive guidance — a new attack surface emerges.

### The finding

A reference implementation (2026-03-26) was found to concatenate hypothesis text and lesson content directly into extraction prompts without sanitization. Any agent that writes a continuity item effectively controls part of the extraction prompt for future agents.

### Why this matters for privacy

- a compromised or confused agent can embed prompt injection payloads in continuity items
- the attack operates through the memory layer, not through direct user input, making it harder to detect
- cross-agent trust assumptions break: agent A writes a scar, agent B's extraction prompt includes it verbatim

### Mitigation posture

- treat all continuity item content as untrusted at the prompt boundary
- structurally isolate continuity content from prompt control flow
- do not assume items written by previous agents are benign
- consider content-length limits and format validation for items that will be injected into prompts

**Status:** This is a proven concern, not a theoretical risk. The vulnerable code was found in production-path functions during adversarial review.
