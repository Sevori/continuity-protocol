# Comparison with Context Engineering, Skills, and Workflows

This document explains how the Continuity Protocol differs from and complements current context engineering patterns.

## 1. Prompt stuffing / rolling context

Typical approach:
- keep pushing more raw context into the prompt

Strength:
- easy to start

Weakness:
- expensive
- unstructured
- does not distinguish important state from ambient history

Protocol advantage:
- bounded, typed, inspectable carryover

## 2. Summaries

Typical approach:
- summarize prior turns or state

Strength:
- compact

Weakness:
- hides status, provenance, and semantic distinctions
- can sound coherent while losing critical constraints

Protocol advantage:
- typed continuity keeps decisions, constraints, incidents, and scars distinct

## 3. RAG / vector retrieval

Typical approach:
- retrieve semantically similar chunks

Strength:
- useful for knowledge lookup

Weakness:
- similarity alone does not tell you whether something is open, resolved, superseded, or operationally dangerous to ignore

Protocol advantage:
- continuity retrieval uses more than similarity

## 4. Skills

Typical approach:
- reusable procedural capability packaged as instructions or tools

Strength:
- capability reuse

Weakness:
- skills tell the agent how to do something, not what state must survive a boundary

Protocol relation:
- skills are procedural memory / capability surfaces
- continuity is state survival and work preservation

## 5. Workflow engines

Typical approach:
- explicit state machines, jobs, DAGs, orchestration logic

Strength:
- strong control over task execution

Weakness:
- may still fail to preserve rich semantic state for the agent layer

Protocol relation:
- workflows coordinate process steps
- continuity preserves the meaning required to resume work truthfully

## 6. Biggest benefit of the protocol

The biggest benefit is not just better recall.

It is this:

> preserving the right state in a form that reduces duplicate work, false confidence, and boundary-induced confusion.

This matters most in:
- handoff
- restart
- crash recovery
- model swap
- multi-agent coordination
- long-running operator systems
