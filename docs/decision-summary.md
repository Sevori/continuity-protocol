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

## Decision 9 — Add survival analysis as evaluation evidence (v0.3.0)

The spec now includes per-item survival analysis as a proven evaluation mechanism, with honest framing of what it does and does not demonstrate.

### Why

A reference implementation produced 300+ survival records per suite run across 10 benchmark classes. The mechanism is real. But the spec is careful not to claim that metacognitive hypothesis generation produces value — only that the mechanism exists and can reject noise. The kill criterion (5 cycles, > 5% absolute improvement) is included to prevent unfalsifiable claims.

## Decision 10 — Add security section for continuity-in-the-loop (v0.3.0)

The spec now explicitly warns that feeding continuity state back into agent prompts creates a prompt injection attack surface.

### Why

This was a concrete finding from adversarial review: functions that render hypothesis text into extraction prompts concatenated content without sanitization. The protocol should not prescribe a sanitization mechanism, but it should make the risk explicit so implementers do not ignore it.

## Decision 11 — Include differential decay as proven, not prescriptive (v0.3.0)

The spec acknowledges that differential decay by continuity kind is practical and running, but keeps exact half-lives as implementation-local.

### Why

The principle is grounded (scars should persist longer than ephemeral state). The specific numbers (720h for scars, 18h for working state) are one implementation's choice, not protocol law.
