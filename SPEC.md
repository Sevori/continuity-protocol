
# Continuity Protocol Specification
 
**Version:** 0.1.0-draft
**Date:** 2026-03-23
**Authors:** Renato Rodrigues de Araujo
**License:** CC-BY-4.0
 
---
 
## 1. Introduction
 
### 1.1 Purpose
 
This specification defines the Continuity Protocol — a set of semantics, data structures, and contracts for preserving meaningful agent state across context boundaries.
 
A "context boundary" is any event that would otherwise cause an agent to lose working state: context window exhaustion, model swap, session restart, agent-to-agent handoff, crash recovery, or scheduled compaction.
 
### 1.2 Scope
 
This protocol defines:
 
- The vocabulary and data model for typed continuity state
- The lifecycle of continuity items (creation, promotion, resolution, supersession)
- The proof capsule format for handoff
- The compiled continuity model for efficient recall
- The handoff and resume contract between agents
- The evaluation contract for measuring continuity survival
 
This protocol does not define:
 
- Storage implementation details (database choice, indexing strategy)
- Transport protocol (HTTP, gRPC, stdio — these are binding-specific)
- Embedding or vector retrieval algorithms
- Agent orchestration or task scheduling
- Authentication or authorization
 
### 1.3 Design Principles
 
1. **Typed over untyped.** Continuity state carries explicit kinds and status, not free-text summaries. A decision is a decision, not a paragraph that happens to mention one.
 
2. **Proof over assumption.** Handoff emits a verifiable capsule of what was transferred. The receiving agent can inspect what survived, not just hope context was available.
 
3. **Bounded over unbounded.** Continuity must survive within token budgets. The protocol favors compiled, high-signal state over raw replay.
 
4. **Durable over ephemeral.** Operational scars, constraints, and resolved decisions persist as first-class state, not as side effects buried in logs.
 
5. **Explicit over implicit.** Status changes, resolution, and supersession are recorded state transitions, not inferred from absence.
 
---
 
## 2. Data Model
 
### 2.1 Continuity Item
 
A continuity item is the fundamental unit of durable agent state. Each item has:
 
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | UUID v7 | yes | Unique identifier, temporally sortable |
| `context` | string | yes | The continuity context this item belongs to |
| `kind` | ContinuityKind | yes | The semantic type of this item |
| `status` | ContinuityStatus | yes | Current lifecycle status |
| `title` | string | yes | Human and machine-readable label |
| `body` | string | yes | Full content of the item |
| `source_agent` | string | no | The agent that created this item |
| `source_event_id` | string | no | The originating event, for provenance |
| `namespace` | string | no | Isolation boundary |
| `task_id` | string | no | Associated task |
| `scope` | Scope | yes | Visibility boundary |
| `salience` | float | yes | Importance weight (0.0–1.0) |
| `confidence` | float | yes | Confidence weight (0.0–1.0) |
| `support_ids` | list[UUID] | no | Items that support or are supported by this item |
| `support_type` | string | no | Relationship type (e.g., "continuity", "evidence") |
| `created_at` | timestamp | yes | Creation time |
| `updated_at` | timestamp | yes | Last modification time |
 
### 2.2 Continuity Kinds
 
The protocol defines the following continuity kinds. Implementations MAY extend this set but MUST support the core kinds.
 
| Kind | Signal Weight | Description |
|------|--------------|-------------|
| `decision` | 0.30 | A choice made by an agent, with rationale |
| `constraint` | 0.28 | A boundary or limitation that must be respected |
| `fact` | 0.26 | A verified piece of information |
| `operational_scar` | 0.42 | A lesson learned from failure — the highest-signal kind |
| `incident` | 0.35 | An operational event that disrupted normal flow |
| `hypothesis` | 0.22 | A proposed explanation or approach, not yet confirmed |
| `derivation` | 0.20 | A conclusion derived from other items |
| `outcome` | 0.18 | The result of an action or decision |
| `lesson` | 0.15 | A general insight extracted from experience |
| `signal` | 0.12 | A coordination or status signal between agents |
| `summary` | 0.10 | A compacted representation of other items |
| `working_state` | 0.08 | Live ephemeral state (lowest signal weight) |
| `work_claim` | 0.10 | A declared ownership of a task or resource |
 
Signal weights indicate relative importance during retrieval scoring. Operational scars carry the highest weight because lessons from failure are the most expensive to lose.
 
### 2.3 Continuity Status
 
| Status | Description |
|--------|-------------|
| `open` | Active and relevant. Score boost: 0.18 |
| `active` | Currently being acted upon. Score boost: 0.14 |
| `resolved` | Completed or addressed. Score boost: 0.03 |
| `superseded` | Replaced by a newer item. Score boost: 0.01 |
| `rejected` | Explicitly discarded. Score boost: 0.00 |
 
### 2.4 Scope
 
| Scope | Description |
|-------|-------------|
| `agent` | Visible only to the creating agent |
| `session` | Visible within the current session |
| `shared` | Visible across agents in the same context |
| `project` | Visible across all contexts in a project |
| `global` | Visible everywhere |
 
---
 
## 3. Continuity Context
 
A continuity context is a named boundary within which continuity items live. Contexts provide isolation — items in different contexts do not interfere with each other unless explicitly bridged through handoff.
 
Each context tracks:
 
- A set of continuity items
- Active attachments (agents currently connected)
- Active work claims
- Coordination signals
- Compiled continuity state (see §5)
 
Contexts are identified by string names and may be hierarchical (e.g., `machine/project/task`).
 
---
 
## 4. Proof Capsule
 
### 4.1 Structure
 
A proof capsule is the bounded packet emitted by `handoff()`. It contains typed registers — selected high-signal continuity items formatted for the receiving agent.
 
```
ProofCapsule {
  registers: list[TypedRegister]
  source_context: string
  emitted_at: timestamp
  register_count: integer
}
```
 
### 4.2 Typed Register
 
Each register in a proof capsule represents one piece of continuity state that the protocol claims survived the handoff.
 
| Field | Type | Description |
|-------|------|-------------|
| `label` | string | Stable identifier (e.g., `pf1`, `pd1`, `pk1`, `ps1`, `pn1`) |
| `register_kind` | ContinuityKind | The kind of the source item |
| `title` | string | Title of the continuity item |
| `body` | string | Content of the item |
| `source_id` | UUID | The originating continuity item |
| `has_provenance` | boolean | Whether the source event chain is intact |
 
### 4.3 Register Label Convention
 
| Prefix | Meaning | Example |
|--------|---------|---------|
| `pf` | Fact | `pf1` — first fact register |
| `pd` | Decision | `pd1` — first decision register |
| `pk` | Constraint | `pk1` — first constraint register |
| `ps` | Operational scar | `ps1` — first scar register |
| `pn` | Next-step intent | `pn1` — first next-step register |
| `ph` | Hypothesis | `ph1` — first hypothesis register |
| `pi` | Incident | `pi1` — first incident register |
 
### 4.4 Capsule Invariants
 
1. The capsule MUST be bounded. Implementations SHOULD enforce a maximum token budget.
2. Registers MUST reference real continuity items, not synthesized summaries.
3. The capsule MUST NOT contain raw transcript or log data.
4. Placeholder or empty registers MUST be filtered before emission.
5. Duplicate registers (same source item appearing under multiple labels) MUST be deduplicated.
 
---
 
## 5. Compiled Continuity
 
### 5.1 Purpose
 
Raw continuity items accumulate over time. Compiled continuity is a lazily-maintained projection of those items into searchable bands, optimized for recall without replaying full history.
 
### 5.2 Compilation Bands
 
| Band | Content | Retention |
|------|---------|-----------|
| `hot` | Open and active items with high salience | Always available for recall |
| `warm` | Recently resolved items, active hypotheses, supporting evidence | Available with moderate cost |
| `cold` | Superseded, rejected, or low-salience resolved items | Available on explicit request |
 
### 5.3 Compilation Rules
 
1. Continuity mutations mark the affected context as dirty.
2. The next recall operation recompiles only dirty contexts.
3. Routine heartbeat or lease-renewal traffic MUST NOT trigger recompilation.
4. Items with `support_type="continuity"` MAY be promoted into the same band as their anchor.
5. Bundled items render as structural envelopes with nested `supported-by` entries.
6. In-band causal anchors rank ahead of unrelated standalone items once support links exist.
 
---
 
## 6. Protocol Surfaces
 
### 6.1 Core Operations
 
The protocol defines four core operations. Implementations MUST provide all four through at least one binding (CLI, HTTP, MCP, library, etc.).
 
#### `handoff()`
 
Emits a proof capsule from the current context. The capsule contains the typed registers that the protocol claims are necessary for work to continue.
 
**Input:**
- `from_agent`: the handing-off agent
- `to_agent`: the receiving agent (may be unknown at handoff time)
- `reason`: why the handoff is happening
- `query_text`: what the next agent needs to do
- `budget_tokens`: maximum token budget for the capsule
- `context`: the continuity context to hand off from
 
**Output:**
- A `ProofCapsule` (§4)
 
#### `resume()`
 
Accepts a proof capsule and reconstructs working state in a new or existing context. The receiving agent uses the typed registers to orient itself.
 
**Input:**
- A `ProofCapsule` or context identifier
- The receiving agent's identity
 
**Output:**
- Reconstructed continuity state with register labels available for reference
 
#### `read_context()`
 
Returns the current continuity state for a context without performing a handoff. This is the non-proof path — it provides context but does not emit a proof capsule.
 
**Input:**
- `context`: the continuity context to read
 
**Output:**
- Current continuity items, compiled state, active claims, coordination signals, and organism snapshot
 
#### `explain()`
 
Returns a diagnostic view of the continuity state, including compiler status, compiled chunk summaries, and provenance chain.
 
**Input:**
- `context`: the continuity context to explain
 
**Output:**
- Compiler state, compiled bands, recall statistics, and provenance information
 
### 6.2 Proof Path vs. Control Path
 
The protocol explicitly distinguishes:
 
- **`handoff-proof(n)`**: The proof-bearing path where `n` typed registers were emitted via `handoff()`.
- **`read-context-control`**: The non-proof path where context was read but no proof capsule was emitted.
 
Benchmark artifacts and evaluation results MUST label which path was used. This distinction prevents implementations from claiming continuity survival when they only used context availability.
 
---
 
## 7. Retrieval Scoring
 
### 7.1 Multi-Dimensional Scoring
 
When assembling a context pack (a bounded set of items for an agent's prompt), the protocol defines scoring across multiple dimensions:
 
| Dimension | Description |
|-----------|-------------|
| `continuity_kind` | Score boost based on the item's kind (§2.2) |
| `continuity_status` | Score boost based on the item's status (§2.3) |
| `lexical` | Text match against query |
| `vector` | Semantic similarity (implementation-specific) |
| `entity` | Named entity overlap |
| `temporal` | Time-based relevance |
| `recency` | How recently the item was created or updated |
| `salience` | The item's declared importance |
| `lineage` | Graph distance to related items |
| `scope` | Match against the requested visibility boundary |
| `view` | Membership in relevant views |
| `selector` | Match against explicit dimension filters |
 
### 7.2 Context Pack Assembly
 
A context pack is the bounded output of retrieval — the items that will actually appear in an agent's prompt.
 
Assembly rules:
 
1. Candidates are scored across all applicable dimensions.
2. Items are selected in score order until the token budget is exhausted.
3. Duplicate items (same scope key or source event) MUST be deduplicated.
4. Rejected candidates MUST be recorded with explicit reasons.
5. The pack MUST include a manifest with timing breakdown and scoring rationale.
 
---
 
## 8. Evaluation Contract
 
### 8.1 Purpose
 
The protocol includes an evaluation contract because continuity claims are meaningless without measurement. Any implementation claiming protocol compliance SHOULD be evaluable using these criteria.
 
### 8.2 Core Metrics
 
| Metric | Abbreviation | Description |
|--------|-------------|-------------|
| Critical Fact Survival Rate | CFSR | Did critical facts survive the boundary? |
| Constraint Survival Rate | CSR | Did active constraints survive? |
| Decision Lineage Fidelity | DLF | Can the receiving agent explain why a decision was made? |
| Operational Scar Retention | OSR | Did lessons from failures survive? |
| Retrieval Answer Score | RAS | Did the retrieved context actually help answer the task? |
| Memory Pollution Rate | MPR | How much irrelevant or contradictory state leaked through? |
| Hallucination Recovery Time | HRT | How many turns to recover from missing continuity? |
| Duplicate Work Rate | DWR | Did the agent repeat already-completed work? |
| Provenance Coverage | PC | What fraction of emitted items have intact provenance chains? |
 
### 8.3 Evaluation Rules
 
1. The same evaluator MUST be used for both proof-path and control-path runs.
2. Evaluation MUST compare proof-bearing handoff against at least one baseline (e.g., `read-context-control`, `recent-window-only`, `vector-only`).
3. External model providers MUST be evaluated through sealed blind packs — the challenged model sees only the prompt and response schema, not kernel state.
4. Evaluation results MUST record the embedding backend and retrieval protocol fingerprint.
 
---
 
## 9. Plasticity Rules
 
Protocol evolution is plastic, but not whimsical. Any mutation to retrieval, compilation, or evaluation MUST satisfy all of the following before promotion:
 
1. Land as a bounded, isolatable change.
2. Carry a focused proof (benchmark evidence).
3. Survive a focused benchmark rerun.
4. Improve a real product path or keep it stable while improving hygiene.
5. Die immediately if it regresses the target metrics.
 
Rejected ideas stay in version history as operational scars, not as hidden drift.
 
---
 
## 10. Conformance
 
### 10.1 Conformance Levels
 
**Level 1 — Minimal Conformance:**
- Implements typed continuity items with at least: `decision`, `constraint`, `fact`, `operational_scar`, and `working_state`
- Implements `handoff()` emitting a proof capsule with typed registers
- Implements `resume()` accepting a proof capsule
- Labels benchmark artifacts with `handoff-proof(n)` or `read-context-control`
 
**Level 2 — Standard Conformance:**
- All of Level 1
- Implements all 13 continuity kinds
- Implements compiled continuity with hot/warm/cold bands
- Implements `read_context()` and `explain()`
- Implements multi-dimensional retrieval scoring
- Reports evaluation metrics from §8.2
 
**Level 3 — Full Conformance:**
- All of Level 2
- Implements support lineage and provenance tracking
- Implements sealed blind evaluation for external models
- Passes a defined benchmark suite with proof-path scores exceeding control-path scores on CFSR, DLF, and OSR
 
### 10.2 Extensions
 
Implementations MAY extend the protocol with additional continuity kinds, scoring dimensions, or compilation strategies. Extensions MUST NOT break the core contract — typed registers, proof capsules, and the proof/control distinction MUST remain intact.
 
---
 
## Appendix A: Relation to Existing Work
 
The Continuity Protocol builds on ideas from event sourcing, operational incident management, and cognitive architecture research. It differs from existing agent memory approaches in several key ways:
 
- **vs. Rolling summaries** (LangChain ConversationSummaryMemory, etc.): Summaries lose structure. A summary that mentions a decision does not carry the decision's status, rationale, or constraints. The protocol preserves these as typed state.
 
- **vs. Vector retrieval** (RAG-based memory): Vector similarity finds semantically related content but cannot distinguish a resolved decision from an open one, or a fact from a hypothesis. The protocol scores across multiple dimensions, not just embedding distance.
 
- **vs. Knowledge graphs** (Zep/Graphiti): Temporal knowledge graphs excel at factual consistency but don't model operational state like work claims, scars, or handoff semantics. The protocol treats these as first-class citizens.
 
- **vs. Memory compression** (Mem0): Token reduction is valuable but orthogonal. Compressing context doesn't ensure the right state survived — a 90% smaller context that lost the active constraint is worse than a larger one that kept it.
 
---
 
## Appendix B: Glossary
 
| Term | Definition |
|------|------------|
| Continuity | The preservation of meaningful agent state across a context boundary |
| Context boundary | Any event that would cause state loss: window exhaustion, model swap, crash, handoff |
| Continuity item | A typed, durable unit of agent state (decision, constraint, scar, etc.) |
| Proof capsule | The bounded packet of typed registers emitted during handoff |
| Typed register | One labeled item in a proof capsule |
| Compiled continuity | A layered projection of items into hot/warm/cold bands for efficient recall |
| Operational scar | A lesson learned from failure, preserved as the highest-signal continuity kind |
| Context pack | The bounded set of items assembled for an agent's prompt |
| Proof path | A handoff that emitted a proof capsule (`handoff-proof(n)`) |
| Control path | A context read without proof emission (`read-context-control`) |
