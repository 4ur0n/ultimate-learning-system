# ULS Assessment Template

Use this template to design one capability-aligned assessment or assessment set. Replace all placeholders before use.

---

## 1. Assessment identity

```yaml
assessment:
  assessment_id: <identifier>
  version: 1
  title: <learner-facing title>
  purpose: diagnostic | formative | progression-check | independent-practice | transfer-check | retention-check | summative
  audience: learner | instructor | system
  language: <language>
  status: DRAFT
```

## 2. Assessment contract

```yaml
assessment_contract:
  target_concepts:
    - <concept-id>
  target_capabilities:
    - recall | explain | predict | compare | classify | trace | calculate | apply | implement | debug | analyze | transfer | teach_back | retain
  intended_independence: fully-guided | partially-guided | prompted | independent | self-corrected-independent
  transfer_distance: same-item | same-structure | near-transfer | far-transfer | novel-domain
  source_scope:
    - <source-id>
  completion_condition: <evidence condition>
```

Checklist:

- [ ] Every item targets a specific capability.
- [ ] The task format can actually produce evidence for that capability.
- [ ] The required independence level is explicit.
- [ ] Transfer distance is stated when relevant.
- [ ] Completion is not based only on total score.

## 3. Validity statement

Write one sentence explaining what successful performance should justify.

> <Example: A correct, independent explanation should support the claim that the learner can distinguish an address from the value stored at that address.>

Also state what it does **not** justify.

> <Example: This item does not establish delayed retention or far transfer.>

## 4. Item template

```yaml
item:
  item_id: <identifier>
  prompt: <learner-facing prompt>
  task_type: free-retrieval | multiple-choice | multi-select | true-false | short-answer | extended-explanation | prediction | comparison | classification | calculation | trace | application | implementation | debugging | diagram-interpretation | teach-back | oral-response | artifact-production
  target_concepts:
    - <concept-id>
  target_capability: <capability>
  transfer_distance: same-item | same-structure | near-transfer | far-transfer | novel-domain
  response_format: text | number | choice | code | diagram | audio | file | mixed
  success_condition: <observable condition>
  estimated_seconds: <number>
  source_refs:
    - <source-id>
```

## 5. Prompt design check

Before using the item, verify:

- [ ] The prompt is unambiguous.
- [ ] The prompt does not accidentally reveal the answer.
- [ ] The task difficulty comes from the target capability, not confusing wording.
- [ ] Unnecessary reading, notation, or interface burden has been removed.
- [ ] The item does not require an unstated prerequisite.
- [ ] The item is safe and authorized for the intended context.

## 6. Choice item block, if applicable

```yaml
options:
  - option_id: <identifier>
    text: <option text>
    is_correct: true | false
    diagnostic_meaning: <what selecting this option may indicate>
```

Choice-item rules:

- Distractors should represent plausible reasoning errors.
- Avoid grammatical clues and unequal option length when possible.
- Do not treat correct recognition as proof of explanation.
- Record the diagnostic meaning of each distractor.

## 7. Answer key

```yaml
answer_key:
  canonical_answer: <answer>
  acceptable_variants:
    - <variant>
  explanation: <why the answer is correct>
  common_errors:
    - <error>
  visibility: hidden | after-attempt | after-completion | instructor-only
```

Answer-key rules:

- Explain the relationship, not only the final answer.
- State when multiple answers are valid.
- Keep the key separate when retrieval matters.
- Do not expose unrelated future content.

## 8. Assistance policy

```yaml
assistance_policy:
  allowed: true | false
  max_hint_level: <0-to-6>
  answer_exposure_invalidates_independence: true | false
  tools_allowed:
    - <tool>
  time_limit_seconds: <number-or-null>
```

Hint ladder:

```yaml
hints:
  level_1: <restate objective>
  level_2: <direct attention>
  level_3: <cue prerequisite>
  level_4: <partial structure>
  level_5: <one intermediate step>
  level_6: <complete solution>
```

After level 6, require a new item or delayed retry before claiming independent performance.

## 9. Rubric

```yaml
rubric:
  rubric_id: <identifier>
  version: <version>
  criteria:
    - criterion_id: <identifier>
      description: <what is evaluated>
      weight: <number>
      levels:
        - label: complete
          score: <number>
          descriptor: <observable performance>
        - label: partial
          score: <number>
          descriptor: <observable performance>
        - label: absent
          score: <number>
          descriptor: <observable performance>
  decision_rule: <how criterion results produce the item decision>
  requires_human_review: true | false
```

Rubric rules:

- Use observable descriptors.
- Separate correctness, completeness, reasoning, and independence when relevant.
- Do not award explanation credit for keyword overlap alone.
- Do not compensate for failure on a required core relationship with unrelated details.

## 10. Misconception indicators

```yaml
misconception_indicators:
  - pattern_id: <misconception-id>
    signal: <specific structural response pattern>
    specificity: low | medium | high
```

A single error should usually be treated as a signal, not a confirmed misconception.

## 11. Scoring policy

```yaml
scoring_policy:
  method: deterministic | rubric | test-suite | human-review | hybrid
  aggregation: all-required | weighted-sum | minimum-per-criterion | capability-specific | manual-judgment
  passing_rule: <rule>
  score_range:
    minimum: <number>
    maximum: <number>
```

Progression should depend on required claims and conditions, not only aggregate score.

## 12. Evidence policy

```yaml
evidence_policy:
  record_independence: true
  record_hint_usage: true
  record_answer_exposure: true
  record_learner_confidence: true | false
  record_transfer_distance: true | false
  record_retention_interval: true | false
  preserve_evaluator_uncertainty: true
  allow_student_model_update: true | false
```

## 13. Attempt record

```yaml
attempt:
  attempt_id: <identifier>
  item_id: <item-id>
  learner_id: <identifier-or-omitted>
  started_at: <timestamp>
  submitted_at: <timestamp>
  response: <response-or-reference>
  learner_confidence: <0-to-1-or-null>
  conditions:
    independence: copied | fully-guided | partially-guided | prompted | independent | self-corrected-independent
    hints_used: <number>
    highest_hint_level: <0-to-6>
    answer_exposure: none | partial | full
    tools_used:
      - <tool>
    elapsed_seconds: <number>
    retention_interval_seconds: <number-or-omitted>
```

An attempt is invalid for independent evidence when its conditions violate the declared assistance policy.

## 14. Evaluation record

```yaml
evaluation:
  correctness: correct | partially-correct | incorrect | indeterminate | not-attempted
  completeness: complete | partial | minimal | none
  criterion_results:
    - criterion_id: <identifier>
      score: <number>
      level: <label>
      rationale: <evidence-grounded rationale>
  total_score: <number-or-null>
  decision: pass | partial | fail | invalid-attempt | needs-review | indeterminate
  evaluator:
    type: deterministic-checker | rubric-based-model | human-reviewer | test-suite | external-platform | learner-self-assessment | hybrid
    confidence: <0-to-1>
    rubric_version: <version>
    review_required: true | false
```

## 15. Feedback block

```yaml
feedback:
  correct_elements:
    - <what was correct>
  smallest_important_gap: <one important gap>
  why_it_matters: <why the gap affects the target capability>
  next_step: <one active repair or progression action>
```

Feedback rules:

- Be specific and evidence-based.
- Preserve what the learner did correctly.
- Focus on the smallest important gap.
- End with an action, not a vague encouragement.

## 16. Evidence output

```yaml
evidence_output:
  evidence_id: <identifier>
  claims_supported:
    - <claim>
  claims_weakened:
    - <claim>
  misconception_signals:
    - <misconception-id>
  student_model_update_allowed: true | false
  update_reason: <reason>
```

Rules:

- Recognition evidence supports recognition or limited recall only.
- Guided success does not support independent mastery.
- Immediate success does not support delayed retention.
- Near transfer does not support far transfer.
- Full answer exposure invalidates independent evidence for the same item.

## 17. Decision routing

```yaml
routing:
  if_pass: <next-unit-or-state>
  if_partial: <repair-or-confirmation-task>
  if_fail: <smallest-prerequisite-or-misconception-check>
  if_invalid_attempt: <fresh-item-or-delay>
  if_indeterminate: <human-review-or-better-task>
```

## 18. Retention check block

Use only when delayed retention is part of the learning contract.

```yaml
retention_check:
  original_claim: <claim>
  delay_seconds: <number>
  retrieval_before_restudy: true
  item_isomorphic_to_original: true | false
  assistance_allowed: true | false
  success_condition: <condition>
```

Do not reuse an identical item when answer memory would contaminate evidence.

## 19. Transfer check block

```yaml
transfer_check:
  invariant_relationship: <relationship that remains stable>
  changed_surface_features:
    - <feature>
  transfer_distance: near-transfer | far-transfer | novel-domain
  expected_reasoning: <reasoning pattern>
  success_condition: <condition>
```

## 20. Assessment validation checklist

### Construct validity

- [ ] Each item targets one declared capability.
- [ ] The response format can reveal that capability.
- [ ] The success condition matches the capability.
- [ ] Hidden prerequisites do not dominate performance.

### Evidence integrity

- [ ] Independence is recorded.
- [ ] Hint usage is recorded.
- [ ] Answer exposure is recorded.
- [ ] Tool use is recorded when relevant.
- [ ] Evaluator confidence is preserved.
- [ ] Invalid attempts cannot update mastery.

### Rubric quality

- [ ] Criteria are observable.
- [ ] Required core relationships cannot be offset by irrelevant detail.
- [ ] Multiple valid answers are supported where appropriate.
- [ ] Open responses are not scored primarily by keyword matching.

### Pedagogical quality

- [ ] Feedback identifies the smallest important gap.
- [ ] Failure routes to a useful next action.
- [ ] The item does not reveal answers prematurely.
- [ ] Transfer and retention claims are not overstated.

### Privacy and safety

- [ ] Raw learner responses are stored only when authorized.
- [ ] Public artifacts contain no learner identifiers.
- [ ] Safety constraints are explicit.
- [ ] Source provenance is preserved.

## 21. Final release gate

An assessment may move to `VALIDATED` only when:

- [ ] Prompt validation passes.
- [ ] Capability alignment passes.
- [ ] Rubric validation passes.
- [ ] Answer key has been checked.
- [ ] Assistance and invalidation rules are explicit.
- [ ] Evidence output is capability-specific.
- [ ] Misconception indicators are tentative and testable.
- [ ] Privacy and safety checks pass.
