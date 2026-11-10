# Scout Vocabulary

These terms define [the shared operating contract](SKILL.md). Specialist security concepts belong in [`security-design/vocabulary.md`](../security-design/vocabulary.md); compatibility dimensions and attributes belong in [`compatibility-review/vocabulary.md`](../compatibility-review/vocabulary.md).

## Three distinct questions

| Concern | Question | Do not confuse it with |
| --- | --- | --- |
| Correctness across behavior | Does the implementation do what it is supposed to do? | Repeating the implementation in a test or treating existing behavior as intended. |
| Coherence across space | Do independently developed parts agree on shared concepts, conventions, and invariants? | Uniformity for its own sake; different domains may need different meanings or mechanisms. |
| Compatibility across time | Can existing consumers continue relying on the system as it evolves? | Correctness alone; a corrective fix may still break consumers. |

A system can be coherently wrong. A change can improve correctness and break compatibility. A current inconsistency is not proof that architecture is deteriorating over time. Keep these claims separate.

## Evidence and claims

- **Surface:** The declared component, interface, path, lifecycle, or set of related concepts under examination. A bounded surface can cross modules; a directory is not necessarily an execution boundary.
- **Invariant:** A scoped property that must hold across relevant states or operations. State the authoritative requirement and where it must hold; not every desirable property is already a requirement.
- **Contract:** The expectations and obligations governing an interface, including meaning, effects, errors, timing, and conditions of use, not just its signature.
- **Authoritative specification:** A requirement designated by the project/user as binding in the relevant scope. Current accepted decisions may be specifications; historical agent interpretations are not.
- **Implementation evidence:** What current code, configuration, or execution establishes about behavior. It can contradict a contract; it does not override the contract merely by existing.
- **Snapshot:** The state to which evidence applies, including relevant uncommitted changes and configuration. A commit ID alone does not identify a dirty working tree or deployed runtime.
- **Lead:** A clue worth investigating. A suspicious API, apparent missing check, or duplicated mechanism is not yet an actionable finding.
- **Signal:** A bounded, evidence-backed observation worth attention. It may report a substantiated defect, supported design concern, coherence question, compatibility change, assurance gap, or material coverage gap. Its kind and evidence limits must be explicit.
- **Finding:** A substantiated mismatch between an applicable guarantee and an affected path or design, supported by a concrete counterexample and consequence. Static evidence may establish a defect; it cannot establish that a runtime experiment occurred.
- **Counterevidence:** A control, different domain meaning, intended delegation, or other fact that weakens or falsifies a concern. Seek it before reporting.
- **Static evidence:** Facts from inspected source, configuration, contracts, and path reasoning. Distinguish proven facts from assumptions about deployment, reachability, or dependency behavior.
- **Runtime evidence:** An observed execution with a recorded scenario, environment, and outcome. A test-double result proves only the behavior exercised with that double; accepted input alone does not prove downstream effect.
- **Severity:** Consequence and reach if the issue occurs. **Confidence:** Strength of support for the claim. High potential severity is not high confidence; neither replaces evidence.
- **Assurance gap:** Existing tests or other evidence do not distinguish an applicable guarantee from a specific plausible violation. Demonstrating a test blind spot does not establish that the product contains that defect.
- **Coverage gap:** A material part of the declared scope that could not be assessed. Name what is missing, what was attempted, and what prerequisite would enable assessment. It is not evidence of a defect.

## Modes and memory

- **Scout:** A skill run against a declared surface by a lightweight harness to produce signals, not automatically repair the target.
- **Harness:** The runtime supplying context, tools, permission boundaries, scheduling, and output delivery. A Markdown skill describes behavior; it cannot enforce a filesystem or network sandbox.
- **Stateless discovery:** Fresh analysis with current code, the review skill, and authoritative project specifications/instructions, but no prior scout findings, suppressions, scratchpads, or historical interpretations. Independence concerns context, not merely whether the process starts anew.
- **Stateful review:** Analysis with the same current inputs plus curated history. It supports longitudinal claims when evidence spans time; it does not turn old conclusions into present truth.
- **Context contamination:** Historical findings or interpretations entering an intended stateless run. Disclose it; do not pretend that ignoring them after reading restores independent discovery.
- **Curated memory:** Reviewed records with provenance, scope, status, and revalidation conditions. Separate observed facts, hypotheses, accepted decisions, exceptions, debt, fixes, and disproved concerns.
- **Accepted decision:** A scoped choice accepted by an accountable owner or authoritative project process. An agent's recommendation or repetition is not acceptance.
- **Exception:** An accepted departure from a stated rule under specified conditions. Preserve rationale, owner/source, scope, and expiry or revalidation trigger; a suppression is not proof of safety.
- **Dedupe identity:** A stable identity for the underlying concern: surface, invariant/concept, causal mechanism, and logical location as needed. Dates, line numbers, wording, and run IDs identify observations, not the concern itself.
- **Material update:** New evidence changing impact, scope, confidence, status, or the applicability of an exception. Rewording an unchanged finding is not a material update.
- **Promote/reopen trigger:** A specific condition for revisiting a below-bar lead or resolved concern, such as a newly reachable path, changed authority model, expired exception, or a counterexample. Not every unverified lead deserves durable storage.
- **Consolidation:** A separate review step that reconciles signals, deduplicates concerns, accepts or rejects proposed memory updates, and records reasons without altering the original discovery evidence.

## Engineering the analysis loop

| Concern | Governing question | Practical obligation |
| --- | --- | --- |
| Context engineering | What does the agent know, and how trustworthy is that knowledge? | Supply scoped evidence with provenance, isolate stateless inputs, and keep retrieved content from becoming instructions. |
| Intent engineering | What is supposed to be true? | Establish invariants, contracts, threat assumptions, and applicable principles before judging implementation. |
| Loop engineering | How are mistakes discovered, challenged, verified, and consolidated? | Separate discovery, falsification, permitted verification, deduplication, and curated learning. |
| Goal engineering | What are we optimizing, and how do we resolve competing objectives? | Optimize actionable risk reduction and preserved guarantees, not finding volume, superficial uniformity, or agent agreement. Escalate unresolved product/security tradeoffs to the responsible owner. |

The loop should update understanding when relevant behavior, contracts, or deployment conditions change. Do not infer a required scan frequency from an analogy or assume that an old clean review covers a new snapshot.
