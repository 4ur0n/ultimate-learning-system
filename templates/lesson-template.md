# ULS Lesson Template

Use this template to design one bounded, evidence-aware lesson. Replace every placeholder before execution.

---

## 1. Lesson identity

```yaml
lesson:
  lesson_id: <identifier>
  version: 1
  title: <learner-facing title>
  language: <language>
  audience: <learner profile without unsupported labels>
  mode: durable-learning | exam-readiness | immediate-task
  estimated_minutes:
    lower: <number>
    upper: <number>
    uncertainty: low | medium | high
```

## 2. Learning contract

```yaml
learning_contract:
  goal: <observable learner outcome>
  target_concepts:
    - <concept-id>
  required_capabilities:
    - recall | explain | predict | compare | trace | apply | debug | transfer | teach_back | retain
  target_depth: overview | conceptual | practical | advanced
  source_scope:
    - <source-id>
  time_budget_minutes: <number-or-null>
  deadline: <timestamp-or-null>
  completion_condition: <evidence-based condition>
```

Checklist:

- [ ] The goal describes what the learner will be able to do.
- [ ] The target capabilities are explicit.
- [ ] Completion requires evidence, not exposure.
- [ ] Time and source limits are visible.

## 3. Knowledge Model slice

```yaml
knowledge_slice:
  target_nodes:
    - id: <concept-id>
      role: core | prerequisite | safety-critical | transfer
      required_capabilities:
        - <capability>
  prerequisite_closure:
    hard:
      - <concept-id>
    soft:
      - <concept-id>
    diagnostic:
      - <concept-id>
    vocabulary:
      - <concept-id>
    optional:
      - <concept-id>
  key_relationships:
    - source: <concept-id>
      type: REQUIRES | CAUSES | PRECEDES | CONTRASTS_WITH | PART_OF | APPLIED_IN
      target: <concept-id>
  misconception_candidates:
    - <misconception-id>
  source_refs:
    - <source-id-and-locator>
  unresolved_conflicts: []
```

## 4. Student Model snapshot

Record only evidence-backed state.

```yaml
student_snapshot:
  known:
    - concept: <concept-id>
      capability: <capability>
      level: weak | moderate | strong
      evidence_refs:
        - <evidence-id>
  uncertain:
    - concept: <concept-id>
      capability: <capability>
      reason: <why unknown>
  fragile:
    - concept: <concept-id>
      capability: <capability>
      evidence_refs:
        - <evidence-id>
  active_misconception_signals:
    - <misconception-id>
  privacy_constraints:
    - <constraint>
```

Do not infer fixed ability, motivation, diagnosis, or learning style.

## 5. Teacher Model initialization

```yaml
teacher_state:
  phase: ORIENT | DIAGNOSE | BUILD_PREREQUISITE | INTRODUCE | MODEL | GUIDED_PRACTICE | CHECK | REMEDIATE | INDEPENDENT_PRACTICE | TRANSFER | CONSOLIDATE | REVIEW
  current_objective:
    concept: <concept-id>
    capability: <capability>
    success_condition: <observable condition>
  active_hypotheses:
    - id: <hypothesis-id>
      claim: <tentative claim>
      confidence: <0-to-1>
      next_test: <smallest discriminating task>
  selected_strategy: <strategy-id>
  fallback_strategy: <meaningfully different strategy-id>
  progression_rule: <condition>
  budgets:
    cognitive_load: low | moderate | high
    interaction: low | moderate | high
    visual: unavailable | limited | available
    curiosity: none | low | moderate | high
```

## 6. Lesson path

Each unit must have one bounded objective and one evidence gate.

```yaml
units:
  - unit_id: <identifier>
    role: orientation | diagnostic | prerequisite | core | practice | assessment | transfer | consolidation | review
    objective: <one teachable objective>
    concepts:
      - <concept-id>
    target_capabilities:
      - <capability>
    prerequisite_units: []
    disposition: diagnostic-check | short-repair | full-instruction | guided-practice | independent-practice | near-transfer | far-transfer | review
    strategy_candidates:
      - <strategy-id>
    selected_strategy: <strategy-id>
    expected_evidence:
      capability: <capability>
      independence: fully-guided | partially-guided | prompted | independent | self-corrected-independent
      transfer_distance: same-item | same-structure | near-transfer | far-transfer | novel-domain
    progression_rule: <observable pass condition>
    fallback_route: <unit-id>
    estimated_minutes: <number>
```

## 7. Instructional block

Use only the blocks needed for the current unit.

### Objective

> <State the learner-facing objective in one sentence.>

### Prerequisite activation or diagnostic

**Prompt:** <smallest useful question>

**Expected evidence:** <what the response should reveal>

**Route if supported:** <next unit>

**Route if unsupported:** <repair unit>

### Core explanation

<Explain the key relationship early, using consistent terminology.>

### Formal model

<Provide the formal definition, rule, process, or relationship.>

### Example

<Use one example that exposes the target structure.>

### Contrast or counterexample

<Show a boundary, non-example, or common error when useful.>

### Analogy, if justified

- Source domain: <domain>
- Target domain: <domain>
- Mapping: <explicit mapping>
- Intended inference: <relationship>
- Limitation: <where it breaks>
- Return to formal model: <formal restatement>

### SVG, if justified

```yaml
svg_ref: <svg-spec-id>
instructional_objective: <relationship externalized>
interpretation_question: <question requiring the learner to read the diagram>
```

Do not use a diagram when text is clearer.

## 8. Active check

```yaml
check:
  item_id: <assessment-item-id>
  task_type: free-retrieval | explanation | prediction | comparison | trace | application | debugging | diagram-interpretation | teach-back
  prompt: <prompt>
  target_concepts:
    - <concept-id>
  target_capability: <capability>
  assistance_policy:
    allowed: true | false
    max_hint_level: <0-to-6>
    answer_exposure_invalidates_independence: true | false
  success_condition: <condition>
```

Avoid using “Do you understand?” when performance can be observed.

## 9. Hint ladder

Use the minimum hint needed.

```yaml
hints:
  level_1: <restate objective>
  level_2: <direct attention>
  level_3: <cue prerequisite>
  level_4: <partial structure>
  level_5: <one intermediate step>
  level_6: <complete solution>
```

After level 6, require a new item or delayed retry before claiming independence.

## 10. Evidence capture

```yaml
evidence_record:
  evidence_id: <identifier>
  target_concepts:
    - <concept-id>
  capability: <capability>
  correctness: correct | partially-correct | incorrect | indeterminate | not-attempted
  completeness: complete | partial | minimal | none
  independence: copied | fully-guided | partially-guided | prompted | independent | self-corrected-independent
  hints_used: <number>
  highest_hint_level: <0-to-6>
  answer_exposure: none | partial | full
  learner_confidence: <0-to-1-or-null>
  transfer_distance: same-item | same-structure | near-transfer | far-transfer | novel-domain
  claims_supported: []
  claims_weakened: []
  misconception_signals: []
  evaluator:
    type: deterministic-checker | rubric-based-model | human-reviewer | test-suite | hybrid
    confidence: <0-to-1>
```

## 11. Decision gate

```yaml
decision:
  result: progress | remediate | review | replan | stop | blocked
  reason: <evidence-grounded reason>
  next_unit: <unit-id-or-null>
  student_model_update:
    allowed: true | false
    changes: []
```

Rules:

- No Student Model update without evidence.
- Assisted success is not independent success.
- Immediate success is not delayed retention.
- Recognition does not prove explanation or application.

## 12. Remediation branch

```yaml
remediation:
  likely_cause: missing-prerequisite | misconception | procedural-error | representation-mismatch | cognitive-overload | language-burden | ambiguous-evidence | careless-slip
  evidence_refs:
    - <evidence-id>
  smallest_repair_objective: <objective>
  strategy_change: <how the new route differs structurally>
  repair_check: <assessment-item-id>
```

Do not repeat the same failed explanation with more words.

## 13. Transfer branch

```yaml
transfer:
  required_by_contract: true | false
  invariant_relationship: <what must remain stable>
  changed_surface_features:
    - <feature>
  transfer_distance: near-transfer | far-transfer | novel-domain
  task_ref: <assessment-item-id>
  success_condition: <condition>
```

## 14. Consolidation and review

### Learner-facing summary

- Core idea: <one sentence>
- Key relationship: <one sentence>
- Common error: <one sentence>
- Next action: <one action>

### Review plan

```yaml
review:
  concepts:
    - concept: <concept-id>
      capability: <capability>
      reason: central | fragile | misconception-risk | safety-critical | retention-required
      earliest_after_seconds: <number>
      latest_after_seconds: <number>
      retrieval_before_restudy: true
```

## 15. Replanning triggers

```yaml
replanning_triggers:
  - failed-prerequisite-check
  - learner-exceeds-assumptions
  - misconception-discovered
  - persistent-cognitive-overload
  - repeated-strategy-failure
  - goal-changed
  - deadline-changed
  - source-updated
  - review-fragility-detected
  - safety-issue
  - source-conflict
  - tool-unavailable
```

When triggered, preserve completed evidence and revise only the affected path.

## 16. Completion record

```yaml
completion:
  result: GOAL_MET | GOAL_PARTIALLY_MET | COMPRESSED_PATH_COMPLETE | REVIEW_PENDING | BLOCKED | ABANDONED_BY_LEARNER
  required_claims:
    - <claim>
  supported_claims:
    - <claim>
  unsupported_or_uncertain_claims:
    - <claim>
  delayed_retention_required: true | false
  next_step: <one action-or-null>
```

## 17. Final compliance check

- [ ] Every unit maps to Knowledge Model concepts.
- [ ] Every unit targets explicit capabilities.
- [ ] Every action is intended to produce evidence.
- [ ] Unknown prerequisite state creates diagnosis, not guessing.
- [ ] Hard prerequisites are not skipped silently.
- [ ] Analogies state limitations.
- [ ] SVGs have instructional purpose, alt text, and an interpretation task.
- [ ] Assessment matches the intended capability.
- [ ] Hint and answer exposure are recorded.
- [ ] Progression is based on matching evidence.
- [ ] Private learner state is not exposed.
- [ ] Source facts remain distinct from enrichment and inference.
- [ ] Safety-critical content is preserved.
- [ ] Completion is not equated with content exposure.
