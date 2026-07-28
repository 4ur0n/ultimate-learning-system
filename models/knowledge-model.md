# Knowledge Model

## Purpose

The Knowledge Model represents the structure of the subject being taught.

It is not a chapter outline, a flat list of terms, or a copy of the source document. It is a structured model of concepts, relationships, prerequisites, misconceptions, representations, applications, and source provenance that allows ULS to reason about what should be taught, in what order, and with which instructional supports.

## Core principle

> A source presents information. A Knowledge Model represents the learnable structure behind that information.

## Responsibilities

The Knowledge Model stores:

- concepts and their meanings;
- prerequisite and dependency relationships;
- part-whole, causal, temporal, comparative, and application relationships;
- concept difficulty and cognitive demands;
- likely misconceptions and confusable neighbors;
- useful examples, non-examples, analogies, and applications;
- appropriate visual representations;
- assessment targets;
- source provenance and uncertainty;
- optional enrichment and world connections;
- domain-specific safety or integrity constraints.

It does not store learner-specific mastery, teacher hypotheses, or raw evidence from student responses.

## Model structure

### Concept node

Each concept should be represented as a structured node.

Recommended fields:

```yaml
id: pointer
name: Pointer
summary: A value that identifies a memory location.
formal_definition: A typed value whose interpretation is a memory address.
source_refs:
  - source-12
concept_type: abstract-mechanism
importance: core
prerequisites:
  - memory-location
  - address
capabilities:
  - recall
  - explain
  - trace
  - apply
  - debug
  - transfer
common_misconceptions:
  - pointer-is-value
  - pointer-is-variable-name
visual_candidates:
  - memory-layout
analogy_candidates:
  - hotel-room-number
  - street-address
applications:
  - linked-structures
  - dynamic-memory
uncertainty: low
```

### Relationship types

The Knowledge Model should support explicit typed edges.

#### `REQUIRES`

A concept cannot be learned reliably without the prerequisite.

Example:

```text
pointer REQUIRES address
```

#### `HELPS`

A concept improves understanding but is not a hard blocker.

```text
binary-numbers HELPS memory-addressing
```

#### `PART_OF`

Represents composition or hierarchy.

```text
retransmission PART_OF tcp-reliability
```

#### `CAUSES`

Represents causal influence.

```text
packet-loss CAUSES retransmission
```

#### `PRECEDES`

Represents temporal or procedural ordering.

```text
syn PRECEDES syn-ack
```

#### `CONTRASTS_WITH`

Represents concepts that should be compared.

```text
tcp CONTRASTS_WITH udp
```

#### `REPRESENTED_BY`

Connects a concept to a notation, diagram, formula, or implementation.

```text
pointer REPRESENTED_BY memory-layout-svg
```

#### `APPLIED_IN`

Connects a concept to real-world or technical uses.

```text
udp APPLIED_IN live-voice
```

#### `MISCONCEIVED_AS`

Represents a common incorrect mapping.

```text
pointer MISCONCEIVED_AS stored-value
```

#### `GENERALIZES_TO`

Represents conceptual extension.

```text
one-dimensional-array GENERALIZES_TO multidimensional-array
```

#### `INSTANCE_OF`

Represents example membership.

```text
quicksort INSTANCE_OF divide-and-conquer
```

## Prerequisite graph

The prerequisite graph must distinguish hard and soft dependencies.

### Hard prerequisite

A hard prerequisite blocks progression when absent.

Criteria:

- the target concept directly uses the prerequisite;
- misunderstanding the prerequisite causes systematic errors;
- instruction cannot be made accurate without it.

### Soft prerequisite

A soft prerequisite improves fluency or depth but can be compressed or deferred.

### Diagnostic prerequisite

A concept that may not be formally required but is useful for detecting a learner's mental model.

### Optional enrichment

Interesting context that must not expand the core learning path unless requested.

## Dependency quality rules

1. Do not infer prerequisites solely from chapter order.
2. Avoid circular hard dependencies.
3. Explain why each hard prerequisite matters.
4. Keep prerequisite detours bounded.
5. Prefer the minimum path that supports the learner's goal.
6. Record alternative paths when multiple valid sequences exist.
7. Distinguish conceptual prerequisites from vocabulary prerequisites.

## Concept granularity

Concepts should be small enough to teach and assess but large enough to carry meaningful structure.

Too broad:

- computer networking;
- calculus;
- cell biology.

Too narrow:

- one isolated symbol with no independent learning objective;
- a single incidental sentence from a source.

Good granularity:

- distinguish address from value;
- explain retransmission after packet loss;
- interpret the slope of a position-time graph;
- compare oxidation and reduction.

A concept node should normally support one primary learning objective.

## Difficulty model

Difficulty should be multidimensional rather than a single number.

Recommended dimensions:

- intrinsic complexity;
- prerequisite count;
- abstraction level;
- number of interacting relationships;
- notation burden;
- procedural demand;
- misconception risk;
- transfer distance;
- visual dependence;
- working-memory demand.

Example:

```yaml
concept: pointer
difficulty:
  intrinsic_complexity: high
  abstraction: high
  notation_burden: medium
  misconception_risk: high
  visual_dependence: high
  prerequisite_count: 3
```

Difficulty is relative to the learner's current model and goal. The Knowledge Model stores baseline difficulty; the Student Model determines learner-specific difficulty.

## Misconception model

Each misconception should include:

```yaml
id: pointer-is-value
target_concepts:
  - pointer
  - address
  - stored-value
incorrect_claim: A pointer is the value stored in memory.
likely_origin:
  - notation confusion
  - missing location-content distinction
diagnostic_prompts:
  - Can two different addresses store equal values?
repair_strategies:
  - contrastive memory layout
  - hotel address analogy
counterexample:
  - two variables with equal values at different addresses
```

Misconceptions belong to the Knowledge Model when they are common domain patterns. Whether a specific learner holds one belongs to the Student Model.

## Representation model

Concepts may have multiple representations:

- verbal;
- symbolic;
- mathematical;
- graphical;
- spatial;
- procedural;
- code;
- simulation;
- physical analogy.

The model should specify transformations between representations.

Example:

```text
pointer code syntax -> memory layout -> verbal explanation
```

Representation translation is itself a learnable capability.

## Visual model

Every concept may define whether a visual is:

- unnecessary;
- optional;
- recommended;
- required for the target objective.

Visual entries should specify:

- diagram type;
- learning objective;
- key relationship;
- labels;
- likely visual misconception;
- interpretation question.

Example:

```yaml
concept: tcp-handshake
visual:
  priority: required
  type: sequence-diagram
  objective: Show ordered exchange and direction of SYN, SYN-ACK, and ACK.
  key_relationship: state establishment through ordered messages
  interpretation_question: What information does the second message acknowledge?
```

Learner-facing ASCII art is not a valid visual representation.

## Analogy model

Analogy candidates should be curated as structural mappings.

Recommended fields:

```yaml
id: hotel-address
source_domain: hotel rooms
 target_domain: memory locations
mapping:
  room-number: address
  room-content: stored-value
  note-with-room-number: pointer
intended_relationship: identifier points to location rather than being the content
limitations:
  - hotel room numbers do not support pointer arithmetic
  - memory access has type and safety constraints
misuse_risk: medium
```

The Knowledge Model stores candidates and limitations. The Teacher Model chooses whether to use them for a specific learner.

## Example and non-example model

Each concept should include examples that expose the underlying structure.

A strong example record includes:

- why it is representative;
- which features matter;
- which features are incidental;
- expected learner inference;
- possible confusion.

Non-examples are especially useful for boundaries and discrimination.

## Capability targets

The Knowledge Model defines which capabilities matter for a concept.

Possible targets:

- recall;
- explanation;
- prediction;
- classification;
- procedure selection;
- execution;
- debugging;
- comparison;
- application;
- transfer;
- teaching-back.

Not every concept requires every capability.

Example:

```yaml
concept: tcp-vs-udp
required_capabilities:
  recall: basic
  explain: required
  compare: required
  apply: required
  transfer: required
  teach_back: optional
```

## Source provenance

Every factual field should carry provenance when practical.

Recommended source classes:

- `SOURCE`
- `VERIFIED_ENRICHMENT`
- `TEACHING_ANALOGY`
- `INFERENCE`
- `UNCERTAIN`

The model should preserve:

- source identifier;
- page, section, or location;
- extraction confidence;
- conflict status;
- verification status;
- last reviewed date when relevant.

Generated explanations must not overwrite source facts.

## Conflict handling

When sources disagree:

1. preserve both claims;
2. mark the relationship as disputed or context-dependent;
3. identify source scope and date;
4. avoid silently merging incompatible claims;
5. teach the disagreement when it matters;
6. narrow the learning objective if resolution is not possible.

## World connections

The Knowledge Model may connect source concepts to broader applications and contexts.

Examples:

- TCP -> web browsing, SSH, game state synchronization;
- UDP -> voice, live video, fast telemetry;
- oxidation-reduction -> batteries, corrosion, metabolism;
- derivatives -> velocity, optimization, sensitivity.

World connections are optional enrichment unless the learning goal requires them. They must be labeled and verified when factual precision matters.

## Safety and domain constraints

Concept nodes may include policy metadata.

Example:

```yaml
concept: buffer-overflow
safety:
  domain: cybersecurity
  allowed_focus:
    - defensive understanding
    - memory-safety concepts
    - authorized lab analysis
  restricted_focus:
    - unauthorized exploitation
```

This metadata informs runtime decisions without changing the underlying concept structure.

## Suggested graph schema

```yaml
knowledge_model:
  domain: computer-science
  version: 0.1
  concepts:
    pointer:
      type: abstract-mechanism
      prerequisites:
        hard: [memory-location, address]
        soft: [variable]
      relations:
        - type: CONTRASTS_WITH
          target: stored-value
        - type: REPRESENTED_BY
          target: memory-layout-svg
      misconceptions:
        - pointer-is-value
      capabilities:
        - explain
        - trace
        - apply
      source_refs:
        - source-12
  unresolved_conflicts: []
```

## Update rules

1. Source-derived changes must preserve provenance.
2. New edges require a reason and relationship type.
3. Hard prerequisites require stronger justification than soft prerequisites.
4. Common misconceptions should be supported by repeated domain evidence or explicit source guidance.
5. Visual and analogy candidates must include their intended learning purpose.
6. Unverified enrichment remains separate from source truth.
7. Changes should be versioned when they alter learning paths.
8. Learner-specific performance must not be written into the Knowledge Model.

## Failure modes

### Outline masquerading as a graph

Problem: chapter order is copied into a hierarchy without meaningful relationships.

Prevention: typed edges and prerequisite justification.

### Over-fragmentation

Problem: every sentence becomes a concept node.

Prevention: one teachable objective per concept.

### Under-fragmentation

Problem: broad topics cannot be diagnosed or assessed.

Prevention: split by mental-model relationship or independent capability.

### Prerequisite inflation

Problem: the learner is forced through unnecessary background material.

Prevention: hard-versus-soft distinction and minimum-path reasoning.

### Analogy contamination

Problem: analogy features become part of the formal concept.

Prevention: explicit limitations and content class labels.

### Decorative visual metadata

Problem: diagrams are requested without a learning objective.

Prevention: require key relationship and interpretation question.

### Source flattening

Problem: conflicting or uncertain source material becomes one confident claim.

Prevention: provenance and conflict status.

### Static difficulty

Problem: baseline difficulty is treated as universal.

Prevention: learner-specific difficulty remains in the Student Model.

## Verification criteria

The Knowledge Model is valid when:

- concepts are teachable and assessable;
- relationships are typed;
- hard prerequisites are justified;
- alternative paths can be represented;
- misconceptions are distinct from learner evidence;
- visuals have instructional purposes;
- analogy limits are explicit;
- capability targets are concept-specific;
- source provenance is preserved;
- runtime components can compute a bounded learning path.

## Minimal compliance

A minimal implementation must represent:

- concept nodes;
- hard and soft prerequisites;
- typed relationships;
- capability targets;
- baseline difficulty;
- common misconceptions;
- visual recommendations;
- source provenance;
- uncertainty.

## Guiding question

> What structure must the system represent so that it can choose the smallest accurate learning path instead of merely following the source order?
