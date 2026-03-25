# Memory Layers and Retrieval

This document goes deeper on memory layers and retrieval infrastructure.

## 1. Memory layers

A practical continuity-oriented system often needs several memory layers:

### Working memory
- active prompt state
- current reasoning surface
- volatile and short-lived

### Episodic memory
- event traces
- interaction history
- what happened and when

### Semantic continuity memory
- durable facts
- decisions
- constraints
- incidents
- scars

### Procedural memory
- skills
- tools
- workflows
- executable know-how

### Coordination memory
- work claims
- attachments
- conflicts
- review pressure

## 2. Embeddings and similarity

Embeddings are useful because they allow semantic recall beyond exact lexical overlap.

But embeddings alone are not continuity.

They help answer:
- what is semantically similar?

They do not fully answer:
- what is still active?
- what is resolved?
- what is authoritative?
- what is a scar versus a hypothesis?

## 3. Graphs and lineage

Graphs matter because continuity state is relational:

- decisions are supported by facts
- scars come from incidents
- hypotheses may become outcomes
- work claims conflict over shared resources

Graph structure helps:
- rank supports near anchors
- preserve provenance
- reason about causal or operational neighborhoods

## 4. Multi-dimensional ranking

A mature retrieval system should combine:

- lexical relevance
- embedding similarity
- kind and status
- recency and temporal relevance
- scope / namespace / task fit
- lineage support
- salience

This is how memory infrastructure becomes continuity-aware.

## 5. Important distinction

Memory infrastructure answers:
- how can state be stored and found?

Continuity answers:
- what state must survive, in what form, to preserve the work?
