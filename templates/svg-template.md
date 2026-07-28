# ULS Instructional SVG Template

Use this template to design one instructional SVG that externalizes a relationship the learner must inspect, explain, predict from, or apply.

Do not use this template for decorative graphics.

---

## 1. SVG identity

```yaml
svg:
  svg_id: <identifier>
  version: 1
  title: <learner-facing title>
  diagram_type: spatial-structure | flow | causal-graph | temporal-sequence | hierarchy | composition | comparison | state-transition | process | concept-map | annotated-example
  language: <language>
  status: DRAFT
```

## 2. Instructional contract

```yaml
instructional_contract:
  objective: <what relationship the learner should understand from the diagram>
  target_concepts:
    - <concept-id>
  target_capabilities:
    - explain | predict | compare | trace | analyze | apply
  learner_action: <what the learner must do with the diagram>
  success_condition: <observable interpretation condition>
```

Checklist:

- [ ] The SVG externalizes a relationship, not just a noun.
- [ ] The learner must actively interpret it.
- [ ] The diagram is more useful than a paragraph or table.
- [ ] The objective is narrow enough for one image.

## 3. Core semantic statement

Write one sentence that the entire diagram must communicate.

> <Example: A pointer variable stores an address, and dereferencing follows that address to access the value in another memory location.>

Every entity and relationship must support this statement.

## 4. Entities

```yaml
entities:
  - id: <entity-id>
    label: <visible label>
    semantic_role: concept | object | state | step | actor | container | input | output | cause | effect | comparison-item | annotation | boundary | value | location | example
    shape: rectangle | rounded-rectangle | circle | ellipse | diamond | hexagon | capsule | line | path | icon | text | custom
    concept_ref: <concept-id-or-null>
    description: <semantic meaning>
    importance: primary | secondary | supporting | annotation
    position_hint: top | bottom | left | right | center | start | middle | end | free
    aria_label: <screen-reader label>
```

Rules:

- Use one stable visual identity per semantic role.
- Labels should name the role, not merely repeat source prose.
- Avoid unnecessary icons and decorative entities.
- Split overloaded entities into smaller semantic units.

## 5. Relationships

```yaml
relationships:
  - id: <relationship-id>
    source: <entity-id>
    target: <entity-id>
    type: connects | causes | precedes | contains | part-of | flows-to | transforms-into | contrasts-with | stores | references | identifies | accesses | depends-on | corresponds-to | annotates | branches-to | returns-to | generalizes-to
    direction: none | forward | backward | bidirectional
    label: <relationship label>
    line_style: solid | dashed | dotted | double
    strength: primary | secondary | conditional | optional
    condition: <condition-or-null>
```

Rules:

- Use arrow direction only when direction has meaning.
- Label non-obvious arrows.
- Do not let line style or color carry meaning alone.
- Prefer direct relationships over visually impressive but ambiguous paths.

## 6. Semantic groups

```yaml
groups:
  - id: <group-id>
    label: <group label>
    member_refs:
      - <entity-id>
    purpose: <why these entities belong together>
    boundary_style: none | solid | dashed | bracket | background-panel
    aria_label: <screen-reader group label>
```

Use groups for:

- stages in a process;
- parts of one system;
- competing cases;
- before-and-after states;
- source and destination regions.

Do not use groups only for decoration.

## 7. Layout plan

```yaml
layout:
  direction: left-to-right | right-to-left | top-to-bottom | bottom-to-top | radial | freeform
  reading_order:
    - <entity-or-group-id>
  view_box:
    width: <number>
    height: <number>
  aspect_ratio: <for example 16:9 or 4:3>
  grid:
    columns: <number>
    rows: <number>
    gap: <number>
  minimum_spacing: <number>
  avoid_edge_crossings: true
  allow_overflow: false
```

Layout rules:

- Reading order must match the instructional explanation.
- Primary relationships should be visually shortest and clearest.
- Avoid crossed arrows.
- Keep labels near their targets.
- Use whitespace to separate conceptual groups.
- Ensure the diagram remains readable at intended print size.

## 8. Style tokens

```yaml
style_tokens:
  font_family: <system-safe or approved font stack>
  base_font_size: <number>
  minimum_font_size: <number, at least 8>
  stroke_width: <number>
  corner_radius: <number>
  use_color: true | false
  semantic_token_names:
    - primary-entity
    - secondary-entity
    - primary-relationship
    - annotation
  prohibit_gradients: true
  prohibit_drop_shadows: true
```

Style rules:

- Use semantic tokens rather than ad hoc styling.
- Color may reinforce meaning but must not be the only encoding.
- Use patterns, labels, shape, or line style as redundant signals.
- Avoid visual effects that reduce print clarity.
- Keep the visual hierarchy simple.

## 9. Accessibility

```yaml
accessibility:
  title: <short accessible title>
  description: <complete text description of the diagram and its relationships>
  non_color_encoding: true
  logical_reading_order: true
  contrast_requirement: WCAG-AA | WCAG-AAA | print-monochrome
  screen_reader_grouping: true
  keyboard_focus_required: false
  text_alternatives:
    - target_ref: <entity-or-relationship-id>
      text: <alternative text>
```

The description should let a learner understand the core relationship without seeing the SVG.

Avoid descriptions such as:

> Diagram showing pointers.

Prefer:

> A pointer variable on the left contains the address 0x200. An arrow labeled “stores address” points to that address. A second arrow labeled “identifies location” leads to a memory cell at 0x200 containing the value 42. Dereferencing follows the address to access 42.

## 10. Interpretation task

```yaml
interpretation_task:
  prompt: <question requiring the learner to inspect the diagram>
  target_capability: explain | predict | compare | trace | analyze | apply
  expected_observation: <relationship the learner should identify>
  answer_policy: hidden | after-attempt | after-completion | instructor-only
  misconception_signals:
    - <misconception-id>
```

Good prompts:

- Which object stores the address, and which object stores the value?
- What path does the data follow after the condition becomes true?
- Which relationship changes between case A and case B?
- Predict the next state if this arrow is removed.

Weak prompt:

- Do you understand the diagram?

## 11. Source and provenance

```yaml
source_refs:
  - id: <source-id>
    content_class: SOURCE | VERIFIED_ENRICHMENT | TEACHING_ANALOGY | INFERENCE | UNCERTAIN
    locator: <page-section-or-location>
    verification_status: unverified | verified | conflicted | not-required
```

Rules:

- Diagrammed claims must remain traceable.
- Analogy elements must not be presented as source facts.
- Conflicted claims must remain visibly unresolved when relevant.

## 12. Rendering constraints

```yaml
constraints:
  instructionally_relevant_only: true
  no_color_only_encoding: true
  print_safe: true
  no_clipping: true
  no_ascii_art: true
  maximum_entities: <number>
  maximum_relationships: <number>
  minimum_label_size_pt: <number, at least 8>
  allow_decorative_elements: false
  allow_external_images: false
  allow_embedded_scripts: false
```

## 13. Output specification

```yaml
output:
  path: <relative/path.svg>
  width_px: <number>
  height_px: <number>
  responsive: true
  preserve_vector: true
  background: transparent | light | dark | print-white
```

## 14. SVG structure template

Use semantic groups and native accessibility elements.

```xml
<svg
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 WIDTH HEIGHT"
  role="img"
  aria-labelledby="svg-title svg-desc"
>
  <title id="svg-title">ACCESSIBLE TITLE</title>
  <desc id="svg-desc">ACCESSIBLE DESCRIPTION</desc>

  <defs>
    <!-- markers, patterns, and reusable semantic definitions only -->
  </defs>

  <g id="primary-group" role="group" aria-label="GROUP LABEL">
    <!-- entities -->
  </g>

  <g id="relationships" role="group" aria-label="Relationships">
    <!-- connectors and labels -->
  </g>

  <g id="annotations" role="group" aria-label="Annotations">
    <!-- limited explanatory labels -->
  </g>
</svg>
```

Implementation rules:

- Include `<title>` and `<desc>`.
- Use unique, stable IDs.
- Prefer text as SVG text rather than outlines.
- Avoid embedded scripts.
- Avoid external network resources.
- Use marker definitions consistently.
- Ensure labels do not overlap nodes or connectors.

## 15. Validation record

```yaml
validation:
  schema:
    status: not-run | passed | failed | blocked
    issues: []
  semantic:
    status: not-run | passed | failed | blocked
    issues: []
  accessibility:
    status: not-run | passed | failed | blocked
    issues: []
  layout:
    status: not-run | passed | failed | blocked
    issues: []
  render:
    status: not-run | passed | failed | blocked
    issues: []
```

## 16. Semantic validation checklist

- [ ] The core semantic statement is visible in the diagram.
- [ ] Every entity has a defined instructional role.
- [ ] Every relationship has a clear meaning.
- [ ] Arrow direction matches the modeled relationship.
- [ ] No decorative entity competes with the target relationship.
- [ ] The diagram does not imply unsupported causation or sequence.
- [ ] Analogy and formal entities remain distinguishable.

## 17. Accessibility validation checklist

- [ ] `<title>` is present and specific.
- [ ] `<desc>` explains the core relationship.
- [ ] Reading order is explicit.
- [ ] Color is not the only semantic encoding.
- [ ] Text meets minimum size.
- [ ] Contrast meets the selected requirement.
- [ ] Groups have meaningful labels.
- [ ] The interpretation task is available in text.

## 18. Layout and render validation checklist

- [ ] No label overlaps another label.
- [ ] No connector crosses an unrelated node.
- [ ] No content is clipped by the viewBox.
- [ ] Arrowheads are visible at target size.
- [ ] The SVG renders at browser and print dimensions.
- [ ] Monochrome printing preserves meaning.
- [ ] The image remains legible when embedded in a PDF.
- [ ] No unsupported external asset is required.

## 19. Revision record

```yaml
change_log:
  - version: <number>
    timestamp: <timestamp>
    reason: <why revision was needed>
    changes:
      - <change>
    affected_entities:
      - <entity-id>
```

Revise the SVG when:

- a learner consistently misreads its relationship;
- labels are ambiguous;
- a source or model node changes;
- accessibility validation fails;
- print rendering introduces clipping;
- the image contains unnecessary complexity.

## 20. Final release gate

The SVG may move to `VERIFIED` only when:

- [ ] Schema validation passes.
- [ ] Semantic validation passes.
- [ ] Accessibility validation passes.
- [ ] Layout validation passes.
- [ ] Render output is visually inspected.
- [ ] The interpretation task matches the target capability.
- [ ] Source and content classes are accurate.
- [ ] The final image contains no hidden learner data.
