---
name: security-design
description: Design, implement, or review security guarantees in applications and services. Use for authentication, authorization, tenant isolation, delegated authority, sensitive data, trust boundaries, state integrity, dependency failures, and security-relevant lifecycle changes. Not a vulnerability scanner, penetration-test procedure, or replacement for applicable domain standards.
metadata:
  author: npalladium
  version: "2.1.0"
---

# Security Design

Reason from intended behavior, invariants, authority, and system paths—not feature names or checklist matches. Apply during design, implementation, and review. Distinguish a supported design concern from a runtime-demonstrated defect. Not every prompt applies; do not invent requirements or perform speculative hardening.

## Start with these questions

- What invariants must this change preserve?
- Where does data or authority cross a trust boundary?
- Does the implementation make invalid states difficult to represent?
- What happens when a dependency lies, fails, or returns unexpected data?
- Can a partial failure, retry, or concurrent operation violate a guarantee?
- Can the system be understood, tested, audited, and repaired when something goes wrong?

Read [principles.md](principles.md) for the reasoning, then select relevant questions from [checklists.md](checklists.md). Use [vocabulary.md](vocabulary.md) for essential distinctions and [worked examples](references/examples.md) to calibrate authority-path concerns. Source links and provenance are collected in [references.md](references.md); ordinary use does not require opening them.

This skill owns security reasoning. Use [`compatibility-review`](../compatibility-review/SKILL.md) for consumer obligations across change, and [`scout-repository`](../scout-repository/SKILL.md) for stateless/stateful context, deduplication, memory, and reporting operation. [`scout-authorization-invariants`](../scout-authorization-invariants/SKILL.md) is the focused read-only authorization review entry point. Load companions only when applicable; if required by the selected mode but unavailable, report the dependency rather than inventing its procedure. Use `code-design` for construction tactics and `architect` for broader architecture design.

## Workflow

1. **Bound the task and state the guarantee.** Identify the requested behavior, protected resources/actions, initiating principals, tenants or authorization domains, and applicable current contracts. State what must remain inaccessible, unchanged, or impossible and where the invariant must hold. Include availability and recovery requirements where relevant. Attribute requirements to project specifications or confirmed owner intent; mark unresolved assumptions rather than inventing policy.
2. **Trace data and authority.** Follow the complete affected path through callers, effective identities, target resources, storage, dependencies, jobs, derived artifacts, and side effects. Identify the authoritative component for each important fact and the enforcing point for each policy. Include alternate entry points and relevant initialization, maintenance, revocation, deletion, and recovery paths.
3. **Inspect boundaries and state.** Ask whether the exact caller/action/target is authorized in each domain—not merely authenticated, permitted to invoke an intermediary, or replaced with a service identity. Inspect caller-controlled resource references, run-as identities, network attachments, callbacks, destinations, and secret-bearing configuration. Verify source permissions survive caching, indexing, exporting, and serving. Types can exclude malformed states but do not eliminate mutable-state or authorization races.
4. **Challenge failure semantics.** Trace denial, bad dependency data, timeout, cancellation, partial success, retry/replay, concurrency, stale state, expiry, and recovery where they affect the guarantee. Identify transaction and external-effect boundaries. A lost response does not establish that nothing happened; compensation is not necessarily rollback.
5. **Try to falsify the concern.** Look for caller-bound checks elsewhere, target-owner policy, deliberate delegation, scoped and revocable grants, tenant-bound identities, safe sibling paths, and other counterevidence. Powerful functionality alone is not a flaw. A suspicious API or missing-looking check is a lead; establish a real affected path and a concrete violating scenario before calling it a defect.
6. **Verify at the enforcing boundary.** Run the authorized isolated scenario or relevant test that distinguishes the intended behavior from the plausible failure. Exercise allowed and denied principal/resource/tenant combinations and applicable lifecycle or failure cases. Check the whole relevant test module after a change, not only the expected-to-flip assertion. Use harmless fixtures; do not probe production or another party's resources without explicit authorization. In scout mode obey its run brief; if execution is not authorized or available, report static support and verification limits instead of claiming reproduction.
7. **Check the whole change and report precisely.** Review affected callers, tests, configuration, migrations, deployment, and recovery behavior—not just edited lines. Check that a fix preserves other intended guarantees and exposes no unrelated data or authority. Report the guarantee, path, evidence, counterexample, consequence, limits, and smallest sound remediation using [`checklists.md`](checklists.md#e-report-significant-findings). Unsupported impact chains remain hypotheses.

## Calibrate security claims

- **Invocation is not escalation.** Identify the caller's baseline authority and the additional data, authority, or effect gained. An accepted request or intermediary-side effect does not by itself establish unauthorized downstream access. For integrity or availability concerns that are not escalation, show the applicable guarantee and effect rather than forcing an escalation model.
- **Delegation may be intended.** Customer-configured, visible, scoped, revocable authority composition can be a valid control model. Conversely, documented setup is not proof of safety if a caller can exceed the intended scope.
- **Control presence is not control sufficiency.** A check in one domain may not authorize the target in another; an index permission may not preserve source-record restrictions. Inspect what the control actually binds.
- **Match the evidence to the claim.** Keep source facts, runtime observations, and assumptions distinct. A static path can substantiate a design defect without exploit execution, but cannot be called runtime-confirmed. No finding is not proof of comprehensive security.

