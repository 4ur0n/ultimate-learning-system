# Evidence Model

## Purpose

The Evidence Model represents observations that support, weaken, or leave unresolved claims about learning.

It is the system's auditable record of what happened, under which conditions, what capability was exercised, how much support was provided, and which Student Model or Teacher Model claims may be updated as a result.

The Evidence Model does not equal a score. It preserves the structure and uncertainty needed to make justified teaching decisions.

## Core principle

> Assessment is evidence collection, not score production.

## Responsibilities

The Evidence Model stores:

- learner responses and observable actions;
- the task and capability being assessed;
- correctness, completeness, and reasoning quality;
- confidence and calibration signals;
- hint, scaffold, and tool usage;
- latency, attempt count, and revision history when useful;
- transfer distance and context novelty;
- evaluator judgment and uncertainty;
- links to concepts, misconceptions, hypotheses, and decisions;
- provenance of automated or human evaluation;
- contradictions among observations;
- temporal decay and review relevance.

It does not store source truth, permanent learner labels, or hidden instructional plans.

## Evidence unit

An evidence unit is one bounded observation.

```yaml
id: evidence-104
timestamp: 2026-07-28T08:17:00Z
learner_id: local-or-anonymous
activity_id: pointer-check-03
concepts:
  - pointer
  - address
capability: explain
prompt: Explain the difference between `p` and `*p`.
response: p is the address and *p is the value at that address.
outcome:
  correctness: correct
  completeness: partial
  reasoning_quality: adequate
support:
  hints_used: 1
  scaffold_level: moderate
  answer_exposure: none
confidence_report:
  value: 0.55
context:
  mode: guided-practice
  representation: code-plus-svg
  transfer_distance: near
evaluator:
  type: model-assisted
  confidence: 0.82
claims_supported:
  - student:pointer:explain:guided
claims_weakened:
  - misconception:pointer-is-value
source_refs:
  - decision-42
```

## Evidence dimensions

### Correctness

Whether the response reaches the expected conclusion.

Suggested values:

- correct;
- partially-correct;
- incorrect;
- indeterminate;
- not-attempted.

Correctness alone is insufficient.

### Completeness

Whether all essential parts of the objective were addressed.

A learner may be correct but incomplete.

### Reasoning quality

Possible dimensions:

- identifies relevant principles;
- connects steps coherently;
- distinguishes cause from correlation;
- justifies the chosen procedure;
- notices constraints and exceptions;
- detects contradictions;
- explains rather than repeats wording.

### Independence

Records how much support was needed.

Suggested levels:

- copied;
- fully-guided;
- partially-guided;
- prompted;
- independent;
- self-corrected-independent.

A guided correct answer is evidence of guided capability, not independent mastery.

### Confidence

Learner-reported confidence should be stored separately from evaluator confidence.

Confidence calibration patterns:

- correct and confident;
- correct but uncertain;
- incorrect and uncertain;
- incorrect and confident.

High-confidence errors may indicate a misconception. Low-confidence correct answers may indicate fragile knowledge or poor calibration.

### Transfer distance

Suggested values:

- same-item;
- same-structure;
- near-transfer;
- far-transfer;
- novel-domain.

Transfer evidence must preserve how different the new context was from instruction.

### Retention interval

Immediate success and delayed retrieval are different forms of evidence.

Record:

- elapsed time since instruction;
- elapsed time since previous successful retrieval;
- whether the learner had access to notes;
- whether the item was previously seen.

### Error structure

Do not record only that an answer was wrong. Record the shape of the error when observable.

Examples:

- omitted prerequisite;
- reversed causal direction;
- confused representation with object;
- applied correct procedure under wrong conditions;
- arithmetic slip;
- vocabulary ambiguity;
- copied surface pattern;
- unsupported guess;
- misconception-consistent reasoning.

## Evidence sources

Evidence may come from:

- diagnostic questions;
- learner explanations;
- worked-example completion;
- multiple-choice responses;
- free-response answers;
- code execution or debugging;
- simulations;
- diagram interpretation;
- prediction tasks;
- teach-back;
- transfer tasks;
- delayed retrieval;
- learner self-report;
- human review;
- external assessment results.

The source type affects evidential strength.

## Evidence strength

Evidence strength should be multidimensional.

Recommended factors:

- alignment with target capability;
- independence;
- task difficulty;
- transfer distance;
- response quality;
- evaluator confidence;
- opportunity for guessing;
- recency;
- repetition across varied contexts;
- consistency with other evidence.

A conceptual representation:

```text
evidence_strength =
  capability_alignment
  × independence
  × task_validity
  × evaluator_confidence
  × context_value
```

This is a reasoning framework, not a universal psychometric formula.

## Claim linkage

Evidence should update explicit claims rather than directly overwriting the Student Model.

Example claims:

```text
student:pointer:recall:independent
student:pointer:explain:guided
student:pointer:apply:independent
student:pointer:transfer:far
student:misconception:pointer-is-value
teacher:hypothesis:pointer-address-confusion
teacher:strategy:contrastive-svg-effective
```

An evidence unit may:

- support a claim;
- weaken a claim;
- contradict a claim;
- be irrelevant to a claim;
- remain ambiguous.

## Aggregation

Aggregated judgments must remain traceable to underlying evidence.

Example:

```yaml
claim: student:pointer:explain:independent
status: provisionally-supported
confidence: 0.73
supporting_evidence:
  - evidence-130
  - evidence-141
contradicting_evidence:
  - evidence-126
coverage:
  contexts: 2
  representations: 2
  delayed_retrieval: false
last_updated: 2026-07-28T09:00:00Z
```

Aggregation must not erase:

- assistance level;
- contradictory evidence;
- task context;
- timing;
- evaluator uncertainty.

## Capability-specific evidence

### Recall

Valid evidence:

- retrieve a term, fact, rule, or step without exposure.

Weak substitutes:

- recognizing the answer after seeing it;
- copying from notes.

### Explain

Valid evidence:

- describe the relationship in original language;
- answer why or how questions;
- distinguish related concepts;
- justify an example.

Weak substitutes:

- repeating the teacher's sentence;
- giving only a definition when a mechanism is requested.

### Apply

Valid evidence:

- select and execute the concept in a suitable task;
- explain why it applies;
- handle relevant constraints.

### Debug or analyze

Valid evidence:

- locate the relevant fault;
- explain why it fails;
- propose and justify a correction.

### Transfer

Valid evidence:

- apply the relationship in a meaningfully changed context;
- identify invariants despite surface differences.

### Teach back

Valid evidence:

- construct a coherent explanation;
- choose a useful example;
- anticipate a likely confusion;
- answer a follow-up question.

Teaching back is strong evidence but not mandatory for every concept.

### Retention

Valid evidence:

- successful retrieval after a meaningful delay;
- successful reapplication without re-teaching.

## Misconception evidence

A single wrong answer does not automatically establish a misconception.

Misconception evidence is stronger when:

- the same incorrect relationship appears repeatedly;
- reasoning is internally coherent but structurally wrong;
- the error persists across representations;
- confidence is high;
- a targeted counterexample produces predictable failure;
- the response matches a known misconception pattern.

Suggested statuses:

- weak signal;
- suspected;
- supported;
- strongly supported;
- weakened;
- repaired provisionally;
- no longer supported;
- recurring.

## Negative evidence

Absence of success is not always evidence of absence.

A failed answer may result from:

- unclear wording;
- language burden;
- excessive cognitive load;
- unfamiliar notation;
- fatigue;
- missing prerequisite;
- tool failure;
- accidental slip;
- lack of motivation to answer fully.

Negative evidence should be interpreted using the Teacher Model's competing hypotheses.

## Contradictory evidence

Contradictions are expected.

When evidence conflicts:

1. preserve all relevant observations;
2. compare capability alignment and independence;
3. compare task difficulty and context;
4. inspect recency and retention interval;
5. inspect evaluator confidence;
6. reduce aggregate confidence when unresolved;
7. collect a discriminating observation.

Do not silently average incompatible evidence into a misleading score.

## Evaluator model

Every evaluation should record who or what judged the response.

Possible evaluator types:

- deterministic checker;
- rubric-based model;
- human reviewer;
- test suite;
- external platform;
- learner self-assessment;
- hybrid.

Recommended fields:

```yaml
evaluator:
  type: rubric-based-model
  rubric_version: explain-v0.1
  confidence: 0.82
  uncertainty_reason: answer is correct but omits type semantics
  review_required: false
```

Evaluation uncertainty must not be hidden.

## Rubric requirements

A rubric should specify:

- target capability;
- essential elements;
- acceptable variation;
- common partial answers;
- misconception indicators;
- independence requirements;
- scoring or classification rules;
- conditions that require human review.

Rubrics should evaluate the target relationship, not superficial wording similarity.

## Evidence lifecycle

Recommended lifecycle:

- `CAPTURED`
- `VALIDATED`
- `LINKED`
- `AGGREGATED`
- `SUPERSEDED`
- `RETRACTED`
- `ARCHIVED`

Evidence may be retracted when:

- the task was invalid;
- the answer was exposed;
- the evaluator was wrong;
- the record was assigned to the wrong learner;
- a technical failure corrupted the activity.

Retraction must be recorded rather than deleting history silently.

## Temporal behavior

Evidence value changes with time.

- Recent evidence is useful for current state.
- Delayed evidence is stronger for retention.
- Old evidence may still matter for recurring misconceptions.
- Strategy effectiveness may become stale as learner expertise changes.

The system should apply temporal decay cautiously and preserve raw evidence.

## Review priority

Review scheduling may use:

- concept importance;
- prerequisite centrality;
- evidence fragility;
- time since retrieval;
- history of recurrence;
- transfer weakness;
- learner deadline;
- confidence calibration.

Review priority is derived from evidence; it is not itself evidence.

## Provenance and auditability

Each record should preserve:

- activity identifier;
- exact prompt or task version;
- response or output reference;
- timestamp;
- model or human evaluator;
- rubric version;
- intervention or decision reference;
- source concept version;
- transformations applied to the raw response.

This allows later reconstruction of why a mastery or misconception claim changed.

## Privacy and minimization

Implementations should:

- collect only evidence useful for learning;
- avoid inferring sensitive traits;
- let learners inspect and challenge records;
- support deletion and reset;
- separate raw responses from public examples;
- avoid unnecessary biometric or behavioral surveillance;
- make retention policy explicit.

Latency, emotion, or engagement data should not be collected merely because it is technically possible.

## Suggested schema

```yaml
evidence_model:
  version: 0.1
  evidence:
    evidence-104:
      activity_id: pointer-check-03
      timestamp: 2026-07-28T08:17:00Z
      concepts: [pointer, address]
      capability: explain
      outcome:
        correctness: correct
        completeness: partial
        reasoning_quality: adequate
      independence: partially-guided
      confidence_report: 0.55
      transfer_distance: same-structure
      evaluator:
        type: rubric-based-model
        confidence: 0.82
      supports:
        - student:pointer:explain:guided
      weakens:
        - student:misconception:pointer-is-value
      decision_ref: decision-42
  claims:
    student:pointer:explain:guided:
      status: supported
      confidence: 0.79
      supporting_evidence: [evidence-104]
      contradicting_evidence: []
```

## Update rules

1. Every aggregate claim must link to raw evidence.
2. Correctness must remain separate from independence.
3. Immediate performance must remain separate from retention.
4. Recognition must not update explanation or application without supporting evidence.
5. Assisted performance must not update independent capability as if unassisted.
6. Contradictory evidence must be preserved.
7. Evaluator uncertainty must propagate into claim confidence.
8. One error must not establish a misconception without structural support.
9. Invalid activities must be retracted explicitly.
10. Runtime components must not create hidden scores outside this model.
11. Learner self-report may inform confidence and experience, but not mastery alone.
12. Evidence should be collected only when it can influence a meaningful decision.

## Failure modes

### Score collapse

Problem: all evidence becomes one percentage.

Prevention: capability, context, independence, and timing remain separate.

### Correctness-only assessment

Problem: a correct answer is treated as full understanding.

Prevention: record completeness, reasoning, transfer, and support.

### Hint blindness

Problem: assisted success is counted as independent mastery.

Prevention: explicit scaffold and answer-exposure fields.

### Misconception overdiagnosis

Problem: one mistake becomes a structural learner belief.

Prevention: repeated and discriminating evidence requirements.

### Evidence laundering

Problem: an uncertain automated judgment becomes a confident Student Model update.

Prevention: evaluator confidence propagation.

### Context erasure

Problem: near-transfer and far-transfer success are treated identically.

Prevention: transfer-distance field.

### Retention illusion

Problem: immediate repetition is treated as durable learning.

Prevention: elapsed-time and previous-exposure fields.

### Hidden assessment

Problem: the runtime changes mastery without an auditable observation.

Prevention: evidence-only model updates.

### Surveillance inflation

Problem: every observable behavior is collected regardless of educational value.

Prevention: data minimization and decision relevance.

## Verification criteria

The Evidence Model is valid when:

- every Student Model change can be traced to evidence;
- correctness and independence remain separate;
- capability alignment is explicit;
- contradictions are preserved;
- evaluator uncertainty is visible;
- misconception claims require structural evidence;
- immediate and delayed evidence are distinguishable;
- transfer distance is represented;
- invalid evidence can be retracted;
- review decisions can explain which evidence made a concept fragile or due;
- no runtime maintains an unaudited mastery score.

## Minimal compliance

A minimal implementation must store:

- evidence identifier and timestamp;
- task and response reference;
- target concept and capability;
- correctness and completeness;
- independence and hint usage;
- learner confidence when available;
- evaluator type and confidence;
- claim links;
- transfer distance;
- retention interval;
- contradiction and retraction status.

## Model relationships

```text
Knowledge Model
  defines concepts, capabilities, and misconception patterns
        ↓
Teacher Model
  selects an intervention and expected evidence
        ↓
Evidence Model
  records the learner's observable response
        ↓
Student Model
  updates evidence-backed learner claims
        ↓
Teacher Model
  revises hypotheses and selects the next action
```

The Evidence Model is the only valid bridge from observation to learner-state change.

## Guiding question

> What exactly did the learner demonstrate, under what conditions, and how strongly should that observation change our beliefs about what they can do next?
