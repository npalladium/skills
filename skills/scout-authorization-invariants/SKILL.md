---
name: scout-authorization-invariants
description: Inspect a bounded repository surface when caller identity, tenant scope, delegation, derived data, or delayed execution may violate authorization guarantees. Trace exact actions and targets across affected paths and distinguish intended authority from bypasses. Not a general vulnerability scan, penetration test, or demand for extra controls without an applicable guarantee.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Authorization Invariants Scout

Trace who may cause which effect on whose resource. Endpoint access, service privileges, and target authorization are different facts; neither broad privileges nor a missing-looking local check establishes a bypass.

## Required shared guides

Before discovery, explicitly load these companion files:
- [scout-repository/SKILL.md](../scout-repository/SKILL.md) for the run brief and operating contract.
- [scout-repository/vocabulary.md](../scout-repository/vocabulary.md) for shared terms and signal distinctions.
- [scout-repository/checklists.md](../scout-repository/checklists.md), selecting questions relevant to authority, boundaries, and lifecycle.

These files must be available with this entry point; install the skill family together. If a companion is unavailable, report the missing dependency and stop rather than silently improvise its contract. Use the shared mode, evidence, output, deduplication, and memory rules; do not invent a local substitute.

For specialized security reasoning, optionally load [security-design](../security-design/SKILL.md). This lens remains independently runnable with the required shared guides.

## Bound the inquiry

Use the shared run brief to name one protected operation or connected authority path, its entry points, relevant tenants, and exclusions. Include alternate routes only where they can reach that operation or consume its protected output.

Relevant inputs are current policy/contracts, handlers, middleware, target resolvers, ownership/membership stores, delegation configuration, workers, derived-data readers/writers, and focused tests. Inspect policy configuration without collecting credentials or protected payloads. Record inaccessible policy or runtime configuration as a limit, not an absent control.

The question is whether the stated policy survives the path, not whether every layer repeats an identical check. Keep evidence acquisition read-only under the shared contract; execute an isolated scenario only when the run brief explicitly permits it.

## Practical tracing workflow

1. **State the guarantee.** Identify the authoritative policy for an action on an exact target and tenant. Separate resource access, identity binding, delegation, and network-resource use when these are different permissions. Mark unresolved policy as an unknown.
2. **Fix the caller baseline.** Establish what the initiating principal can already do, including legitimate grants and administration rights. Record the original caller and effective execution identity separately; an intermediary's credentials are not the caller's baseline.
3. **Follow target selection.** Trace IDs, aliases, tenant selectors, object references, and destinations through resolution and canonicalization. Determine who establishes target ownership and tenant membership rather than trusting the name of a parameter.
4. **Locate effective enforcement.** Trace policy decisions through gateway, handler, shared service, target domain, and storage as applicable. Identify the facts each decision uses and whether caller context survives identity changes. Search for a legitimate downstream control that would falsify the concern.
5. **Trace every affected path.** Include alternate APIs, internal callers, cache hits, exports, indexes, static objects, queues, callbacks, and jobs where they reach the same protected effect. A correct request handler does not establish the authority of a separate reader or worker.
6. **Examine delegation and time.** Locate the grant/binding gate, allowed target/action scope, effective identity, visibility, expiry, and revocation semantics promised by the system. Check scheduling versus execution and source policy versus derived data against the stated consistency guarantee, not an invented instantaneous-revocation rule.
7. **Construct and challenge the scenario.** Name the caller, action, exact target/tenant, initial authority, relevant state change, bypassed control, and resulting observation/effect. Test the reasoning against intended delegation, target-owner checks, tenant isolation, and existing negative tests.
8. **Classify within the shared output contract.** Separate demonstrated behavior from statically supported risk and unresolved policy questions. Give the narrowest verification or owner decision needed; do not fix code or broaden the mission.

## Signal discriminator

An escalation claim needs an actual reachable target path, evidence of the missing or bypassed applicable control, and an effect beyond the initiating caller's baseline. Distinguish permission to invoke an operation from permission to select another party's target or attach an execution identity.

A supported static concern can be useful without execution, but identify the unverified hop. Accepted input alone proves neither private-network reachability nor disclosure; a broad identity alone proves neither delegation failure nor cross-tenant access.

For non-escalation availability or integrity concerns, do not require proof of newly acquired authority. Establish the caller's baseline, the applicable guarantee, the affected path, and the concrete harmful outcome; do not relabel ordinary authorized cost as a policy violation.

An unknown is meaningful when it identifies the exact unresolved policy or enforcement dependency and how that answer changes the conclusion. Report it as an unknown, not a verified defect.

## Disqualifiers and counterevidence

- A powerful feature, broad service identity, or unusual permission name without an unauthorized target/effect.
- A missing handler check when an effective target-domain control enforces the same guarantee.
- A visible, deliberately granted, appropriately scoped delegation that permits the demonstrated effect.
- A caller invoking an administrator-bound workload without evidence that the caller can bypass the separate binding gate.
- A cache, export, or curated derivative whose sharing policy intentionally differs from its source and is authorized to do so.
- A hypothetical stale/revoked identity without a reachable timing scenario and applicable revocation contract.
- A local convention or preferred hardening measure presented as mandatory policy without an authority source.

## Calibration examples

**Candidate signal:** A tenant-A reader chooses tenant B's document ID; a shared service reads it and returns content. Trace the resolver, membership policy, and downstream checks. If the reader lacks document access and no effective control binds that access to the read, the scenario supports an authorization bypass; if downstream enforcement is unknown, say so.

**Safe contrast:** The same service uses delegated caller credentials or checks access to that exact document. Broad service permissions do not overturn the observed enforcement.

**Delayed/derived candidate:** A revoked reader still obtains protected record content through a search index or queued export. Identify the promised policy freshness, index/export authorization, and actual serving path. Source revocation alone does not prove a violation if authorized curated sharing or a documented consistency interval explains it.

**Non-escalation candidate:** An already authorized export caller can force a documented per-tenant execution bound to be exceeded through an unbounded fan-out path. Establish that bound and reachable consequence without inventing an additional-authority requirement; execution remains subject to the run brief.

## Handoff

Use the shared signal format, including scoped evidence and limits. Make the original/effective caller, action, target/tenant, policy owner, enforcement path, and caller baseline explicit where relevant. Reference meaningful alternate or delayed paths checked, and identify material unknowns without implying exhaustive authorization coverage.
