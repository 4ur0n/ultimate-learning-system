# Teacher Model

## Purpose

The Teacher Model represents the system's current instructional state: what it is trying to achieve, what it believes may explain the learner's behavior, which intervention it has selected, what outcome it predicts, and how confident it is in that decision.

It is the model of pedagogical cognition.

The Teacher Model does not contain source truth or learner mastery. It coordinates decisions across the Knowledge Model, Student Model, and Evidence Model.

## Core principle

> Teaching is iterative hypothesis testing over a learner model.

## Responsibilities

The Teacher Model stores:

- the current learning objective;
- the current instructional phase;
- active hypotheses about learner understanding;
- predicted misconceptions and likely failure points;
- selected teaching strategy;
- the current intervention;
- expected observable outcomes;
- stopping and progression criteria;
- cognitive-load, curiosity, time, and visual-use budgets;
- strategy outcome history;
- unresolved instructional risks;
- decision confidence and rationale.

It must not silently invent learner traits or duplicate facts stored in another model.

## Pedagogical cognition loop

```text
Observe
  ↓
Interpret evidence
  ↓
Generate competing hypotheses
  ↓
Predict learner behavior
  ↓
Select intervention
  ↓
Teach
  ↓
Observe outcome
  ↓
Update hypotheses and plan
```

The loop should remain explicit even when several steps occur inside one response.

## Model structure

### Instructional goal

Every active lesson requires a bounded goal.

```yaml
goal:
  concept: pointer
  capability: explain
  success_condition: distinguish address from stored value without hints
  depth: conceptual
  priority: core
```

Weak goal:

```text
Teach pointers.
```

Strong goal:

```text
The learner can explain why a pointer and the value found through dereferencing are different.
```

### Instructional phase

Recommended phases:

- `ORIENT`
- `DIAGNOSE`
- `BUILD_PREREQUISITE`
- `INTRODUCE`
- `MODEL`
- `GUIDED_PRACTICE`
- `CHECK`
- `REMEDIATE`
- `INDEPENDENT_PRACTICE`
- `TRANSFER`
- `CONSOLIDATE`
- `REVIEW`
- `COMPLETE`

The phase constrains which actions are appropriate.

### Hypothesis set

A hypothesis is a testable explanation for observed learner behavior.

```yaml
id: hypothesis-17
claim: The learner confuses an address with the value stored at that address.
target_concepts:
  - address
  - pointer
  - stored-value
supporting_evidence:
  - evidence-104
contradicting_evidence:
  - evidence-097
confidence: 0.68
status: active
next_test: Ask the learner to compare two equal values stored at different addresses.
```

The model should preserve competing hypotheses when evidence is ambiguous.

Example:

```text
Observed error: learner says `p` and `*p` are the same.

Hypothesis A: address-value distinction is missing.
Hypothesis B: dereference notation is misunderstood.
Hypothesis C: the answer was a careless wording error.
```

The next action should discriminate among these explanations.

### Prediction

Every intervention should state an expected learner response.

```yaml
prediction:
  intervention: contrastive-memory-diagram
  expected_observation: learner identifies pointer, address, and stored value as three distinct roles
  failure_signal: learner still labels the arrow and destination value as identical
  confidence: medium
```

Prediction makes teaching decisions falsifiable.

### Strategy

A strategy is a reusable instructional pattern, not the final wording.

Examples:

- prerequisite repair;
- contrastive example;
- worked example;
- faded guidance;
- analogy with explicit mapping;
- SVG externalization;
- Socratic diagnosis;
- prediction-before-explanation;
- debugging task;
- teach-back;
- retrieval practice;
- transfer challenge;
- spaced review.

A strategy record should include:

```yaml
strategy: contrastive-example
reason: learner distinguishes individual terms but merges their roles
expected_information_gain: high
cognitive_cost: medium
time_cost: low
risk: may appear repetitive if misconception hypothesis is wrong
```

### Intervention

An intervention is one concrete teaching action.

```yaml
intervention:
  action: show-svg
  artifact: memory-layout-02
  prompt: Identify what the variable stores and what dereferencing retrieves.
  intended_evidence:
    - relationship-recognition
    - explanation
```

Responses are outputs of interventions. Responses are not themselves the Teacher Model.

### Decision record

Every major teaching action should be reconstructable.

```yaml
decision_id: decision-42
timestamp: 2026-07-28T08:00:00Z
goal: distinguish pointer from pointee
phase: remediate
observations:
  - evidence-104
active_hypotheses:
  - hypothesis-17
selected_strategy: contrastive-example
selected_action: show-svg
alternatives_considered:
  - repeat-definition
  - code-tracing-question
reason: highest expected diagnostic value with manageable cognitive load
confidence: 0.74
expected_outcome: learner explains the three-role distinction
```

## Resource budgets

Budgets are parameters inside the Teacher Model, not separate engines.

### Cognitive-load budget

Tracks the number and interaction of new elements.

Possible states:

- low;
- moderate;
- high;
- exceeded.

When exceeded, the teacher should reduce novelty, split the step, externalize relationships, or return to a prerequisite.

### Time budget

Represents the available instructional time and acceptable detour length.

### Curiosity budget

Controls optional stories, surprising examples, and enrichment. Curiosity supports attention but must not displace the core objective.

### Visual budget

Controls when visual generation is justified by a spatial, causal, temporal, structural, or comparative learning need.

### Interaction budget

Controls how much the teacher explains before requesting learner activity.

These budgets should be adaptable and visible in decision rationale.

## Strategy selection criteria

The Teacher Model should rank candidate actions using:

- alignment with the current objective;
- expected information gain;
- expected learning gain;
- prerequisite readiness;
- misconception relevance;
- learner independence;
- cognitive cost;
- time cost;
- engagement risk;
- accessibility;
- source integrity;
- safety constraints;
- prior strategy outcomes;
- reversibility.

A conceptual scoring form may be used:

```text
utility(action) =
  learning_gain
  + information_gain
  + transfer_value
  - cognitive_cost
  - time_cost
  - misconception_risk
  - source_risk
```

This is a decision aid, not a claim that pedagogy can be perfectly reduced to one number.

## Action taxonomy

Recommended actions:

- ask diagnostic question;
- confirm goal;
- activate prior knowledge;
- repair prerequisite;
- explain briefly;
- tell a bounded story;
- introduce analogy;
- expose analogy limits;
- show SVG;
- show worked example;
- ask prediction;
- ask comparison;
- request explanation;
- request teach-back;
- provide hint;
- fade hint;
- present counterexample;
- run simulation;
- ask learner to debug;
- assign independent practice;
- assign transfer task;
- schedule review;
- stop and summarize.

Each action should specify what evidence it is expected to produce.

## Progression policy

Progress only when the required evidence matches the goal.

Examples:

- A correct recognition answer does not establish explanation ability.
- A correct guided solution does not establish independent application.
- Immediate success does not establish retention.
- Repeating the teacher's wording does not establish transfer.

The Teacher Model should define progression and stopping criteria before or during the intervention.

## Remediation policy

When evidence indicates failure:

1. determine whether the failure is conceptual, procedural, representational, attentional, linguistic, or accidental;
2. preserve multiple hypotheses if uncertain;
3. choose a discriminating test when information is insufficient;
4. change representation or strategy rather than merely repeating more words;
5. repair the smallest blocking prerequisite;
6. reduce cognitive load;
7. collect new evidence;
8. return to the target objective.

Repeated explanation using the same structure is not valid adaptation.

## Strategy memory

The Teacher Model may store how strategies performed for this learner and concept family.

```yaml
strategy_history:
  - strategy: sequence-diagram
    concept_family: protocols
    outcome: improved causal explanation
    evidence: evidence-211
    confidence: medium
  - strategy: pure-verbal-repetition
    concept_family: protocols
    outcome: no measurable improvement
    evidence: evidence-198
    confidence: medium
```

This memory informs future choices without becoming a fixed learning-style label.

## Teacher uncertainty

The system must distinguish:

- uncertainty about the subject;
- uncertainty about the learner;
- uncertainty about the best strategy;
- uncertainty about the meaning of the learner's response.

When uncertainty is high, prefer information-gathering actions over confident remediation.

## Source and epistemic discipline

The Teacher Model must not use pedagogical creativity to overwrite factual uncertainty.

It should:

- distinguish source content from enrichment;
- avoid unsupported certainty;
- surface relevant source conflicts;
- select examples that preserve the formal relationship;
- avoid analogies when their limits would dominate the lesson;
- request verification when factual precision is essential and sources are incomplete.

## Safety and respect

Teaching decisions must:

- avoid humiliation, manipulation, and coercion;
- avoid diagnosing the learner;
- preserve learner agency;
- explain major adaptations when helpful;
- use the minimum personal data needed;
- comply with domain safety constraints;
- avoid using confusion as evidence of low ability.

## Suggested record

```yaml
teacher_model:
  goal:
    concept: pointer
    capability: explain
  phase: REMEDIATE
  active_hypotheses:
    - hypothesis-17
    - hypothesis-18
  selected_strategy: contrastive-memory-layout
  current_intervention:
    action: show-svg-and-predict
  prediction:
    expected_observation: learner separates address from stored value
  budgets:
    cognitive_load: moderate
    time_minutes: 8
    curiosity: low
    visual: available
  progression_rule: independent explanation without copied wording
  fallback_strategy: two-address counterexample
  confidence: 0.74
```

## Update rules

1. Every active hypothesis must be linked to evidence.
2. Predictions must name an observable outcome.
3. Completed interventions must record their result.
4. Failed strategies should change future ranking.
5. New evidence may reverse a decision.
6. Unknown learner states must remain unknown.
7. Teacher confidence must decrease when predictions fail.
8. The current objective must remain bounded.
9. Resource budgets must constrain optional enrichment.
10. Runtime components must not keep hidden instructional state outside this model.

## Failure modes

### Answer-first behavior

Problem: the system generates an explanation before diagnosing what is needed.

Prevention: require goal, hypothesis, and expected evidence.

### Single-hypothesis lock-in

Problem: one error is immediately assigned one cause.

Prevention: competing hypotheses and discriminating tests.

### Strategy repetition

Problem: the same explanation is repeated with greater length.

Prevention: strategy history and representation change.

### Activity without evidence

Problem: an engaging action produces no useful mastery signal.

Prevention: intended evidence field.

### Unbounded enrichment

Problem: stories and applications consume the lesson.

Prevention: curiosity and time budgets.

### Visual decoration

Problem: diagrams are used because they look impressive.

Prevention: visual objective and interpretation task.

### Hidden state

Problem: runtime decisions depend on state not represented in the models.

Prevention: decision records and model-only persistence.

### Premature progression

Problem: recognition is treated as mastery.

Prevention: capability-specific progression rules.

## Verification criteria

The Teacher Model is valid when:

- every action serves a bounded objective;
- hypotheses are testable and evidence-linked;
- predictions are observable;
- competing explanations can coexist;
- strategy choices are reconstructable;
- failed predictions update the model;
- resource budgets affect decisions;
- progression is capability-specific;
- runtime components do not keep hidden teacher state;
- the next action can be explained without exposing private chain-of-thought.

## Minimal compliance

A minimal implementation must store:

- current goal;
- instructional phase;
- active hypotheses;
- selected strategy;
- current intervention;
- expected outcome;
- progression rule;
- fallback action;
- decision confidence;
- evidence references.

## Guiding question

> Given the current goal, models, and uncertainty, which teaching action is most likely to improve understanding while also producing the evidence needed for the next decision?
