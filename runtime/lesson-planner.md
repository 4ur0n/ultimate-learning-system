# Lesson Planner Runtime

## Purpose

The Lesson Planner Runtime converts a learner goal and a Knowledge Model into a bounded, adaptive learning path.

It does not follow chapter order by default. It computes the smallest defensible sequence of concepts, capabilities, assessments, and review checkpoints required to reach the learner's target.

The planner produces a provisional plan. The Teaching Runtime may revise that plan whenever new evidence changes the Student Model or Teacher Model.

## Core principle

> Plan from dependencies and learner state, not from source order.

## Responsibilities

The Lesson Planner Runtime must:

- interpret the learner's target outcome;
- map the target to Knowledge Model concepts and capabilities;
- identify hard and soft prerequisites;
- compare prerequisites against the Student Model;
- compute a minimum viable learning path;
- insert diagnostic checkpoints where uncertainty is high;
- sequence instruction, guided practice, independent practice, transfer, and review;
- estimate time and cognitive load;
- preserve alternative paths where several valid routes exist;
- mark assumptions, source gaps, and unresolved conflicts;
- revise the plan when evidence contradicts its assumptions;
- avoid unbounded prerequisite detours and unnecessary enrichment.

It must not declare mastery or store learner evidence directly.

## Inputs

Required inputs:

- learner goal;
- target depth;
- Knowledge Model graph;
- Student Model state;
- relevant Evidence Model records;
- Teacher Model constraints and current priorities;
- available time or deadline;
- source-integrity and safety constraints.

Optional inputs:

- output medium;
- preferred language;
- accessibility requirements;
- desired session length;
- exam or project requirements;
- required artifacts;
- review horizon;
- allowed tools.

## Outputs

Each planning cycle produces:

1. normalized learning contract;
2. target capability set;
3. prerequisite closure;
4. excluded and deferred concepts;
5. ordered learning path;
6. session or phase boundaries;
7. diagnostic checkpoints;
8. progression and stopping criteria;
9. review checkpoints;
10. risk and uncertainty record;
11. replanning triggers.

## Planning loop

```text
NORMALIZE GOAL
      ↓
MAP GOAL TO CONCEPTS AND CAPABILITIES
      ↓
COMPUTE PREREQUISITE CLOSURE
      ↓
OVERLAY STUDENT STATE
      ↓
REMOVE SATISFIED NODES
      ↓
MARK UNCERTAIN NODES FOR DIAGNOSIS
      ↓
GENERATE ALTERNATIVE PATHS
      ↓
ESTIMATE COST AND RISK
      ↓
SELECT MINIMUM VIABLE PATH
      ↓
INSERT PRACTICE, TRANSFER, AND REVIEW
      ↓
PUBLISH PROVISIONAL PLAN
```

## Learning contract

The planner should convert the learner's request into a testable contract.

Example:

```yaml
learning_contract:
  goal: Understand pointers well enough to use them safely in basic C programs.
  target_concepts:
    - pointer
    - address
    - dereference
    - null-pointer
  required_capabilities:
    explain: required
    trace: required
    apply: required
    debug: required
    transfer: near
  target_depth: practical-conceptual
  time_budget_minutes: 120
  deadline: null
  source_scope:
    - uploaded-course-notes
  completion_condition: independently explain, trace, and debug basic pointer use
```

Weak contracts such as "learn networking" must be narrowed before a detailed path is trusted. The planner may create an initial orientation step when clarification cannot be resolved from context.

## Goal mapping

The learner's language may not match Knowledge Model node names.

The planner should map:

- topic names;
- intended tasks;
- desired depth;
- assessment demands;
- real-world use;
- explicit exclusions.

Example:

```text
Learner request: "I want to understand why Discord voice can still work when packets disappear."

Mapped targets:
- packet
- packet loss
- UDP
- latency trade-offs
- concealment or recovery at the application layer

Capabilities:
- explain
- predict
- compare
```

Mapping confidence must be recorded.

## Target capability set

A learning goal is incomplete without capability requirements.

Possible capabilities:

- recall;
- explain;
- predict;
- compare;
- classify;
- trace;
- calculate;
- apply;
- implement;
- debug;
- transfer;
- teach back;
- retain after delay.

Not every concept needs every capability.

The planner should use the Knowledge Model's required capabilities and narrow them to the learner's goal.

## Prerequisite closure

Prerequisite closure is the set of concepts needed to support the target.

The planner should distinguish:

- hard prerequisites;
- soft prerequisites;
- diagnostic prerequisites;
- vocabulary prerequisites;
- optional enrichment.

Example:

```yaml
prerequisite_closure:
  target: pointer
  hard:
    - memory-location
    - address
  soft:
    - variable
    - type
  diagnostic:
    - stored-value
  optional:
    - pointer-arithmetic
```

Hard prerequisites enter the path unless strong evidence shows they are already available. Soft prerequisites may be compressed, tested, or deferred.

## Student-state overlay

The planner overlays capability-specific Student Model claims onto the prerequisite graph.

Example:

```yaml
concept: address
required_capability: explain
student_state:
  recall: strong
  explain: weak
  apply: unknown
planning_result: include-short-repair
```

A concept is not removed merely because one capability is strong.

Possible planning states:

- `SATISFIED`
- `PARTIALLY_SATISFIED`
- `UNSUPPORTED`
- `UNCERTAIN`
- `FRAGILE`
- `MISCONCEPTION_RISK`
- `REVIEW_DUE`
- `OUT_OF_SCOPE`

## Evidence sufficiency

A prerequisite may be considered satisfied only when available evidence matches the capability needed by the target path.

Examples:

- recall evidence may satisfy a vocabulary prerequisite;
- explanation evidence may satisfy a conceptual prerequisite;
- guided success may not satisfy an independent application prerequisite;
- old evidence may require a short review probe;
- contradictory evidence should create a diagnostic checkpoint.

## Minimum viable learning path

The planner should prefer the smallest path that supports the contract.

A path should include only concepts that are:

- required by hard dependency;
- necessary to repair an active misconception;
- required for a target capability;
- needed for valid assessment;
- central to near-term transfer or safety.

Optional historical detail, enrichment, and distant applications should remain outside the core path unless requested.

## Path representation

```yaml
learning_path:
  id: pointers-v0.1
  target: safe-basic-pointer-use
  nodes:
    - id: memory-location
      role: prerequisite
      disposition: diagnostic-check
    - id: address
      role: prerequisite
      disposition: short-repair
    - id: pointer
      role: core
      disposition: full-instruction
    - id: dereference
      role: core
      disposition: full-instruction
    - id: null-pointer
      role: safety-critical
      disposition: full-instruction
  excluded:
    - pointer-arithmetic
    - function-pointers
  completion_condition: independent explain, trace, apply, and debug evidence
```

## Ordering rules

The planner should order nodes using:

1. hard dependencies;
2. learner gaps;
3. misconception repair needs;
4. cognitive-load control;
5. representation continuity;
6. opportunities for early useful application;
7. transfer requirements;
8. review requirements.

Source order may be used when it aligns with these constraints, but it is not authoritative.

## Alternative paths

Multiple valid learning paths may exist.

Example:

```text
Path A: memory diagram -> address -> pointer -> code
Path B: code behavior -> prediction failure -> memory diagram -> pointer
```

Alternative paths should be preserved when:

- prerequisite relationships are soft;
- the learner has relevant experience;
- time constraints require a compressed route;
- different representations may reduce cognitive load;
- one route has failed previously.

The planner should select one provisional path and record the fallback.

## Lesson phases

A path should be divided into bounded phases.

Recommended phase pattern:

```text
Orientation
Diagnosis
Prerequisite repair
Core model construction
Guided practice
Independent check
Transfer
Consolidation
Review scheduling
```

Not every lesson requires every phase. The planner should omit phases that add no value.

## Learning unit contract

Each unit should define:

```yaml
unit:
  id: pointer-address-distinction
  objective: Explain how a pointer stores an address rather than the destination value.
  concepts:
    - address
    - pointer
    - stored-value
  prerequisites:
    - memory-location
  target_capability: explain
  initial_strategy_candidates:
    - contrastive-svg
    - hotel-address-analogy
    - two-location-counterexample
  expected_evidence:
    - independent explanation
  progression_rule: learner distinguishes all roles without answer-revealing hints
  fallback_route: repair address-location concept
  estimated_minutes: 15
```

The unit contract constrains the Teaching and Assessment Runtimes without dictating final wording.

## Diagnostic checkpoints

Insert a diagnostic checkpoint when:

- prerequisite evidence is missing;
- evidence is old or contradictory;
- a common misconception is likely to block the target;
- the learner claims prior knowledge without performance evidence;
- skipping a node could save substantial time;
- several alternative paths depend on the result.

A checkpoint should test the smallest relevant claim.

## Practice allocation

Practice should be allocated by target capability and evidence fragility.

Possible practice sequence:

```text
worked example
    ↓
completion task
    ↓
guided application
    ↓
independent application
    ↓
near transfer
```

The planner should not prescribe a fixed number of exercises. Practice continues until the progression condition is supported or the plan replans.

## Transfer planning

Transfer tasks should be included only when required by the learning contract.

The plan should specify:

- invariant relationship;
- surface features to vary;
- intended transfer distance;
- prerequisites for attempting transfer;
- success condition.

Far transfer should not appear before stable same-structure or near-transfer performance unless used diagnostically.

## Review planning

The planner should insert review checkpoints for:

- central prerequisites;
- high-misconception-risk concepts;
- safety-critical concepts;
- capabilities requiring retention;
- concepts learned only through immediate performance;
- recurring errors.

Review timing remains provisional and should be updated by the Review Runtime using actual evidence.

## Time estimation

Time estimates should be ranges, not false precision.

Example:

```yaml
estimated_time:
  lower_minutes: 75
  upper_minutes: 135
  uncertainty: high
  assumptions:
    - memory-location prerequisite is mostly available
    - no recurring pointer-value misconception
```

Estimation factors:

- number of unsupported hard prerequisites;
- abstraction and notation burden;
- misconception risk;
- required capability depth;
- transfer distance;
- review horizon;
- learner evidence quality;
- accessibility needs;
- available session length.

## Cognitive-load planning

The planner should control:

- number of new concepts per unit;
- number of interacting relationships;
- notation introduced at once;
- representation changes;
- prerequisite detour depth;
- complexity of examples;
- amount of optional context.

When intrinsic complexity is high, the plan should use external representations, smaller units, and staged integration.

## Compression policy

When time is limited, the planner may compress:

- soft prerequisites;
- historical context;
- optional examples;
- distant transfer;
- teach-back;
- long review horizons.

It must not silently compress:

- safety-critical content;
- hard prerequisites;
- required assessment capabilities;
- source uncertainty;
- misconception repair required for correct application.

The plan should state what has been deferred and the consequence.

## Deadline modes

### Durable-learning mode

Prioritizes:

- mental-model construction;
- independent performance;
- transfer;
- delayed review.

### Exam-readiness mode

Prioritizes:

- assessed capability coverage;
- representative item types;
- high-frequency errors;
- deadline-aware review.

### Immediate-task mode

Prioritizes:

- minimum prerequisites;
- task-specific application;
- safety and correctness;
- explicit limitations of the compressed path.

A compressed task path must not be described as complete domain mastery.

## Source coverage

The planner should track which source sections support each path node.

```yaml
coverage:
  pointer:
    source_refs:
      - notes-page-12
      - textbook-section-4.2
    confidence: high
  null-pointer:
    source_refs:
      - notes-page-16
    confidence: medium
```

When source coverage is missing:

- label enrichment separately;
- verify externally when allowed and needed;
- narrow the goal;
- or enter a blocked state.

## Source conflict handling

When sources disagree, the plan should:

- preserve competing claims;
- identify context or scope;
- avoid sequencing one claim as settled truth;
- include a clarification unit when the difference matters;
- defer the conflict when irrelevant to the learner's goal.

## Safety-aware planning

The planner should include safety-critical prerequisites and constraints even when they increase path length.

Examples:

- null and lifetime risks in pointer instruction;
- authorization boundaries in cybersecurity labs;
- contraindications and uncertainty in health education;
- unit and sign conventions in engineering calculations.

Safety metadata comes from the Knowledge Model and policy layer.

## Plan adaptation

The plan must be revised when:

- a prerequisite check fails;
- a misconception is discovered;
- learner performance exceeds assumptions;
- cognitive overload persists;
- strategy predictions fail repeatedly;
- time or deadline changes;
- the learner changes goals;
- new source material appears;
- review evidence shows fragility;
- a source conflict or safety issue emerges.

Replanning should preserve completed evidence and avoid restarting the course unnecessarily.

## Replanning operations

Supported operations:

- insert prerequisite repair;
- remove satisfied node;
- shorten or expand a unit;
- change representation route;
- split a high-load unit;
- merge redundant units;
- postpone transfer;
- promote review priority;
- change completion criteria;
- mark plan blocked;
- switch to an alternative path.

## Plan versioning

```yaml
plan_version:
  plan_id: pointers-v0.1
  version: 3
  previous_version: 2
  reason: diagnostic evidence showed address-location misconception
  changes:
    - inserted address-location-repair
    - postponed pointer-application
  evidence_refs:
    - evidence-410
```

Plan changes must remain auditable.

## Completion policy

A path is complete only when the learning contract's required capabilities have matching evidence.

Completion may be:

- `GOAL_MET`
- `GOAL_PARTIALLY_MET`
- `COMPRESSED_PATH_COMPLETE`
- `BLOCKED`
- `ABANDONED_BY_LEARNER`
- `REVIEW_PENDING`

The planner should not equate content exposure with completion.

## Example plan

```yaml
lesson_plan:
  id: tcp-loss-path-v0.1
  contract:
    goal: Predict what happens when a packet is lost in TCP and UDP applications.
    capabilities: [explain, predict, compare]
  assumptions:
    - packet concept is available
    - basic client-server vocabulary is available
  path:
    - unit: packet-and-loss-diagnostic
      type: diagnostic
      estimated_minutes: 5
    - unit: reliability-latency-tradeoff
      type: concept-construction
      estimated_minutes: 12
    - unit: tcp-retransmission-sequence
      type: svg-plus-prediction
      estimated_minutes: 15
    - unit: udp-application-responsibility
      type: contrastive-example
      estimated_minutes: 12
    - unit: discord-voice-near-transfer
      type: transfer
      estimated_minutes: 10
    - unit: cumulative-check
      type: assessment
      estimated_minutes: 8
  completion_condition:
    - independent causal explanation
    - correct prediction in changed context
    - accurate TCP versus UDP comparison
  deferred:
    - congestion-control-details
    - codec-specific-loss-concealment
```

## Runtime record

```yaml
planning_cycle:
  id: planning-cycle-12
  learner_goal_ref: goal-8
  target_concepts:
    - pointer
    - dereference
  target_capabilities:
    - explain
    - apply
    - debug
  prerequisite_nodes_considered: 7
  nodes_included: 5
  nodes_deferred: 2
  selected_path: pointers-v0.1
  alternative_path: code-first-pointers-v0.1
  estimated_minutes:
    lower: 75
    upper: 135
  major_uncertainty:
    - address-explanation-capability
  first_checkpoint: address-location-diagnostic
  replanning_triggers:
    - failed-prerequisite-check
    - recurring-pointer-value-confusion
```

## Invariants

1. Every plan begins with a bounded learning contract.
2. Every path node maps to a Knowledge Model concept or relationship.
3. Every required concept has a stated role.
4. Hard prerequisites cannot be skipped without explicit justification.
5. Source order is not treated as prerequisite order.
6. Student Model capabilities determine whether a node is satisfied.
7. Unknown prerequisite state creates diagnosis, not assumption.
8. Optional enrichment remains outside the core path by default.
9. Every unit has a progression condition.
10. Completion requires evidence, not exposure.
11. Time estimates state assumptions and uncertainty.
12. Replanning preserves evidence and plan history.
13. No hidden learner state exists inside the plan.
14. Safety-critical nodes cannot be compressed silently.

## Failure modes

### Chapter-order planning

Problem: the source table of contents becomes the lesson path.

Prevention: prerequisite closure and Student Model overlay.

### Prerequisite explosion

Problem: every related topic is treated as required background.

Prevention: hard, soft, diagnostic, and optional classifications.

### False personalization

Problem: the plan changes based on inferred preferences rather than evidence.

Prevention: capability-specific learner state and tentative strategy history.

### Content exposure as completion

Problem: finishing every section is treated as learning success.

Prevention: evidence-based completion criteria.

### Rigid plan execution

Problem: the path remains unchanged after new evidence.

Prevention: explicit replanning triggers and versioning.

### Time-estimate theater

Problem: the system presents precise durations without support.

Prevention: ranges, assumptions, and uncertainty.

### Enrichment drift

Problem: interesting context displaces the target goal.

Prevention: deferred enrichment list and curiosity budget.

### Capability collapse

Problem: one concept is planned once despite several required capabilities.

Prevention: concept-capability pairs in path nodes.

### Safety compression

Problem: deadline pressure removes essential safety content.

Prevention: non-compressible safety-critical nodes.

### Overassessment

Problem: frequent checkpoints interrupt learning without changing decisions.

Prevention: insert diagnostics only where uncertainty has planning value.

## Verification scenarios

A compliant implementation should pass these scenarios:

1. A textbook's chapter order conflicts with prerequisites, and the planner follows the prerequisite graph.
2. Strong explanation evidence removes a prerequisite explanation unit but retains an application check when application is unknown.
3. Missing evidence creates a diagnostic checkpoint rather than an assumption of weakness.
4. A short deadline compresses optional enrichment but preserves safety-critical content.
5. A failed prerequisite check inserts the smallest repair path.
6. A learner who exceeds assumptions skips redundant instruction.
7. Repeated overload causes a unit to split and use external representation.
8. Near-transfer is included only when the contract requires transfer.
9. Completion is withheld when content was covered but independent evidence is missing.
10. Every plan revision records its trigger and evidence references.
11. Conflicting sources remain visible rather than merged silently.
12. The planner can produce an alternative route after a strategy family fails.

## Minimal compliance

A minimal Lesson Planner Runtime must:

- normalize a learning goal;
- map it to concepts and capabilities;
- compute hard and soft prerequisite closure;
- overlay capability-specific Student Model state;
- generate a minimum viable path;
- insert diagnostics for important unknowns;
- define unit progression conditions;
- estimate time with uncertainty;
- include review and transfer only when required;
- support evidence-triggered replanning;
- preserve source provenance and safety constraints.

## Guiding question

> What is the smallest evidence-aware sequence of concepts and capabilities that can move this learner from their current state to the requested outcome without skipping essential dependencies or teaching unnecessary material?
