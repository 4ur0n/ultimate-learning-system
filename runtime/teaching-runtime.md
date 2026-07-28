# Teaching Runtime

## Purpose

The Teaching Runtime executes the teaching loop by reading the four core models, selecting one bounded instructional action, collecting evidence, and updating model state.

It does not own hidden learner state, source truth, or permanent strategy state. All persistent state must remain in the Knowledge Model, Student Model, Teacher Model, or Evidence Model.

## Core principle

> Runtime chooses. LLM writes.

The runtime decides what pedagogical action should occur. The language model realizes that action as an explanation, question, example, SVG specification, story, hint, or feedback message.

## Inputs

Required inputs:

- current Knowledge Model slice;
- current Student Model slice;
- current Teacher Model state;
- relevant Evidence Model records;
- learner message or activity result;
- session constraints;
- safety and source-integrity policies.

Optional inputs:

- time limit;
- output medium;
- preferred language;
- accessibility requirements;
- publishing target;
- external tool availability.

## Outputs

Each runtime cycle produces:

1. one decision record;
2. zero or one learner-facing intervention;
3. expected evidence specification;
4. model update proposal;
5. next-state transition;
6. stop, continue, review, or escalate signal.

## Runtime loop

```text
READ MODELS
    ↓
NORMALIZE OBSERVATION
    ↓
UPDATE EVIDENCE
    ↓
REVISE STUDENT CLAIMS
    ↓
REVISE TEACHER HYPOTHESES
    ↓
SELECT NEXT OBJECTIVE
    ↓
GENERATE CANDIDATE ACTIONS
    ↓
RANK ACTIONS
    ↓
EXECUTE ONE INTERVENTION
    ↓
REQUEST OR OBSERVE EVIDENCE
    ↓
TRANSITION STATE
```

The loop may stop before generating an intervention when the current input only requires evidence capture, confirmation, publication, or completion.

## State machine

Recommended states:

- `INITIALIZE`
- `ORIENT`
- `DIAGNOSE`
- `PLAN`
- `TEACH`
- `CHECK`
- `REMEDIATE`
- `PRACTICE`
- `TRANSFER`
- `CONSOLIDATE`
- `REVIEW`
- `PUBLISH`
- `COMPLETE`
- `BLOCKED`

### Transition rules

```text
INITIALIZE -> ORIENT
ORIENT -> DIAGNOSE | PLAN
DIAGNOSE -> PLAN | REMEDIATE
PLAN -> TEACH | PRACTICE | REVIEW
TEACH -> CHECK
CHECK -> PRACTICE | REMEDIATE | TRANSFER | COMPLETE
REMEDIATE -> CHECK
PRACTICE -> CHECK
TRANSFER -> CHECK | CONSOLIDATE
CONSOLIDATE -> REVIEW | COMPLETE | PUBLISH
REVIEW -> CHECK
ANY -> BLOCKED
```

Transitions must be justified by evidence or explicit session constraints.

## Cycle contract

Every cycle must answer these questions internally:

1. What is the current bounded objective?
2. What does the learner currently appear able to do?
3. What remains uncertain?
4. Which active hypothesis best explains the latest evidence?
5. What observation would reduce uncertainty most?
6. Which action is likely to produce learning and useful evidence at acceptable cost?
7. What result would cause progression, remediation, or stopping?

The learner-facing output should not expose private internal reasoning. It may provide a concise rationale such as, "Let's test the address-value distinction before moving on."

## Observation normalization

Raw learner input should be converted into an observation record before instructional decisions are made.

Possible observation types:

- answer;
- question;
- self-report;
- correction;
- request for example;
- request for summary;
- refusal or disengagement;
- tool result;
- code execution result;
- diagram interpretation;
- silence or incomplete response.

Normalization should preserve the raw input reference and avoid overinterpreting short responses.

## Evidence capture

The runtime must create or update Evidence Model records before changing mastery or misconception claims.

It should capture:

- target concept;
- target capability;
- task context;
- correctness and completeness;
- independence;
- hint usage;
- learner confidence when available;
- evaluator confidence;
- transfer distance;
- contradiction status.

No Student Model transition may occur without evidence linkage.

## Student Model update

Updates should be conservative and capability-specific.

Examples:

- a correct definition updates recall, not transfer;
- a guided explanation updates guided explanation, not independent explanation;
- a repeated structural error may strengthen a misconception hypothesis;
- delayed retrieval may strengthen retention;
- a confident error may reduce calibration confidence;
- a self-correction may support monitoring and debugging capability.

Unknown fields remain unknown.

## Teacher Model update

After evidence capture, the runtime updates:

- active hypothesis confidence;
- instructional phase;
- selected strategy history;
- prediction accuracy;
- cognitive-load estimate;
- fallback strategy;
- progression readiness;
- unresolved risks.

Failed predictions must reduce confidence or change strategy ranking.

## Objective selection

Objective selection should follow this priority order:

1. repair a hard prerequisite blocking the active goal;
2. resolve a high-impact misconception;
3. complete the current capability target;
4. test independence after guided success;
5. test transfer after independent success;
6. consolidate or schedule review;
7. add enrichment only when core progress is secure.

The runtime should choose the smallest objective that meaningfully advances the learner's goal.

## Candidate actions

Candidate generation may include:

- ask a diagnostic question;
- provide a concise explanation;
- show a worked example;
- ask for a prediction;
- present a contrastive example;
- present a counterexample;
- generate an SVG specification;
- introduce an analogy;
- state analogy limitations;
- ask the learner to trace or simulate;
- provide a hint;
- fade a scaffold;
- ask for teach-back;
- assign independent practice;
- assign near-transfer;
- assign far-transfer;
- summarize and stop;
- schedule review.

Candidates must be compatible with the current state, goal, safety policy, and available tools.

## Action ranking

Rank candidate actions using:

- objective alignment;
- expected learning gain;
- expected information gain;
- prerequisite suitability;
- misconception relevance;
- independence promotion;
- cognitive cost;
- time cost;
- learner effort;
- accessibility;
- source fidelity;
- safety;
- prior strategy outcome;
- reversibility.

Suggested decision structure:

```yaml
action: contrastive-svg-question
learning_gain: high
information_gain: high
cognitive_cost: medium
time_cost: low
source_risk: low
misconception_relevance: high
selected: true
```

The runtime selects one primary action per cycle unless multiple tightly coupled actions are necessary, such as a diagram followed by one interpretation question.

## Intervention realization

The LLM receives a bounded action specification.

Example:

```yaml
action_type: show-svg-and-question
objective: distinguish address from stored value
required_elements:
  - variable holding an address
  - labeled memory location
  - value stored at destination
  - arrows showing reference and dereference
prohibited_elements:
  - decorative imagery
  - unexplained pointer arithmetic
question: Which item is the address, and which item is the stored value?
max_words: 120
```

The LLM may choose wording, but may not change the objective, evidence target, or progression rule.

## SVG policy

Use SVG when spatial, causal, temporal, structural, or comparative relationships are central.

Every SVG action must define:

- learning objective;
- required nodes and relationships;
- labels;
- reading order;
- accessibility text;
- interpretation question;
- prohibited decorative elements.

Do not use learner-facing ASCII art as a substitute for diagrams.

## Analogy policy

Use analogy only when it maps a target relationship clearly.

Each analogy action must include:

- source domain;
- target domain;
- explicit mapping;
- intended inference;
- limitations;
- misuse risk;
- exit from analogy back to the formal model.

The runtime should stop using an analogy when its limitations create more cognitive load than benefit.

## Story policy

Stories should be short, structurally relevant, and bounded by the curiosity and time budgets.

A story must support one of:

- motivation;
- causal sequence;
- memory cue;
- real-world context;
- misconception contrast.

A story is not evidence of understanding.

## Question policy

Questions should be selected by intended evidence.

Examples:

- recall question -> retrieval evidence;
- why question -> explanation evidence;
- prediction question -> mental-model evidence;
- debugging question -> analysis evidence;
- changed-context task -> transfer evidence;
- teach-back -> integrated explanation evidence.

Avoid asking vague questions such as "Do you understand?" when a performance check is possible.

## Hint policy

Hints should reveal the minimum information needed to restart productive reasoning.

Suggested ladder:

1. restate the goal;
2. direct attention to the relevant feature;
3. remind the learner of a prerequisite;
4. provide a partial structure;
5. show one intermediate step;
6. show the full worked solution.

The Evidence Model must record the highest hint level used.

## Progression rules

Progress only when evidence matches the required capability and independence level.

Example progression:

```text
INTRODUCED
  -> GUIDED after supported explanation
  -> INDEPENDENT after unassisted application
  -> TRANSFERRED after changed-context success
  -> MASTERED after sufficient breadth and retention
```

Mastery must remain reversible when later evidence shows fragility.

## Remediation routing

Route failure according to the most likely class:

### Missing prerequisite

Action: repair the smallest blocking prerequisite.

### Misconception

Action: use contrast, counterexample, prediction, or representation change.

### Procedural error

Action: isolate the step, model it, then fade support.

### Representation mismatch

Action: translate between verbal, symbolic, visual, procedural, or code forms.

### Cognitive overload

Action: reduce interacting elements, externalize structure, and shorten the step.

### Ambiguous evidence

Action: ask a discriminating question rather than reteaching immediately.

### Careless slip

Action: request self-check before changing the Student Model substantially.

## Stop conditions

Stop or pause when:

- the learner reaches the current success condition;
- the learner requests a pause or topic change;
- the time budget is exhausted;
- evidence quality is too low for further adaptation;
- required source material is missing;
- safety policy blocks continuation;
- cognitive overload remains high after simplification;
- the session has reached a natural consolidation point.

Stopping should produce a brief state summary and next-step record.

## Blocked state

The runtime enters `BLOCKED` when it cannot proceed responsibly.

Possible reasons:

- missing source;
- unresolved source conflict;
- unsupported tool requirement;
- insufficient learner input;
- policy restriction;
- corrupted model state;
- incompatible objective;
- inaccessible output format.

A blocked result should state what is missing and preserve current state without inventing progress.

## Runtime record

```yaml
runtime_cycle:
  id: cycle-52
  previous_state: CHECK
  next_state: REMEDIATE
  observation_ref: evidence-104
  objective:
    concept: pointer
    capability: explain
  student_claims_read:
    - student:pointer:explain:guided
  teacher_hypotheses_read:
    - hypothesis-17
  selected_action: contrastive-svg-question
  expected_evidence:
    capability: explain
    independence: prompted
  progression_rule: distinguish address and value in original wording
  fallback_action: two-address counterexample
  model_updates:
    teacher_model:
      phase: REMEDIATE
    student_model: none-before-new-evidence
```

## Invariants

1. No hidden persistent state outside the four models.
2. No Student Model update without Evidence Model linkage.
3. No teaching action without a bounded objective.
4. No progression based on capability-mismatched evidence.
5. No analogy without explicit limitations.
6. No SVG without an instructional purpose.
7. No repeated failed strategy without a justified reason.
8. No source enrichment presented as source fact.
9. No mastery claim that ignores assistance level.
10. No learner label inferred from one observation.

## Failure modes

### Prompt-driven drift

Problem: the LLM changes the objective while generating a response.

Prevention: bounded action specification and output validation.

### Hidden-score runtime

Problem: the runtime keeps an internal mastery number not represented in Evidence or Student Models.

Prevention: model-only persistence invariant.

### Explanation loop

Problem: repeated explanation replaces diagnosis.

Prevention: ambiguous evidence routes to discriminating questions.

### Activity inflation

Problem: many exercises are produced without a progression rule.

Prevention: every action defines expected evidence and transition criteria.

### Premature mastery

Problem: immediate guided success becomes `MASTERED`.

Prevention: capability, independence, transfer, and retention checks.

### Decorative multimodality

Problem: SVG, analogy, and story are used simultaneously without need.

Prevention: rank by instructional utility and select the smallest sufficient action.

### Runtime-model duplication

Problem: runtimes maintain separate copies of learner or teacher state.

Prevention: read and write the canonical models only.

## Verification scenarios

A compliant implementation should pass scenarios such as:

1. Correct but heavily hinted answer does not update independent mastery.
2. High-confidence repeated error strengthens a misconception claim.
3. Ambiguous wrong answer triggers diagnosis rather than immediate explanation.
4. Failed verbal explanation causes a representation change rather than a longer repetition.
5. Visual action includes an interpretation question.
6. Analogy output includes limitations and returns to formal language.
7. Near-transfer success does not automatically establish far transfer.
8. Delayed failure moves a concept from mastered to fragile.
9. Missing source provenance blocks confident factual enrichment.
10. Every state transition can be reconstructed from model records.

## Minimal compliance

A minimal Teaching Runtime must:

- read all four core models;
- maintain an explicit state;
- select one bounded objective;
- create a decision record;
- produce one evidence-targeted action;
- capture assistance level;
- update models only through auditable records;
- support remediation and progression;
- support stopping and blocked states;
- expose a concise rationale without revealing private chain-of-thought.

## Guiding question

> What is the smallest next action that is likely to improve the learner's state and produce the evidence required to choose what happens after it?
