# Continuity Protocol Specification

**Version:** 0.3.0-draft
**Date:** 2026-03-26
**Status:** Draft, updated with metacognitive evaluation evidence and security findings
**Author:** Renato Rodrigues de Araujo  
**License:** CC-BY-4.0

---

## 1. Purpose

The Continuity Protocol defines a contract for preserving meaningful agent state across context boundaries.

A context boundary is any event that would normally break working continuity, including:

- context window exhaustion
- model swap
- agent handoff
- session restart
- crash recovery
- bounded compaction

The protocol exists to answer a practical question:

> What state must survive for work to continue truthfully, efficiently, and without redundant confusion?

This specification is intentionally explicit about epistemic status. It distinguishes:

- **Proven** — behavior already observed in running implementations and validated by tests or benchmarks
- **Theory** — design claims that are coherent and partially shaped by real systems, but not yet fully demonstrated as protocol invariants
- **Assumption** — working hypotheses, expectations, or placeholders that may change as implementations mature

The protocol should be read as a living contract, not as mythology.

---

## 2. Scope

This specification defines:

- the vocabulary for continuity state
- the semantic model for continuity items
- the distinction between proof-bearing and non-proof continuity paths
- the contract for handoff, resume, context reading, and explanation
- the idea of compiled continuity as a bounded recall substrate
- the evaluation posture required to make continuity claims falsifiable
- the epistemic classification of each major claim where practical

This specification does **not** define:

- a required database or storage engine
- a required transport binding
- a required embedding backend
- a required orchestration framework
- a required UI or dashboard model
- authentication, authorization, or tenancy policy

---

## 3. Design Principles

### 3.1 Typed over untyped

Continuity state should be represented as explicit semantic objects, not only as raw transcript or rolling summaries.

**Status:** Proven in current implementations of typed continuity systems.

### 3.2 Proof over availability

There is a meaningful difference between:

- state being merely *available somewhere in storage*, and
- state being *explicitly surfaced as the continuity proof for a boundary crossing*.

A protocol-compliant system should expose that distinction instead of hiding it.

**Status:** Proven.

### 3.3 Bounded over replay-heavy

Continuity must survive under prompt and latency constraints. Systems should prefer bounded, structured carryover over full-history replay.

**Status:** Proven as an engineering necessity; exact mechanisms remain implementation-specific.

### 3.4 Durable over ephemeral

The protocol cares most about state that is expensive to lose: decisions, constraints, facts, incidents, lessons from failure, active ownership, and unresolved pressure.

**Status:** Partly proven, partly theory. The specific ranking of what matters most is still being refined.

### 3.5 Honest over flattering

A system must not claim continuity success just because a human can still recover the truth by digging. If the system required a fallback path, missed typed recall, or repaired itself through a weaker mechanism, that should be representable.

**Status:** Proven as a practical requirement.

### 3.6 Privacy as a continuity constraint

Continuity should preserve the right state across a boundary, not maximize carryover indiscriminately. Scope, minimization, and leakage control are part of continuity quality, not side concerns.

**Status:** Strong theory grounded by practical privacy risk.

---

## 4. Core Insight

The core claim of this protocol should be explicit:

> **Continuity is not the same thing as memory.**
> Memory is stored or recoverable state. Continuity is the operational property that the right state survives a boundary in a truthful, bounded, and inspectable way.

This leads to a second claim:

> **Recoverability is not the same thing as survival.**
> If a system can recover the answer later through a fallback path, that is useful — but it is not equivalent to having preserved the right continuity state at the boundary itself.

And a third:

> **Continuity should be evaluated as a work-preservation property, not as a storage feature.**
> The protocol exists to preserve live work: decisions, constraints, incidents, scars, active ownership, and other state that prevents duplicate effort and operational confusion.

In short:

- **memory** answers: "what can still be retrieved?"
- **continuity** answers: "what actually survived in the form needed for work to continue correctly?"

**Status:** This framing is now strong enough to be treated as the central thesis of the draft.

---

## 5. Reality Map

This section summarizes what appears real today versus what is still theory.

### 5.1 What appears real and already demonstrated

The following capabilities have been demonstrated in at least one serious implementation and should be treated as protocol-grounded reality:

1. **Typed continuity items are workable.**
   Decisions, constraints, facts, incidents, scars, hypotheses, and related states can be stored and retrieved as typed items rather than only as unstructured text.

2. **Proof-bearing handoff is meaningfully different from plain context reading.**
   A system can distinguish between a proof path that emits bounded carryover state and a control path that merely reads context.

3. **Bounded continuity compilation is practical.**
   Dirty-context recompilation, bounded recall bands, and selective surfacing are implementable and useful.

4. **Multi-dimensional retrieval outperforms naive recall framing.**
   Retrieval quality improves when continuity kind, status, scope, lineage, recency, and lexical/semantic relevance all matter rather than just one similarity score.

5. **Raw event memory and typed continuity are not the same thing.**
   Event-backed fallback may recover truth that typed continuity missed; that difference must be visible rather than silently collapsed.

6. **Evaluation must separate proof-path behavior from control-path behavior.**
   If benchmarks do not label which path was used, continuity claims become ambiguous and easy to overstate.

7. **Operational scars and other failure-derived state are high-value continuity material.**
   Losing lessons from prior failures causes repeated mistakes and duplicate work.

8. **Agent/work ownership and coordination pressure are continuity-relevant.**
   Continuity is not only about facts and summaries; live claims, conflicts, and coordination signals also matter.

9. **Per-item survival analysis is implementable and produces actionable classification.**
   A benchmark suite can classify each ground truth item as SURVIVED or LOST after a boundary crossing and extract features (keyword density, memory layer, continuity kind, retention status). This has been demonstrated across 10 benchmark classes producing 300+ survival records per suite run.

10. **Differential decay by continuity kind is practical.**
    Assigning different half-lives to different kinds (e.g. ephemeral working state decaying in hours, operational scars persisting for weeks) produces useful retention behavior. Resolved items decaying faster than open items is a workable and intuitive policy.

11. **Continuity claims can be self-falsified.**
    A system generated a statistical hypothesis about its own survival patterns ("items with higher keyword density survive better"), tested it via A/B injection into the extraction pipeline, and correctly rejected it as a ceiling artefact. The falsification mechanism works. This does not yet prove the mechanism produces net-positive improvements — only that it can avoid false positives.

### 5.2 What is theory but increasingly plausible

The following ideas are promising and aligned with observed systems, but should still be treated as theory rather than settled truth:

1. There is a stable universal core taxonomy that most continuity systems will converge on.
2. A proof capsule can become a broadly portable interoperability primitive across many agent runtimes.
3. Hot/warm/cold compiled continuity will prove to be a widely reusable canonical model.
4. A fixed conformance ladder can be standardized without overfitting to one implementation family.
5. Continuity quality can be captured by a relatively small, stable benchmark metric set.
6. Metacognitive self-evaluation — systems analysing their own forgetting patterns to generate hypotheses about what survives and why — can produce actionable improvements to continuity quality. The mechanism exists and can reject noise; whether it can find real signal remains unproven.
7. Differential half-lives by continuity kind (shorter for ephemeral state, longer for scars and constraints) produce measurably better retention than uniform decay. Implemented and running, but not yet tested across multiple independent implementations.

### 5.3 What remains assumption

The following should be treated as assumptions until broader validation exists:

1. exact weights assigned to different continuity kinds
2. exact status boosts or retrieval coefficients
3. exact register prefix conventions beyond their descriptive usefulness
4. any claim that a particular benchmark suite is sufficient for full protocol validation
5. any claim that a single storage or retrieval architecture is the canonical realization of the protocol
6. whether metacognitive hypothesis generation produces net-positive improvements to continuity metrics (one test correctly rejected one hypothesis — mechanism exists, value unproven, n=1)
7. whether adversarial multi-agent review processes (e.g. tribunal-style code review with integrity auditing) are a scalable continuity quality mechanism or an expensive novelty

---

## 6. Core Vocabulary

### 5.1 Continuity

The preservation of meaningful agent state across a context boundary.

### 5.2 Continuity item

A typed, durable unit of state that may survive a boundary and influence future work.

### 5.3 Context

A named isolation boundary within which continuity state, active claims, and coordination signals coexist.

### 5.4 Proof path

A continuity path where a system emits an explicit bounded carryover object claiming what survived the boundary.

### 5.5 Control path

A continuity path where the system reads or exposes state without emitting proof of boundary carryover.

### 5.6 Compiled continuity

A bounded projection of continuity state optimized for recall rather than raw replay.

### 5.7 Provenance

The recoverable lineage linking surfaced continuity state back to its origin and supporting chain.

### 5.8 Operational scar

A durable lesson derived from failure, conflict, breakage, or expensive confusion.

---

## 7. Continuity Item Model

A continuity item is the fundamental protocol object.

### 6.1 Required semantic fields

A conforming implementation should represent, explicitly or losslessly, at least the following fields:

| Field | Description | Status |
|---|---|---|
| `id` | Stable identifier for the item | Proven |
| `kind` | Semantic kind of item | Proven |
| `status` | Lifecycle status | Proven |
| `title` | Compact human/machine label | Proven |
| `body` | The substantive content | Proven |
| `context` | Isolation boundary | Proven |
| `scope` | Visibility boundary | Proven |
| `created_at` | Creation timestamp | Proven |
| `updated_at` | Last mutation timestamp | Proven |
| `source_agent` | Originating agent, when known | Proven |
| `source_event_id` | Originating event or trace handle, when available | Proven |
| `namespace` | Higher-level isolation partition, when used | Proven |
| `task_id` | Task association, when used | Proven |
| `support_ids` | Supporting lineage references | Proven |
| `salience` | Relative importance signal | Proven |
| `confidence` | Confidence signal | Proven |
| `supersedes_id` | Explicit lineage to the continuity item this item replaces | Proven |

### 6.2 Core continuity kinds

The following kinds are grounded enough to be treated as core protocol vocabulary:

| Kind | Description | Status |
|---|---|---|
| `decision` | A choice that should survive and remain explainable | Proven |
| `constraint` | A boundary that future work must respect | Proven |
| `fact` | A state claim treated as true enough to reuse | Proven |
| `incident` | A disruptive event worth preserving | Proven |
| `operational_scar` | A lesson from failure or conflict | Proven |
| `hypothesis` | A plausible but not yet settled explanation or path | Proven |
| `outcome` | A result of work or decision | Proven |
| `working_state` | Live, ephemeral, lower-trust state | Proven |
| `work_claim` | Ownership over work or a resource | Proven |
| `signal` | Coordination or pressure signal | Proven |
| `summary` | Bounded compacted representation | Theory leaning proven |
| `derivation` | A conclusion derived from other items | Theory leaning proven |
| `lesson` | Generalized learning extracted from work | Theory leaning proven |

### 6.3 Lifecycle status

These statuses are useful and already reflected in real systems:

| Status | Meaning | Status of concept |
|---|---|---|
| `open` | relevant and unresolved | Proven |
| `active` | currently being acted on | Proven |
| `resolved` | completed or addressed | Proven |
| `superseded` | replaced by newer state | Proven |
| `rejected` | explicitly discarded | Proven |

**Important:** exact numeric boosts for these statuses are still assumptions unless backed by benchmark evidence.

### 6.4 Scope

Scope determines who may see or reuse continuity state.

Useful core scopes include:

- `agent`
- `session`
- `shared`
- `project`
- `global`

**Status:** Proven as a useful pattern, though exact scope semantics remain binding-specific.

### 6.5 Supersession Lineage

Supersession is a first-class continuity transition, not an implementation detail.

- When a newer item replaces an older one, the newer item MAY declare `supersedes_id`.
- Read-oriented projections SHOULD expose `superseded_by` when it is cheaply derivable so agents can detect stale state without a second round trip.
- Compact read projections MUST preserve enough supersession information to distinguish "still active" from "shown only for lineage."


---

## 8. Context Model

A continuity context is the protocol-level container for continuity-relevant state.

A context may include:

- continuity items
- active attachments
- active work claims
- coordination signals
- compiled continuity projections
- provenance and explanation state

### 7.1 What is real

It is already real that continuity systems need some isolation boundary stronger than “whatever is nearby in storage”.

### 7.2 What remains open

The protocol does not yet standardize:

- whether contexts are hierarchical
- whether machine/project/task decomposition is canonical
- how cross-context bridging should be encoded

These are still implementation choices.

---

## 9. Proof Capsule

### 8.1 Definition

A proof capsule is a bounded payload emitted during a proof path. It is the protocol’s explicit answer to:

> What continuity state is being claimed as surviving this boundary?

**Status:** Proven as a useful and implementable concept.

### 8.2 Required semantic properties

A proof capsule must be:

1. **bounded** — it cannot be an unbounded transcript dump
2. **typed** — its contents must preserve semantic distinctions
3. **traceable** — surfaced state should retain provenance where available
4. **honest** — it should not pretend replay or control-path access was proof-path carryover
5. **deduplicated enough** — repeated echoes should be collapsed where possible

### 8.3 Typed registers

A proof capsule may encode its contents as typed registers.

Representative register classes include:

- fact register
- decision register
- constraint register
- scar register
- incident register
- hypothesis register
- next-step register

**Status:** Proven in spirit; exact register naming conventions are still assumptions.

### 8.4 Register labeling

Labels such as `pf1`, `pd1`, `pk1`, `ps1`, `pn1` are useful conventions, but they are not yet protocol law.

**Status:** Assumption / convenience convention.

---

## 10. Proof Path vs Control Path

This distinction is now central and should be treated as normative.

### 9.1 Proof path

A proof path emits an explicit continuity artifact describing what survived a boundary.

Examples:

- handoff with typed carryover
- resume using bounded registers
- benchmark run explicitly marked as proof-bearing

### 9.2 Control path

A control path reads context or state without asserting that the boundary was survived through explicit carryover.

Examples:

- direct context read
- operator inspection
- replay or fallback retrieval without handoff proof

### 9.3 Normative requirement

Any benchmark, report, or product surface that claims continuity quality **should label which path was used**.

Without this distinction, systems will overclaim continuity simply because the answer was still recoverable somewhere.

**Status:** Proven and should be considered normative.

---

## 11. Compiled Continuity

### 10.1 Definition

Compiled continuity is a bounded projection of raw state into a form optimized for fast, truthful recall.

### 10.2 What is proven

The following are already grounded in reality:

- dirty contexts can be recompiled lazily
- not every mutation should trigger a full rebuild
- bounded projections outperform transcript replay for many continuity tasks
- support lineage can influence rank and surfacing
- duplicate or low-value echoes need suppression

### 10.3 Common band model

The hot / warm / cold band model is a strong working abstraction:

| Band | Typical role | Status |
|---|---|---|
| `hot` | open, active, high-salience state | Proven |
| `warm` | recently resolved or supporting state | Proven |
| `cold` | lower-priority, superseded, archival state | Proven |

The exact thresholds, promotion rules, and eviction policy remain implementation-specific.

### 10.4 Important non-goal

Compiled continuity is **not** just lossy summarization. It is structured state projection.

**Status:** Proven as a crucial conceptual distinction.

---

## 12. Retrieval Model

### 11.1 Principle

Continuity retrieval should be multi-dimensional, not monocausal.

A useful retrieval system may consider:

- kind
- status
- scope
- practice_state
- support_signal
- lexical match
- semantic similarity
- lineage proximity
- temporal relevance
- recency
- salience
- selector or view membership
- task / namespace affinity

### 11.2 What is proven

The following are proven enough to influence the spec:

1. typed continuity retrieval should not be reduced to vector similarity alone
2. same-task and same-namespace distinctions materially affect retrieval quality
3. fallback retrieval should report when it came from fallback rather than typed continuity
4. provenance-aware suppression and de-duplication improve continuity truthfulness

### 11.3 What remains assumption

Exact score weights are not standardized by this spec.

This specification describes retrieval dimensions, not a mandatory formula.

---

## 13. Event Memory vs Typed Continuity

This distinction has become too important to leave implicit.

### 12.1 Typed continuity

Typed continuity is state intentionally promoted or represented as protocol-native semantic objects.

### 12.2 Event memory

Event memory is raw or lightly structured historical record that may still be truthful and useful even when it was not promoted into typed continuity.

### 12.3 Normative implication

If a system answers from event fallback rather than typed continuity, it should be able to say so.

This avoids the lie where continuity appears stronger than it really was.

**Status:** Proven.

### 12.4 Open question

Whether and when raw event memory should be auto-promoted into typed continuity remains open.

**Status:** Theory / assumption depending on strategy.

---

## 14. Coordination State as Continuity

The protocol should explicitly recognize that continuity is not only epistemic memory.

It also includes live coordination state such as:

- work ownership
- collisions and conflicts
- agent attachment or presence
- pressure signals
- targeted review or escalation state
- lane- or resource-specific contention

### 13.1 Why this matters

A system that preserves facts but forgets who is already working on what still fails continuity in practice.

### 13.2 Status

This claim is strongly proven by modern multi-agent and multi-worker systems.

What remains open is how much of that coordination surface should be standardized by the core protocol versus optional extensions.

---

## 15. Explanation and Observability

A continuity system should not only surface state; it should also help explain:

- why state was surfaced
- which compilation path produced it
- whether provenance survived
- whether the answer came from proof-path carryover, typed recall, or fallback memory
- what was excluded and why, where practical

**Status:** Proven as necessary for operator trust.

Observability formats are implementation-specific, but explanation capability should be considered part of serious protocol conformance.

### 15.1 Current Practice Projection

The protocol should treat active operating guidance as a derived projection rather than a raw list of recent decisions.

- `current_practice` summarizes the active operating guidance for a context or task.
- The projection SHOULD retain `items` plus a compact evidence bundle for each live practice item, including the supporting lessons, outcomes, or decisions that justify it.
- Each evidence bundle SHOULD expose at least `support_signal`, `evidence_count`, and the supporting continuity items themselves.
- Guidance-like items that remain `open` or `active` but age past a retirement horizon without recent reinforcement SHOULD stop counting as live operating pressure in default reads.
- Historical guidance MUST remain available for provenance and learning-line reconstruction. The protocol may not fake recency by deleting lineage.
- When the objective explicitly asks for history, timeline, lineage, evolution, or why a practice changed, implementations SHOULD relax stale-guidance suppression so the full operating line can be reconstructed.

### 15.2 Learning Projection

The protocol should also expose learning as a first-class projection derived from continuity.

- The default learning view SHOULD surface the newest learnings first so an agent can answer "what did we learn recently?" without reconstructing chronology manually.
- The learning view MUST support a lineage mode. When the objective explicitly asks for history, timeline, evolution, lineage, or "what we learned over time", implementations SHOULD return the full learning line in chronological order.
- The learning view SHOULD be derived from continuity kinds that represent actual adaptation pressure, especially `lesson`, `outcome`, `decision`, `incident`, and `operational_scar`.
- The lineage learning view SHOULD preserve sequence. It MUST help an agent explain how current practice emerged from earlier mistakes, decisions, outcomes, and lessons.

### 15.3 Control Surfaces

The protocol's control surfaces should expose current practice and learning without duplicating the same payload in multiple forms.

- `read_context(context, detail)` SHOULD return current continuity items, compiled state, `current_practice`, `learning`, active claims, coordination signals, and organism snapshot.
- `recall(context, query_text, budget_tokens, detail)` SHOULD return ranked continuity hits, `answer_hint`, candidate counts, compiler metadata, and retrieval timings.
- `compact` SHOULD be the default detail mode for read-heavy bindings, while `full` is reserved for inspection and diagnostics.
- Compact projections MUST preserve `answer_hint`, `answer_hint_item_id`, item identity, ranking order, `support_count`, and `superseded_by` where present.
- Human-readable text SHOULD stay short and should not repeat the full structured payload.


---

## 16. Evaluation Contract

Continuity without falsification is branding.

A serious implementation should support evaluation that can distinguish:

- proof path vs control path
- typed continuity recall vs fallback memory
- useful survival vs noisy survival
- truthful carryover vs flattering carryover

### 15.1 Useful metric families

The following metric families are real enough to keep in the spec:

| Metric family | Meaning | Status |
|---|---|---|
| Critical fact survival | essential facts survive boundary | Proven |
| Constraint survival | active constraints survive | Proven |
| Decision lineage fidelity | receiver can explain why a choice exists | Proven |
| Scar retention | failure lessons survive | Proven |
| Pollution / contradiction rate | irrelevant or conflicting state leaks through | Proven |
| Duplicate work rate | already-done work gets repeated | Proven |
| Provenance coverage | surfaced state remains traceable | Proven |
| Recovery latency | turns required to recover from continuity failure | Theory leaning proven |

### 15.2 Benchmark posture

A serious benchmark should:

1. compare proof path against at least one control path
2. use the same evaluator posture across conditions
3. record retrieval/protocol fingerprinting sufficient for reproducibility
4. avoid rewarding systems for hidden access to internal state that the challenged agent did not actually receive

**Status:** Proven in principle.

### 15.3 Survival analysis as evaluation

A benchmark suite can go beyond pass/fail by classifying each item as SURVIVED or LOST and extracting features about what correlates with survival. This produces a `SurvivalReport` — a per-item, per-boundary record of what the system kept and what it dropped.

This has been demonstrated in practice:

- 10 benchmark classes covering agent swap, strong-to-small continuation, crash recovery, memory pollution, and more
- 300+ survival records per suite run with extracted features (keyword density, file path presence, continuity kind, memory layer, retention status)
- statistical hypothesis generation via chi-squared proportion tests with FDR correction
- one closed-loop A/B test that correctly rejected a hypothesis as a ceiling artefact

**What this does NOT prove (yet):**

- That metacognitive hypothesis generation produces net-positive improvements to continuity. The mechanism can avoid false positives, but no hypothesis has yet been validated as an improvement.
- That the features extracted are the right features. The feature set is still implementation-local.
- That the statistical tests are appropriate for small sample sizes. Chi-squared with Yates correction is unreliable for sparse cells (expected count < 5); Fisher's exact test is more appropriate but not yet standard.

**Kill criterion from reference implementation:** If after 5 closed-loop cycles no survival metric improves by > 5% absolute, the patterns are noise.

**Status:** The survival analysis mechanism is proven. The metacognitive closed loop is theory under active testing. The claim that it produces value is still assumption.

### 15.4 Blind evaluation

Sealed or blind evaluation for external models is a strong protocol-aligned practice, but should still be treated as a higher-maturity feature rather than a minimal requirement.

**Status:** Theory leaning proven.

---

## 17. Conformance Levels

These levels are useful, but should be read as draft guidance rather than permanent law.

### Level 1 — Core semantic conformance

A Level 1 system should:

- represent typed continuity items
- preserve at least decisions, constraints, facts, operational scars, and working state
- expose an explicit proof-bearing handoff path
- expose a distinguishable non-proof control path

**Status:** Reasonable and grounded.

### Level 2 — Bounded continuity conformance

A Level 2 system should additionally:

- support compiled continuity or equivalent bounded recall projection
- support explanation / provenance surfaces
- support multi-dimensional retrieval
- expose `current_practice` and `learning` projections, including default and lineage modes
- support evaluation labeling for proof vs control paths

**Status:** Reasonable and grounded.

### Level 3 — High-assurance continuity conformance

A Level 3 system should additionally:

- preserve support lineage and provenance robustly
- distinguish typed continuity from fallback memory truthfully
- support benchmark evidence showing proof-path advantage over control-path baselines on key metrics

**Status:** Strong theory grounded by emerging evidence.

---

## 18. Non-Goals and Anti-Claims

The Continuity Protocol does **not** claim that:

1. continuity can be solved by one perfect summary
2. vector memory alone is enough
3. a larger context window eliminates the need for continuity structure
4. all useful state should be treated equally
5. any implementation with storage automatically has continuity
6. replay and proof are the same thing
7. continuity state fed back into agent prompts is automatically safe (see Section 20)

These anti-claims are now grounded enough to be considered part of the protocol’s identity.

---

## 19. Open Questions

The following are intentionally unresolved:

1. What is the smallest stable universal taxonomy of continuity kinds?
2. How should cross-context and cross-project bridging be standardized?
3. Which parts of coordination state belong in the core protocol versus extension specs?
4. What is the best portable encoding for proof capsules across bindings?
5. When should event memory be promoted into typed continuity?
6. Which benchmark suite is sufficient for claiming broad protocol maturity?
7. How much should numeric scoring be standardized versus left implementation-local?
8. Can systems that analyse their own forgetting patterns (metacognitive survival analysis) produce measurable improvements to continuity quality, or is the signal too weak relative to noise?
9. What are the security surfaces created by feeding continuity state back into agent extraction prompts? (See Section 20.)
10. What is the right statistical test for survival analysis with small, sparse samples — and how many closed-loop cycles are needed to distinguish signal from noise?
11. Should differential decay rates (by continuity kind and lifecycle status) be part of the protocol, or remain implementation-local?

---

## 20. Security: Continuity-in-the-Loop

When continuity state is fed back into agent prompts — whether as context, extraction hints, or metacognitive guidance — it creates a novel attack surface.

### 20.1 Prompt injection via continuity items

A concrete finding from a reference implementation (2026-03-26): functions that render hypothesis text into extraction prompts concatenated continuity item content directly into the prompt without sanitization. Any agent that writes a continuity item controls part of the extraction prompt for future agents.

This means:

- a malicious or confused agent can craft continuity items that manipulate downstream extraction behaviour
- hypothesis text, lesson content, or scar descriptions can contain prompt injection payloads
- the attack is subtle because it operates through the memory layer, not through direct user input

### 20.2 Normative implication

A serious continuity system that feeds state back into agent prompts should:

1. treat continuity item content as untrusted input at the prompt boundary
2. sanitize or structurally isolate continuity content from prompt control flow
3. not assume that items written by previous agents are benign

### 20.3 Status

This is a **proven security concern**, discovered during adversarial review of a closed-loop metacognitive implementation. It is not theoretical — the vulnerable code was found in production-path functions.

The protocol does not prescribe a specific sanitization mechanism, but it should be explicit that continuity-in-the-loop introduces prompt injection risk that must be addressed by conforming implementations.

---

## 21. Practical Reading Guide

If you are implementing against this spec today, treat the following as the safest practical core:

### Build this as real

- typed continuity items
- explicit lifecycle status
- explicit proof path vs control path labeling
- bounded carryover / proof capsule behavior
- compiled or otherwise bounded recall
- multi-dimensional retrieval
- provenance-aware explanation
- truthful distinction between typed continuity and fallback memory
- per-item survival analysis in benchmark evaluation
- differential decay by continuity kind (longer for scars, shorter for ephemeral state)
- sanitization of continuity content before prompt injection (Section 20)

### Treat this as strong theory

- universal register conventions
- exact conformance ladder semantics
- stable canonical benchmark suite
- exact hot/warm/cold policy portability across systems
- metacognitive hypothesis generation from survival patterns (mechanism works, value unproven)

### Treat this as assumption

- exact score weights
- exact field optionality for every binding
- exact numeric thresholds for salience, status boosts, or promotion
- whether closed-loop metacognitive validation produces net-positive continuity improvements

---

## Appendix A — Suggested Minimal JSON Shape

This appendix is illustrative, not normative.

```json
{
  "id": "uuid-v7",
  "context": "machine/project/task",
  "kind": "decision",
  "status": "open",
  "title": "Prefer bounded proof over replay",
  "body": "The system should emit typed carryover instead of relying on transcript replay.",
  "scope": "shared",
  "source_agent": "planner",
  "source_event_id": "evt_123",
  "namespace": "default",
  "task_id": "continuity-spec",
  "support_ids": ["uuid-v7-2"],
  "salience": 0.91,
  "confidence": 0.87,
  "created_at": "2026-03-25T12:00:00Z",
  "updated_at": "2026-03-25T12:00:00Z"
}
```

---

## Appendix B — Suggested Proof Capsule Shape

Illustrative only.

```json
{
  "source_context": "machine/project/task",
  "emitted_at": "2026-03-25T12:00:00Z",
  "registers": [
    {
      "label": "pd1",
      "register_kind": "decision",
      "title": "Prefer bounded proof over replay",
      "body": "The system should emit typed carryover instead of relying on transcript replay.",
      "source_id": "uuid-v7",
      "has_provenance": true
    }
  ]
}
```

---

## Appendix C — Glossary

| Term | Meaning |
|---|---|
| Continuity | survival of meaningful agent state across a boundary |
| Context boundary | an event that would otherwise break working state |
| Continuity item | a typed durable state unit |
| Proof path | explicit bounded carryover path |
| Control path | non-proof context access path |
| Proof capsule | bounded proof-bearing continuity payload |
| Compiled continuity | bounded projected recall substrate |
| Provenance | origin and support lineage of surfaced state |
| Operational scar | durable lesson from failure |
| Event fallback | truthful recovery from non-typed memory when typed continuity misses |
| Survival analysis | per-item classification of what survived vs was lost across a boundary |
| Metacognitive evaluation | a system analysing its own forgetting patterns to generate hypotheses about continuity |
| Differential decay | varying retention half-lives by continuity kind and lifecycle status |
| Continuity-in-the-loop | feeding continuity state back into agent prompts as context or extraction guidance |
