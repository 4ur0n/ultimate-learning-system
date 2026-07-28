# End-to-End Example: C Pointers

This example shows how ULS turns a learning goal into a model-grounded lesson, gathers evidence, updates learner state conservatively, and prepares a publishable artifact.

---

## 1. Learning contract

```yaml
lesson_id: c-pointers-001
mode: durable-learning
audience: beginner C learner
estimated_minutes:
  lower: 35
  upper: 55
  uncertainty: medium
learning_contract:
  goal: Explain what a C pointer stores, trace one dereference, and distinguish an address from the value stored at that address.
  target_concepts:
    - c.object
    - c.address
    - c.pointer-object
    - c.address-of-operator
    - c.dereference-operator
  required_capabilities:
    - explain
    - trace
    - apply
  target_depth: conceptual
  completion_condition: The learner independently explains the address/value distinction and correctly traces a fresh pointer example without answer exposure.
```

This contract does not establish pointer arithmetic, dynamic memory management, or arbitrary pointer debugging.

## 2. Knowledge Model slice

| Concept | Role | Required capability |
|---|---|---|
| `c.object` | prerequisite | explain |
| `c.address` | prerequisite | explain |
| `c.pointer-object` | core | explain |
| `c.address-of-operator` | core | apply |
| `c.dereference-operator` | core | trace |

```text
c.object --HAS--> c.address
c.pointer-object --STORES--> c.address
c.address-of-operator --PRODUCES--> c.address
c.dereference-operator --FOLLOWS--> c.address
c.dereference-operator --ACCESSES--> c.object
```

Misconception candidates:

```yaml
- id: ptr-is-target-value
  claim: A pointer stores the target value directly.
- id: address-equals-value
  claim: An address and the value stored there are interchangeable.
- id: star-is-decoration
  claim: Dereference syntax does not perform an access operation.
```

These are hypotheses to test, not fixed learner labels.

## 3. Initial Student Model

```yaml
student_snapshot:
  known:
    - concept: c.object
      capability: explain
      level: moderate
      evidence_refs: [prior-c-vars-01]
  uncertain:
    - concept: c.address
      capability: explain
      reason: No direct evidence.
    - concept: c.pointer-object
      capability: explain
      reason: New target concept.
  fragile: []
  active_misconception_signals: []
```

Teacher hypothesis:

```yaml
id: h-address-gap
claim: The learner may understand variables but not distinguish an object's address from its stored value.
confidence: 0.55
next_test: Ask for the value and address of one object separately.
```

## 4. Lesson path

```yaml
units:
  - unit_id: u1-diagnose-address
    role: diagnostic
    objective: Distinguish a stored value from a location identifier.
    progression_rule: Learner identifies value and address as different properties.
    fallback_route: u2-repair-object-location

  - unit_id: u2-repair-object-location
    role: prerequisite
    objective: Build the object/value/address model.
    progression_rule: Learner explains all three terms in one example.
    fallback_route: u2-alternative-representation

  - unit_id: u3-introduce-pointer
    role: core
    objective: Explain that a pointer object stores an address.
    progression_rule: Learner distinguishes pointer value from target value.
    fallback_route: u3-pointer-table

  - unit_id: u4-trace-dereference
    role: practice
    objective: Trace dereference from pointer to target object.
    progression_rule: Learner independently follows a fresh mapping.
    fallback_route: u4-guided-trace

  - unit_id: u5-apply
    role: assessment
    objective: Predict the effect of writing through a pointer.
    progression_rule: Learner predicts the changed target value and unchanged pointer address.
    fallback_route: u5-remediate-write-through

  - unit_id: u6-transfer
    role: transfer
    objective: Apply the model with different names and addresses.
    progression_rule: Learner preserves the address/value distinction.
```

## 5. Diagnostic interaction

```c
int x = 42;
```

Suppose `x` is stored at address `0x200`.

**Prompt:** What is the value of `x`, and what is its address?

Expected evidence:

- value: `42`
- address: `0x200`

Runtime routing:

```text
Correct distinction -> u3-introduce-pointer
Confuses 42 and 0x200 -> u2-repair-object-location
Strong hint required -> record prompted evidence, then continue with repair
```

## 6. Core explanation

A C object has a location and a value stored at that location.

```text
object:  x
address: 0x200
value:   42
```

A pointer is another object whose value is an address.

```c
int x = 42;
int *p = &x;
```

Conceptually:

```text
object p stores 0x200
0x200 identifies object x
object x stores 42
```

Therefore:

- `p` evaluates to `0x200`;
- `*p` follows `0x200` to access `x`;
- reading `*p` produces `42` in this state.

> The pointer stores the address. The target object stores the target value.

## 7. Formal model

For a valid pointer `p` that points to object `x`:

```text
value(p) = address(x)
object_at(value(p)) = x
value(*p) = value(x)
```

After:

```c
*p = 99;
```

we have:

```text
value(x) = 99
value(p) = address(x)
```

The write changes the target object's value. It does not change which address `p` stores.

## 8. Instructional SVG specification

```yaml
svg_id: c-pointer-memory-model-v1
diagram_type: flow
instructional_objective: Show that a pointer stores an address and dereference follows that address to access another object.
entities:
  - id: pointer-p
    label: p
    semantic_role: object
  - id: address-value
    label: 0x200
    semantic_role: value
  - id: target-x
    label: x
    semantic_role: object
  - id: target-value
    label: 42
    semantic_role: value
relationships:
  - source: pointer-p
    target: address-value
    type: stores
    direction: forward
  - source: address-value
    target: target-x
    type: identifies
    direction: forward
  - source: target-x
    target: target-value
    type: stores
    direction: forward
interpretation_task:
  prompt: Which object stores the address, and which object stores 42?
  target_capability: explain
  expected_observation: p stores 0x200; x stores 42.
accessibility:
  non_color_encoding: true
  logical_reading_order: true
  contrast_requirement: WCAG-AA
```

The SVG is justified because the learner must coordinate two objects, two stored values, and one access path.

## 9. Guided trace

```c
int x = 42;
int *p = &x;
int y = *p;
```

Trace:

1. `&x` produces the address of `x`.
2. `p` stores that address.
3. `*p` follows the stored address.
4. The access reaches `x`.
5. The current value of `x`, `42`, is copied into `y`.

Check:

> Does `y` store the address of `x`, or the value read from `x`?

Expected answer: `y` stores the value `42`.

## 10. Independent assessment

```yaml
assessment_id: c-pointer-check-01
purpose: progression-check
item:
  item_id: ptr-write-01
  task_type: extended-explanation
  target_concepts:
    - c.pointer-object
    - c.dereference-operator
  target_capability: apply
  transfer_distance: same-structure
  prompt: |
    Assume a is stored at 0x500.

    int a = 7;
    int *q = &a;
    *q = 11;

    After all statements run, what does q store, what does a store, and why?
  success_condition: Learner states that q still stores 0x500 and a stores 11 because dereference writes to the target object.
assistance_policy:
  allowed: true
  max_hint_level: 5
  answer_exposure_invalidates_independence: true
```

Rubric:

| Criterion | Complete | Partial | Absent |
|---|---|---|---|
| Pointer value | States `q` stores `0x500` | Says it stores an address without identifying it | Says it stores `11` |
| Target value | States `a` stores `11` | Gives `11` without connecting it to `a` | Incorrect |
| Mechanism | Explains that `*q` follows the address and writes to `a` | Mentions dereference without mechanism | No valid explanation |
| Independence | No answer exposure and no structural hint | Prompted or partially guided | Full solution exposed |

## 11. Hint ladder

```yaml
level_1: Identify the two objects in the program.
level_2: Ask what value is stored in q immediately after &a.
level_3: Trace the address stored in q to the object at that address.
level_4: Separate the value of q from the value of *q.
level_5: The assignment through *q changes the object reached through q.
level_6: q stores 0x500; a stores 11 because *q accesses a.
```

Level 6 requires a fresh item before independent capability can be claimed.

## 12. Evidence example

Learner response:

> `q` still holds `0x500`. The expression `*q` goes to the object at that address, so assigning 11 changes `a` to 11. It does not replace the address inside `q`.

Evidence record:

```yaml
evidence_id: ev-ptr-001
capability: apply
correctness: correct
completeness: complete
independence: independent
hints_used: 0
highest_hint_level: 0
answer_exposure: none
transfer_distance: same-structure
claims_supported:
  - A pointer object can store a target object's address.
  - Dereference can access and modify the target object.
  - Pointer value and target value remain distinct.
claims_weakened:
  - ptr-is-target-value
misconception_signals: []
evaluator:
  type: rubric-based-model
  confidence: 0.94
```

## 13. Student Model update

```yaml
updates:
  - concept: c.pointer-object
    capability: explain
    previous_state: uncertain
    new_state: moderate
    evidence_refs: [ev-ptr-001]
  - concept: c.dereference-operator
    capability: apply
    previous_state: uncertain
    new_state: moderate
    evidence_refs: [ev-ptr-001]
limitations:
  - Delayed retention has not been measured.
  - Far transfer has not been measured.
  - Pointer arithmetic has not been assessed.
```

The runtime does not mark global pointer mastery.

## 14. Transfer task

```c
int score = 80;
int *selected = &score;
```

Assume `score` is at address `0x900`.

**Prompt:** Predict the result of `*selected = *selected + 5;`. State the final value stored by `selected` and the final value stored by `score`.

Expected evidence:

```text
selected stores 0x900
score stores 85
```

This is near transfer because the surface names and operation change while the same pointer relationship remains.

## 15. Review plan

```yaml
review:
  - concept: c.pointer-object
    capability: explain
    reason: central
    earliest_after_seconds: 86400
    latest_after_seconds: 259200
    retrieval_before_restudy: true
  - concept: c.dereference-operator
    capability: apply
    reason: retention-required
    earliest_after_seconds: 259200
    latest_after_seconds: 604800
    retrieval_before_restudy: true
```

Review items should use fresh addresses and variable names.

## 16. Publishing artifact

```yaml
artifact_id: c-pointers-learner-handout-v1
type: printable-handout
audience: learner
format: pdf
purpose: Help a beginner distinguish pointer value from target value and trace one dereference.
content_blocks:
  - objective
  - object-value-address-model
  - formal-pointer-model
  - instructional-svg
  - guided-trace
  - independent-check
  - summary
  - review-prompts
excluded_content:
  - teacher-hypotheses
  - raw-student-model
  - evaluator-confidence
  - internal-routing-scores
privacy:
  profile: learner-safe
  contains_personal_data: false
accessibility:
  alt_text: true
  non_color_encoding: true
  selectable_text: true
  print_readability: true
```

Learner-facing summary:

> A pointer is an object whose value is an address. Dereference follows that address to access the target object. Changing `*p` changes the target object's value; it does not necessarily change the address stored in `p`.

## 17. Runtime trace

```text
Learning Contract
  -> Knowledge Model slice
  -> Unknown address capability detected
  -> Diagnostic item
  -> Core pointer model
  -> Guided trace
  -> Independent application item
  -> Evidence record
  -> Conservative Student Model update
  -> Near-transfer task
  -> Review scheduling
  -> Privacy-safe publishing artifact
```

## 18. What this example demonstrates

This example shows that ULS:

1. starts from an observable capability contract;
2. models knowledge as concepts and relationships;
3. treats learner state as evidence-backed and uncertain;
4. selects instruction through runtime decisions;
5. records assistance and answer exposure;
6. updates learner state conservatively;
7. separates immediate performance from retention and transfer;
8. publishes only audience-appropriate state.

It also demonstrates the core architectural rule:

> Runtime chooses. The language model writes.
