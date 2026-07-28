# Review Runtime

## Purpose

The Review Runtime decides what should be revisited, when, in which form, and with which success criteria so that learning remains retrievable, connected, and transferable over time.

It does not simply repeat previous lessons. It uses the Knowledge Model, Student Model, Teacher Model, and Evidence Model to select the smallest review action that can strengthen or re-evaluate a fragile claim.

## Core principle

> Review is delayed evidence collection guided by forgetting risk and knowledge importance.

## Responsibilities

The Review Runtime must:

- identify concepts and capabilities at risk of becoming unavailable;
- prioritize review using evidence, importance, dependency, and deadlines;
- distinguish review from remediation and re-teaching;
- schedule retrieval at appropriate intervals;
- vary context and representation without changing the target construct;
- preserve independence and answer-exposure data;
- update retention, fragility, transfer, and misconception claims;
- stop reviewing concepts that are stable enough for the current goal;
- avoid creating an endless queue of low-value repetition.

It must not maintain an unaudited memory score outside the four core models.

## Inputs

Required inputs:

- Student Model review state;
- Knowledge Model concept graph and prerequisite centrality;
- Evidence Model history;
- Teacher Model current goal, constraints, and review policy;
- current date or elapsed interval;
- learner availability and deadline constraints.

Optional inputs:

- preferred session length;
- assessment stakes;
- domain-specific retention requirements;
- review medium;
- accessibility needs;
- tool availability.

## Outputs

Each cycle produces:

1. a prioritized review queue;
2. one bounded review objective;
3. one review task specification;
4. expected evidence;
5. schedule update;
6. Student Model and Teacher Model update proposals;
7. route to continue, postpone, remediate, re-teach, transfer, or complete.

## Review loop

```text
READ REVIEW STATE
        ↓
COMPUTE ELIGIBLE CONCEPTS
        ↓
ESTIMATE REVIEW PRIORITY
        ↓
SELECT ONE TARGET
        ↓
CHOOSE RETRIEVAL FORM
        ↓
DELIVER TASK
        ↓
CAPTURE EVIDENCE
        ↓
UPDATE RETENTION AND FRAGILITY
        ↓
RESCHEDULE OR ROUTE
```

## Review eligibility

A concept becomes eligible when one or more conditions hold:

- scheduled review time has arrived;
- evidence is old relative to the target requirement;
- prior mastery is supported only by immediate performance;
- delayed retrieval has not yet been observed;
- the concept is a central prerequisite for an upcoming objective;
- contradictory evidence appears;
- a misconception has a history of recurrence;
- transfer evidence is weak;
- the learner has an approaching deadline;
- the learner explicitly requests review.

Eligibility does not guarantee selection.

## Review priority

Review priority should consider:

- concept importance;
- prerequisite centrality;
- fragility of evidence;
- time since last independent retrieval;
- history of retrieval success and failure;
- misconception recurrence risk;
- transfer weakness;
- target deadline;
- future learning-path dependency;
- estimated review cost;
- learner fatigue and session budget.

A conceptual priority model:

```text
priority =
  importance
  + prerequisite_centrality
  + fragility
  + deadline_pressure
  + misconception_risk
  + transfer_gap
  - recent_stability
  - review_cost
```

This is a ranking aid, not a universal memory formula.

## Fragility classification

Suggested states:

- `UNTESTED_DELAY`
- `STABLE`
- `FRAGILE`
- `DECAYING`
- `RECURRING_ERROR`
- `RETEACH_REQUIRED`
- `REVIEW_DUE`
- `DEFERRED`

A concept may be strong in one capability and fragile in another.

Example:

```yaml
concept: pointer
review_state:
  recall: stable
  explain: fragile
  apply: review-due
  transfer: untested-delay
```

## Review target

Every review action must name:

- concept;
- capability;
- independence requirement;
- retention interval;
- expected transfer distance;
- success condition.

Example:

```yaml
review_target:
  concept: pointer
  capability: explain
  independence: independent
  retention_interval_days: 7
  transfer_distance: near
  success_condition: distinguish pointer, address, and value without hints
```

## Review forms

Supported forms include:

- free retrieval;
- one-question explanation;
- mixed-concept comparison;
- prediction;
- trace or simulation;
- debugging;
- worked-example completion;
- near-transfer task;
- far-transfer task;
- teach-back;
- cumulative mini-check;
- concept-map reconstruction.

The runtime should select the smallest valid form for the target capability.

## Retrieval-first policy

Review should normally begin with retrieval before re-exposure.

Recommended sequence:

1. ask the learner to retrieve or apply;
2. capture confidence and independence when useful;
3. evaluate the response;
4. provide feedback;
5. re-teach only if evidence supports the need;
6. test again after repair.

Showing the answer before retrieval creates exposure evidence, not retention evidence.

## Spacing policy

The next interval should depend on evidence quality rather than a fixed schedule alone.

Increase the interval when:

- retrieval is independent and correct;
- reasoning is complete;
- the learner succeeds after meaningful delay;
- performance generalizes across context or representation;
- confidence is calibrated.

Shorten the interval when:

- retrieval fails;
- success requires hints;
- the concept is central to upcoming learning;
- a high-confidence misconception recurs;
- evidence conflicts;
- performance is correct but fragile or incomplete.

Do not claim optimal intervals without validated learner-specific evidence.

## Interleaving policy

Interleaving is useful when discrimination among related concepts is a target.

Use interleaving to:

- compare confusable concepts;
- select among procedures;
- recognize which principle applies;
- connect prerequisite chains;
- prevent surface-pattern matching.

Avoid interleaving when:

- the learner has not yet formed the basic individual concepts;
- switching costs exceed the diagnostic value;
- the task becomes linguistically or procedurally overloaded.

## Variation policy

Review should vary nonessential surface features while preserving the target relationship.

Possible variation:

- numbers;
- names;
- application context;
- representation;
- problem order;
- diagram orientation;
- code syntax details that do not alter the concept.

Variation must not accidentally increase transfer distance beyond the intended level.

## Cumulative review

Cumulative review may combine:

- recently learned concepts;
- older central prerequisites;
- known misconception pairs;
- concepts needed for the next unit.

Each item must still map to explicit claims. A cumulative review must not collapse all outcomes into one score.

## Review versus remediation

### Review

Use when the learner previously demonstrated the target capability and the current question is availability after time or context change.

### Remediation

Use when evidence shows a current gap, misconception, or unstable procedure.

### Re-teaching

Use when the original mental model was not established or has substantially collapsed.

Routing rule:

```text
successful prior evidence + delayed uncertainty -> REVIEW
structural recurring error -> REMEDIATE
missing prerequisite or collapsed model -> RETEACH
```

## Review evidence

Every review task must create Evidence Model records containing:

- elapsed time since instruction;
- elapsed time since last successful independent retrieval;
- whether the item or structure was previously seen;
- access to notes or examples;
- target capability;
- transfer distance;
- hint usage;
- correctness, completeness, and reasoning;
- learner confidence when available;
- evaluator confidence.

## Retention update

Retention claims should be capability-specific.

Example:

```yaml
claim: student:pointer:explain:retained
status: provisionally-supported
confidence: 0.77
supporting_evidence:
  - evidence-301
retention_intervals_days:
  - 7
contexts:
  - code
  - diagram
```

One delayed success supports retention at that interval and context; it does not prove permanent memory.

## Forgetting and decay

The runtime should infer decay only from evidence or elapsed-risk policy.

It may mark a concept `REVIEW_DUE` because evidence is old, but should not mark the learner as having forgotten without an observation.

Distinguish:

- expected risk of forgetting;
- observed retrieval failure;
- observed partial retrieval;
- observed misconception recurrence.

## Misconception recurrence

When a previously repaired misconception reappears:

1. create new evidence;
2. mark the misconception as recurring;
3. compare contexts and representations;
4. determine whether repair was surface-level;
5. select a stronger contrast, counterexample, or generative explanation;
6. shorten review interval;
7. test transfer after repair.

Do not merely repeat the original correction.

## Review queue

Suggested structure:

```yaml
review_queue:
  generated_at: 2026-07-28T10:00:00Z
  items:
    - concept: pointer
      capability: explain
      priority: high
      reason:
        - seven-days-since-retrieval
        - central-prerequisite
        - prior-misconception
      estimated_minutes: 3
      recommended_form: contrastive-explanation
    - concept: tcp-retransmission
      capability: predict
      priority: medium
      reason:
        - transfer-untested
      estimated_minutes: 4
      recommended_form: packet-loss-prediction
```

Queue generation should be deterministic enough to audit but flexible enough to adapt.

## Queue selection policy

Select the next item using:

1. deadline-critical prerequisites;
2. high-impact fragile concepts;
3. recurring misconceptions;
4. review-due core concepts;
5. transfer gaps;
6. optional enrichment.

Within equal priority, prefer:

- shorter high-value checks;
- concepts that unlock several future nodes;
- varied task forms to manage fatigue;
- opportunities to interleave meaningful contrasts.

## Session composition

A review session should remain bounded.

Example composition:

```text
1 high-priority delayed retrieval
1 prerequisite-central check
1 misconception contrast
1 transfer item, if readiness exists
```

The exact composition should depend on available time and learner state.

Avoid exhausting the queue in one session merely because items are due.

## Feedback policy

After successful retrieval:

- confirm the correct relationship;
- avoid unnecessary full re-explanation;
- briefly strengthen organization or transfer when useful;
- schedule the next interval.

After partial retrieval:

- identify the missing relationship;
- provide the smallest cue;
- ask for active completion;
- record assistance.

After failure:

- distinguish retrieval failure from conceptual failure;
- route to cue, remediation, or re-teaching;
- test again after repair.

## Scheduling record

```yaml
review_schedule:
  concept: pointer
  capability: explain
  last_review: 2026-07-28
  outcome: independent-correct
  next_review_window:
    earliest: 2026-08-11
    latest: 2026-08-18
  rationale:
    - successful-seven-day-retrieval
    - near-transfer-success
  confidence: medium
```

A review window is preferable to false precision when the system lacks validated scheduling parameters.

## Deadlines

When a learner has a deadline, review policy may compress intervals and prioritize coverage.

The runtime must distinguish:

- long-term retention objective;
- exam readiness;
- short-term performance objective.

Cramming may improve near-term availability but should not be described as equivalent to durable learning.

## Learner control

The learner should be able to:

- view due concepts;
- postpone a review;
- request a topic;
- challenge the model's claim of fragility;
- reduce session length;
- reset review history;
- mark constraints such as an upcoming test.

Postponement should reschedule the item, not create negative evidence.

## Runtime record

```yaml
review_cycle:
  id: review-cycle-18
  queue_snapshot: queue-2026-07-28
  selected_target:
    concept: pointer
    capability: explain
  selection_reason:
    - high-priority
    - central-prerequisite
    - prior-misconception
  task: pointer-delay-check-02
  retention_interval_days: 7
  evidence_created:
    - evidence-301
  result: independent-correct
  route: RESCHEDULE
  next_review_window:
    earliest: 2026-08-11
    latest: 2026-08-18
```

## Invariants

1. Review priority is derived from model state and evidence.
2. A due date is not evidence of forgetting.
3. Review begins with retrieval when valid.
4. Re-exposure does not count as independent retention.
5. Review outcomes remain capability-specific.
6. Delayed success is recorded with its interval and context.
7. Failed review does not automatically erase all prior evidence.
8. Recurring misconceptions trigger stronger repair, not identical repetition.
9. Queue items map to explicit claims.
10. Learner postponement is not failure evidence.
11. No hidden memory-strength score exists outside the models.
12. Scheduling uncertainty must remain visible.

## Failure modes

### Calendar-only review

Problem: concepts are reviewed because a timer expired, regardless of value.

Prevention: combine timing with importance, fragility, and dependency.

### Re-reading masquerading as review

Problem: the learner sees the explanation again without retrieval.

Prevention: retrieval-first policy.

### Fixed-interval dogma

Problem: one schedule is treated as universally optimal.

Prevention: evidence-guided intervals and visible uncertainty.

### Queue inflation

Problem: every concept remains permanently due.

Prevention: bounded goals, stable-state thresholds, and pruning.

### Capability collapse

Problem: recalled vocabulary is treated as retained application skill.

Prevention: capability-specific review targets.

### Repetition without variation

Problem: the learner memorizes an item instead of the underlying relationship.

Prevention: controlled variation and transfer checks.

### Misdiagnosed forgetting

Problem: a confusing task is interpreted as memory decay.

Prevention: task validation and competing hypotheses.

### Punitive postponement

Problem: delaying a review lowers learner state.

Prevention: postponement changes schedule only.

## Verification scenarios

A compliant implementation should pass these scenarios:

1. A concept becomes due without being marked forgotten.
2. Re-reading an answer does not create retention evidence.
3. Independent retrieval after seven days strengthens retention at that interval.
4. A failed delayed task with ambiguous wording is invalidated or retested.
5. A recurring misconception routes to stronger remediation.
6. Stable recall and fragile application remain separate.
7. Near-transfer review success does not establish far transfer.
8. A learner postponing review receives no negative evidence.
9. Review priority favors a central prerequisite over low-value enrichment.
10. Every rescheduling decision cites evidence and rationale.

## Minimal compliance

A minimal Review Runtime must:

- identify review-eligible claims;
- prioritize by evidence, importance, and timing;
- select one bounded target;
- use retrieval before answer exposure;
- record retention interval and assistance;
- distinguish review, remediation, and re-teaching;
- update evidence-backed fragility and retention claims;
- reschedule with visible rationale;
- support postponement and stopping;
- avoid hidden memory scores.

## Guiding question

> Which delayed retrieval opportunity will most improve the learner's future access to important knowledge at the lowest necessary review cost?
