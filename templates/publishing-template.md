# ULS Publishing Template

Use this template to turn approved lesson state into a durable, accessible, privacy-safe artifact. Replace all placeholders before publication.

---

## 1. Artifact identity

```yaml
artifact:
  artifact_id: <identifier>
  version: 1
  title: <title>
  type: learner-lesson-notes | printable-handout | pdf-lesson-booklet | review-sheet | practice-worksheet | answer-key | concept-map | glossary | instructor-guide | assessment-report | progress-summary | interactive-lesson-package | presentation-outline | study-guide | lesson-archive
  audience: learner | instructor | public | system
  format: markdown | html | pdf | docx | slide-source | json-package | svg-bundle | plain-text
  language: <language>
  publication_state: DRAFT
```

## 2. Artifact contract

```yaml
contract:
  purpose: <what this artifact must help the audience do>
  learning_contract_ref: <identifier>
  lesson_plan_ref: <identifier>
  scope:
    concepts:
      - <concept-id>
    capabilities:
      - <capability>
  page_limit: <integer-or-null>
  length_limit_words: <integer-or-null>
  answer_key_policy: none | inline | separate-final-section | separate-file | instructor-only
  include_personal_progress: true | false
  citation_style: <style-or-null>
  print_dimensions: <size-or-null>
```

Checklist:

- [ ] Audience and purpose are explicit.
- [ ] Scope is bounded.
- [ ] Output limits are realistic.
- [ ] Personal progress is included only when necessary and authorized.

## 3. Content selection

Choose blocks by instructional function, not chat order.

```yaml
content_blocks:
  - block_id: <identifier>
    role: title | orientation | objective | prerequisite-reminder | formal-definition | mental-model-explanation | worked-example | counterexample | analogy | analogy-limitation | story | visual | interpretation-prompt | guided-practice | independent-practice | transfer-prompt | retrieval-review | misconception-warning | summary | source-note | answer-key | instructor-note
    title: <title>
    concepts:
      - <concept-id>
    capabilities:
      - <capability>
    content_class: SOURCE | VERIFIED_ENRICHMENT | TEACHING_ANALOGY | INFERENCE | UNCERTAIN
    visibility: learner | instructor | public | system
    source_refs:
      - <source-id>
    required: true | false
    order: <integer>
```

Selection rules:

- Include only blocks that serve the artifact contract.
- Remove greetings, repetition, abandoned routes, and incorrect intermediate explanations.
- Preserve misconception examples only when clearly labeled and instructionally useful.
- Keep analogies separate from formal claims.

## 4. Excluded content

```yaml
excluded_content:
  - item: <content-or-field>
    reason: privacy | out-of-scope | redundant | incorrect-intermediate-content | unsupported | source-conflict | copyright | safety | format-limit | deferred-enrichment
    detail: <reason>
```

## 5. Recommended learner-facing structure

Use only the sections needed:

```text
Title
Learning objectives
Prerequisite snapshot
Core mental model
Formal definition or procedure
SVG diagram
Worked example
Contrast or misconception warning
Guided check
Independent practice
Transfer challenge
Summary
Review prompts
Sources
Answer key
```

## 6. Writing policy

Published prose should:

- state the key relationship early;
- use consistent terminology;
- assume only supported prerequisites;
- separate formal content from analogy and inference;
- avoid conversational filler;
- preserve uncertainty;
- avoid unsupported claims about learner ability;
- remain concise enough for the target format.

## 7. Analogy block

```yaml
analogy:
  source_domain: <domain>
  target_domain: <domain>
  mapping: <explicit mapping>
  intended_inference: <relationship>
  limitations:
    - <limitation>
  formal_restated_model: <formal explanation>
```

Never publish an analogy without its limitations.

## 8. Visual assets

```yaml
visual_assets:
  - asset_id: <identifier>
    format: svg | png | jpeg | webp
    path: <relative-path>
    objective: <instructional objective>
    concepts:
      - <concept-id>
    interpretation_question: <question>
    accessibility:
      title: <short title>
      description: <complete description>
      non_color_encoding: true
      reading_order:
        - <element-id>
    source_refs:
      - <source-id>
```

Requirements:

- Prefer SVG for generated instructional diagrams.
- No learner-facing ASCII art.
- No decorative asset without instructional value.
- No color-only encoding.
- Every visual requires alt text and a learner interpretation task.

## 9. Assessment assets

```yaml
assessment_assets:
  - asset_id: <identifier>
    assessment_ref: <identifier>
    visibility: learner | instructor | public | system
    answer_policy: hidden | after-attempt | after-completion | separate-file | instructor-only
    path: <relative-path>
```

Keep answers separate when retrieval matters.

## 10. Provenance manifest

```yaml
provenance:
  entries:
    - id: <identifier>
      content_class: SOURCE | VERIFIED_ENRICHMENT | TEACHING_ANALOGY | INFERENCE | UNCERTAIN
      source_uri: <uri-or-null>
      locator: <page-section-or-location>
      block_refs:
        - <block-id>
      verification_status: unverified | verified | conflicted | not-required
      license: <license-or-null>
  unresolved_conflicts:
    - <conflict>
```

Rules:

- Every factual block must be traceable when provenance matters.
- Do not merge conflicting claims silently.
- Do not present enrichment as source content.
- Record licensing restrictions before public release.

## 11. Privacy manifest

```yaml
privacy:
  profile: learner-safe | instructor-authorized | public-anonymized | system-private
  contains_personal_data: true | false
  contains_raw_learner_responses: true | false
  contains_student_model_data: true | false
  student_model_fields_included: []
  authorization_ref: <reference-or-null>
  anonymization_applied: true | false
  validation_status: not-run | passed | failed | blocked
  issues: []
```

Exclude by default:

- inferred misconceptions;
- confidence calibration history;
- cognitive-load estimates;
- strategy history;
- raw responses;
- private goals and deadlines;
- internal hypotheses and utility scores.

## 12. Evidence summary, when authorized

Good form:

> The learner independently explained the target relationship and completed one near-transfer task. Delayed retention has not yet been checked.

Avoid unsupported percentages, fixed labels, or “learning style” claims.

## 13. Accessibility manifest

```yaml
accessibility:
  language_metadata: true
  heading_hierarchy: true
  selectable_text: true
  alt_text: true
  non_color_encoding: true
  sufficient_contrast: true
  logical_reading_order: true
  accessible_tables: true
  captions_or_transcripts: true | false
  print_readability: true
  issues: []
```

## 14. Print and PDF specification

```yaml
print_spec:
  page_size: <A4-Letter-or-other>
  orientation: portrait | landscape
  margins: <value>
  minimum_font_size_pt: <number>
  page_numbers: true | false
  keep_headings_with_content: true
  prevent_orphaned_prompts: true
  preserve_vector_graphics: true
  embed_or_substitute_fonts_safely: true
```

## 15. Render manifest

```yaml
render:
  target_format: <format>
  source_path: <relative-path>
  output_path: <relative-path>
  renderer: <tool>
  renderer_version: <version>
  status: not-run | rendered | failed | blocked
  page_count: <integer-or-null>
  verified_page_count: <integer-or-null>
  verification_status: not-run | passed | failed | blocked
  issues: []
```

A successful renderer exit code is not proof of a valid artifact.

## 16. Validation manifest

```yaml
validation:
  content:
    status: not-run | passed | failed | blocked
    issues: []
  privacy:
    status: not-run | passed | failed | blocked
    issues: []
  accessibility:
    status: not-run | passed | failed | blocked
    issues: []
  format:
    status: not-run | passed | failed | blocked
    issues: []
  pedagogical:
    status: not-run | passed | failed | blocked
    issues: []
  render:
    status: not-run | passed | failed | blocked
    issues: []
```

## 17. Content validation checklist

- [ ] Required learning objectives are covered.
- [ ] Explanations match the Knowledge Model.
- [ ] Source classes are correct.
- [ ] Analogies include limitations.
- [ ] Answer keys match assessment items.
- [ ] Safety-critical content is preserved.
- [ ] Uncertainty is not written as fact.

## 18. Privacy validation checklist

- [ ] No unauthorized learner identifier appears.
- [ ] No private Student Model field appears unnecessarily.
- [ ] Raw responses are approved or removed.
- [ ] Public output is anonymized.
- [ ] Metadata contains no private local paths, account names, or hidden state.

## 19. Accessibility validation checklist

- [ ] Heading order is logical.
- [ ] Meaningful visuals have alt text.
- [ ] Color is not the only encoding.
- [ ] Tables have semantic headers.
- [ ] Reading order is clear.
- [ ] Text is readable at target size.
- [ ] Audio or video has captions or transcripts when included.

## 20. Format and render validation checklist

- [ ] No clipping or overflow.
- [ ] Page breaks are sensible.
- [ ] Prompts are not separated from answer choices.
- [ ] Diagrams remain readable.
- [ ] Special characters render correctly.
- [ ] Links and citations work.
- [ ] Answer-key separation is preserved.
- [ ] Every rendered page has been inspected.

## 21. Pedagogical validation checklist

- [ ] Every block serves the artifact purpose.
- [ ] Tasks match declared capabilities.
- [ ] Visuals require interpretation.
- [ ] The artifact includes learner activity where appropriate.
- [ ] Enrichment does not dominate the core path.
- [ ] Answers are not exposed prematurely.
- [ ] Content exposure is not described as mastery.

## 22. Dependencies and stale detection

```yaml
dependencies:
  - type: knowledge-node | lesson-plan | assessment | source | visual | renderer | template
    ref: <reference>
    version: <version>
    required: true | false
```

Mark the artifact stale when:

- a source changes;
- a Knowledge Model node changes;
- a source conflict is resolved;
- an assessment is invalidated;
- a visual is corrected;
- safety guidance changes;
- the lesson plan changes materially.

## 23. Change record

```yaml
change_log:
  - version: <integer>
    timestamp: <timestamp>
    reason: <reason>
    changes:
      - <change>
    affected_blocks:
      - <block-id>
    dependency_refs:
      - <reference>
```

## 24. Publication lifecycle

```text
DRAFT
  -> CONTENT_VALIDATED
  -> PRIVACY_VALIDATED
  -> ACCESSIBILITY_VALIDATED
  -> RENDERED
  -> RENDER_VERIFIED
  -> PUBLISHED
```

Alternative terminal states:

```text
SUPERSEDED
RETRACTED
BLOCKED
```

## 25. Retraction conditions

Retract or supersede the artifact when:

- factual content is wrong;
- private data is exposed;
- an answer key is invalid;
- a source conflict changes the lesson materially;
- required safety guidance is missing;
- accessibility failure makes the artifact unusable;
- the rendered artifact cannot be verified.

## 26. Final release gate

An artifact may move to `PUBLISHED` only when:

- [ ] Artifact contract is complete.
- [ ] Content selection is bounded and ordered by learning purpose.
- [ ] Provenance is complete enough for the use case.
- [ ] Privacy validation passes.
- [ ] Accessibility validation passes.
- [ ] Content and pedagogical validation pass.
- [ ] Render output exists and is inspected.
- [ ] No required asset is missing.
- [ ] Answer-key policy is preserved.
- [ ] Dependencies and version are recorded.
- [ ] Blocked conditions are absent.

Publishing does not update the Student Model or declare mastery.
