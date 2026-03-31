# Theory Sources

This document explains where the Continuity Protocol's theory comes from.

The spec is not pulled from one paper or one codebase. It is a synthesis of several streams of practice and thought.

## 1. Event sourcing and durable logs

The protocol inherits the idea that history matters and that durable state should be reconstructable from meaningful recorded events.

What it borrows:

- state should be inspectable after the fact
- provenance matters
- replayable history is useful

What it does **not** borrow wholesale:

- replay alone is not enough for bounded continuity
- operational continuity needs typed carryover, not only append-only logs

## 2. Incident management and operations

A major source of protocol intuition is operational work:

- incidents
- postmortems
- action items
- constraints discovered through failure
- repeated mistakes caused by forgotten lessons

This is where the protocol's emphasis on operational scars, constraints, and explicit status comes from.

## 3. Agent memory systems

The protocol is shaped by practical failures in agent memory systems, especially where:

- summaries hide important distinctions
- vector retrieval returns plausible but wrong context
- state exists in storage but does not surface when needed
- handoff appears smooth but is actually reconstructed from hidden fallback paths

That is the origin of the spec's insistence on:

- typed continuity
- proof path vs control path
- truthful fallback labeling

## 4. Retrieval and ranking systems

The protocol assumes that no single retrieval signal is enough.

This comes from real retrieval practice:

- lexical match matters
- semantic similarity matters
- recency matters
- scope/task fit matters
- lineage/support fit matters

This is why the spec treats multi-dimensional retrieval as core rather than optional decoration.

The current implementation also separates two operational priors:

- a broader live-state prior that seeds from `current_practice`
- a narrower next-step prior that can elevate the freshest working-state item when an immediate-action prompt is asking what to do next
- a recent-update prior that seeds from the newest high-signal pivots and recently validated guidance when the prompt is asking about recent progress, latest decisions, latest lessons, or what changed
- an active-thread prior that seeds from the freshest validated local decision or lesson when the prompt is a vague resumption or verification of the current work thread

This keeps the priors distinct from one another: live-state answers "what is active now", next-step answers "what should happen next", recent-update answers "what materially changed most recently", and active-thread answers "what should I resume in this thread".
## 5. Multi-agent coordination practice

Single-agent memory is not sufficient once several workers, agents, or sessions are involved.

The protocol's treatment of:

- work claims
- conflict pressure
- coordination signals
- active ownership

comes from the practical reality that continuity failure often shows up as duplicated or conflicting work, not just forgotten facts.

## 6. Benchmark and evaluation discipline

The protocol is also shaped by a simple operational truth:

> if a continuity claim cannot be falsified, it will drift into marketing.

This is the source of the evaluation posture:

- compare proof path against control paths
- make fallback visible
- prefer blind or sealed evaluation when possible
- record enough metadata to reproduce the result

## 7. What remains intentionally non-canonical

The spec deliberately avoids claiming canonical authority over:

- exact numeric retrieval weights
- exact register naming
- exact storage topology
- exact benchmark canon

Those areas are still under active discovery.
