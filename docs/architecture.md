# System Architecture

## Purpose

This document defines the conceptual architecture of Ultimate Learning System (ULS). It separates responsibilities so that teaching quality, source integrity, assessment, visualization, and learner adaptation can evolve independently without collapsing into one oversized prompt.

ULS is a framework, not a single model. A compatible implementation may use one model or many agents, but the logical responsibilities below must remain explicit.

## Architectural goals

ULS should be:

- model-agnostic;
- source-grounded;
- mastery-based;
- adaptive without becoming unpredictable;
- visual-first when relationships are hard to express in prose;
- auditable through explicit state and evidence;
- modular enough to test each teaching responsibility separately;
- useful in both conversational and document-generation workflows.

## High-level pipeline

```text
Source Material
    ↓
Ingestion and Source Mapping
    ↓
Concept Extraction
    ↓
Knowledge and Dependency Graph
    ↓
Learner Diagnosis
    ↓
Learning Contract and Lesson Plan
    ↓
Teaching Loop
    ↓
Mastery Decision
    ↓
Review Scheduling and Learning Record
    ↓
Lesson Notes / SVG / PDF-ready Output
```

The text diagram above describes system structure for maintainers. Learner-facing instructional diagrams must follow the SVG policy and must not use ASCII art.

## Core engines

### 1. Source Ingestion Engine

Responsibilities:

- read supported source formats;
- preserve document structure, headings, page references, and media references;
- split content into traceable source units;
- identify unreadable, missing, or ambiguous content;
- avoid silently inventing text from extraction failures.

Outputs:

- normalized source units;
- source metadata;
- unresolved extraction warnings;
- provenance identifiers used by later engines.

The engine does not decide how to teach the material.

### 2. Source Integrity Engine

Responsibilities:

- distinguish source content from generated enrichment;
- label analogies, examples, inferences, and uncertain claims;
- maintain citations or source pointers where available;
- prevent generated explanations from being misrepresented as source text;
- flag conflicts between sources rather than silently merging them.

Recommended content classes:

- `SOURCE`: directly supported by provided material;
- `VERIFIED_ENRICHMENT`: externally verified explanatory material;
- `TEACHING_ANALOGY`: deliberately simplified comparison;
- `INFERENCE`: reasoned conclusion not explicitly stated in the source;
- `UNCERTAIN`: unresolved or insufficiently supported claim.

### 3. Concept Extraction Engine

Responsibilities:

- identify concepts, terms, procedures, rules, models, and skills;
- separate core concepts from examples and incidental detail;
- identify likely misconceptions and overloaded terms;
- estimate concept importance relative to the learner's goal;
- represent each concept as a teachable unit.

A concept unit should include:

- identifier and name;
- concise meaning;
- source references;
- prerequisites;
- expected learning outcomes;
- likely misconceptions;
- suggested diagram types;
- candidate examples and assessment forms.

### 4. Knowledge Graph Engine

Responsibilities:

- represent relationships among concepts;
- record prerequisite, part-whole, causal, temporal, comparative, and application relationships;
- detect disconnected or circular dependencies;
- support multiple valid learning paths;
- expose the learner's current position in the topic.

The graph is not merely a chapter outline. It should describe what must be understood before another concept becomes learnable.

### 5. Dependency and Path Engine

Responsibilities:

- identify missing prerequisites;
- compute a minimum viable learning path for the stated goal;
- reorder source material when the source order is pedagogically weak;
- separate required, useful, and optional concepts;
- prevent teaching an advanced concept on top of an unstable foundation.

The engine may recommend a prerequisite detour, but must explain why the detour matters and keep it proportionate.

### 6. Learner Model Engine

Responsibilities:

- store goals, prior knowledge, preferences, and constraints;
- track concept-level evidence rather than one global score;
- distinguish recall, explanation, application, transfer, and teaching-back ability;
- record misconceptions, confidence, hint dependence, and review history;
- adapt examples and explanation depth without stereotyping the learner.

The learner model must not infer stable traits from one response. Adaptation should be based on repeated evidence and remain reversible.

### 7. Diagnostic Engine

Responsibilities:

- determine what the learner already knows before teaching;
- locate the root cause of confusion;
- distinguish a knowledge gap from a reading error, vocabulary issue, calculation mistake, or attention lapse;
- choose whether to review, teach, challenge, or skip a concept;
- avoid unnecessary testing when the learner's goal and prior evidence are already clear.

Diagnosis should use the smallest number of high-information questions necessary.

### 8. Learning Contract Engine

A learning contract defines the scope and expectations of the current lesson.

It should specify:

- the lesson goal;
- the concepts included and excluded;
- required prerequisites;
- expected duration or depth where relevant;
- what evidence will count as mastery;
- what artifact may be produced, such as notes or a PDF-ready handout;
- when the lesson should pause or stop.

The contract should be concise and learner-facing. It is not a legal agreement.

### 9. Lesson Planning Engine

Responsibilities:

- convert the selected path into teachable segments;
- limit each segment to a manageable cognitive load;
- decide where analogies, examples, diagrams, and questions are needed;
- sequence explanation, guided practice, independent practice, and review;
- reserve difficult assessment for after sufficient instruction;
- include retrieval of relevant prior knowledge.

Each lesson segment should have one primary learning objective.

### 10. Teaching Engine

Responsibilities:

- present intuition before unnecessary formalism;
- connect new knowledge to relevant prior knowledge;
- explain using clear language appropriate to the learner;
- use examples, non-examples, and worked examples;
- reveal complexity gradually;
- stop at meaningful checkpoints rather than delivering a lecture-sized response;
- ask the learner to actively produce an answer.

The Teaching Engine coordinates other engines but must not invent assessment evidence or mark mastery by itself.

### 11. Analogy Engine

Responsibilities:

- generate concrete analogies for abstract concepts;
- choose examples compatible with the learner's experience;
- explicitly map corresponding parts of the analogy and target concept;
- state where the analogy breaks down;
- replace an ineffective analogy rather than repeating it;
- avoid analogies that introduce more complexity than they remove.

An analogy is a bridge, not a definition. Formal understanding must eventually replace or refine it.

### 12. Visual Explanation Engine

Responsibilities:

- decide whether a visual would materially improve understanding;
- select an appropriate diagram type;
- generate SVG or an SVG-exportable representation;
- preserve semantic structure and readable labels;
- validate layout, accessibility, and print readability;
- produce diagrams with a single clear learning objective.

Learner-facing ASCII art is prohibited. Suitable visual forms include:

- process and flow diagrams;
- sequence diagrams;
- hierarchy and classification diagrams;
- timelines;
- comparison diagrams;
- memory and spatial layouts;
- concept maps;
- system architecture diagrams;
- state diagrams;
- mathematical and scientific plots.

The engine must not create decorative visuals with no instructional purpose.

### 13. Question Engine

Responsibilities:

- generate questions aligned with the current learning objective;
- vary question forms and cognitive demand;
- avoid giving away the answer in the wording;
- use follow-up questions to distinguish shallow recall from understanding;
- generate hints that preserve productive effort;
- create parallel questions after remediation rather than repeating identical items.

Question categories may include:

- recall;
- explanation in the learner's own words;
- comparison;
- classification;
- prediction;
- application;
- debugging or error analysis;
- transfer to a novel situation;
- teaching-back.

### 14. Evaluation and Misconception Engine

Responsibilities:

- evaluate the reasoning as well as the final answer;
- identify correct, partially correct, guessed, and unsupported responses;
- locate the specific misconception or missing prerequisite;
- recognize when the question itself was ambiguous;
- give feedback without exposing unnecessary answer details before another attempt;
- record evidence in the learner model.

The engine must not reduce every response to a binary correct/incorrect label.

### 15. Remediation Engine

Responsibilities:

- select a new explanation strategy after difficulty;
- target the root cause instead of repeating the original explanation;
- reduce complexity temporarily when cognitive load is too high;
- use a different analogy, representation, example, or modality;
- revisit prerequisites when required;
- verify repair with a fresh question.

A preferred remediation sequence is:

1. clarify the exact confusing step;
2. restate with simpler language;
3. provide a concrete example;
4. provide or revise an SVG visual;
5. contrast with a non-example;
6. revisit a prerequisite;
7. ask the learner to teach the repaired idea back.

This sequence is adaptive, not mandatory in every case.

### 16. Mastery Engine

Responsibilities:

- define evidence thresholds for each concept;
- consider accuracy, independence, reasoning quality, and transfer;
- distinguish "introduced," "practicing," "provisional," and "mastered" states;
- prevent progression when a prerequisite remains unstable;
- avoid requiring exhaustive testing when strong evidence already exists;
- downgrade mastery when later evidence reveals fragility.

A verbal claim such as "I get it" is not sufficient mastery evidence.

### 17. Review Engine

Responsibilities:

- schedule retrieval practice after learning;
- choose review intervals based on performance and forgetting risk;
- interleave related concepts when useful;
- prioritize weak or prerequisite-heavy concepts;
- avoid turning review into passive rereading;
- update the learner model after each review.

A future implementation may use FSRS or another spaced-repetition algorithm, but the architecture must not depend exclusively on one algorithm.

### 18. Publishing Engine

Responsibilities:

- transform lesson content into structured learner materials;
- produce Markdown, HTML, and PDF-ready layouts;
- embed SVG as vector content where the output pipeline supports it;
- include objectives, explanations, examples, diagrams, questions, summaries, and review prompts;
- keep source labels and uncertainty visible;
- avoid publishing raw chat transcripts as finished instructional material.

## The AI Teacher Loop

The central runtime behavior of ULS is the following loop:

1. **Diagnose** — determine the learner's goal, current knowledge, and likely gaps.
2. **Prepare** — choose a concept-sized objective, prerequisites, and mastery evidence.
3. **Connect** — activate relevant prior knowledge and explain why the concept matters.
4. **Teach** — build intuition, then introduce the formal model.
5. **Visualize** — add an SVG when relationships, structure, sequence, or spatial reasoning benefit from it.
6. **Demonstrate** — show a worked example and explain the decisions, not only the steps.
7. **Elicit** — ask the learner to retrieve, explain, predict, or apply.
8. **Evaluate** — inspect reasoning, confidence, independence, and misconceptions.
9. **Repair** — change strategy or revisit prerequisites when evidence is weak.
10. **Verify** — ask a fresh question that tests the repaired understanding.
11. **Master** — update state only when evidence meets the concept's threshold.
12. **Integrate** — connect the concept to the larger knowledge graph.
13. **Review** — schedule future retrieval and record what should be revisited.

The loop operates at concept level. A long chapter should contain multiple loops rather than one explanation followed by one final quiz.

## State model

Each concept should use explicit learning states:

- `UNSEEN`: no meaningful exposure;
- `DIAGNOSED`: prior knowledge has been sampled;
- `INTRODUCED`: explanation has been presented;
- `GUIDED`: learner can respond with support;
- `INDEPENDENT`: learner can solve or explain without support;
- `TRANSFERRED`: learner can use the concept in a novel context;
- `MASTERED`: evidence is sufficiently strong across required dimensions;
- `REVIEW_DUE`: retrieval is needed to confirm retention;
- `FRAGILE`: later evidence suggests mastery was unstable.

Implementations may add states, but they should not collapse all progress into a single percentage.

## Data boundaries

The following records should remain logically separate:

- source graph;
- generated teaching content;
- learner profile;
- concept mastery evidence;
- misconception history;
- review schedule;
- generated artifacts.

This separation reduces contamination between source facts, learner-specific adaptation, and generated material.

## Orchestration rules

1. No engine may mark mastery without assessment evidence.
2. No generated enrichment may be labeled as source content.
3. The Teaching Engine must not present an entire dense chapter when concept-level checkpoints are required.
4. The Visual Engine must not use ASCII art in learner-facing output.
5. The Analogy Engine must state important limitations of an analogy when misunderstanding is likely.
6. The Remediation Engine must change something material after repeated failure.
7. The Dependency Engine may block progression when a required prerequisite is missing.
8. The Publishing Engine must preserve source and uncertainty labels.
9. The Learner Model must remain editable and avoid irreversible personality assumptions.
10. The system should prefer the least complex intervention that produces reliable understanding.

## Failure modes to prevent

ULS must explicitly guard against:

- summarization disguised as teaching;
- excessive lecturing without learner production;
- decorative or unreadable diagrams;
- ASCII diagrams shown to learners;
- analogies treated as exact definitions;
- quizzes that test wording rather than understanding;
- moving forward after one lucky answer;
- repeating the same explanation after failure;
- overwhelming learners with unnecessary prerequisites;
- inventing source claims;
- hiding uncertainty;
- collecting detailed learner state without using it to improve teaching;
- generating polished PDF material that contains unverified errors.

## Minimal viable implementation

A minimal ULS-compatible system must provide:

1. source-aware concept extraction;
2. prerequisite-aware lesson ordering;
3. a concept-level teaching loop;
4. concrete analogies for abstract material;
5. SVG-first learner-facing diagrams;
6. understanding checks with diagnostic feedback;
7. explicit mastery states;
8. source-versus-generated content labeling;
9. PDF-ready structured lesson output.

Spaced repetition, advanced analytics, multiple agents, and automated SVG layout validation may be added incrementally, but the core teaching contract must already be present.

## Architectural quality test

Before adding a feature, ask:

- Which engine owns this responsibility?
- What information does it consume?
- What evidence or artifact does it produce?
- Can its output be inspected or tested?
- Could it accidentally blur source content and generated content?
- Does it improve learner capability, or merely make the system look more sophisticated?
