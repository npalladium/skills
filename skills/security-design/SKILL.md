---
name: security-design
description: Design, implement, or review security guarantees in applications and services. Use for authentication, authorization, tenant isolation, delegated authority, sensitive data, trust boundaries, state integrity, dependency failures, and security-relevant lifecycle changes. Not a vulnerability scanner, penetration-test procedure, or replacement for applicable domain standards.
metadata:
  author: npalladium
  version: "2.0.0"
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

Use [`vocabulary.md`](vocabulary.md) to make claims precise, [`checklists.md`](checklists.md) for the relevant implementation/review stage, and [`references/examples.md`](references/examples.md) to calibrate authority-path concerns against safe behavior.

This skill owns security reasoning. Use [`compatibility-review`](../compatibility-review/SKILL.md) for consumer obligations across change, and [`scout-repository`](../scout-repository/SKILL.md) for stateless/stateful context, deduplication, memory, and reporting operation. [`scout-authorization-invariants`](../scout-authorization-invariants/SKILL.md) is the focused read-only authorization review entry point. Load companions only when applicable; if required by the selected mode but unavailable, report the dependency rather than inventing its procedure. Use `code-design` for construction tactics and `architect` for broader architecture design.

## Workflow

1. **Bound the task and state the guarantee.** Identify the requested behavior, protected resources/actions, initiating principals, tenants or authorization domains, and applicable current contracts. State what must remain inaccessible, unchanged, or impossible and where the invariant must hold. Include availability and recovery requirements where relevant. Attribute requirements to project specifications or confirmed owner intent; mark unresolved assumptions rather than inventing policy.
2. **Trace data and authority.** Follow the complete affected path through callers, effective identities, target resources, storage, dependencies, jobs, derived artifacts, and side effects. Identify the authoritative component for each important fact and the enforcing point for each policy. Include alternate entry points and relevant initialization, maintenance, revocation, deletion, and recovery paths.
3. **Inspect boundaries and state.** Ask whether the exact caller/action/target is authorized in each domain—not merely authenticated, permitted to invoke an intermediary, or replaced with a service identity. Inspect caller-controlled resource references, run-as identities, network attachments, callbacks, destinations, and secret-bearing configuration. Verify source permissions survive caching, indexing, exporting, and serving. Types can exclude malformed states but do not eliminate mutable-state or authorization races.
4. **Challenge failure semantics.** Trace denial, bad or misleading dependency data, timeout, cancellation, partial success, retry/replay, concurrency, stale state, expiry, and recovery where they can affect the guarantee. Identify transaction and external-effect boundaries. A lost response does not establish that nothing happened; compensation is not necessarily rollback. Deny protected actions when authority cannot be established; define safe degraded behavior explicitly rather than imposing one failure outcome on every system.
5. **Try to falsify the concern.** Look for caller-bound checks elsewhere, target-owner policy, deliberate delegation, scoped and revocable grants, tenant-bound identities, safe sibling paths, and other counterevidence. Powerful functionality alone is not a flaw. A suspicious API or missing-looking check is a lead; establish a real affected path and a concrete violating scenario before calling it a defect.
6. **Verify at the enforcing boundary.** Run the authorized isolated scenario or relevant test that distinguishes the intended behavior from the plausible failure. Exercise allowed and denied principal/resource/tenant combinations and applicable lifecycle or failure cases. Check the whole relevant test module after a change, not only the expected-to-flip assertion. Use harmless fixtures; do not probe production or another party's resources without explicit authorization. In scout mode obey its run brief; if execution is not authorized or available, report static support and verification limits instead of claiming reproduction.
7. **Check the whole change and report precisely.** Review affected callers, tests, configuration, migrations, deployment, and recovery behavior—not just edited lines. Check that a fix preserves other intended guarantees and exposes no unrelated data or authority. Report the guarantee, path, evidence, counterexample, consequence, limits, and smallest sound remediation using [`checklists.md`](checklists.md#e-report-significant-findings). Unsupported impact chains remain hypotheses.

## Calibrate security claims

- **Invocation is not escalation.** Identify the caller's baseline authority and the additional data, authority, or effect gained. An accepted request or intermediary-side effect does not by itself establish unauthorized downstream access. For integrity or availability concerns that are not escalation, show the applicable guarantee and effect rather than forcing an escalation model.
- **Delegation may be intended.** Customer-configured, visible, scoped, revocable authority composition can be a valid control model. Conversely, documented setup is not proof of safety if a caller can exceed the intended scope.
- **Control presence is not control sufficiency.** A check in one domain may not authorize the target in another; an index permission may not preserve source-record restrictions. Inspect what the control actually binds.
- **Simple, usable designs are easier to protect.** Prefer clear ownership, narrow authority, explicit transitions, and controls that make correct operation natural. Challenge complexity and duplicated security logic for concrete consequences—not to enforce a stylistic preference.
- **Match the evidence to the claim.** Keep source facts, runtime observations, and assumptions distinct. A static path can substantiate a design defect without exploit execution, but cannot be called runtime-confirmed. No finding is not proof of comprehensive security.

## Foundation and external delegation

These sources are the preferred starting set, not competing local constitutions. Consult the relevant source when its deeper treatment is better than a local summary. External principles guide analysis; they become project requirements only when applicable and adopted, or mandated by a governing standard.

- [Saltzer–Schroeder, The Protection of Information in Computer Systems](https://www.cs.virginia.edu/~evans/cs551/saltzer/), I.A.3—economy of mechanism, fail-safe defaults, complete mediation, open design, separation of privilege, least privilege, least common mechanism, and psychological acceptability. Use the principles as warnings worth investigating, not automatic findings.
- [OWASP Developer Guide: Security Principles](https://github.com/OWASP/DevGuide/blob/main/docs/en/02-foundations/03-security-principles.md)—practical application-security design, defaults, defense in depth, least privilege, and manageability. Do not reproduce the whole catalog here.
- [NIST SP 800-160 Vol. 1 Rev. 1, Appendix E: Principles for Trustworthy Secure Design](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-160v1r1.pdf)—the broad external reference for systems-security design reasoning. Select E.2 Clear Abstractions, E.7 Compositional Trustworthiness, E.8 Continuous Protection, E.19 Mediated Access, E.23 Protective Failure, E.24 Protective Recovery, or E.29 Substantiated Trustworthiness as relevant. NIST explicitly frames these as reasoning principles, not a compliance checklist.
- [Kat Traxler's Security Constitution](https://github.com/KatTraxler/security-constitution/blob/main/phase%202%20-%20security%20constitution/security-constitution.md)—cloud-service authority/deputy reasoning, trust boundaries, negative calibration, and evidence discipline. Its cloud taxonomy and execution threshold for confirmed exploitable design findings are specialized; do not import them as universal application requirements or dismiss useful static analysis.

For concrete application verification requirements, select applicable, versioned [OWASP ASVS](https://asvs.owasp.org/) requirements. For authorization mechanisms, consult the [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html). The [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org/) supports iterative, fit-for-purpose analysis rather than one prescribed methodology. [OWASP FIASSE securability](https://github.com/OWASP/FIASSE/blob/main/skills/securability/SKILL.md) offers further boundary-control and integrity guidance; [GitLab's permissions review guidelines](https://docs.gitlab.com/development/permissions/review_guidelines/) supply concrete review examples, not universal role or framework conventions.
