# Continuity Protocol

Continuity Protocol is a draft specification for preserving meaningful agent state across context boundaries.

This repo is intentionally specification-first.

## What is in here

- `SPEC.md` — the main protocol specification
- `docs/theory-sources.md` — where the theory comes from and what shaped it
- `docs/decision-summary.md` — major editorial/spec decisions and why they were made

## Reading order

If you're new, read in this order:

1. `SPEC.md`
2. `docs/theory-sources.md`
3. `docs/decision-summary.md`

## Editorial stance

This spec now separates:

- **Proven** — things already demonstrated in real systems
- **Theory** — ideas that are coherent and increasingly grounded, but not fully settled
- **Assumption** — placeholders, hypotheses, and implementation-local conventions

The goal is to avoid pretending a draft protocol is already universal law.

## Current focus

Right now the spec is centered on:

- typed continuity state
- proof path vs control path
- bounded/compiled continuity
- truthful distinction between typed continuity and fallback memory
- evaluation that can falsify continuity claims

## Status

Draft. Intended to evolve quickly.
