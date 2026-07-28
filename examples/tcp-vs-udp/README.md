# End-to-End Example: TCP vs UDP

This example demonstrates how ULS teaches a comparison concept without reducing it to a memorized feature table. The learner must explain the trade-off structure, classify application needs, and justify a protocol choice.

---

## 1. Learning contract

```yaml
lesson_id: tcp-vs-udp-001
mode: durable-learning
audience: beginner networking learner
estimated_minutes:
  lower: 40
  upper: 60
  uncertainty: medium
learning_contract:
  goal: Explain the core delivery-model differences between TCP and UDP and justify a protocol choice for a simple application requirement.
  target_concepts:
    - net.transport-layer
    - net.tcp
    - net.udp
    - net.connection-state
    - net.reliability
    - net.ordering
    - net.message-boundary
    - net.latency-overhead-tradeoff
  required_capabilities:
    - explain
    - compare
    - classify
    - apply
  target_depth: conceptual
  completion_condition: The learner independently compares TCP and UDP using delivery guarantees and application requirements, then justifies a fresh protocol choice without answer exposure.
```

This lesson does not establish detailed congestion control, socket API mastery, QUIC behavior, or protocol security.

## 2. Knowledge Model slice

| Concept | Role | Required capability |
|---|---|---|
| `net.transport-layer` | prerequisite | explain |
| `net.application-requirement` | prerequisite | classify |
| `net.tcp` | core | explain |
| `net.udp` | core | explain |
| `net.connection-state` | core | compare |
| `net.reliability` | core | compare |
| `net.ordering` | core | compare |
| `net.message-boundary` | core | compare |
| `net.latency-overhead-tradeoff` | transfer | apply |

```text
net.tcp --PROVIDES--> net.reliable-ordered-byte-stream
net.tcp --MAINTAINS--> net.connection-state
net.tcp --ADDS--> recovery-and-ordering-overhead
net.udp --PROVIDES--> best-effort-datagrams
net.udp --PRESERVES--> message-boundaries
net.udp --OMITS--> built-in-retransmission-and-ordering
application requirements --DETERMINE--> useful transport properties
```

Misconception candidates:

```yaml
- id: tcp-always-better
  claim: TCP is always superior because it is reliable.
- id: udp-always-faster
  claim: UDP guarantees lower application latency in every situation.
- id: udp-no-reliability-possible
  claim: An application using UDP cannot implement reliability itself.
- id: tcp-message-oriented
  claim: TCP preserves application message boundaries.
- id: udp-guarantees-delivery
  claim: A UDP datagram is guaranteed to arrive once sent.
```

## 3. Initial Student Model

```yaml
student_snapshot:
  known:
    - concept: net.packet
      capability: explain
      level: moderate
      evidence_refs: [prior-network-basics-01]
  uncertain:
    - concept: net.transport-layer
      capability: explain
      reason: No direct evidence.
    - concept: net.reliability
      capability: compare
      reason: Learner may equate reliability with general quality.
    - concept: net.message-boundary
      capability: explain
      reason: New distinction.
  fragile: []
  active_misconception_signals: []
```

Teacher hypothesis:

```yaml
id: h-feature-list-without-tradeoff
claim: The learner may remember that TCP is reliable and UDP is fast without understanding why application requirements determine the choice.
confidence: 0.65
next_test: Present two applications with conflicting requirements and ask whether one protocol is always best.
```

## 4. Lesson path

```yaml
units:
  - unit_id: u1-diagnose-transport-goal
    role: diagnostic
    objective: Identify whether the learner distinguishes delivery requirements from protocol names.
    progression_rule: Learner names application requirements before choosing a protocol.
    fallback_route: u2-repair-requirements

  - unit_id: u2-repair-requirements
    role: prerequisite
    objective: Classify delivery requirements such as completeness, order, timeliness, and message boundaries.
    progression_rule: Learner correctly classifies requirements in two examples.
    fallback_route: u2-requirement-matrix

  - unit_id: u3-build-tcp-model
    role: core
    objective: Explain TCP as a connection-oriented reliable ordered byte stream.
    progression_rule: Learner explains both guarantees and costs.
    fallback_route: u3-tcp-flow-diagram

  - unit_id: u4-build-udp-model
    role: core
    objective: Explain UDP as connectionless best-effort datagrams with preserved message boundaries.
    progression_rule: Learner explains both omissions and useful properties.
    fallback_route: u4-udp-datagram-diagram

  - unit_id: u5-compare
    role: practice
    objective: Compare TCP and UDP using the same dimensions.
    progression_rule: Learner completes a fresh comparison without absolute claims.
    fallback_route: u5-contrastive-remediation

  - unit_id: u6-apply
    role: assessment
    objective: Justify a protocol choice from application requirements.
    progression_rule: Learner selects a suitable protocol and explains the trade-off.
    fallback_route: u6-requirement-first-remediation

  - unit_id: u7-transfer
    role: transfer
    objective: Recognize when neither simple protocol stereotype is sufficient.
    progression_rule: Learner explains that applications may add reliability over UDP or tolerate partial loss over TCP alternatives.
```

## 5. Diagnostic interaction

**Scenario A:** A file-transfer tool must deliver every byte in order.

**Scenario B:** A live game sends frequent position updates, and old updates quickly become useless.

**Prompt:** Before naming TCP or UDP, list the most important delivery requirement in each scenario.

Expected evidence:

- File transfer: completeness and ordering dominate.
- Live position updates: timeliness and freshness may dominate; some loss may be tolerable.

Runtime routing:

```text
Requirements identified -> continue to protocol models
Chooses protocol without requirements -> repair requirement classification
Says TCP is always best -> test tcp-always-better hypothesis
Says UDP guarantees speed -> test udp-always-faster hypothesis
```

## 6. Core mental model

TCP and UDP are not “good protocol” versus “bad protocol.” They expose different transport behavior to applications.

### TCP

Think of TCP as a managed byte stream:

- it establishes and tracks connection state;
- it presents bytes in order;
- it detects loss and retransmits;
- it hides packet boundaries from the application;
- these behaviors require state, acknowledgments, buffering, and recovery work.

### UDP

Think of UDP as independent addressed messages:

- each datagram is a separate message;
- message boundaries are preserved;
- delivery, order, and duplicate suppression are not guaranteed by UDP;
- the application may accept loss or implement selected mechanisms itself;
- the simpler service avoids TCP’s built-in stream recovery machinery, but that does not guarantee lower end-to-end latency in every real system.

## 7. Formal comparison

| Dimension | TCP | UDP |
|---|---|---|
| Service model | Reliable ordered byte stream | Best-effort datagrams |
| Connection state | Maintained | No transport connection establishment |
| Delivery guarantee | Retransmission and acknowledgment mechanisms | No built-in guarantee |
| Ordering | In-order delivery to application | No built-in ordering |
| Message boundaries | Not preserved | Preserved per datagram |
| Duplicate handling | Hidden by stream service | Application may observe or handle duplicates |
| Overhead and delay behavior | More built-in control and recovery | Less built-in transport machinery |
| Application responsibility | Simpler when full reliable stream is desired | Greater when custom reliability or timing policy is needed |

The choice depends on what failure behavior the application can tolerate.

## 8. Life analogy with limitations

### Analogy

TCP resembles sending numbered pages through a service that confirms missing pages and presents the complete stack in order.

UDP resembles sending separate postcards: each postcard keeps its own boundary, but some may be lost, duplicated, or arrive out of order.

### Mapping

```text
numbered page stream -> TCP byte stream
missing-page recovery -> retransmission
ordered completed stack -> in-order delivery
individual postcard -> UDP datagram
postcard boundary -> datagram boundary
```

### Limitation

Real networks and protocols are not postal systems. UDP applications can add acknowledgments, sequencing, or recovery. TCP can also suffer latency from loss recovery and head-of-line blocking. The analogy illustrates service behavior, not implementation details or universal performance.

## 9. Instructional SVG specification

```yaml
svg_id: tcp-udp-delivery-model-v1
diagram_type: comparison
instructional_objective: Contrast TCP's ordered recovered byte stream with UDP's independent best-effort datagrams.
entities:
  - id: tcp-sender
    label: TCP sender
    semantic_role: actor
  - id: tcp-segments
    label: bytes split across segments
    semantic_role: input
  - id: tcp-recovery
    label: ordering and recovery
    semantic_role: step
  - id: tcp-receiver-stream
    label: ordered byte stream
    semantic_role: output
  - id: udp-sender
    label: UDP sender
    semantic_role: actor
  - id: udp-datagrams
    label: separate datagrams
    semantic_role: input
  - id: udp-network-outcomes
    label: may arrive, be lost, or reorder
    semantic_role: step
  - id: udp-receiver-messages
    label: received message boundaries
    semantic_role: output
relationships:
  - source: tcp-sender
    target: tcp-segments
    type: flows-to
    direction: forward
  - source: tcp-segments
    target: tcp-recovery
    type: flows-to
    direction: forward
  - source: tcp-recovery
    target: tcp-receiver-stream
    type: transforms-into
    direction: forward
  - source: udp-sender
    target: udp-datagrams
    type: flows-to
    direction: forward
  - source: udp-datagrams
    target: udp-network-outcomes
    type: flows-to
    direction: forward
  - source: udp-network-outcomes
    target: udp-receiver-messages
    type: flows-to
    direction: forward
interpretation_task:
  prompt: Which path restores ordering before delivery, and which path preserves individual message boundaries without built-in recovery?
  target_capability: compare
  expected_observation: TCP restores an ordered stream; UDP preserves datagram boundaries but does not provide built-in recovery or ordering.
```

## 10. Guided comparison

Classify each statement:

1. “The application reads a continuous sequence of bytes.”
2. “One send corresponds to one independently bounded message.”
3. “Missing transport data is retransmitted by the transport protocol.”
4. “The application may prefer a new update over retransmitting an old one.”

Expected classification:

```text
1 -> TCP
2 -> UDP
3 -> TCP
4 -> Often motivates UDP or another timing-aware design, depending on requirements
```

The fourth answer is deliberately not “UDP always.” The requirement motivates a design decision; it does not prove one protocol is universally correct.

## 11. Independent assessment

```yaml
assessment_id: tcp-udp-choice-01
purpose: progression-check
item:
  item_id: voice-call-choice-01
  task_type: extended-explanation
  target_concepts:
    - net.tcp
    - net.udp
    - net.latency-overhead-tradeoff
  target_capability: apply
  transfer_distance: near-transfer
  prompt: |
    A live voice application sends small audio frames continuously.
    A late frame is often less useful than a missing frame because playback has already moved on.
    The application can tolerate occasional loss but must preserve low delay.

    Which transport service model is the better starting point: TCP or UDP?
    Explain the relevant trade-off and name one responsibility the application may need to handle.
  success_condition: Learner chooses UDP as the better starting point, explains timeliness versus built-in recovery, and names an application responsibility such as loss concealment, sequencing, jitter buffering, or optional recovery.
assistance_policy:
  allowed: true
  max_hint_level: 5
  answer_exposure_invalidates_independence: true
```

Rubric:

| Criterion | Complete | Partial | Absent |
|---|---|---|---|
| Requirement extraction | Identifies low delay and tolerance for some loss | Mentions speed without explaining lateness | Ignores requirements |
| Protocol choice | Chooses UDP as a starting point, not a universal rule | Chooses UDP with weak justification | Chooses by stereotype only |
| Trade-off explanation | Explains why retransmitting stale audio may be harmful | Says UDP is faster without mechanism | No valid trade-off |
| Application responsibility | Names one valid responsibility | Vague “handle errors” | None |
| Independence | No answer exposure | Prompted | Full solution exposed |

## 12. Hint ladder

```yaml
level_1: Identify which is worse here: losing one frame or receiving it too late.
level_2: Recall which service performs built-in retransmission and ordered delivery.
level_3: Ask whether retransmitting an old audio frame still helps real-time playback.
level_4: Compare a reliable ordered stream with best-effort independent datagrams.
level_5: The application may prefer timely new frames and handle some loss itself.
level_6: UDP is the better starting point because stale retransmitted audio can miss its playback deadline; the application may handle jitter, sequencing, or loss concealment.
```

## 13. Evidence example

Learner response:

> UDP is the better starting point because the application values timely audio more than recovering every old frame. TCP could delay newer data while recovering missing bytes. With UDP, the application may need sequence numbers and a jitter buffer, and it may conceal missing audio.

Evidence record:

```yaml
evidence_id: ev-tcp-udp-001
capability: apply
correctness: correct
completeness: complete
independence: independent
hints_used: 0
highest_hint_level: 0
answer_exposure: none
transfer_distance: near-transfer
claims_supported:
  - Protocol choice should follow application requirements.
  - TCP provides ordered reliable stream behavior with recovery costs.
  - UDP omits built-in recovery and may require application mechanisms.
claims_weakened:
  - tcp-always-better
  - udp-always-faster
misconception_signals: []
evaluator:
  type: rubric-based-model
  confidence: 0.93
```

## 14. Student Model update

```yaml
updates:
  - concept: net.tcp
    capability: explain
    previous_state: uncertain
    new_state: moderate
    evidence_refs: [ev-tcp-udp-001]
  - concept: net.udp
    capability: explain
    previous_state: uncertain
    new_state: moderate
    evidence_refs: [ev-tcp-udp-001]
  - concept: net.latency-overhead-tradeoff
    capability: apply
    previous_state: uncertain
    new_state: moderate
    evidence_refs: [ev-tcp-udp-001]
limitations:
  - Delayed retention has not been measured.
  - The learner has not designed a real transport protocol.
  - Security and congestion-control implications have not been assessed.
```

## 15. Transfer task

**Scenario:** A telemetry device sends one temperature reading every second. Missing one reading is acceptable, but each received reading must be identifiable as one complete sample. The system does not need old readings retransmitted after several seconds.

**Prompt:** Choose a transport service model and justify the choice. Then state one condition that could make TCP a better choice instead.

Expected reasoning:

- UDP is a reasonable starting point because samples are independent, message boundaries matter, and stale recovery may be unnecessary.
- TCP might be better if every reading must be delivered, if the application needs a simple reliable stream, or if operational constraints make application-level recovery undesirable.

## 16. Review plan

```yaml
review:
  - concept: net.tcp
    capability: explain
    reason: central
    earliest_after_seconds: 86400
    latest_after_seconds: 259200
    retrieval_before_restudy: true
  - concept: net.udp
    capability: compare
    reason: misconception-risk
    earliest_after_seconds: 172800
    latest_after_seconds: 432000
    retrieval_before_restudy: true
  - concept: net.latency-overhead-tradeoff
    capability: apply
    reason: retention-required
    earliest_after_seconds: 259200
    latest_after_seconds: 604800
    retrieval_before_restudy: true
```

## 17. Publishing artifact

```yaml
artifact_id: tcp-vs-udp-learner-handout-v1
type: printable-handout
audience: learner
format: pdf
purpose: Help a beginner compare TCP and UDP from application requirements rather than stereotypes.
content_blocks:
  - objective
  - requirement-diagnostic
  - tcp-service-model
  - udp-service-model
  - comparison-table
  - analogy-and-limitations
  - instructional-svg
  - guided-comparison
  - independent-check
  - transfer-task
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

> TCP gives an application a reliable ordered byte stream. UDP gives an application independent best-effort datagrams. Neither is universally better. The useful choice depends on whether the application prioritizes completeness, order, message boundaries, timeliness, or custom control.

## 18. Runtime trace

```text
Learning Contract
  -> Knowledge Model slice
  -> Requirement-first diagnostic
  -> TCP service model
  -> UDP service model
  -> Contrastive comparison
  -> Application-choice assessment
  -> Evidence record
  -> Conservative Student Model update
  -> Transfer scenario
  -> Review scheduling
  -> Privacy-safe publishing artifact
```

## 19. What this example demonstrates

This example shows that ULS can:

1. teach a comparison as a causal trade-off rather than a feature list;
2. diagnose protocol stereotypes before instruction;
3. use analogy while explicitly stating its limitations;
4. require application-requirement reasoning;
5. distinguish protocol service properties from universal performance claims;
6. gather evidence through justification, not recognition alone;
7. publish a generic handout without private learner state.

The architectural rule remains:

> Runtime chooses. The language model writes.
