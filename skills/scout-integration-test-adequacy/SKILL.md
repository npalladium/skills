---
name: scout-integration-test-adequacy
description: Assess whether tests substantiate a bounded consumer-visible or cross-component guarantee at its real integration boundary, using plausible incorrect implementations and concrete counterexamples. Not a test-count, coverage, or generic test-quality audit, and not permission to run tests unless the run brief allows it.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Integration Test Adequacy Scout

Assess whether available test evidence distinguishes the claimed behavior from a plausible incorrect implementation at the boundary where the guarantee matters. A test suite's size, labels, or green status do not answer that question.

## Required shared guides

Before discovery, explicitly load these companion files:
- [scout-repository/SKILL.md](../scout-repository/SKILL.md) for the run brief and operating contract.
- [scout-repository/vocabulary.md](../scout-repository/vocabulary.md) for shared terms and signal distinctions.
- [scout-repository/checklists.md](../scout-repository/checklists.md), selecting relevant behavior, boundary, and evidence questions.

These files must be available with this entry point; install the skill family together. If a companion is unavailable, report the missing dependency and stop rather than silently improvise its contract. Use the shared mode, evidence, output, deduplication, and memory rules; do not invent a local substitute.

For security-sensitive guarantees, optionally load [security-design](../security-design/SKILL.md) and use its focused contract as context. This lens remains independently runnable with the required shared guides.

## Bound the inquiry

Use the shared run brief to name the changed or claimed consumer-visible behavior, its boundary, relevant consumers, and exclusions. Choose tests that purport to exercise that guarantee; do not audit every test in the repository.

Relevant read-only evidence may include test source and fixtures, production boundary code, configuration, persistence/serialization adapters, protocol contracts, and test-runner reports already supplied. Trace which components are real, substituted, or bypassed. Do not infer a missing test from an incomplete search; record search limits.

Tests may be executed only when the run brief explicitly permits isolated runtime verification. Do not contact production or external parties. If execution is not permitted or dependencies are unavailable, do static analysis and state that runtime behavior remains unverified.

## Practical tracing workflow

1. **Name the guarantee.** Describe an observable outcome or invariant and the consumer that relies on it. Use the authoritative contract or changed behavior; if intent is unresolved, report a question rather than inventing expected behavior.
2. **Locate the real boundary.** Follow the production path to the relevant API, persistence, transaction, message, protocol, authorization, or downstream integration boundary. Identify what the test actually crosses and what it replaces with mocks, stubs, fakes, or in-memory substitutes.
3. **Map assertion to guarantee.** Determine whether assertions establish outcomes and state visible to the consumer, or only calls, wiring, internal ordering, or a helper's return value. Invocation assertions can support a claim about invocation but not by themselves a cross-component effect.
4. **Construct a plausible wrong implementation.** For example: omit a permission, swap a tenant, serialize a field incorrectly, reorder a transition, skip commit, mishandle a duplicate, or treat a partial failure as success. Choose only a mutation that could violate the named contract on the traced path.
5. **Ask whether this test would fail.** Use fixture setup, substitute behavior, assertions, and real boundary semantics to decide. A mock returning the expected value may make the test blind to a plausible wrong integration behavior. Do not call a test inadequate merely because it mocks something; ask whether that dependency contract is inside scope.
6. **Isolate relevant variables.** Where authorization is the guarantee, compare permitted and denied principals/tenants against the same target and relevant state. Avoid conflating denial with an unrelated validation error, missing fixture, or infrastructure failure. When authorized execution is permitted, make only the minimal isolated scenario that distinguishes the claim.
7. **Check material lifecycle cases.** Consider transaction boundaries, delayed execution, retries, revocation, serialization, and partial effects only when the guarantee or changed path depends on them. Identify whether test setup makes the relevant interleaving or failure reachable.
8. **State the gap precisely.** Give the concrete bug that could pass, the affected contract and why it matters, the evidence for test blind spot, and the narrow test evidence or owner decision that would resolve uncertainty. Do not prescribe broad test expansion.

## Signal discriminator

A meaningful adequacy signal identifies: (a) a specific guarantee and real boundary, (b) a plausible incorrect implementation that violates it, and (c) why the observed tests would not distinguish that implementation. This counterexample is required to make a test gap actionable.

Use `assurance-gap` in the shared signal format for a supported test-evidence gap. A test blind to an error does not prove the product contains the error. Use `coverage-gap` only when a material part of this assessment could not be performed, and `defect` only for a separately substantiated product violation. An unresolved expected behavior is a question, not an invented contract.

For authorization-isolated runs, hold target, operation, and relevant state constant while varying only the principal/tenant authority under test. Run such cases only when the brief permits; otherwise describe the needed evidence without attempting it.

## Disqualifiers

- Low test count, a percentage threshold, missing snapshot, or coverage metric without a plausible missed contract violation.
- A test not asserting internal detail that is not part of the guarantee.
- A mock used for a dependency whose behavior is explicitly outside the bounded contract.
- A proposed test for an unsubstantiated requirement or merely preferred implementation.
- A green test suite treated as proof of every consumer, failure path, or alternate entry point.
- A static reading described as executed or runtime-confirmed.
- A test-run failure caused by unavailable infrastructure presented as evidence that application behavior is defective.

## Calibrated examples

**Concrete gap:** A test submits a tenant-A identity and asserts the service method was called once. Its fixture uses a permissive mock and never checks persisted or returned tenant ownership. A plausible wrong implementation that substitutes tenant B could pass. Report an evidence gap for the tenant-isolation guarantee; do not claim the product leaks data without a reachable defect path.

**Adequate for a bounded claim:** A focused test sends an unauthorized principal through the real authorization boundary, observes denial, and confirms the target state is unchanged. This supports that route and state transition, not untested workers or exports.

**Not a gap by itself:** The suite has few tests, but existing integration evidence reaches the relevant boundary and would fail for the plausible wrong implementation. Do not issue a volume-based finding.

## Handoff

Use the shared signal format and operating rules. State which consumers and boundary were inspected, what was real versus substituted, whether tests were merely read or explicitly executed, and the limits of any conclusion. Keep evidence gaps separate from verified product behavior and avoid implying suite-wide assurance.
