# Hypotheses and Validation Map

This document separates what is strongly validated from what is still being tested.

## 1. Strongly validated claims

These claims have enough implementation and evaluation grounding to be treated as strong protocol premises:

1. typed continuity is more truthful than pure summary-only carryover
2. proof-bearing handoff is materially different from plain context access
3. bounded continuity artifacts are necessary in real systems
4. multi-dimensional retrieval beats single-signal retrieval framing
5. fallback memory should be labeled honestly when it rescues recall
6. coordination state is part of continuity in multi-agent systems

## 2. Good hypotheses still under validation

These are strong hypotheses with real support, but not yet protocol law:

1. a stable universal continuity taxonomy will emerge
2. hot/warm/cold style compilation will generalize well across implementations
3. proof capsules can become a portable interoperability primitive
4. a compact metric family will be sufficient to evaluate continuity quality broadly
5. continuity conformance levels can stabilize without overfitting

## 3. Weakly settled / implementation-local areas

These should remain flexible:

- exact retrieval weights
- exact register naming
- exact promotion thresholds
- exact salience formulas
- exact lifecycle scoring coefficients

## 4. Recent evidence (2026-03-26)

The following observations come from a reference implementation running 10 benchmark classes with 60 runs per suite:

### What was demonstrated

1. **Per-item survival analysis produces usable data.** 300+ survival records per run, each classified as SURVIVED or LOST with extracted features (keyword density, file path presence, continuity kind, memory layer, retention status).

2. **Statistical hypothesis generation works mechanically.** Chi-squared proportion tests with Benjamini-Hochberg FDR correction can identify candidate patterns in survival data.

3. **One closed-loop A/B test correctly rejected a false hypothesis.** The system generated "items with higher keyword density survive better", injected it as an extraction hint in a treatment arm, and rejected it as a ceiling artefact. The falsification mechanism works.

4. **Differential decay is running and intuitive.** OperationalScar at 720h half-life vs WorkingState at 18h, with resolved items decaying 3.3x faster. This produces behaviour operators describe as natural.

5. **Prompt injection via continuity items is a real attack surface.** Functions that render hypothesis text into extraction prompts were found to concatenate content without sanitization. This was discovered during adversarial multi-agent review.

### What was NOT demonstrated

1. That metacognitive hypothesis generation produces net-positive improvements to continuity. The mechanism can reject noise, but no hypothesis has been validated as an improvement. n=1.

2. That the extracted features are the right features. The feature set is implementation-local and untested for generality.

3. That chi-squared is the right test for these sample sizes. Reviewers flagged that Fisher's exact test is more appropriate for sparse cells.

4. That adversarial multi-agent review scales. One tribunal found real bugs (dead code, prompt injection, confound control issues), but this is an anecdote, not evidence of a scalable process.

### Honest summary

The survival analysis and hypothesis generation mechanisms exist and run. The falsification mechanism works (it can reject noise). But the core value claim — that the system can improve its own continuity by learning from its own forgetting patterns — is unproven. It remains theory under active testing with a kill criterion: 5 closed-loop cycles without > 5% absolute improvement means the patterns are noise.

## 5. Validation posture

A good validation program should keep asking:

- did the right state survive?
- did the system prove that it survived?
- did fallback rescue a miss?
- was the result truthful rather than flattering?
- did the preserved state reduce duplicate work, confusion, or recovery time?
- did the system correctly reject hypotheses that were noise? (new)
- did feeding continuity state back into prompts create security risk? (new)
