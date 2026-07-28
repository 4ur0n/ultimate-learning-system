# Publishing Runtime

## Purpose

The Publishing Runtime converts model-grounded teaching state into durable learning artifacts such as printable notes, PDFs, lesson handouts, review sheets, interactive lesson packages, and instructor-facing summaries.

It does not dump chat transcripts, expose private learner state, or treat every generated response as publication-ready content. It transforms selected instructional material into a structured artifact with explicit audience, purpose, provenance, and privacy boundaries.

## Core principle

> Publishing is the controlled projection of teaching state into a learner-facing artifact.

## Responsibilities

The Publishing Runtime must:

- define the artifact's audience and purpose;
- select content from the Knowledge Model, Teacher Model, Evidence Model, and approved lesson outputs;
- exclude private or unnecessary Student Model data;
- organize material by the learning path rather than raw conversation order;
- preserve source provenance and distinguish source facts from enrichment;
- include explanations, examples, diagrams, checks, and review prompts only when they serve the artifact's objective;
- generate accessible SVG-based visuals where appropriate;
- validate formatting, structure, references, and privacy;
- produce reproducible artifact manifests;
- support revision when the underlying plan, source, or evidence changes.

## Inputs

Required inputs:

- artifact request;
- intended audience;
- intended use;
- target format;
- selected learning contract or lesson-plan scope;
- relevant Knowledge Model slice;
- approved Teacher Model decisions;
- approved Evidence Model summaries when needed;
- privacy and source-integrity policy.

Optional inputs:

- learner language;
- reading level;
- accessibility requirements;
- page or length limit;
- branding constraints;
- print dimensions;
- answer-key policy;
- instructor-only sections;
- review horizon;
- citation style.

## Outputs

Each publishing cycle produces:

1. artifact specification;
2. content selection record;
3. artifact outline;
4. visual and assessment asset specifications;
5. rendered artifact or render-ready source;
6. provenance manifest;
7. privacy report;
8. validation report;
9. version record.

## Publishing loop

```text
DEFINE ARTIFACT CONTRACT
        ↓
SELECT APPROVED CONTENT
        ↓
APPLY PRIVACY FILTER
        ↓
ORGANIZE BY LEARNING PURPOSE
        ↓
GENERATE TEXT AND VISUAL ASSETS
        ↓
ASSEMBLE ARTIFACT
        ↓
VALIDATE CONTENT AND FORMAT
        ↓
RENDER
        ↓
VERIFY OUTPUT
        ↓
VERSION AND PUBLISH
```

## Artifact contract

Every artifact must begin with a bounded contract.

```yaml
artifact_contract:
  type: printable-lesson-notes
  audience: learner
  purpose: consolidate pointer fundamentals after guided lesson
  scope:
    concepts:
      - memory-location
      - address
      - pointer
      - dereference
    capabilities:
      - explain
      - trace
      - apply
  format: pdf
  page_limit: 8
  language: en
  answer_key: separate-final-section
  include_personal_progress: false
  source_citations: endnotes
```

Weak contract:

```text
Export the lesson.
```

Strong contract:

```text
Create an eight-page printable learner handout that explains and visually distinguishes addresses, pointers, and dereferenced values, includes two worked examples, three retrieval checks, and a separate answer key.
```

## Artifact classes

Supported artifact classes may include:

- learner lesson notes;
- printable handout;
- PDF lesson booklet;
- review sheet;
- practice worksheet;
- answer key;
- concept map;
- glossary;
- instructor guide;
- assessment report;
- progress summary;
- interactive lesson package;
- presentation outline;
- source-grounded study guide;
- lesson archive.

Each class has different privacy and evidence requirements.

## Audience policy

### Learner-facing artifact

May include:

- learning objectives;
- explanations;
- worked examples;
- SVG diagrams;
- misconception warnings;
- practice tasks;
- review prompts;
- concise next steps.

Must exclude:

- hidden teacher hypotheses;
- private diagnostic labels;
- raw confidence inferences;
- internal strategy utility scores;
- irrelevant response history;
- sensitive learner metadata.

### Instructor-facing artifact

May include, when authorized:

- evidence-backed capability profile;
- current misconceptions;
- assistance dependence;
- recommended next actions;
- unresolved uncertainty;
- source and evaluator limitations.

It must still avoid unsupported learner labels and unnecessary private data.

### Public artifact

Must remove:

- learner identifiers;
- raw learner responses unless explicitly approved and anonymized;
- personal goals or deadlines;
- private progress history;
- individualized misconception records;
- hidden model state.

## Content selection

Content should be selected by instructional function.

Possible content roles:

- orientation;
- prerequisite reminder;
- formal definition;
- mental-model explanation;
- worked example;
- counterexample;
- analogy;
- analogy limitation;
- SVG visual;
- interpretation prompt;
- guided practice;
- independent practice;
- transfer prompt;
- retrieval review;
- misconception warning;
- summary;
- source note.

Every included block should serve the artifact contract.

## Source hierarchy

Published claims should preserve the following distinction:

- `SOURCE`
- `VERIFIED_ENRICHMENT`
- `TEACHING_ANALOGY`
- `INFERENCE`
- `UNCERTAIN`

Source facts and generated explanations must not be flattened into one indistinguishable voice when provenance matters.

## Source provenance

Each factual section should be traceable to:

- source identifier;
- page, section, or location;
- Knowledge Model node version;
- verification status;
- publication date or version when relevant;
- conflict status.

Possible manifest entry:

```yaml
content_block: pointer-formal-definition
knowledge_nodes:
  - pointer
source_refs:
  - textbook-section-4.2
content_class: SOURCE
verification: confirmed
```

## Conversation filtering

Raw conversation order is not a publishing structure.

The runtime should:

1. remove greetings, repetition, and abandoned paths;
2. remove incorrect intermediate explanations unless included as explicit misconception examples;
3. remove private teacher reasoning;
4. group content by concept and capability;
5. reorder by prerequisite and learning purpose;
6. rewrite conversational fragments into durable prose;
7. preserve learner questions only when they improve the artifact and privacy allows it.

## Student Model privacy filter

The Publishing Runtime must default to excluding Student Model data.

Potentially publishable only with purpose and authorization:

- learner-selected goal;
- completed concepts;
- review schedule;
- broad next-step recommendation;
- self-authored reflection.

Normally excluded:

- inferred misconceptions;
- confidence calibration;
- cognitive-load estimates;
- strategy preference history;
- raw evidence links;
- private responses;
- sensitive profile data;
- internal model confidence.

## Evidence summaries

Evidence may be summarized in authorized progress artifacts.

Good summary:

```text
The learner independently explained the address-value distinction and completed two near-transfer tracing tasks. Delayed retention has not yet been checked.
```

Bad summary:

```text
The learner is 82% proficient and is a visual learner.
```

Evidence summaries must state capability, independence, and limitations.

## Artifact structure

A learner lesson artifact may use:

```text
Title
Learning objectives
Prerequisite snapshot
Core mental model
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

The exact structure should remain bounded by the artifact contract.

## Learning-block contract

Each instructional block should specify:

```yaml
learning_block:
  id: address-vs-value
  purpose: explain a core distinction
  concepts:
    - address
    - stored-value
  target_capability: explain
  content_type: explanation-plus-svg
  source_class: SOURCE
  learner_action: answer interpretation question
  expected_time_minutes: 4
```

This enables selective rendering and later reuse.

## Explanation policy

Published explanations should:

- begin from the learner's required prerequisite level;
- state the key relationship early;
- use consistent terminology;
- separate formal meaning from analogy;
- include exceptions or limitations when relevant;
- avoid conversational filler;
- remain aligned with the Knowledge Model;
- avoid unsupported certainty.

## Analogy policy

A published analogy must include:

- the intended mapping;
- the target relationship;
- one or more limitations;
- a transition back to formal language.

Example structure:

```text
Analogy
Mapping
Where it helps
Where it breaks
Formal model
```

Analogies must not replace the formal explanation.

## Story policy

Stories may be included when they:

- motivate the topic;
- show causal order;
- provide a realistic application;
- create a useful memory cue.

Stories should be short and clearly separated from source-derived factual claims.

## SVG policy

SVG is the default generated diagram format when a visual is needed.

Every published SVG must define:

- instructional objective;
- entities;
- relationships;
- labels;
- reading order;
- accessible title and description;
- interpretation prompt;
- print-safe layout;
- prohibited decorative elements.

Requirements:

- no learner-facing ASCII diagrams;
- sufficient contrast;
- readable labels at print size;
- no color-only encoding;
- semantic grouping;
- consistent notation;
- source or generation attribution where required.

## Visual asset record

```yaml
visual_asset:
  id: memory-layout-pointer-v1
  format: svg
  objective: distinguish pointer variable, address, location, and stored value
  nodes:
    - pointer-variable
    - address-label
    - destination-location
    - stored-value
  relationships:
    - pointer-stores-address
    - address-identifies-location
    - dereference-accesses-value
  accessibility:
    title: Pointer and dereference memory layout
    description: A pointer variable contains the address of another memory location, whose stored value is accessed by dereferencing.
  interpretation_question: Which element is the address, and which is the stored value?
```

## Assessment content

Practice and review items should map to explicit concepts and capabilities.

Each item should include:

- target claim;
- task type;
- assistance policy;
- success condition;
- answer-key entry;
- misconception indicators when useful;
- transfer distance;
- source linkage.

Recognition tasks must not be described as proof of explanation or application mastery.

## Answer-key policy

Answer keys should:

- remain separate from the task when retrieval matters;
- explain the core relationship, not only give the answer;
- name common errors where useful;
- state when multiple answers are valid;
- avoid revealing unrelated future material;
- preserve rubric alignment.

## Personalization policy

Published personalization should be useful, minimal, and reversible.

Acceptable examples:

- use the learner's chosen application domain;
- emphasize the prerequisite they requested;
- adjust language and reading level;
- choose accessible visual descriptions;
- include the learner's own approved example.

Unacceptable examples:

- publicizing inferred weaknesses;
- embedding private performance history;
- labeling a learning style;
- including sensitive personal details for decoration;
- generating claims about ability unsupported by evidence.

## Accessibility

Artifacts should support:

- logical heading hierarchy;
- selectable text;
- alt text for meaningful visuals;
- no color-only distinctions;
- sufficient contrast;
- readable type size;
- clear reading order;
- descriptive link text;
- accessible tables;
- language metadata;
- captions or transcripts for audiovisual content;
- page-break control for print.

Accessibility requirements should be part of the artifact contract, not an afterthought.

## Print and PDF requirements

For printable artifacts:

- use defined page dimensions and margins;
- prevent clipped content;
- keep headings with following content;
- avoid orphaned question prompts and separated answer choices;
- ensure diagrams fit at intended scale;
- embed or safely substitute fonts;
- preserve vector graphics where possible;
- include page numbers when multi-page;
- provide consistent headers and footers when useful;
- verify the rendered PDF page by page.

The runtime should validate both source structure and rendered output.

## Format targets

Possible targets:

- Markdown;
- HTML;
- PDF;
- DOCX;
- slide source;
- JSON lesson package;
- LMS-compatible package;
- SVG bundle;
- plain-text accessible edition.

Format conversion must not silently remove critical learning relationships, citations, alt text, or answer separation.

## Interactive lesson package

An interactive package may contain:

```yaml
lesson_package:
  manifest: lesson.json
  content:
    - blocks/*.md
  visuals:
    - visuals/*.svg
  assessments:
    - assessments/*.json
  rubrics:
    - rubrics/*.yaml
  sources:
    - provenance.yaml
  accessibility:
    - alt-text.yaml
```

The package should preserve model identifiers without including private model data.

## Artifact manifest

```yaml
artifact_manifest:
  artifact_id: pointers-handout-v1
  artifact_type: printable-lesson-notes
  version: 1
  generated_at: 2026-07-28T12:00:00Z
  audience: learner
  contract_ref: contract-17
  plan_ref: pointers-v0.1
  concepts:
    - memory-location
    - address
    - pointer
    - dereference
  capabilities:
    - explain
    - trace
    - apply
  source_refs:
    - textbook-section-4.2
    - course-notes-page-12
  included_assets:
    - memory-layout-pointer-v1
  privacy_profile: learner-safe
  validation_status: passed
```

## Versioning

Artifacts should be versioned when changes affect:

- factual content;
- learning sequence;
- examples;
- diagrams;
- assessment items;
- answer keys;
- source provenance;
- privacy scope;
- accessibility.

Example:

```yaml
artifact_version:
  artifact_id: pointers-handout
  version: 2
  previous_version: 1
  reason: corrected dereference diagram and added source citation
  affected_blocks:
    - pointer-diagram
    - worked-example-2
```

## Change propagation

A published artifact may become stale when:

- a source is updated;
- a Knowledge Model node changes;
- a source conflict is resolved;
- the lesson plan changes materially;
- an assessment item is invalidated;
- a diagram error is found;
- safety guidance changes.

The runtime should record dependencies so affected artifacts can be identified and regenerated.

## Validation pipeline

### Content validation

Check:

- all required objectives are covered;
- explanations match the Knowledge Model;
- source classes are labeled correctly;
- analogies include limitations;
- answer keys match tasks;
- no required prerequisite is omitted;
- uncertainty is not overstated as fact.

### Privacy validation

Check:

- no unauthorized learner identifier appears;
- no private Student Model fields are included;
- raw responses are approved or removed;
- public artifacts are anonymized;
- metadata does not leak private paths or accounts.

### Accessibility validation

Check:

- heading order;
- alt text;
- contrast;
- reading order;
- table semantics;
- non-color encoding;
- print readability.

### Format validation

Check:

- page count;
- clipping and overflow;
- broken links;
- missing assets;
- incorrect page breaks;
- unsupported characters;
- citation rendering;
- answer-key separation.

### Pedagogical validation

Check:

- each block serves the artifact purpose;
- tasks match capabilities;
- visuals require interpretation;
- explanations do not replace all learner activity;
- enrichment does not dominate;
- assessment does not expose answers prematurely.

## Render verification

Rendered artifacts must be inspected, not assumed correct from source generation.

For PDFs, verify:

- every page renders;
- text is legible;
- diagrams are complete;
- page breaks are sensible;
- no content is clipped;
- special characters display correctly;
- links, references, and page numbers are correct.

A successful renderer exit code is not sufficient validation.

## Publication states

Suggested states:

- `DRAFT`
- `CONTENT_VALIDATED`
- `PRIVACY_VALIDATED`
- `ACCESSIBILITY_VALIDATED`
- `RENDERED`
- `RENDER_VERIFIED`
- `PUBLISHED`
- `SUPERSEDED`
- `RETRACTED`
- `BLOCKED`

An artifact should reach `PUBLISHED` only after required validations pass.

## Retraction

An artifact should be retracted or marked superseded when:

- it contains factual errors;
- it exposes private data;
- an assessment answer was invalid;
- a source conflict materially changes the lesson;
- required safety guidance is missing;
- accessibility failure makes the artifact unusable;
- generated content cannot be verified.

Retraction should preserve an audit record.

## Failure handling

The runtime enters `BLOCKED` when:

- source provenance is missing for critical claims;
- private data cannot be safely separated;
- rendering tools are unavailable;
- required visual assets cannot be generated accessibly;
- the target format cannot preserve necessary content;
- source licensing or usage constraints are unresolved;
- validation repeatedly fails.

A blocked publishing cycle must not invent successful output.

## Runtime record

```yaml
publishing_cycle:
  id: publishing-cycle-9
  contract_ref: contract-17
  artifact_id: pointers-handout-v1
  selected_blocks:
    - objectives
    - prerequisite-snapshot
    - pointer-mental-model
    - memory-layout-svg
    - worked-example-1
    - retrieval-checks
    - answer-key
  excluded_data:
    - student-confidence-history
    - raw-responses
    - teacher-hypotheses
  validations:
    content: passed
    privacy: passed
    accessibility: passed
    format: passed
    pedagogical: passed
  render_status: verified
  publication_state: PUBLISHED
```

## Invariants

1. Every artifact has an explicit audience and purpose.
2. Raw conversation order is never the default artifact structure.
3. Private Student Model data is excluded by default.
4. Every factual claim preserves provenance when required.
5. Source facts remain distinguishable from analogy, inference, and enrichment.
6. Every visual has an instructional objective and accessibility text.
7. Learner-facing ASCII diagrams are not used as visual assets.
8. Every assessment item maps to a capability and answer policy.
9. Rendered output is inspected before publication.
10. Successful generation does not imply successful validation.
11. Artifact versions preserve change reasons and dependencies.
12. Public artifacts contain no unauthorized learner evidence.
13. Publishing never changes mastery or misconception state.
14. Blocked publication is reported honestly.

## Failure modes

### Transcript dumping

Problem: the chat log is exported as lesson notes.

Prevention: content selection and learning-purpose organization.

### Privacy leakage

Problem: learner weaknesses, responses, or identifiers appear in a shareable artifact.

Prevention: privacy filter and audience-specific schemas.

### Provenance loss

Problem: source facts, generated explanations, and analogy become indistinguishable.

Prevention: content-class metadata and manifests.

### Decorative publication

Problem: visual polish replaces instructional structure.

Prevention: pedagogical validation and learning-block contracts.

### Render blindness

Problem: source files are trusted without checking final pages.

Prevention: render verification.

### Answer leakage

Problem: answer keys appear before retrieval tasks.

Prevention: answer separation and format validation.

### Personalization overreach

Problem: private learner data is embedded to make the artifact feel customized.

Prevention: minimal and authorized personalization.

### Stale artifact drift

Problem: source or model updates do not invalidate published material.

Prevention: dependency manifest and supersession state.

### Accessibility afterthought

Problem: inaccessible diagrams and reading order are discovered after publication.

Prevention: accessibility contract and validation state.

### Format-induced meaning loss

Problem: conversion removes relationships, citations, or alt text.

Prevention: target-format capability checks.

## Verification scenarios

A compliant implementation should pass these scenarios:

1. A learner handout excludes internal misconception confidence and raw responses.
2. An instructor report includes capability-specific evidence only when authorized.
3. A public artifact is anonymized and contains no learner identifiers.
4. A chat transcript is reorganized into prerequisite-based lesson sections.
5. An analogy is published with explicit limitations and a formal explanation.
6. An SVG includes accessible description and an interpretation question.
7. A PDF with clipped diagram content fails render verification.
8. An assessment worksheet keeps its answer key separate.
9. A changed Knowledge Model node marks dependent artifacts stale.
10. A source conflict remains visible rather than being silently resolved.
11. A target format that cannot preserve critical notation causes a blocked result.
12. Publishing an artifact does not update the Student Model.

## Minimal compliance

A minimal Publishing Runtime must:

- define audience, purpose, scope, and format;
- select approved model-grounded content;
- remove private learner state by default;
- organize content by learning purpose;
- preserve provenance;
- support accessible SVG assets;
- include capability-aligned practice and answer handling;
- validate content, privacy, accessibility, and formatting;
- inspect rendered output;
- version and manifest the artifact;
- report blocked or failed publication honestly.

## Model relationships

```text
Knowledge Model
  provides concepts, relationships, representations, and provenance
        ↓
Lesson Planner
  defines scope and sequence
        ↓
Teacher Model
  provides approved instructional decisions and artifact purpose
        ↓
Evidence Model
  optionally provides authorized capability summaries
        ↓
Publishing Runtime
  selects, transforms, validates, and renders
        ↓
Learner, instructor, or public artifact
```

The Student Model is accessed only through an explicit privacy policy and minimum-necessary projection.

## Guiding question

> What is the smallest accurate, accessible, privacy-safe artifact that preserves the intended learning relationships and remains traceable to its sources and instructional purpose?
