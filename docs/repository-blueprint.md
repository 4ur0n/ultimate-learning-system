# Repository Blueprint

## Purpose

This blueprint defines the intended structure of Ultimate Learning System (ULS) before the project expands further. It exists to prevent architecture drift, duplicated responsibilities, oversized prompts, and late-stage rewrites.

ULS is organized in layers:

1. theory explains why the system behaves as it does;
2. runtime specifications define how decisions are made;
3. skill instructions orchestrate those runtimes;
4. templates and schemas make outputs consistent;
5. scripts implement deterministic operations;
6. examples demonstrate end-to-end behavior;
7. tests verify that the framework teaches rather than merely generates fluent content.

## Design rule

Every significant file must answer four questions:

1. What problem does this solve?
2. Why is it needed?
3. How does it operate?
4. How can its behavior be verified?

A file that cannot answer these questions should not be added.

## Target repository structure

```text
ultimate-learning-system/
├── README.md
├── ROADMAP.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── LICENSE
├── SECURITY.md
├── SKILL.md
├── AGENTS.md
│
├── theory/
│   ├── README.md
│   ├── learning-as-state-transition.md
│   ├── learning-as-graph-construction.md
│   ├── teaching-as-decision-making.md
│   ├── assessment-as-evidence.md
│   ├── understanding-as-model-alignment.md
│   ├── confusion-taxonomy.md
│   ├── analogy-as-structural-mapping.md
│   ├── visuals-as-cognitive-tools.md
│   ├── memory-and-forgetting.md
│   ├── transfer-and-generalization.md
│   ├── motivation-and-curiosity.md
│   └── runtime-over-prompt.md
│
├── docs/
│   ├── vision.md
│   ├── architecture.md
│   ├── pedagogy.md
│   ├── repository-blueprint.md
│   ├── glossary.md
│   ├── design-principles.md
│   ├── compatibility.md
│   ├── limitations.md
│   └── threat-model.md
│
├── runtime/
│   ├── README.md
│   ├── decision-engine.md
│   ├── teaching-runtime.md
│   ├── teaching-state-machine.md
│   ├── strategy-selection.md
│   ├── learner-model.md
│   ├── mental-model-engine.md
│   ├── evidence-engine.md
│   ├── confusion-engine.md
│   ├── mastery-engine.md
│   ├── dependency-engine.md
│   ├── lesson-planner.md
│   ├── learning-contract.md
│   ├── exit-contract.md
│   ├── review-runtime.md
│   ├── curiosity-runtime.md
│   ├── reflection-runtime.md
│   └── orchestration.md
│
├── source/
│   ├── README.md
│   ├── ingestion.md
│   ├── normalization.md
│   ├── provenance.md
│   ├── source-integrity.md
│   ├── conflict-resolution.md
│   ├── extraction-failures.md
│   └── supported-formats.md
│
├── teaching/
│   ├── README.md
│   ├── explanation-patterns.md
│   ├── analogy-engine.md
│   ├── story-engine.md
│   ├── worked-examples.md
│   ├── examples-and-nonexamples.md
│   ├── socratic-dialogue.md
│   ├── scaffolding.md
│   ├── hint-policy.md
│   ├── remediation.md
│   ├── cognitive-load.md
│   ├── adaptive-difficulty.md
│   ├── domain-adaptation.md
│   └── stopping-rules.md
│
├── assessment/
│   ├── README.md
│   ├── question-engine.md
│   ├── assessment-runtime.md
│   ├── capability-matrix.md
│   ├── mastery-rubric.md
│   ├── confidence-calibration.md
│   ├── misconception-diagnosis.md
│   ├── retrieval-practice.md
│   ├── transfer-assessment.md
│   ├── teach-back.md
│   ├── review-scheduling.md
│   └── question-quality-checklist.md
│
├── visual/
│   ├── README.md
│   ├── svg-style-guide.md
│   ├── visual-trigger-policy.md
│   ├── diagram-selection-matrix.md
│   ├── svg-accessibility.md
│   ├── svg-validation.md
│   ├── pdf-vector-workflow.md
│   ├── diagram-failure-modes.md
│   └── templates/
│       ├── concept-map.svg
│       ├── comparison.svg
│       ├── hierarchy.svg
│       ├── timeline.svg
│       ├── sequence.svg
│       ├── memory-layout.svg
│       ├── state-machine.svg
│       ├── layered-system.svg
│       └── annotated-process.svg
│
├── publishing/
│   ├── README.md
│   ├── publishing-runtime.md
│   ├── lesson-document-model.md
│   ├── markdown-output.md
│   ├── html-output.md
│   ├── pdf-output.md
│   ├── typography.md
│   ├── print-layout.md
│   ├── source-labels.md
│   └── accessibility.md
│
├── schemas/
│   ├── concept.schema.json
│   ├── source-unit.schema.json
│   ├── learner-profile.schema.json
│   ├── learning-evidence.schema.json
│   ├── mastery-state.schema.json
│   ├── misconception.schema.json
│   ├── lesson-plan.schema.json
│   ├── learning-contract.schema.json
│   ├── review-item.schema.json
│   └── diagram-spec.schema.json
│
├── templates/
│   ├── lesson.md
│   ├── learning-contract.md
│   ├── exit-contract.md
│   ├── concept-card.md
│   ├── worked-example.md
│   ├── misconception-repair.md
│   ├── review-session.md
│   ├── learning-journal.md
│   └── pdf-handout.md
│
├── prompts/
│   ├── README.md
│   ├── source-analysis.md
│   ├── concept-extraction.md
│   ├── learner-diagnosis.md
│   ├── lesson-planning.md
│   ├── teaching-turn.md
│   ├── analogy-generation.md
│   ├── svg-specification.md
│   ├── question-generation.md
│   ├── answer-evaluation.md
│   ├── misconception-repair.md
│   ├── mastery-decision.md
│   └── lesson-reflection.md
│
├── scripts/
│   ├── README.md
│   ├── extract_document.py
│   ├── normalize_sources.py
│   ├── validate_schemas.py
│   ├── validate_svg.py
│   ├── render_svg_preview.py
│   ├── build_pdf.py
│   ├── inspect_pdf.py
│   ├── build_knowledge_graph.py
│   ├── update_mastery.py
│   ├── schedule_review.py
│   └── lint_repository.py
│
├── examples/
│   ├── README.md
│   ├── programming-pointer/
│   ├── networking-tcp/
│   ├── cybersecurity-buffer-overflow/
│   ├── mathematics-quadratic-functions/
│   ├── biology-cellular-respiration/
│   ├── chemistry-electrochemical-cells/
│   ├── history-causal-analysis/
│   └── language-vocabulary-patterns/
│
├── evals/
│   ├── README.md
│   ├── teaching-vs-summary.md
│   ├── prerequisite-detection.md
│   ├── misconception-diagnosis.md
│   ├── analogy-quality.md
│   ├── svg-utility.md
│   ├── question-quality.md
│   ├── mastery-calibration.md
│   ├── source-integrity.md
│   └── learner-transfer.md
│
└── tests/
    ├── fixtures/
    ├── test_schema_validation.py
    ├── test_svg_validation.py
    ├── test_source_labels.py
    ├── test_state_transitions.py
    ├── test_mastery_rules.py
    ├── test_dependency_rules.py
    └── test_repository_structure.py
```

## Layer responsibilities

### 1. Theory layer

The theory layer contains stable propositions about learning and teaching. It should not contain tool-specific commands or model-specific prompt syntax.

It answers questions such as:

- What counts as learning?
- Why is teaching modeled as a state transition?
- Why is assessment treated as evidence collection?
- Why are analogies structural mappings rather than decorative examples?
- Why should visuals serve cognition rather than appearance?

Theory files change slowly and require strong justification.

### 2. Runtime layer

The runtime layer defines the decision process executed before and after every teaching turn.

It owns:

- observation;
- diagnosis;
- state transitions;
- strategy selection;
- evidence updates;
- progression decisions;
- review scheduling;
- reflection.

Runtime files must be implementable as deterministic rules, agent policies, or state-machine logic.

### 3. Source layer

The source layer protects factual integrity and traceability. It owns ingestion, normalization, provenance, conflict handling, and extraction failure reporting.

No teaching module may silently convert generated content into source content.

### 4. Teaching layer

The teaching layer defines how explanations, analogies, stories, examples, scaffolds, and remediation are constructed.

It does not decide mastery. It produces instruction and gathers opportunities for evidence.

### 5. Assessment layer

The assessment layer owns question design, answer evaluation, misconception diagnosis, capability evidence, transfer checks, and mastery rubrics.

Assessment must inform the next teaching decision rather than only produce a score.

### 6. Visual layer

The visual layer owns when diagrams are needed, which diagram type is appropriate, how SVGs are generated, and how quality is validated.

Learner-facing ASCII art is prohibited. SVGs must be instructional, accessible, printable, and semantically meaningful.

### 7. Publishing layer

The publishing layer turns structured lesson content into coherent artifacts such as Markdown, HTML, and PDF-ready handouts.

It must preserve vector graphics, source labels, uncertainty, and assessment sections. Raw chat transcripts are not acceptable final teaching artifacts.

### 8. Schemas and templates

Schemas define machine-readable contracts. Templates define human-readable output structure.

These directories prevent each implementation from inventing incompatible state formats and lesson layouts.

### 9. Prompts

Prompts are adapters between runtime decisions and a language model. They are not the source of truth.

A prompt must reference or implement runtime policy. Prompt text should remain replaceable without changing the theory or state model.

### 10. Scripts

Scripts implement deterministic, testable operations such as extraction, schema validation, SVG validation, PDF building, and review scheduling.

A task that can be reliably handled by code should not be delegated entirely to free-form model generation.

### 11. Examples

Every major domain example should include:

- source material or a compact fixture;
- extracted concept graph;
- prerequisite analysis;
- learning contract;
- lesson segments;
- at least one everyday analogy;
- at least one meaningful SVG;
- learner answers including mistakes;
- misconception diagnosis;
- remediation;
- mastery evidence;
- PDF-ready output.

Examples are executable specifications, not marketing demos.

### 12. Evaluations and tests

Evaluations measure teaching quality. Tests measure deterministic correctness.

Examples of evaluation questions:

- Did the system teach or merely summarize?
- Did it identify the missing prerequisite?
- Did the analogy preserve the relevant relationship?
- Did the SVG reduce cognitive load?
- Did the question distinguish multiple misconceptions?
- Was mastery awarded too early?
- Could the learner transfer the concept?

## Dependency order

The recommended implementation order is:

### Phase 0 — Foundation

1. vision;
2. architecture;
3. pedagogy;
4. blueprint;
5. glossary;
6. design principles.

### Phase 1 — Theory

1. learning as state transition;
2. teaching as decision-making;
3. assessment as evidence;
4. understanding as model alignment;
5. runtime over prompt;
6. visual and analogy theory.

### Phase 2 — Core runtime

1. teaching state machine;
2. learner model;
3. evidence engine;
4. decision engine;
5. strategy selection;
6. teaching runtime;
7. mastery engine;
8. orchestration.

### Phase 3 — Source and lesson planning

1. source integrity;
2. ingestion and normalization;
3. concept extraction;
4. dependency graph;
5. learning contract;
6. lesson planner.

### Phase 4 — Teaching and assessment

1. explanation patterns;
2. analogy engine;
3. visual trigger policy;
4. question engine;
5. misconception diagnosis;
6. remediation;
7. transfer and teach-back.

### Phase 5 — Publishing

1. lesson document model;
2. SVG templates and validation;
3. PDF-ready workflow;
4. source labels and accessibility.

### Phase 6 — Examples and evaluation

1. pointer example;
2. TCP example;
3. one non-technical example;
4. evaluation suites;
5. deterministic tests.

### Phase 7 — Skill packaging

Only after the runtime contracts stabilize:

1. write the root `SKILL.md`;
2. add model-specific compatibility notes;
3. create install instructions;
4. publish v0.1.

## Merge criteria

A new module should be merged only when:

- its responsibility is not already owned elsewhere;
- inputs and outputs are explicit;
- failure modes are documented;
- at least one concrete example is included or planned;
- verification criteria exist;
- it does not blur theory, runtime, prompt, and implementation layers;
- it improves learner capability rather than repository size.

## Scope controls

The following features are intentionally deferred until the core runtime is proven:

- animated SVG lessons;
- voice tutoring;
- full web application;
- multiplayer classrooms;
- badges and gamification;
- large public analogy databases;
- automated web research pipelines;
- model fine-tuning;
- recommendation feeds;
- teacher dashboards.

These may become useful later, but none should block a strong v0.1 framework.

## v0.1 completion definition

Version 0.1 is complete when the repository can guide a compatible agent through this end-to-end flow:

1. accept a small source document;
2. preserve source provenance;
3. extract a concept and its prerequisites;
4. create a learning contract;
5. teach using intuition, a daily-life analogy, formal explanation, and SVG when useful;
6. pause for a diagnostic question;
7. classify a wrong answer;
8. change teaching strategy;
9. verify understanding with a fresh problem;
10. record capability-level evidence;
11. decide whether progression is justified;
12. produce a structured PDF-ready lesson artifact.

## Blueprint stability policy

This blueprint is a planning contract, not an immutable law.

Changes are allowed when they:

- remove duplicated responsibility;
- simplify the architecture;
- improve testability;
- reveal a missing foundational concern;
- respond to evidence from examples or evaluations.

Changes should not be made merely to add fashionable terminology or inflate the apparent scope of the project.

## Final architectural test

Before any future commit, ask:

> Does this change make ULS better at producing measurable learning, or does it only make the repository look more ambitious?
