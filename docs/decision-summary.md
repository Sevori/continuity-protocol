# Decision Summary

This document records major editorial and structural decisions in the draft Continuity Protocol spec.

## Decision 1 — Separate reality from aspiration

The spec now explicitly marks ideas as:

- Proven
- Theory
- Assumption

### Why

Because the old style of draft spec made it too easy to read working hypotheses as universal law.

## Decision 2 — Make proof path vs control path central

The distinction between explicit proof-bearing handoff and ordinary context access is now a first-class concept.

### Why

A system should not get to claim continuity success merely because the operator can still dig the answer out of storage.

## Decision 3 — Treat fallback memory honestly

The spec now explicitly distinguishes typed continuity from fallback event memory.

### Why

Real systems often recover truth via fallback. That's useful, but it is not the same thing as successful typed continuity recall.

## Decision 4 — Keep taxonomy grounded but not over-locked

The spec preserves a core set of continuity kinds, but stops pretending the full taxonomy is permanently settled.

### Why

The shape is strong enough to be useful, but still too young to be frozen as universal ontology.

## Decision 5 — Include coordination state in continuity

Continuity is not limited to facts, summaries, and decisions.

### Why

In real agent systems, forgotten work ownership or unresolved conflicts cause major continuity failures.

## Decision 6 — Keep scoring dimensions, drop fake precision

The spec keeps the idea of multi-dimensional retrieval but avoids pretending exact coefficients are protocol truth.

### Why

The dimensions are real; the exact numbers are not yet universal.

## Decision 7 — Keep conformance levels as guidance, not dogma

Conformance levels remain in the spec, but as draft guidance.

### Why

They help orient implementers, but the ecosystem is not mature enough to treat them as fixed law.

## Decision 8 — Remove private implementation references

The spec and supplemental docs do not rely on naming private repositories.

### Why

The protocol should stand on its own and avoid leaking implementation-specific provenance that is not meant for public framing.
