# Assessment Runtime

## Purpose

The Assessment Runtime designs, delivers, evaluates, and records learning checks that produce valid evidence for the current instructional decision.

It does not exist to manufacture scores. Its job is to answer a narrower question:

> What did the learner actually demonstrate, under what conditions, and how should that change the next teaching action?

The runtime operates across the Knowledge Model, Student Model, Teacher Model, and Evidence Model.

## Core principle

> Assessment is an intervention for reducing uncertainty about learning.

## Responsibilities

The Assessment Runtime must:

- identify the exact claim that needs evidence;
- select an assessment task aligned with the target capability;
- control assistance, answer exposure, and transfer distance;
- define a rubric before evaluating the response;
- evaluate correctness, completeness, reasoning, and independence separately;
- detect ambiguity, slips, and misconception-consistent errors;
- create auditable Evidence Model records;
- propose capability-specific Student Model updates;
- revise Teacher Model hypotheses and predictions;
- route the next action to progress, remediate, review, or collect more evidence.

It must not maintain a hidden mastery score.

## Inputs

Required inputs:

- target concept or concept relationship;
- target capability;
- current Student Model claims;
- relevant Knowledge Model structure;
- current Teacher Model objective and hypotheses;
- prior Evidence Model records;
- assessment constraints.

Assessment constraints may include:

- time limit;
- learner language;
- accessibility needs;
- available tools;
- open-book or closed-book mode;
- permitted hint level;
- required transfer distance;
- safety and source-integrity policies.

## Outputs

Each assessment cycle produces:

1. an assessment specification;
2. a learner-facing task;
3. a rubric;
4. an evaluation record;
5. one or more Evidence Model records;
6. proposed Student Model claim updates;
7. Teacher Model hypothesis updates;
8. a routing decision.

## Assessment cycle

```text
IDENTIFY UNCERTAINTY
        ↓
SELECT TARGET CLAIM
        ↓
CHOOSE TASK TYPE
        ↓
CONTROL SUPPORT AND CONTEXT
        ↓
DEFINE RUBRIC
        ↓
DELIVER TASK
        ↓
CAPTURE RESPONSE
        ↓
VALIDATE TASK CONDITIONS
        ↓
EVALUATE RESPONSE
        ↓
CREATE EVIDENCE
        ↓
UPDATE CLAIM CONFIDENCE
        ↓
ROUTE NEXT ACTION
```

## Target claim

Every assessment must target one or more explicit claims.

Examples:

```text
student:pointer:recall:independent
student:pointer:explain:independent
student:pointer:apply:independent
student:pointer:transfer:near
student:misconception:pointer-is-value
teacher:hypothesis:pointer-address-confusion
```

An assessment without a target claim is activity, not evidence collection.

## Capability alignment

Task type must match the capability being evaluated.

### Recall

Use:

- free retrieval;
- short-answer naming;
- sequence recall;
- formula or rule retrieval.

Do not infer recall from answer recognition alone.

### Explanation

Use:

- why or how questions;
- relationship explanation;
- comparison;
- causal reconstruction;
- learner-generated examples;
- teach-back with follow-up.

Do not infer explanation from copied wording.

### Application

Use:

- problem solving;
- procedure selection;
- implementation;
- classification with justification;
- context-specific decision making.

Application evidence should include why the concept applies, not only the final answer when reasoning matters.

### Debugging or analysis

Use:

- identify the fault;
- explain failure behavior;
- repair a flawed solution;
- compare competing diagnoses;
- trace a system or procedure.

### Prediction

Use:

- predict an outcome before revealing it;
- explain what changes when one variable changes;
- identify likely failure behavior.

Prediction is strong evidence of an operative mental model.

### Transfer

Use a changed context while preserving the target relationship.

Transfer distance should be explicit:

- same item;
- same structure;
- near transfer;
- far transfer;
- novel domain.

### Retention

Use delayed retrieval or delayed application without re-teaching immediately beforehand.

### Teach-back

Ask the learner to:

- explain the concept coherently;
- choose an example;
- warn about a likely misconception;
- answer a follow-up question.

Teach-back is integrated evidence, but not a universal mastery requirement.

## Task specification

```yaml
assessment_id: pointer-explain-07
target_claims:
  - student:pointer:explain:independent
concepts:
  - pointer
  - address
  - stored-value
capability: explain
prompt_type: contrastive-explanation
prompt: Explain the difference between an address, a pointer, and the value obtained by dereferencing.
conditions:
  open_book: false
  hints_allowed: true
  initial_hint_level: 0
  answer_exposure: none
  transfer_distance: same-structure
rubric_ref: rubric-pointer-explain-v1
success_condition: distinguishes all three roles and connects them accurately
misconception_targets:
  - pointer-is-value
  - address-is-content
```

## Task selection policy

Choose the smallest task that can discriminate among the active hypotheses.

Prefer tasks with:

- high capability alignment;
- high information gain;
- low guessing opportunity;
- manageable cognitive load;
- clear scoring criteria;
- minimal language burden unrelated to the target;
- meaningful but controlled context novelty.

Avoid broad tasks when a focused probe can resolve the uncertainty.

## Diagnostic assessment

Diagnostic tasks should distinguish competing explanations.

Example observation:

```text
The learner says `p` and `*p` are the same.
```

Possible hypotheses:

- address-value distinction is missing;
- dereference notation is misunderstood;
- the learner made a wording slip.

A useful diagnostic sequence might ask:

1. Can two addresses contain the same value?
2. What does the variable `p` store?
3. What operation does `*` perform here?

The assessment should stop once sufficient discriminating evidence exists.

## Assessment forms

Supported forms include:

- selected response;
- short answer;
- free explanation;
- worked problem;
- worked-example completion;
- code execution;
- debugging;
- diagram interpretation;
- diagram construction;
- prediction;
- simulation;
- comparison;
- teach-back;
- project artifact;
- delayed retrieval.

No form is inherently superior. Validity depends on alignment with the target claim.

## Selected-response policy

Multiple-choice and similar formats may be useful for:

- rapid diagnosis;
- misconception discrimination;
- broad coverage;
- recognition checks.

Requirements:

- distractors should represent meaningful alternative models or common errors;
- the item should not be solvable through grammar or formatting clues;
- answer position should not follow a predictable pattern;
- explanation or confidence may be requested when diagnostic value matters;
- recognition evidence must not be treated as free recall or explanation.

## Free-response policy

Free responses are useful for reasoning and mental-model evidence.

Evaluation should tolerate:

- different wording;
- valid alternative methods;
- concise but complete answers;
- equivalent representations.

Evaluation should not rely mainly on keyword overlap.

## Rubric contract

A rubric must exist before final evaluation.

Recommended structure:

```yaml
rubric_id: rubric-pointer-explain-v1
target_capability: explain
essential_elements:
  - an address identifies a memory location
  - a pointer stores or represents an address
  - dereferencing accesses the value at the referenced location
acceptable_variations:
  - pointer contains the address
  - pointer refers to a memory location
partial_patterns:
  - distinguishes pointer from value but omits address role
misconception_indicators:
  pointer-is-value:
    - states that the pointer itself is the pointee value
  address-is-content:
    - treats the address as the stored data
independence_requirement: no answer-revealing hint
evaluator_escalation:
  - response uses ambiguous domain-specific terminology
```

## Evaluation dimensions

Evaluate separately:

- correctness;
- completeness;
- reasoning quality;
- independence;
- confidence calibration;
- transfer distance;
- misconception signals;
- retention interval;
- evaluator confidence.

A final classification may summarize these dimensions, but it must not replace them.

## Assistance tracking

The runtime must record:

- number of hints;
- highest hint level;
- whether a prerequisite was restated;
- whether part of the solution was revealed;
- whether the full answer was exposed;
- whether tools, notes, or examples were available;
- whether the learner self-corrected before receiving help.

Suggested hint ladder:

1. restate objective;
2. direct attention;
3. cue prerequisite;
4. provide structure;
5. reveal one step;
6. reveal full solution.

Evidence strength must decrease as answer exposure increases.

## Response capture

Preserve:

- raw learner response or artifact reference;
- revisions;
- tool outputs;
- execution results;
- timing when educationally relevant;
- confidence report when available;
- request for clarification;
- skipped or abandoned state.

Do not collect behavioral data without a clear educational purpose.

## Task validation

Before evaluating the learner, validate the assessment itself.

Check:

- was the prompt clear?
- did the task test the intended capability?
- was the required prerequisite available?
- did tool failure affect the result?
- was the answer accidentally exposed?
- was language complexity unrelated to the target?
- did the task contain multiple plausible answers not covered by the rubric?
- did the task violate source or safety constraints?

Invalid tasks must produce retracted or indeterminate evidence, not learner penalties.

## Evaluation routing

### Deterministic evaluation

Use when correctness can be checked reliably through:

- exact computation;
- tests;
- type checking;
- formal validation;
- known output constraints.

### Rubric-based model evaluation

Use for:

- explanations;
- comparisons;
- conceptual reasoning;
- open-ended transfer.

The evaluator must return uncertainty and cited rubric criteria.

### Human review

Escalate when:

- stakes are high;
- the answer is novel but plausibly valid;
- source interpretation is disputed;
- automated evaluators disagree;
- evaluator confidence is below threshold;
- fairness or accessibility concerns exist.

## Misconception diagnosis

A misconception should not be inferred from one incorrect answer alone.

Evidence becomes more diagnostic when:

- the same structural relationship recurs;
- the error appears across representations;
- the learner explains the incorrect reasoning coherently;
- confidence is high;
- the learner fails a targeted counterexample predictably;
- the error matches a Knowledge Model misconception pattern.

Possible diagnosis result:

```yaml
misconception_signal:
  id: pointer-is-value
  status: supported
  confidence: 0.76
  evidence:
    - evidence-104
    - evidence-109
  alternative_explanations:
    careless-wording: 0.18
    notation-confusion: 0.41
```

## Slip detection

Before substantially lowering a capability claim, consider whether the response may be a slip.

Signals of a possible slip:

- strong prior evidence in varied contexts;
- correct reasoning with one isolated error;
- immediate self-correction;
- mismatch between final answer and intermediate work;
- tool or transcription error;
- low confidence and explicit uncertainty.

A self-check prompt may resolve the ambiguity.

## Evidence creation

The runtime must create a structured Evidence Model record.

```yaml
id: evidence-220
assessment_id: pointer-explain-07
concepts: [pointer, address, stored-value]
capability: explain
outcome:
  correctness: partially-correct
  completeness: partial
  reasoning_quality: structurally-incomplete
independence: independent
confidence_report: 0.78
support:
  hints_used: 0
  answer_exposure: none
transfer_distance: same-structure
error_structure:
  - omitted-address-location-distinction
misconception_signals:
  - id: pointer-is-value
    strength: weak
claims_supported:
  - student:pointer:explain:partial-independent
claims_weakened:
  - student:pointer:explain:complete-independent
evaluator:
  type: rubric-based-model
  confidence: 0.87
```

## Student Model update proposal

The Assessment Runtime proposes updates; the canonical Student Model remains the source of learner state.

Examples:

```yaml
student_model_updates:
  - claim: student:pointer:explain:independent
    operation: decrease-confidence
    amount: modest
    evidence_ref: evidence-220
  - misconception: pointer-is-value
    operation: add-weak-signal
    evidence_ref: evidence-220
```

Updates must remain capability-specific and reversible.

## Teacher Model update proposal

Assessment results should update:

- hypothesis confidence;
- prediction accuracy;
- progression readiness;
- fallback strategy;
- phase;
- expected information gain of future tasks.

Example:

```yaml
teacher_model_updates:
  - hypothesis: pointer-address-confusion
    operation: increase-confidence
  - prediction: learner-will-separate-three-roles
    outcome: failed
  - phase:
      from: CHECK
      to: REMEDIATE
  - next_test: two-address-same-value-counterexample
```

## Routing policy

After evaluation, route to one of:

- `PROGRESS`
- `MORE_EVIDENCE`
- `REMEDIATE`
- `PRACTICE`
- `TRANSFER`
- `REVIEW`
- `CONSOLIDATE`
- `HUMAN_REVIEW`
- `INVALIDATE_TASK`
- `STOP`

### Progress

Use when evidence matches the capability, independence, and breadth required by the current success condition.

### More evidence

Use when:

- evidence is ambiguous;
- evaluator confidence is low;
- prior evidence conflicts;
- the task did not discriminate active hypotheses.

### Remediate

Use when evidence supports a blocking misconception, missing prerequisite, procedural gap, or representation mismatch.

### Practice

Use when the concept appears understood but performance is not yet stable or fluent.

### Transfer

Use when independent same-structure performance is established and the goal requires generalization.

### Review

Use when delayed retrieval or fragility is the relevant issue.

## Mastery policy

The Assessment Runtime must not declare mastery from one successful item.

A mastery claim should consider:

- required capability coverage;
- independence;
- varied contexts;
- transfer when required;
- delayed retention;
- absence or repair of major misconceptions;
- evidence consistency;
- evaluator confidence.

Mastery remains provisional and reversible.

## Adaptive task difficulty

Difficulty should adapt using both the Knowledge Model and Student Model.

Increase difficulty when:

- performance is independently correct and stable;
- reasoning is complete;
- hints are unnecessary;
- the learner succeeds across representations.

Decrease or restructure difficulty when:

- cognitive load obscures the target capability;
- prerequisite gaps dominate;
- task language creates irrelevant difficulty;
- repeated failure yields little new information.

Do not confuse harder tasks with better assessment.

## Confidence calibration

When useful, ask the learner for confidence before feedback.

Use calibration evidence to distinguish:

- secure knowledge;
- fragile knowledge;
- productive uncertainty;
- high-confidence misconception;
- guessing.

Do not pressure the learner to report confidence on every item.

## Feedback policy

Feedback should follow the assessment purpose.

### Diagnostic feedback

May delay full correction until the runtime gathers enough evidence.

### Formative feedback

Should:

- identify what was correct;
- name the specific gap;
- explain why it matters;
- provide the smallest useful correction;
- request active repair where possible.

### Summative feedback

Should separate:

- result;
- capability coverage;
- limitations of the assessment;
- next learning recommendation.

Do not expose private evaluator reasoning or hidden chain-of-thought.

## Batch assessment

For multi-item assessments:

- map each item to explicit claims;
- balance coverage and redundancy;
- avoid item-order leakage;
- preserve per-item evidence;
- aggregate only after item-level evaluation;
- inspect systematic error patterns;
- avoid reducing all results to one percentage.

A summary may include capability profiles rather than one total score.

## Assessment integrity

The runtime should detect and record:

- answer exposure;
- repeated-item familiarity;
- copied responses;
- unavailable tools;
- invalid timing assumptions;
- ambiguous scoring;
- evaluator disagreement;
- inaccessible task design.

Integrity flags affect evidence strength, not the learner's character.

## Privacy and fairness

Assessment design should:

- minimize irrelevant language and cultural assumptions;
- support accessibility adaptations without lowering the target construct;
- avoid inferring fixed ability from limited evidence;
- avoid unnecessary surveillance;
- let learners inspect and challenge records;
- distinguish accommodations from answer-revealing assistance;
- use human review when fairness is uncertain.

## Runtime record

```yaml
assessment_cycle:
  id: assessment-cycle-31
  target_claim: student:pointer:explain:independent
  active_hypotheses:
    - pointer-address-confusion
    - notation-confusion
  selected_task: pointer-explain-07
  selection_reason: discriminates role confusion with low guessing risk
  rubric: rubric-pointer-explain-v1
  task_validity: valid
  evidence_created:
    - evidence-220
  route: REMEDIATE
  next_action: two-address-same-value-counterexample
```

## Invariants

1. Every assessment targets an explicit claim.
2. Every evaluated response uses a declared rubric or deterministic checker.
3. Correctness remains separate from independence.
4. Recognition does not become recall or explanation evidence.
5. Immediate success does not become retention evidence.
6. Near transfer does not become far-transfer evidence.
7. One error does not establish a misconception.
8. Invalid tasks do not penalize the learner.
9. Aggregate claims remain traceable to item-level evidence.
10. No hidden mastery score exists outside the models.
11. Evaluator uncertainty propagates to evidence confidence.
12. Assistance and answer exposure are always recorded.

## Failure modes

### Test-first teaching

Problem: assessment volume replaces instruction.

Prevention: assess only when the result can change a meaningful decision.

### Capability mismatch

Problem: multiple-choice recognition is treated as explanation mastery.

Prevention: explicit capability-task mapping.

### Score collapse

Problem: varied evidence becomes a single percentage.

Prevention: capability profiles and traceable claim aggregation.

### Rubric-after-answer

Problem: criteria are invented to fit the observed response.

Prevention: rubric contract before evaluation.

### Hint blindness

Problem: heavily assisted performance counts as independent ability.

Prevention: assistance tracking.

### Misconception overreach

Problem: one wrong answer creates a structural diagnosis.

Prevention: repeated, cross-context, discriminating evidence.

### Invalid-task blame

Problem: an unclear or broken task lowers learner state.

Prevention: task validation and evidence retraction.

### Evaluator laundering

Problem: uncertain model grading becomes a confident learner claim.

Prevention: uncertainty propagation and escalation.

### Endless probing

Problem: the runtime keeps testing without teaching.

Prevention: information-gain threshold and routing limits.

## Verification scenarios

A compliant implementation should pass these scenarios:

1. A correct answer after a full-solution hint is recorded as exposed, not independent.
2. A correct multiple-choice answer updates recognition only.
3. A novel but valid explanation is accepted despite low keyword overlap.
4. One isolated arithmetic slip does not erase strong conceptual evidence.
5. A repeated high-confidence structural error strengthens a misconception claim.
6. An ambiguous task is invalidated rather than scored against the learner.
7. Conflicting evaluators trigger reduced confidence or human review.
8. Immediate retrieval and delayed retrieval remain separate.
9. Near-transfer success does not establish far transfer.
10. Every Student Model update points to one or more Evidence Model records.

## Minimal compliance

A minimal Assessment Runtime must:

- identify the target claim;
- choose a capability-aligned task;
- define a rubric;
- record assistance and answer exposure;
- validate the task;
- evaluate correctness, completeness, and independence separately;
- create Evidence Model records;
- propose reversible model updates;
- route to progress, more evidence, remediation, or review;
- preserve evaluator uncertainty.

## Guiding question

> Which observation would most validly reduce uncertainty about the learner's current capability and improve the next teaching decision?
