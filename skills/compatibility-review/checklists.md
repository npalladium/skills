# Compatibility Checklists

Use with [the workflow](SKILL.md) and [exact taxonomy](vocabulary.md). Select only relevant questions; these are prompts, not universal support promises. Scout execution and reporting remain governed by the required `scout-repository` companion files.

## Before implementation or review

- What exact baseline, proposed release, support window, deprecation policy, and consumer set apply? Which contract source establishes each obligation, and which expectations remain inferred or incidental?
- What concrete input/state produces different old/new meaning, output, effect, error, or timing? Include changed configuration, generated code, and dependencies, not only edited source.
- Which APIs, SDKs, integrations, jobs, data artifacts, messages, deployment combinations, and rollback paths consume the behavior? What cannot be inspected?
- Which expectations does a corrective change restore, and which other expectations can it disrupt? Are security requirements relevant to the decision?

## Dimension prompts

| Dimension | Questions to resolve on the affected path |
| --- | --- |
| `structural` | Are fields, types, nullability, enum variants, required inputs, endpoints, encodings, or representation rules changing? Do actual readers tolerate the change? |
| `semantic` | Do names and types still express the same units, formula, population, time basis, or domain concept? Could valid-looking data cause a wrong consumer decision? |
| `behavioral` | Are defaults, validation, accepted inputs, transitions, errors, partial success, idempotent effects, or side effects different? |
| `temporal` | Are ordering, tie-breaking, pagination, visibility, consistency, concurrency, delivery, or expiry guarantees different? |
| `operational` | Can supported consumers still meet authentication, timeout, rate, retention, capacity, and configuration conditions? |
| `client` | Do supported source consumers still compile and behave correctly with generated symbols, imports, overloads, types, and library/runtime requirements? |

Record `basis`, `cause`, and `impact` independently for each material expectation. A shape-only pass does not answer the other dimensions.

## Consumer scenarios and distinguishing tests

Choose version pairings from actual support policy. For each test specify the consumer, baseline/new producer or artifact, input/state, expected consumer outcome, and plausible wrong implementation that would fail it.

| Boundary | Concrete scenario and test |
| --- | --- |
| API and SDK | Exercise supported old clients against the new server; exercise new clients against old servers where that combination is supported. Compile a representative source consumer for client-surface changes. Through the real wire format, test defaults, null/absent/unknown values, enums, errors, and the consumer's resulting decision—not just deserialization. |
| Meaning and calculations | Use deposits 100, spending 40, and hold 10 to distinguish available balance 50 from historical deposits 100 under the stated policy. Assert the consumer's withdrawal decision. For gross/net changes, use a nonzero deduction and assert reconciliation totals so both formulas cannot accidentally pass. |
| Ordering and pagination | Use equal sort keys, multiple pages, and a relevant concurrent insertion/update. Assert promised order, tie-breaking, and allowed duplicate/omission behavior under the actual pagination contract. A one-page fixture with unique keys cannot distinguish the relevant failures. |
| Persisted data and exports | Read supported historical rows/files with the new reader. Where required, let the old reader consume new writes. Cover defaults, units, absent fields, unknown variants, and read-modify-write preservation. Test a migration against representative old data and assert interpreted values, not just schema acceptance. |
| Messages and replay | Pair supported old/new producers and consumers in each supported direction. Deserialize real serialized envelopes; cover replay of retained old messages and, where relevant, duplicate/reordered delivery. Assert state transitions and effects under the delivery contract, not only handler invocation. |
| Rolling deployment | Run the supported mixed-version reader/writer or service combination against shared state and configuration. Check that a new writer does not create values an old instance misinterprets, and that version negotiation actually selects compatible behavior. Include caches or background jobs when they consume the changed representation. |
| Rollback | Write data or enqueue messages with the new version, then restore the supported older reader/service. Verify it can correctly interpret or explicitly reject those artifacts according to policy. Include migration reversibility or the documented forward-recovery constraint; reverting code alone is not proof of rollback safety. |
| Operational use | Exercise the actual supported credential/configuration path and meaningful limit boundaries. For a changed timeout, rate, or retention policy, identify a real consumer workflow and assert its documented success/failure or recovery outcome rather than inventing a universal threshold. |
| Incidental JSON order | Trace an identified positional parser or raw-byte comparison across a member-order change. Contrast with a conforming object parser. Demonstrate actual reliance before claiming impact; do not assert that every consumer requires preserved member order. |

## Challenge the evidence

- Does the identified consumer actually read the changed value or depend on the altered outcome? Can an adapter, tolerant parser, negotiated version, feature gate, or completed migration falsify the concern?
- Does a test exercise the actual enforcing/consuming boundary, or mock away serialization, storage, protocol, or calculation behavior? Could a plausible wrong implementation pass its assertions?
- Does a claimed contract violation have an applicable authoritative or justified implicit source? Is consumer impact separately demonstrated, concrete but unverified, or unknown within scope?
- Are unknown external consumers and unexecuted version pairings explicit limits? Do not infer full compatibility from a green suite, unchanged schema, or absence of reports.

## Implementation and decision gate

- Choose preservation, caller migration, negotiation/versioning, staged migration, deprecation, or an accepted break based on actual support policy and evidence. Do not add compatibility layers for hypothetical consumers.
- When mixed-version support is required, define rollout order, compatible intermediate states, old-artifact handling, and rollback/forward-recovery boundaries before contracting old representations.
- For authorized implementation, update affected callers, generated interfaces, focused behavioral tests, contracts, and deployment/migration instructions together. Do not weaken tests to make a changed contract appear compatible.
- In review/scout mode, report the narrow decision or remediation; do not automatically apply it. Keep security obligations intact even when a consumer depends on defective behavior.
