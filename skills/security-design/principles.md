# Security Design Principles

Use these principles to reason about the guarantees a system must preserve, not to score compliance or prescribe controls without a relevant requirement. Apply them together: a design choice can strengthen one property while weakening another.

## Establish what loss is unacceptable

Identify what must be protected, from whom or what, and under which operating conditions. Include unauthorized disclosure or modification, loss of availability, and inability to recover where relevant. Scale protection and verification rigor to the consequences of failure—not the apparent size of the code change. Distinguish established requirements, reasonable domain expectations, and unresolved assumptions; a principle alone does not establish project policy.

## Make trust assumptions explicit and narrow

Identify the components whose correct behavior the guarantee depends on. State what each is trusted to do, why that reliance is justified, and where it stops. “Internal,” “authenticated,” or “managed” does not establish every property needed from a component. Keep security-critical components and their interactions as small and understandable as practicable. Consider both dependency failure and the enforcing component's own mistakes; assuming an arbitrary compromise is not a substitute for establishing the relevant threat model.

## Protect the enforcement mechanism

Cover every path to the protected effect, including paths that modify its policy, configuration, or identity inputs. A control is ineffective if a caller can bypass or alter it. Its implementation and decision dependencies must be protected from unauthorized modification and simple enough to evaluate. Cached authorization decisions are acceptable when their scope, lifetime, and invalidation preserve the guarantee; complete mediation does not require a database query on every access.

## Bind authority to the actual effect

Establish who may cause this operation on this target under these conditions. Permission to invoke a service, configure a workload, or supply an identifier is not necessarily permission for the resulting effect. Follow the initiating identity, effective identity, target ownership, and delegated grant across service hops. Identify which component joins those facts into an authorization decision; authenticating a peer service does not validate its claims about a caller-selected target.

An intermediary may intentionally exercise authority its caller does not directly possess. Assess whether the caller can exceed that delegation; requiring direct underlying permissions can defeat the intended service. Authority includes using a credential to sign, decrypt, or connect, not merely extracting its bytes.

## Minimize capability, duration, and unnecessary sharing

Give each component only the authority and functionality needed for its task, for the time they are needed. Consider enabled features, credentials, retained data, network access, and privileged execution contexts—not just roles. Shorter lifetimes and on-demand capability help only when renewal, restoration, and failure behavior remain trustworthy.

Examine shared credentials, state, caches, queues, and other mechanisms as paths for cross-user influence or failure propagation. Share deliberately where justified; neither centralization nor duplication is inherently safer. Minimize unnecessary exposure without removing required capability or inventing retention policy.

## Evaluate the composition, not just its parts

Individually correct components do not establish a correct or trustworthy system. Check whether their assumptions, identities, representations, state transitions, and failure behavior agree at each interaction. Parse and validate boundary data in the representation used for the decision and effect; well-formed data is not necessarily authoritative. Encode output for its destination.

For defense in depth, identify the failure each protection addresses and what remains effective when another fails. Repeated checks sharing a mistaken identity, policy source, configuration, or dependency may fail together. Where an operation requires independent approval, one principal must not be able to satisfy all approvals or rewrite the rules governing them. Additional layers are useful only when their protection justifies their complexity and failure modes.

## Preserve protection through failure and recovery

Determine the protected outcome for initialization, normal and degraded operation, maintenance, shutdown, and recovery wherever they affect the guarantee. Deny protected actions when authority cannot be established, but do not assume stopping all activity is the safest availability or safety outcome. Define intentional overrides and their authorization explicitly rather than allowing accidental fail-open behavior.

Consider both the immediate effect of a failure and whether it propagates into another loss. Recovery must not silently restore stale authority, repeat irreversible effects, expose protected data, or introduce an unsafe state. Detection and response can also fail: evaluate the evidence behind an automated response and whether the response itself can cause unacceptable loss or foreclose recovery options.

## Make correct operation understandable and practical

Prefer small, explicit mechanisms and interfaces whose security effects are visible. Safe defaults establish protection without requiring operators to discover and repair an unsafe initial state. Make the protected resource, requested authority, consequential action, and denial understandable. Confusing prompts and difficult secure workflows encourage mistakes and workarounds.

Do not rely on secrecy of the design for protection; keep designated secrets secret. Reuse suitable established mechanisms, but verify their assumptions and integration rather than treating reuse as proof. Challenge complexity for its concrete effects on correctness, auditability, and repair—not merely for stylistic uniformity.
