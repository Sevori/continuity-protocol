# Continuity Protocol

Continuity Protocol is a draft specification for preserving meaningful agent state across context boundaries.

This repository is specification-first and intentionally separates demonstrated behavior from theory and assumptions.

## What is here

- `SPEC.md` — the main specification
- `docs/theory-sources.md` — conceptual and engineering influences behind the draft
- `docs/decision-summary.md` — editorial and structural decisions in the spec
- `docs/hypotheses-and-validation.md` — what appears strongly validated vs still under active validation
- `docs/neuroscience-context.md` — cognitive and neuroscientific context used as explanatory background
- `docs/context-engineering-comparison.md` — comparison with current context engineering patterns
- `docs/memory-layers-and-retrieval.md` — deeper framing for memory layers, graphs, embeddings, and retrieval
- `docs/privacy-considerations.md` — privacy posture and protocol-level privacy constraints

## Reading order

1. `SPEC.md`
2. `docs/hypotheses-and-validation.md`
3. `docs/neuroscience-context.md`
4. `docs/context-engineering-comparison.md`
5. `docs/memory-layers-and-retrieval.md`
6. `docs/privacy-considerations.md`
7. `docs/theory-sources.md`
8. `docs/decision-summary.md`

## Editorial stance

This draft distinguishes:

- **Proven** — behavior already demonstrated in real systems
- **Theory** — coherent claims shaped by practice, but not fully settled
- **Assumption** — provisional conventions, hypotheses, or implementation-local choices

The goal is to make the protocol intellectually honest while keeping it useful.

## Current focus

The protocol currently focuses on:

- typed continuity state
- proof path vs control path
- bounded carryover and compiled continuity
- truthful distinction between continuity survival and fallback recovery
- evaluation that can falsify continuity claims
- privacy-aware continuity boundaries
- per-item survival analysis and metacognitive evaluation (new in 0.3.0)
- security of continuity-in-the-loop (feeding state back into prompts) (new in 0.3.0)
- differential decay by continuity kind and lifecycle status (new in 0.3.0)
- explicit `supersedes` lineage for belief and guidance updates
- current-practice projection as a first-class read surface
- current-practice packs carrying a bounded provenance-backed evidence chain
- learning digests and full learning lineage as first-class read surfaces
- stale open guidance retirement in normal recall while preserving full history on demand

## Status

Draft (v0.3.0). Updated 2026-03-31 with metacognitive evaluation evidence, security findings, and current-practice evidence-chain carry-through from a reference implementation. Expected to evolve quickly.
