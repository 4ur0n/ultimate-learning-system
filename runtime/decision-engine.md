# Decision Engine

## Purpose

The Decision Engine determines what the system should do next before any learner-facing response is generated.

Its responsibility is not to write the response. Its responsibility is to select the next instructional action that is most likely to improve learning while preserving source integrity, learner autonomy, and cognitive manageability.

The Decision Engine exists because a language model that responds directly to the latest message will often optimize for conversational fluency rather than learning outcomes. ULS inserts an explicit decision layer between observation and generation.

## Core proposition

> Every teaching turn is a decision under uncertainty about the learner's current mental model.

The engine therefore must:

1. inspect the current lesson state;
2. interpret the learner's latest evidence;
3. identify the most important uncertainty;
4. select one primary instructional action;
5. define the evidence expected from that action;
6. prevent progression when the decision is not justified;
7. produce a structured decision record before response generation.

## Non-responsibilities

The Decision Engine does not:

- generate final prose;
- create SVG markup;
- decide factual truth independently of the Source Integrity layer;
- assign mastery without evidence;
- permanently classify the learner;
- replace the Lesson Planner;
- schedule long-term review by itself;
- choose a teaching strategy only because it is entertaining.

It coordinates those responsibilities through explicit outputs.

## Required inputs

A decision should consume the following state when available.

### Lesson state

- current lesson objective;
- current concept;
- current concept state;
- lesson mode;
- current segment;
- concepts already covered;
- concepts intentionally excluded;
- remaining lesson budget;
- stopping conditions.

### Learner state

- stated goal;
- prior knowledge;
- recent answers;
- confidence reports;
- hints used;
- recurring misconceptions;
- preferred or effective example domains;
- signs of fatigue or overload;
- current engagement level, when observable without overclaiming.

### Knowledge state

- prerequisite graph;
- expert concept model;
- unresolved dependencies;
- concept difficulty;
- misconception risks;
- source coverage;
- uncertainty or conflict flags.

### Evidence state

- recall evidence;
- explanation evidence;
- application evidence;
- transfer evidence;
- teach-back evidence;
- independence level;
- response quality;
- evidence recency;
- contradictory evidence.

### Interaction state

- latest learner message;
- prior tutor action;
- whether the last strategy succeeded;
- how many repair attempts have occurred;
- whether the learner requested a specific format or pace;
- whether the learner asked to stop, skip, review, or go deeper.

## Decision output

The engine must output a structured decision record.

Recommended shape:

```yaml
decision_id: unique identifier
current_concept: pointer
observed_signal: learner confuses address with stored value
primary_uncertainty: whether memory address is understood
instructional_goal: repair address-value distinction
selected_action: contrastive_example
secondary_actions:
  - svg_memory_layout
  - focused_question
blocked_actions:
  - advance_to_pointer_arithmetic
reason: prerequisite relation is unstable
evidence_requested: explain why two addresses may contain equal values
success_condition: learner distinguishes location from content without hints
fallback_action: everyday_analogy
stop_condition: repeated confusion reveals missing variable-memory prerequisite
source_policy: no new factual enrichment required
```

The exact serialization may vary, but the semantic fields should remain available for inspection and testing.

## Primary action taxonomy

The Decision Engine selects one primary action per turn. Secondary actions may support it, but they must not compete with the primary goal.

### `DIAGNOSE`

Use when the cause of difficulty is unclear.

Typical behavior:

- ask one high-information question;
- distinguish between two or more plausible misconceptions;
- avoid teaching a large amount before the uncertainty is resolved.

### `ACTIVATE_PREREQUISITE`

Use when the learner likely knows the prerequisite but has not retrieved it recently.

Typical behavior:

- ask for a short recall or connection;
- provide a minimal cue if needed;
- connect the prerequisite to the current concept.

### `TEACH_NEW_CONCEPT`

Use when prerequisites are sufficiently stable and the concept is new.

Typical behavior:

- establish relevance;
- build intuition;
- introduce one concrete example;
- add formal language progressively;
- pause before overload.

### `DEMONSTRATE`

Use when the learner needs to observe a complete or partially complete worked example.

Typical behavior:

- show decisions and reasons;
- ask the learner to predict an important step;
- avoid presenting unexplained procedures.

### `ELICIT_RECALL`

Use when evidence of memory is needed.

Typical behavior:

- ask for free or focused recall;
- avoid recognition cues unless necessary;
- delay summary until after an attempt.

### `ELICIT_EXPLANATION`

Use when the learner may remember a term but the mental model is uncertain.

Typical behavior:

- request explanation in original words;
- ask for a relationship, mechanism, or reason;
- avoid grading based only on vocabulary match.

### `ELICIT_APPLICATION`

Use when the concept appears understood but independent use is unproven.

Typical behavior:

- present a representative problem;
- require method selection or prediction;
- avoid copying the worked example too closely.

### `ELICIT_TRANSFER`

Use when application is stable and broader generalization must be tested.

Typical behavior:

- change the surface context;
- preserve the underlying structure;
- ask the learner to identify why the concept applies.

### `REMEDIATE`

Use when the learner's evidence is weak and the cause is sufficiently diagnosed.

Typical behavior:

- target the specific misconception;
- materially change representation or strategy;
- verify with a fresh question.

### `COMPARE`

Use when confusion comes from neighboring concepts.

Typical behavior:

- contrast similarities and differences;
- include examples and non-examples;
- ask the learner to classify a new case.

### `VISUALIZE`

Use when a structural, temporal, spatial, hierarchical, or causal relationship is difficult to maintain in working memory.

Typical behavior:

- request one SVG with one learning objective;
- define the relationship the learner must notice;
- follow with a visual interpretation question.

### `SUMMARIZE_MODEL`

Use when multiple details need to be compressed into a coherent mental model.

Typical behavior:

- state the few relationships that organize the topic;
- omit nonessential detail;
- ask the learner to reconstruct the model afterward.

### `REVIEW`

Use when earlier learning should be retrieved after delay or before dependent material.

Typical behavior:

- use active recall;
- interleave related concepts when appropriate;
- update evidence rather than merely repeating notes.

### `ADVANCE`

Use when progression is justified.

Typical behavior:

- connect the mastered concept to the next one;
- make the dependency explicit;
- update lesson state.

### `PAUSE`

Use when cognitive load, fatigue, uncertainty, or lesson boundaries make continuation unproductive.

Typical behavior:

- summarize current progress;
- record fragile concepts;
- suggest the next starting point;
- avoid framing a pause as failure.

### `STOP`

Use when the learning contract is complete, the learner requests termination, or safe and accurate teaching cannot continue.

Typical behavior:

- produce the exit contract;
- record mastery evidence and unresolved areas;
- identify review actions.

## Decision sequence

Each turn should follow this sequence.

### Step 1: Observe

Extract observable signals without interpretation.

Examples:

- learner gave the correct output but no reasoning;
- learner used the term "address" to mean "value";
- learner requested a simpler explanation;
- learner answered confidently and incorrectly;
- learner stopped responding to the requested task;
- learner asked to skip ahead.

Do not immediately convert observations into stable traits.

### Step 2: Validate source conditions

Before teaching, check whether:

- the source supports the intended claim;
- important source content is unreadable;
- multiple sources conflict;
- external enrichment would require verification;
- the lesson is about a high-stakes domain requiring stronger caution.

If source integrity is insufficient, the decision may become `DIAGNOSE`, `PAUSE`, or `STOP` rather than generation.

### Step 3: Identify the current instructional objective

The engine must select one concept-sized objective.

Bad objective:

- understand computer memory.

Better objective:

- distinguish a memory address from the value stored at that address.

If the objective cannot be stated precisely, the engine should not produce a long teaching response.

### Step 4: Check prerequisite readiness

For each required prerequisite, determine:

- is there recent evidence?
- is the evidence independent?
- is the prerequisite stable enough for the current task?
- is the prerequisite merely helpful or truly required?

Do not force unnecessary prerequisite detours.

### Step 5: Classify the learner's current need

Classify the primary need as one of:

- missing knowledge;
- unstable recall;
- incomplete mental model;
- confusion between concepts;
- procedural weakness;
- inability to apply;
- inability to transfer;
- vocabulary or notation issue;
- cognitive overload;
- low engagement;
- source uncertainty;
- no problem: ready to advance.

The classification may remain probabilistic.

### Step 6: Estimate uncertainty

The engine should identify the most important unknown that affects the next action.

Examples:

- Does the learner understand the concept but lack vocabulary?
- Was the answer guessed?
- Is the wrong answer caused by a missing prerequisite?
- Did the previous analogy create a false mapping?
- Is the learner overloaded or merely requesting brevity?

When uncertainty is high, prefer a diagnostic question over a large explanation.

### Step 7: Select a primary action

Choose the action that is expected to produce the most useful improvement or evidence with the least unnecessary complexity.

The engine should prefer:

- diagnosis before remediation;
- retrieval before restudy;
- focused repair before complete reteaching;
- one strong analogy before several weak ones;
- one meaningful SVG before decorative visuals;
- a fresh verification question after feedback;
- progression only after sufficient evidence.

### Step 8: Select a strategy

The primary action is what to do. The strategy is how to do it.

Possible strategies include:

- intuitive explanation;
- formal definition;
- everyday analogy;
- story;
- worked example;
- contrastive example;
- non-example;
- SVG diagram;
- guided discovery;
- Socratic questioning;
- code tracing;
- debugging;
- simulation;
- physical or spatial model;
- learner teach-back.

Strategy selection must consider recent effectiveness and the misconception being targeted.

### Step 9: Define requested evidence

Every instructional action should specify what learner evidence should follow.

Examples:

- explain the distinction in their own words;
- predict the next event in a sequence;
- classify a new example;
- apply the concept without hints;
- identify what is wrong with an explanation;
- transfer the concept to a new domain.

An explanation without planned evidence collection is incomplete teaching.

### Step 10: Apply blockers

Block an action when:

- a required prerequisite is fragile;
- source evidence is insufficient;
- the learner explicitly requested a pause;
- cognitive load is too high;
- mastery evidence is too narrow;
- the next action would repeat a failed strategy without modification;
- the request exceeds the system's safe or factual capability.

### Step 11: Generate the decision record

The record should be available before the learner-facing response is produced.

### Step 12: Reflect after the response

After the learner responds, compare the outcome with the success condition.

Ask:

- Did the selected action reduce uncertainty?
- Did the learner produce the intended evidence?
- Did the strategy reveal or repair the misconception?
- Did cognitive load increase unnecessarily?
- Should the learner model be updated?
- Is progression now justified?

## Confusion routing

When the learner is confused, use the following routing logic.

### Vocabulary confusion

Signals:

- concept is described correctly but formal term is missing;
- wrong label is used for a correct relationship.

Preferred actions:

- map plain language to formal vocabulary;
- use brief contrast;
- verify with a labeling task.

Avoid:

- reteaching the whole concept.

### Representation confusion

Signals:

- learner understands prose but not formula, code, graph, diagram, or notation;
- learner cannot translate between representations.

Preferred actions:

- align two representations side by side;
- explain mapping explicitly;
- ask learner to translate a new example.

### Missing prerequisite

Signals:

- learner repeatedly fails at the same dependency;
- answers reveal no usable model of a required prior concept.

Preferred actions:

- explain why the prerequisite matters;
- create a short prerequisite detour;
- return to the original concept after verification.

### Concept confusion

Signals:

- two related concepts are treated as identical;
- correct properties are assigned to the wrong concept.

Preferred actions:

- comparison;
- examples and non-examples;
- classification task;
- comparison SVG when useful.

### Procedure confusion

Signals:

- learner knows what a concept means but cannot execute steps;
- method selection or step ordering is unstable.

Preferred actions:

- worked example;
- faded example;
- step prediction;
- independent parallel problem.

### Application weakness

Signals:

- explanation is correct but learner cannot use the concept.

Preferred actions:

- guided application;
- representative scenario;
- reduce scaffolding gradually.

Avoid:

- repeating definitions.

### Transfer weakness

Signals:

- learner succeeds only when surface details match the example.

Preferred actions:

- vary context;
- ask which features matter;
- use mixed classification;
- request learner-generated example.

### Analogy overextension

Signals:

- learner applies features of the analogy that do not belong to the target concept.

Preferred actions:

- state the analogy boundary;
- provide a counterexample;
- transition to the formal model;
- verify the corrected mapping.

### Overload

Signals:

- declining answer quality across a long segment;
- learner reports losing track;
- many new terms appear in one turn;
- learner answers only the latest detail and loses the main model.

Preferred actions:

- pause;
- reduce the objective;
- summarize the mental model;
- remove optional detail;
- ask one small question.

## Confidence and correctness matrix

### Correct and confident

Possible interpretation:

- stable knowledge, or overconfidence after a lucky answer.

Decision:

- request reasoning or a transfer item before mastery if evidence is narrow.

### Correct and uncertain

Possible interpretation:

- fragile knowledge;
- correct reasoning with poor calibration.

Decision:

- reinforce the reasoning;
- ask a parallel question;
- help calibrate confidence.

### Incorrect and uncertain

Possible interpretation:

- open knowledge gap;
- productive attempt.

Decision:

- diagnose and provide minimal support;
- preserve learner agency.

### Incorrect and confident

Possible interpretation:

- strong misconception;
- ambiguous question;
- source conflict.

Decision:

- inspect reasoning;
- correct explicitly after diagnosis;
- use contrast or counterexample;
- verify with a different surface form.

## Progression rules

The engine may select `ADVANCE` only when:

- required prerequisites are stable enough;
- the current objective has sufficient evidence;
- the learner can respond independently at the required level;
- no unresolved misconception is likely to contaminate the next concept;
- progression remains consistent with the learning contract.

Progression does not always require full mastery. Some lessons may permit provisional progression when:

- the concept is not a hard prerequisite;
- the learner can continue safely;
- review is scheduled;
- the provisional status is recorded.

## Remediation strategy rotation

A failed explanation must not be repeated unchanged.

After a failed attempt, choose among:

1. clarify the confusing point;
2. simplify language;
3. reduce the number of relationships;
4. change analogy domain;
5. add an SVG;
6. use a worked example;
7. contrast with a non-example;
8. revisit a prerequisite;
9. ask guided questions;
10. request learner teach-back.

The engine should record which strategy failed and why it may have failed.

## Visual decision policy

Select `VISUALIZE` when the learner must understand:

- sequence;
- hierarchy;
- spatial arrangement;
- state transition;
- causality;
- component interaction;
- comparison across several dimensions;
- quantitative shape or trend;
- memory or system layout.

Do not select it when:

- the concept is a simple verbal definition;
- the visual would duplicate already clear prose;
- the diagram would contain excessive text;
- the learner's current issue is vocabulary rather than structure;
- no clear visual learning objective exists.

When selected, the decision record must specify:

- diagram type;
- learning objective;
- key relationship;
- required labels;
- what the learner will be asked to infer from it.

ASCII art is not a valid learner-facing visual action.

## Analogy decision policy

Select an analogy when:

- the target is abstract;
- the learner has no stable prior model;
- a familiar relational structure can bridge the gap;
- formal explanation alone has failed or would impose high load.

Do not select an analogy when:

- the target is already concrete;
- the mapping is superficial;
- likely limitations would create more confusion;
- a direct demonstration is clearer;
- the learner already has a correct formal model and needs practice instead.

The decision record should include:

- source domain;
- target domain;
- intended mapping;
- likely failure point;
- transition back to the formal model.

## Story decision policy

Select a story when:

- temporal or causal structure matters;
- the learner benefits from contextualized events;
- narrative can reduce abstraction without distorting the concept.

A story should not be selected merely for entertainment. It must preserve the target structure and lead back to explicit concepts.

## Lesson-mode modifiers

### Quick mode

Prefer:

- essential path only;
- compact explanation;
- one high-information check;
- minimal optional enrichment.

Do not remove prerequisite safeguards.

### Deep mode

Prefer:

- mechanisms;
- multiple representations;
- edge cases;
- stronger transfer checks;
- explicit model limitations.

### Exam mode

Prefer:

- syllabus alignment;
- common traps;
- retrieval and discrimination;
- time-aware practice;
- explanation of why distractors are wrong.

Do not reduce learning to answer memorization.

### Socratic mode

Prefer:

- guided questions;
- learner-generated reasoning;
- delayed explanation.

Do not withhold essential facts the learner cannot infer.

### Expert mode

Prefer:

- compressed basics;
- assumptions;
- tradeoffs;
- exceptions;
- formal detail;
- integration with adjacent models.

Expertise is per topic, not global.

## Safety and integrity overrides

The normal teaching policy may be overridden when:

- the source appears corrupted or incomplete;
- the domain is medical, legal, financial, or otherwise high stakes;
- the learner requests harmful operational guidance;
- the system lacks sufficient factual confidence;
- the requested artifact could misrepresent uncertain information.

Possible actions:

- narrow the scope;
- request clarification;
- distinguish general education from professional advice;
- verify externally when permitted;
- refuse unsafe operational detail;
- stop publication until uncertainty is resolved.

## Determinism and flexibility

The Decision Engine should be predictable enough to test but flexible enough to handle varied learners.

Use hard rules for:

- source integrity;
- mastery evidence;
- prerequisite blockers;
- learner-facing ASCII prohibition;
- repeated-strategy failure;
- explicit learner stop requests.

Use scored or heuristic selection for:

- analogy domain;
- explanation depth;
- diagram type;
- question difficulty;
- amount of scaffolding;
- optional curiosity connections.

## Suggested action scoring

An implementation may score candidate actions using:

```text
action_score =
  expected_learning_gain
  + expected_information_gain
  + relevance_to_objective
  + learner_fit
  - cognitive_load
  - repetition_cost
  - source_risk
  - progression_risk
```

The exact formula is implementation-specific. The important requirement is that action selection be explainable.

## Tie-breaking rules

When two actions score similarly, prefer:

1. the action that gathers more useful evidence;
2. the less cognitively demanding action;
3. the action that preserves learner production;
4. the action that changes a previously failed strategy;
5. the action that stays closer to the current objective;
6. the action requiring fewer unsupported assumptions.

## Failure modes

### Response-first behavior

Symptom:

- the model begins writing before deciding the objective.

Prevention:

- require a decision record before generation.

### Explanation reflex

Symptom:

- every learner difficulty triggers another explanation.

Prevention:

- diagnose whether the problem is recall, application, representation, or misconception.

### Premature advancement

Symptom:

- one correct answer triggers the next topic.

Prevention:

- require evidence appropriate to the learning contract.

### Excessive diagnosis

Symptom:

- the learner is repeatedly tested before receiving useful instruction.

Prevention:

- ask the smallest number of questions needed to choose an action.

### Strategy repetition

Symptom:

- the same analogy or definition is repeated after failure.

Prevention:

- store failed strategies and require a material change.

### Visual overproduction

Symptom:

- every concept receives a decorative diagram.

Prevention:

- require a visual learning objective and interpretation question.

### Prerequisite rabbit hole

Symptom:

- the system expands into many background topics and never returns.

Prevention:

- distinguish required from optional prerequisites;
- define a bounded detour;
- return explicitly to the original objective.

### Learner-style stereotyping

Symptom:

- one successful sports analogy causes every future lesson to use sports.

Prevention:

- treat strategy preferences as tentative and topic-dependent.

### Confidence substitution

Symptom:

- confidence is treated as evidence of mastery.

Prevention:

- compare confidence with independent performance.

### Fluent-source contamination

Symptom:

- generated enrichment is presented as if it came from the source.

Prevention:

- carry content-class labels into every decision.

## Runtime examples

### Example 1: Learner says "I understand pointers" after explanation

Observed signal:

- self-report only;
- no independent evidence.

Decision:

- `ELICIT_EXPLANATION`.

Requested evidence:

- explain the difference between an address and the value at that address.

Blocked action:

- `ADVANCE` to pointer arithmetic.

### Example 2: Learner explains TCP correctly but chooses TCP for live voice because it is "more reliable"

Observed signal:

- core definition known;
- tradeoff reasoning incomplete.

Decision:

- `COMPARE` TCP and UDP under latency-sensitive conditions.

Strategy:

- scenario comparison plus sequence SVG if needed.

Requested evidence:

- choose a protocol for a new scenario and justify the tradeoff.

### Example 3: Learner repeatedly confuses stack with heap

Observed signal:

- neighboring concepts merged;
- previous definition-based repair failed.

Decision:

- `REMEDIATE` using a comparison SVG and code-lifetime example.

Blocked strategy:

- repeat definitions.

Requested evidence:

- classify three variables and explain lifetime and ownership.

### Example 4: Learner answers a formula problem correctly but cannot explain why

Observed signal:

- procedural success;
- conceptual evidence absent.

Decision:

- `ELICIT_EXPLANATION` or `COMPARE` depending on the concept.

Do not mark conceptual mastery yet.

### Example 5: Learner asks to skip prerequisites for a quick exam review

Observed signal:

- explicit speed preference;
- exam mode.

Decision:

- evaluate whether prerequisites are hard blockers.

If not hard blockers:

- provide compressed refresher and continue provisionally.

If hard blockers:

- explain the minimum detour and keep it bounded.

## Verification criteria

The Decision Engine is functioning correctly when:

- different learner errors route to different actions;
- repeated failure changes strategy;
- mastery is not awarded from self-report alone;
- high uncertainty produces focused diagnosis;
- clear readiness produces progression without unnecessary testing;
- SVG is selected for structural need, not decoration;
- analogies include intended mapping and limits;
- source uncertainty blocks unsupported teaching;
- each turn has one primary objective;
- decision records explain why an action was chosen.

## Evaluation scenarios

At minimum, implementations should test:

1. correct answer with weak reasoning;
2. incorrect answer with strong confidence;
3. missing prerequisite;
4. vocabulary-only confusion;
5. representation translation failure;
6. application failure after correct explanation;
7. transfer failure after familiar practice;
8. repeated failure under the same strategy;
9. learner overload;
10. learner request to pause;
11. source conflict;
12. unnecessary SVG temptation;
13. premature mastery temptation;
14. quick-mode prerequisite tradeoff;
15. successful mastery and progression.

## Minimal compliance

A minimal ULS-compatible Decision Engine must:

- identify one current objective;
- inspect prerequisite readiness;
- classify the learner's primary need;
- choose one primary action;
- state why that action was selected;
- define requested evidence;
- block unjustified progression;
- change strategy after repeated failure;
- preserve source-integrity constraints;
- output a decision record before learner-facing generation.

## Final decision question

Before every teaching turn, ask:

> What single next action will create the strongest useful evidence of learning without adding unnecessary cognitive load?
