# Strategy Selection Runtime

## Purpose

The Strategy Selection Runtime converts model state into a ranked set of pedagogical actions.

It reads the current instructional goal, learner evidence, knowledge structure, active hypotheses, and resource constraints, then selects the smallest action that is most likely to improve learning and reduce uncertainty.

It does not generate the final learner-facing wording. It produces an action specification that another component or language model realizes.

## Core principle

> Strategy selection is a constrained decision problem over uncertain learner state.

## Responsibilities

The runtime must:

- identify the active instructional objective;
- determine the dominant uncertainty;
- generate valid candidate strategies;
- reject strategies that violate prerequisites, source integrity, safety, or accessibility constraints;
- estimate learning gain and information gain;
- estimate cognitive, time, and interaction cost;
- incorporate strategy history without creating fixed learner-style labels;
- select one primary strategy and one fallback;
- define the expected observable result;
- record why alternatives were rejected;
- update Teacher Model strategy state after outcomes are observed.

## Inputs

Required inputs:

- Teacher Model current goal and phase;
- active Teacher Model hypotheses;
- Student Model concept and capability state;
- Evidence Model records relevant to the current objective;
- Knowledge Model concept relationships, misconceptions, and representations;
- available instructional actions;
- runtime budgets and constraints.

Optional inputs:

- learner request;
- time remaining;
- preferred medium;
- accessibility requirements;
- tool availability;
- session history;
- domain-specific policy constraints.

## Outputs

Each selection cycle produces:

1. candidate strategy set;
2. exclusion record;
3. ranked candidate list;
4. selected primary strategy;
5. selected fallback strategy;
6. expected evidence specification;
7. decision confidence;
8. Teacher Model update proposal.

## Selection loop

```text
READ CURRENT OBJECTIVE
        ↓
IDENTIFY DECISION TYPE
        ↓
GENERATE CANDIDATES
        ↓
APPLY HARD CONSTRAINTS
        ↓
ESTIMATE UTILITY
        ↓
ESTIMATE UNCERTAINTY
        ↓
RANK CANDIDATES
        ↓
SELECT PRIMARY + FALLBACK
        ↓
DEFINE PREDICTION
        ↓
RECORD DECISION
```

## Decision types

The runtime should first classify the current decision.

Suggested types:

- `ORIENT`
- `DIAGNOSE`
- `INTRODUCE`
- `BUILD_PREREQUISITE`
- `MODEL`
- `GUIDE`
- `CHECK`
- `REMEDIATE`
- `PRACTICE`
- `TRANSFER`
- `CONSOLIDATE`
- `REVIEW`
- `STOP`

Candidate strategies must be appropriate for the decision type.

## Strategy taxonomy

### Orientation strategies

- goal clarification;
- relevance framing;
- prior-knowledge activation;
- bounded roadmap;
- source overview.

### Diagnostic strategies

- targeted question;
- prediction task;
- contrastive probe;
- counterexample probe;
- representation translation;
- confidence check;
- self-explanation prompt.

### Instruction strategies

- concise direct explanation;
- worked example;
- analogy with explicit mapping;
- short causal story;
- SVG externalization;
- simulation;
- demonstration;
- example and non-example pair;
- comparison table;
- concept-map construction.

### Guided-practice strategies

- completion problem;
- faded worked example;
- hint ladder;
- scaffolded tracing;
- guided debugging;
- guided classification.

### Remediation strategies

- prerequisite repair;
- contrastive case;
- targeted counterexample;
- representation change;
- misconception confrontation;
- error-location task;
- reduced-complexity subproblem;
- analogy replacement;
- reconstruction from first principles.

### Independence strategies

- unassisted application;
- explanation in original wording;
- prediction without cues;
- independent debugging;
- learner-generated example;
- teach-back.

### Transfer strategies

- near-transfer variation;
- far-transfer application;
- cross-representation task;
- changed-domain analogy evaluation;
- procedure-selection task;
- novel case analysis.

### Consolidation strategies

- retrieval summary;
- compare-and-connect;
- misconception warning;
- learner-generated notes;
- concept-map reconstruction;
- next-step planning.

### Review strategies

- delayed free retrieval;
- cumulative mini-check;
- interleaved comparison;
- spaced application;
- recurring-misconception test;
- transfer review.

## Candidate generation

Candidate generation should use the intersection of:

- current phase;
- target capability;
- active hypotheses;
- available representations;
- learner independence level;
- remaining budgets;
- safety and source constraints.

Example:

```yaml
objective:
  concept: pointer
  capability: explain
phase: REMEDIATE
active_hypothesis: pointer-address-confusion
candidate_strategies:
  - contrastive-svg
  - two-address-counterexample
  - concise-definition-repeat
  - guided-code-trace
```

Candidate generation should be broad enough to avoid repetitive behavior but bounded enough to remain auditable.

## Hard constraints

A candidate must be excluded when it:

- depends on an unavailable hard prerequisite;
- targets the wrong capability;
- exceeds the cognitive-load budget;
- requires unavailable tools;
- violates safety policy;
- would expose the answer before a required independent check;
- presents uncertain enrichment as source fact;
- conflicts with accessibility requirements;
- repeats a failed strategy without a new justification;
- cannot produce the required evidence;
- exceeds the remaining time budget.

Exclusions should be recorded.

Example:

```yaml
strategy: far-transfer-task
excluded: true
reason: independent same-structure application not yet established
```

## Utility dimensions

Each valid candidate should be evaluated across separate dimensions.

### Objective alignment

How directly does the strategy support the current target concept and capability?

### Expected learning gain

How likely is the strategy to improve the learner's state?

### Expected information gain

How much will the resulting evidence reduce uncertainty among active hypotheses?

### Misconception relevance

Does the strategy directly expose or repair the suspected incorrect relationship?

### Independence value

Does the strategy move the learner toward less support?

### Transfer value

Does the strategy build generalization appropriate to the current stage?

### Cognitive cost

How many new or interacting elements does it impose?

### Time cost

How much instructional time will it likely consume?

### Interaction cost

How much learner effort is required before useful evidence appears?

### Source risk

Could the strategy blur source facts, analogy, inference, or enrichment?

### Safety risk

Could the action produce unsafe or disallowed content?

### Accessibility fit

Is the action usable in the learner's current medium and accommodations?

### Strategy-history fit

Has this strategy worked or failed for this learner and concept family?

### Reversibility

How easy is it to recover if the strategy is poorly matched?

## Utility representation

A strategy may be represented as:

```yaml
strategy: contrastive-svg
scores:
  objective_alignment: 0.95
  learning_gain: 0.82
  information_gain: 0.88
  misconception_relevance: 0.94
  independence_value: 0.45
  transfer_value: 0.35
  cognitive_cost: 0.42
  time_cost: 0.28
  interaction_cost: 0.31
  source_risk: 0.10
  safety_risk: 0.00
  accessibility_fit: 0.90
  strategy_history_fit: 0.65
  reversibility: 0.90
uncertainty: 0.18
```

Exact numerical scoring is optional. Implementations may use ordinal values or rule-based ranking, provided the rationale remains reconstructable.

## Conceptual utility function

```text
utility(strategy) =
  objective_alignment
  + expected_learning_gain
  + expected_information_gain
  + misconception_relevance
  + independence_value
  + transfer_value
  + accessibility_fit
  + strategy_history_fit
  + reversibility
  - cognitive_cost
  - time_cost
  - interaction_cost
  - source_risk
  - safety_risk
```

Weights should depend on the current decision type.

For diagnosis, information gain may dominate.

For remediation, misconception relevance and learning gain may dominate.

For independent practice, independence value may dominate.

For review, retention evidence value and cost may dominate.

## Uncertainty-aware selection

The runtime should distinguish:

- uncertainty in the Student Model;
- uncertainty in the active hypothesis;
- uncertainty in predicted strategy effectiveness;
- uncertainty in source content;
- uncertainty in evaluator reliability.

When learner-state uncertainty is high, prefer diagnostic strategies.

When strategy-effect uncertainty is high but risks are low, prefer reversible actions.

When source uncertainty is high, prefer clarification or verification over explanation.

## Exploration versus exploitation

The runtime should balance:

- exploitation: reuse a strategy with good prior evidence;
- exploration: try a different strategy when current evidence is weak or prior strategies failed.

Exploration is justified when:

- repeated strategies produce no progress;
- prediction accuracy is low;
- the concept family is new;
- multiple strategies have similar utility;
- the learner requests another approach.

Exploration must remain bounded and evidence-producing.

## Strategy history

Strategy history belongs in the Teacher Model and must remain context-specific.

Example:

```yaml
strategy_history:
  - strategy: sequence-diagram
    concept_family: network-protocols
    target_capability: explain-causal-order
    outcome: successful
    evidence_refs: [evidence-211]
    confidence: medium
  - strategy: verbal-definition-repeat
    concept_family: memory-models
    target_capability: explain
    outcome: ineffective
    evidence_refs: [evidence-104, evidence-109]
    confidence: high
```

The runtime must not convert this into a permanent label such as "visual learner."

## Failed-strategy handling

A strategy counts as failed when:

- its predicted observation did not occur;
- no measurable learning or diagnostic gain appeared;
- cognitive load increased materially;
- the learner requested another approach;
- it reinforced a misconception;
- it produced invalid evidence.

After failure:

1. record the outcome;
2. reduce future rank in the matching context;
3. revise the active hypothesis if appropriate;
4. select a structurally different fallback;
5. avoid merely increasing explanation length.

## Representation selection

The runtime may choose among:

- verbal;
- symbolic;
- mathematical;
- graphical;
- spatial;
- procedural;
- code;
- simulation;
- analogy;
- story.

Representation should match the relationship being learned.

Examples:

- spatial structure -> SVG or physical layout;
- temporal exchange -> sequence diagram;
- causal mechanism -> causal graph or prediction;
- procedural skill -> worked example and fading;
- contrast -> side-by-side cases;
- abstraction boundary -> example and non-example.

Do not select multimodality merely for decoration.

## SVG selection rule

Prefer SVG when:

- relationships are spatial;
- direction matters;
- multiple entities interact;
- temporal sequence is central;
- hierarchy or composition matters;
- external working memory would reduce overload.

Every SVG strategy must define an interpretation task.

## Analogy selection rule

Prefer analogy when:

- the learner lacks an accessible schema;
- a familiar source domain maps the target relationship cleanly;
- limitations can be stated briefly;
- the analogy reduces rather than increases cognitive load.

Reject analogy when:

- superficial similarity dominates structural similarity;
- limitations overwhelm the intended mapping;
- the learner already confuses the formal terms;
- the analogy risks becoming the remembered false model.

## Story selection rule

Prefer a story when it supports:

- motivation;
- causal sequence;
- contextual memory;
- a realistic decision;
- a misconception contrast.

Stories should be short and should return to the formal concept.

## Question selection rule

Questions should be selected by the evidence they are expected to produce.

```yaml
question_type: prediction
expected_evidence:
  capability: mental-model-application
  claim: student:tcp-loss:predict
```

Avoid vague confirmation questions when a performance task is possible.

## Hint selection rule

Select the lowest hint level likely to restart productive reasoning.

The runtime should prefer:

- attention cue before structural cue;
- structural cue before intermediate step;
- intermediate step before full solution.

Each escalation must be recorded as assistance.

## Primary and fallback selection

The selected primary strategy should maximize expected utility under current uncertainty.

The fallback should be structurally different enough to test another explanation or representation.

Weak pairing:

```text
Primary: longer verbal explanation
Fallback: even longer verbal explanation
```

Strong pairing:

```text
Primary: contrastive SVG
Fallback: two-address counterexample with prediction
```

## Prediction contract

Every selected strategy must specify:

- expected learner behavior;
- expected evidence;
- failure signal;
- stopping condition;
- fallback trigger.

Example:

```yaml
prediction:
  strategy: contrastive-svg
  expected_behavior: learner distinguishes pointer, address, and stored value
  expected_evidence:
    capability: explain
    independence: prompted
  failure_signal: learner still merges address and content
  fallback_trigger: one misconception-consistent response
```

## Decision record

```yaml
strategy_decision:
  id: strategy-decision-44
  objective:
    concept: pointer
    capability: explain
  phase: REMEDIATE
  active_hypotheses:
    - pointer-address-confusion
    - dereference-notation-confusion
  candidates:
    - contrastive-svg
    - two-address-counterexample
    - guided-code-trace
    - verbal-definition-repeat
  excluded:
    - strategy: far-transfer-task
      reason: independence not established
  selected_primary: contrastive-svg
  selected_fallback: two-address-counterexample
  reason: highest misconception relevance and information gain at moderate cost
  confidence: 0.76
  expected_evidence:
    capability: explain
    independence: prompted
```

## Model updates

After observing the strategy outcome, update the Teacher Model with:

- prediction success or failure;
- strategy effectiveness;
- context;
- learner effort;
- evidence produced;
- cognitive-load result;
- whether fallback was triggered;
- revised strategy confidence.

Do not update the Student Model directly without Evidence Model records.

## Tie-breaking policy

When candidates have similar utility, prefer:

1. the action with higher information gain;
2. the action with lower cognitive cost;
3. the action with lower source or safety risk;
4. the action that increases learner independence;
5. the more reversible action;
6. the less recently repeated representation;
7. the shorter action.

Randomization may be used among equivalent safe candidates for exploration, but the decision should remain recorded.

## Stop selection

`STOP` is a valid strategy when:

- the current success condition is met;
- the session budget is exhausted;
- the learner requests a pause;
- further action would add little information;
- cognitive overload remains high;
- source uncertainty blocks responsible teaching;
- consolidation is more valuable than another intervention.

Stopping should not be treated as failure.

## Invariants

1. Every strategy serves a bounded objective.
2. Every strategy targets an explicit capability or uncertainty.
3. Every selected action predicts observable evidence.
4. Hard constraints are applied before utility ranking.
5. Failed strategies affect future ranking.
6. Strategy history remains context-specific.
7. No fixed learning-style labels are created.
8. SVG, analogy, and story require explicit instructional purposes.
9. Primary and fallback strategies must not be trivial repetitions.
10. Student Model updates require Evidence Model linkage.
11. Source and safety risks cannot be outweighed by engagement value.
12. A stop action may outrank continued teaching.

## Failure modes

### Strategy roulette

Problem: actions are chosen arbitrarily without model justification.

Prevention: candidate scoring and decision records.

### Repetition bias

Problem: the runtime repeats the same familiar strategy despite failure.

Prevention: failed-strategy penalties and exploration.

### Modality fetish

Problem: diagrams, stories, or analogies are selected because they are attractive.

Prevention: relationship-specific representation rules.

### Learning-style labeling

Problem: temporary strategy success becomes a permanent learner trait.

Prevention: context-specific strategy history.

### Engagement dominance

Problem: entertaining actions outrank learning and evidence value.

Prevention: curiosity budget and objective alignment.

### Diagnostic neglect

Problem: the runtime teaches confidently when learner-state uncertainty is high.

Prevention: information-gain weighting.

### Cost blindness

Problem: a theoretically strong strategy exceeds time or cognitive limits.

Prevention: hard budgets and cost dimensions.

### Fallback duplication

Problem: fallback repeats the same structure with more words.

Prevention: require structural difference.

### Hidden preference state

Problem: strategy choice depends on undocumented memory.

Prevention: Teacher Model strategy history only.

## Verification scenarios

A compliant implementation should pass these scenarios:

1. High learner-state uncertainty causes a diagnostic question to outrank explanation.
2. A failed verbal strategy causes a representation change.
3. Far-transfer work is excluded before independent application is established.
4. SVG is selected for a spatial relationship and includes an interpretation question.
5. Analogy is rejected when its limitations dominate.
6. A previously successful strategy gains rank only in a matching context.
7. A learner request for another approach triggers bounded exploration.
8. A high-risk source-enrichment strategy is excluded despite engagement value.
9. Primary and fallback actions are structurally different.
10. Every selected strategy has an observable prediction and evidence target.
11. The runtime can select `STOP` when continued teaching has low utility.
12. No strategy decision creates a direct Student Model update without evidence.

## Minimal compliance

A minimal Strategy Selection Runtime must:

- read all four core models;
- classify the decision type;
- generate multiple valid candidates;
- apply hard constraints;
- rank candidates by learning gain, information gain, and cost;
- select a primary and fallback strategy;
- define expected evidence and failure signals;
- record the rationale and confidence;
- update strategy history after outcomes;
- avoid fixed learner-style labels.

## Guiding question

> Which valid instructional action has the best expected learning and diagnostic value for this learner, objective, and moment at the lowest necessary cost and risk?
