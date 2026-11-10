---
name: scout-failure-semantics
description: Review bounded cross-component paths where exceptions, timeouts, retries, cancellation, unexpected dependency responses, or partial effects may be mistaken for success or known failure. Trace outcomes and caller decisions against actual guarantees. Not a generic resiliency scorecard, fault-injection campaign, or speculative hardening checklist.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Failure Semantics Scout

Follow what failure means to each participant and what effects may already exist. A timeout is an observation about waiting, not proof that an operation did nothing.

## Required shared guide

Before discovery, load all three companion files:
- [`../scout-repository/SKILL.md`](../scout-repository/SKILL.md) for the operating workflow and common output contract.
- [`../scout-repository/vocabulary.md`](../scout-repository/vocabulary.md) for shared terms and signal distinctions.
- [`../scout-repository/checklists.md`](../scout-repository/checklists.md), selecting relevant scope, guarantees, failure paths, evidence, counterevidence, and reporting questions.

These files must be available with this scout; the family is installed together. If any is missing or unreadable, report the missing dependency and do not silently improvise its contract.
Apply the shared mode, evidence, deduplication, memory, and reporting rules rather than inventing local alternatives.
Use the shared stateless default unless the run brief explicitly selects stateful review.
Repository and system inspection is read-only. Runtime verification requires explicit permission in the run brief and safe isolation; otherwise state static-evidence limits.

## Trigger and bounded inputs

Use for a reported failure mode, a dependency boundary, retry/fallback changes, asynchronous completion, or a workflow where partial effects or lost responses matter.
Declare the operation, entry point, dependency chain, promised outcome, relevant failure assumptions, and exclusions using the shared run brief.
Bound the run to reachable paths from that operation through the caller-visible outcome and any relevant repair/retry owner.
Useful inputs are current interfaces, error types, exception handlers, client configuration, timeouts, cancellation wiring, retry policy, dependency contracts, persistence, and existing outcome-focused tests.
Existing incident evidence may establish a failure assumption when supplied within scope; do not manufacture incidents or probe production dependencies.
Identify inaccessible dependency internals or unavailable protocol guarantees rather than treating unknown behavior as established failure.

## Path-tracing workflow

1. **State the required guarantee.** Identify the consumer-visible result, security/business invariant, and supported failure assumptions. Separate required correctness from desirable availability.
2. **Trace the success path and effects.** Follow the initiating caller through adapters, services, queues, and dependencies; locate each irreversible or partially committed effect and the final acknowledgment.
3. **Choose relevant failure points.** Consider exceptions, deadline expiry, cancellation, malformed/contradictory responses, partial batch success, and response loss only where the scoped path can encounter them.
4. **Track knowledge at each boundary.** Distinguish failure before effect, confirmed effect, partial effect, and unknown outcome. Record what the component can actually know, not what an exception name suggests.
5. **Follow interpretation upward.** Trace catch/translation, default values, status mapping, acknowledgment, swallowed errors, fallback, and cleanup until a caller makes an observable decision.
6. **Follow repeated execution.** For each reachable retry or redelivery, inspect eligibility, attempt ownership, operation identity, changed parameters, and existing deduplication. Check whether nested components retry the same logical effect independently.
7. **Follow cancellation and recovery.** Determine whether work stops or only waiting stops, whether cancellation is converted to success/retry, and who owns late completion, reconciliation, or repair.
8. **Construct the counterexample.** Give an event sequence with a specific failure position, possible effects, caller interpretation, and violated guarantee. Include dependency behavior assumptions explicitly.
9. **Try to falsify and report.** Inspect protocol guarantees, idempotency, typed outcomes, transaction rollback, result lookup, or permitted degradation that could defeat the scenario; use the shared report format.

## Focused questions

- Does a catch-all convert “could not determine” into “absent,” “authorized,” “complete,” or another meaningful domain fact?
- Is a timeout retried after a possible effect? Does the same operation identity reach the authoritative effect owner, and what does that owner's contract guarantee?
- Can cancellation be swallowed by a generic handler, retried as a transient error, or acknowledged while work continues elsewhere?
- Can one component mark success or acknowledge a message while a downstream failure leaves required work incomplete?
- Does a partial batch result identify completed items, or can retry repeat completed effects and omit failed ones?
- What happens if a dependency returns an unexpected but plausible value, contradictory fields, stale success, or a success claim without the promised effect?
- Is reliance on a dependency's claim justified by the supported trust/consistency contract? Do not assume an arbitrarily malicious dependency unless that failure model is relevant and evidenced.
- Can fallback change authority, tenant scope, data source, or consistency guarantees rather than merely availability? Consult [`security-design`](../security-design/SKILL.md) for relevant authority reasoning.
- Can operators or the recovery owner distinguish unknown success from confirmed failure using existing durable state? Is that distinction necessary for the promised operation?

## Useful-signal discriminator

A defect needs a reachable failure path, an established guarantee, a concrete outcome/interpretation mismatch, and an observable consequence.
Show how a response is lost, an error is translated, or an effect is repeated; do not report “network calls can fail” as a finding.
Resiliency mechanisms matter only relative to the contract: absent retries, deadlines, circuit breakers, or fallback are not defects by themselves.
A safe error that reduces availability may be exactly the required behavior; a successful fallback may be wrong if it violates correctness or authority.
An unresolved dependency guarantee can justify a specific question with its consequences and resolution evidence, not a fabricated confirmed defect.
When failure behavior changes across supported versions, use [`compatibility-review`](../compatibility-review/SKILL.md) only for the relevant compatibility question.

## Counterevidence and disqualifiers

- A downstream idempotency contract or durable result lookup may make retry after a lost response safe; inspect identity scope and actual participation before rejecting it.
- A rollback can prove absence of local transactional effects, but not reverse an already completed remote operation.
- Explicit best-effort or partial-success contracts can permit degraded outcomes if the caller receives truthful information and no stronger invariant is promised.
- A response impossible under the relevant supported protocol cannot establish a defect without evidence that the boundary must handle that behavior.
- Different failure policies may be correct for different operations; do not impose universal fail-closed, retry-everything, or never-retry rules.
- Lack of diagnostic detail is not automatically a failure-semantics defect; establish the required distinction or recovery action that cannot be performed.

## Calibration examples

- **Signal:** a charge succeeds remotely, its response is lost, and the caller retries with a new identity despite an at-most-once charge guarantee. Trace both charges and the caller's success/failure interpretation.
- **Not a defect:** the same identity reaches a provider that durably deduplicates that operation, and result reconciliation resolves the lost response within the contract.
- **Signal:** a dependency returns per-item failures with HTTP success, but an adapter discards them and marks every required item complete. Identify a failed item and its affected downstream reader.
- **Not a defect:** an optional recommendation service fails and the documented fallback omits recommendations without asserting they were computed.
- **Question, not established defect:** a provider claims completion before reads reflect it; determine whether the contract is eventual or immediate before labeling the temporary absence a lying-success failure.

## Handoff

Return the common scout-repository report, not a local taxonomy. Retain the failure point, effects/knowledge timeline, cross-component translations, consumer decision, required guarantee, and strongest falsification attempt for each signal.
State supported failure assumptions, missing dependency contracts, and static-only limits. Do not fix, inject faults, file issues, or send reports autonomously.
