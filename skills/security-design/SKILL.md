---
name: security-design
description: Design, implement, or review security guarantees in applications and services. Use for authentication, authorization, tenant isolation, delegated authority, sensitive data, trust boundaries, state integrity, dependency failures, and security-relevant lifecycle changes. Not a vulnerability scanner, penetration-test procedure, or replacement for applicable domain standards.
metadata:
  author: npalladium
  version: "2.2.0"
---

# Security Design

Reason from guarantees and complete authority paths, not feature names or checklist matches. Establish the relevant threat model; do not invent requirements or speculative hardening.

## Start with these questions

- What invariants must be preserved?
- Where does data or authority cross a trust boundary?
- Can the system be understood, tested, audited, and repaired when something goes wrong?
  - Does the implementation make invalid states difficult to represent?
- Can a partial failure, retry, or concurrent operation violate a guarantee?

Use [checklists.md](checklists.md) for implementation and review questions, [vocabulary.md](vocabulary.md) for disputed terms, and [worked examples](references/examples.md) for calibration.

Use [`compatibility-review`](../compatibility-review/SKILL.md) for consumer obligations and [`scout-repository`](../scout-repository/SKILL.md) for scout operation, memory, and reporting. [`scout-authorization-invariants`](../scout-authorization-invariants/SKILL.md) is the focused authorization scout; `code-design` covers construction tactics and `architect` broader design. Load companions only when applicable; report missing required guidance rather than inventing it.

## Design principles

### Establish and preserve required protection

Identify whose resources and operations are protected, against what threats, and under which conditions. Establish applicable security requirements and their sources; distinguish requirements, justified domain expectations, and unresolved assumptions.

### Bound and justify trust

Identify the components whose failure would invalidate the guarantee and the evidence supporting reliance on them. Minimize security-critical components and interactions; account for their own mistakes as well as dependency failure.

### Bind authority across domains

Operation permission, permission to reference a resource, and authority over its contents are different grants. Trace the original caller, acting identity, target owner, and grant at each domain transition. Assign enforcement to a component that can join the verified caller with the target's policy; two correct checks on different principals do not compose into caller authorization.

Distinguish authority derived from the caller from an independent grant the caller can steer. An orchestration service may legitimately exercise permissions the caller lacks; the question is whether target selection escapes its authorized purpose or scope. Binding a run-as identity requires delegation authority; invoking a workload already bound by an authorized administrator is a different operation.

Owner consent to a shared service or fleet does not authorize every caller using it. Likewise, delivering a credential to an execution environment gives usable authority to principals able to run code there: signing, decrypting, or connecting matters even when extraction is impossible.

### Identify what carries trust

Trust may ride on a network position, global name, hierarchy, user selector, derivative, or earlier decision—not just a shared identity. Ask who can occupy, register, change, or redirect that carrier and what binds it to the intended owner at the effect.

A parent governance role may intentionally control descendants; an incidental metadata role changing their security posture is different. Separate intra-tenant separation of duties from cross-tenant reach. Authorization to ingest data does not authorize everyone who can query its derivative; deliberate owner-granted sharing of curated output is not an ACL bypass.

### Keep authority governable

Where the protection model relies on administrator control, grants and durable credentials must be discoverable, scopeable, and revocable. Define direct disablement and account-remediation effects separately; resetting a password need not revoke independent credentials. A schedule or cached artifact may retain authority after its creator loses access: define whether authority belongs to the creator or an independently authorized workload, and when it must be rechecked.

### Minimize capability and sharing

Limit human and workload authority, enabled functionality, exposure time, and retained data to the task. Short lifetimes help only if renewal and recovery remain trustworthy. Shared state, credentials, and mechanisms create influence and failure-propagation paths; neither centralization nor duplication is inherently safer.

### Evaluate composition

Check agreement on representations, identities, state transitions, and failure behavior between components. Validation and use must interpret the same target; well-formed input is not authoritative. For layered defenses, identify what remains effective after one fails: controls sharing policy, identity, or configuration may fail together. Required independent approvals are ineffective if one principal can satisfy all of them or rewrite their rules.

### Preserve protection through failure and recovery

Deny protected actions when authority is unknown, but do not assume stopping all activity is the safest availability or safety outcome. Specify protection during initialization, maintenance, degraded operation, recovery, and intentional overrides.

Contain both the original failure and its propagation. Restore and replay must not resurrect revoked authority or repeat irreversible effects. Detection and response also need a failure model: a mistaken alert or disruptive defensive action can create another loss or foreclose recovery.

### Make secure operation usable

Protective defaults should not depend on operators discovering an unsafe initial state. Make granted authority and consequential actions visible; confusing prompts and difficult workflows invite workarounds. Do not depend on secrecy of the design. Established mechanisms still need their integration assumptions checked; structural simplicity is valuable when it improves enforcement, analysis, or repair.

## Workflow

1. **State the guarantee.** Bound the behavior, protected resources, principals, and authorization domains. Identify the requirement's source, where the invariant must hold, and unresolved assumptions; include availability and recovery where relevant.
2. **Trace data and authority.** Follow the complete affected path, including alternate entry points and lifecycle operations. Identify effective identities, authoritative facts, policy enforcement, and downstream effects.
3. **Inspect boundaries and state.** Apply the relevant checklist questions to exact caller/action/target authorization, derived-data permissions, and state transitions. Types do not eliminate mutable-state or authorization races.
4. **Challenge failure semantics.** Examine failure, concurrency, and recovery across transaction and external-effect boundaries. A lost response does not establish that nothing happened; compensation is not necessarily rollback.
5. **Try to falsify the concern.** Seek checks elsewhere, valid delegation, and safe sibling paths. Establish a real affected path and concrete violating scenario before calling a suspicious pattern a defect.
6. **Verify at the enforcing boundary.** Use isolated scenarios that distinguish correct behavior from plausible failure, covering relevant allow/deny and lifecycle cases. Check the whole relevant test module after a change. Use harmless fixtures; production or third-party probes require explicit authorization. Obey the scout run brief; when execution is unavailable or unauthorized, report static support and verification limits.
7. **Review and report.** Apply the [review checklist](checklists.md#d-during-review) and [reporting fields](#report-significant-findings).

## Calibrate security claims

- **State the contradiction.** Contrast the documented boundary or justified expectation with achievable behavior. Surprise alone is not a security requirement.
- **Trace the effect to its consumer.** For escalation, identify the caller's baseline and the surface that turns a deputy's output into additional capability. Acceptance or intermediate staging alone does not prove access. Integrity and availability failures need a violated guarantee, not capability transfer.
- **Test the actual gate.** A relationship field is not proof of enforcement. A cross-tenant denial may only mean the deputy lacks target permission; it does not establish a caller-bound check. Vary caller permission and the purported gate independently in authorized fixtures.
- **Distinguish consent from scope excess.** Owner-granted, visible, revocable authority chains can be intended composition. Neither customer ownership nor documented setup excuses an effect beyond the grant. Attribute a failed identity binding to the component that owns it, not automatically to the platform.
- **Assess configuration, not blame.** Recommended setup or generated templates may assemble a flaw's preconditions. That can widen exposure, not excuse the flaw; establish which configurations and paths are actually affected.
- **Bound the impact.** Shared identities are a possible carrier, not proof of cross-tenant reach; per-tenant identities do not exclude namespace, hierarchy, or derived-data crossings. Establish the affected owner and final-target restriction.
- **Bound the evidence.** Separate source facts, runtime observations, and assumptions at every link; identify evidence needed to resolve unverified links. An unverified downstream step limits the claimed impact, not necessarily the independently established defect. Static analysis is not runtime confirmation; no finding is not comprehensive assurance. Keep severity separate from confidence.

## Report significant findings

For each significant finding, provide:

1. **Claim and evidence level:** a substantiated defect, supported design concern, or unresolved question; state whether support is static, runtime, or mixed. In scout mode use the shared signal kinds from `scout-repository`.
2. **Invariant or contract:** what should hold, its scope and authority source. If the concern also changes a consumer expectation, use `compatibility-review` for its independent classification.
3. **Affected path:** entry point, caller identity, relevant boundary/enforcement point, dependency or state transition, and affected resource/consumer.
4. **Evidence:** file/line or other locator, test or runtime evidence, counterevidence, assumptions, and what remains unverified.
5. **Counterexample:** the request, state, interleaving, or lifecycle that violates the guarantee; for escalation, the consumer of any staged artifact and capability beyond baseline. For compatibility, give old/new behavior.
6. **Consequence and extent:** confidentiality, integrity, availability, compatibility, or maintainability impact; prerequisites, baseline caller authority, and plausible blast radius.
7. **Narrowest sound next step:** remediation, verification, or owner decision. Do not prescribe a control broader than the demonstrated requirement.
