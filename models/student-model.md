# Student Model

## Purpose

The Student Model represents the system's current, revisable beliefs about what the learner knows, can do, misunderstands, remembers, and needs next.

It is not a score, personality profile, or permanent judgment. It is an evidence-backed working model used to choose better teaching actions.

## Core principle

> Teaching adapts to the learner only through explicit, revisable evidence.

## Responsibilities

The Student Model stores:

- learner goals and constraints;
- concept-level learning state;
- capability-level evidence;
- inferred mental-model relationships;
- misconceptions and competing hypotheses;
- confidence calibration;
- hint dependence and independence;
- review history and retention signals;
- strategy history and tentative preferences;
- cognitive-load and engagement signals when observable.

It does not store source truth, teacher plans, or generated lesson content.

## Model structure

### Learner profile

Stable or semi-stable information explicitly provided or repeatedly supported:

- goals;
- target depth;
- language;
- deadlines;
- accessibility needs;
- preferred pace;
- relevant prior experience.

Avoid inferring intelligence, fixed learning style, motivation, or diagnosis.

### Concept state

Each concept should carry an explicit state:

- `UNSEEN`
- `DIAGNOSED`
- `INTRODUCED`
- `GUIDED`
- `INDEPENDENT`
- `TRANSFERRED`
- `MASTERED`
- `REVIEW_DUE`
- `FRAGILE`

State transitions require evidence and may be reversed.

### Capability graph

A concept is represented across distinct capabilities rather than one percentage:

- recall;
- explain;
- recognize relationships;
- apply;
- debug or analyze errors;
- transfer;
- teach back;
- retain after delay.

Example:

```yaml
concept: pointer
capabilities:
  recall: strong
  explain: moderate
  apply: weak
  transfer: unknown
  teach_back: unknown
```

### Mental graph

The model records inferred conceptual relationships in the learner's current understanding.

Example expert relation:

```text
memory location -> address -> pointer -> dereference -> stored value
```

Possible learner relation:

```text
pointer -> stored value
```

The mismatch suggests a missing address-location distinction. Mental graph edges must be treated as hypotheses, not facts about the learner.

### Misconception graph

Misconceptions should be represented as structured claims:

```yaml
id: pointer-is-value
concepts: [pointer, address, value]
claim: a pointer directly is the stored value
confidence: medium
supporting_evidence:
  - evidence-104
contradicting_evidence: []
status: active
```

Statuses:

- suspected;
- active;
- repaired-provisionally;
- resolved;
- recurring.

### Confidence model

Confidence should be compared with performance:

- correct and confident;
- correct but uncertain;
- incorrect and uncertain;
- incorrect and confident.

Confidence is useful for calibration but never substitutes for mastery evidence.

### Strategy history

Store instructional strategy outcomes at a local and tentative level:

```yaml
strategy: spatial-analogy
concept_family: memory-models
attempts: 3
observed_outcomes:
  - improved explanation
  - successful transfer
confidence: low
```

Do not conclude that a learner has a permanent style. Strategy effectiveness may vary by topic.

### Review state

Track:

- last successful retrieval;
- last failed retrieval;
- current review priority;
- retention evidence;
- scheduled review;
- concept importance;
- prerequisite centrality.

## Update rules

1. Every update must cite evidence.
2. One answer should not create a permanent learner trait.
3. Strong contradictory evidence should reduce confidence in prior inferences.
4. Correct answers with heavy hints update guided performance, not independent performance.
5. Later failures may move a concept from `MASTERED` to `FRAGILE`.
6. Learner self-report may update confidence or preference, but not mastery by itself.
7. Missing data must remain unknown rather than being guessed.
8. Adaptation should be reversible.

## Suggested record

```yaml
student_id: local-or-anonymous
current_goal: understand pointer semantics
lesson_mode: deep
concepts:
  memory:
    state: mastered
  address:
    state: independent
  pointer:
    state: guided
active_hypotheses:
  - id: pointer-address-confusion
    confidence: 0.68
recent_evidence:
  - evidence-104
cognitive_state:
  overload_risk: low
  fatigue_signal: unknown
```

## Privacy and minimization

Implementations should:

- store only information needed to improve learning;
- let the learner inspect and correct their model;
- avoid sensitive inference;
- support deletion and reset;
- separate learner data from source and public examples;
- avoid exposing private learning records in generated artifacts.

## Failure modes

### Single-score collapse

Problem: a learner is labeled "80%" without showing which capability is weak.

Prevention: capability-level evidence.

### Permanent-label bias

Problem: one response creates a fixed label such as "visual learner."

Prevention: tentative, topic-specific strategy evidence.

### Conversation-as-model

Problem: raw chat history is treated as sufficient state.

Prevention: explicit structured updates.

### Mastery inflation

Problem: assisted answers count as independent ability.

Prevention: record hint dependence.

### Hidden inference

Problem: the system silently assumes motives, intelligence, or background.

Prevention: confidence-tagged hypotheses and learner-editable state.

## Verification criteria

The Student Model is valid when:

- capability differences are preserved;
- every inference links to evidence;
- unknowns remain unknown;
- states can move backward;
- strategy preferences remain tentative;
- misconceptions are represented separately from simple mistakes;
- the learner can inspect or correct the model;
- runtime decisions can explain which model field influenced them.

## Minimal compliance

A minimal implementation must store:

- learner goal;
- concept state;
- recall, explanation, application, and transfer evidence;
- active misconceptions;
- hint dependence;
- review status;
- confidence of inferences.

## Guiding question

> What is the smallest evidence-backed representation of this learner that improves the next teaching decision without turning uncertainty into a label?
