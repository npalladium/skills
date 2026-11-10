# Security Engineering Vocabulary

Use these terms to make claims precise, not to impose a new domain model on a project. Project-specific meanings and authoritative contracts take precedence; surface conflicts rather than silently resolving them. See [the workflow](SKILL.md) and [the checklists](checklists.md).

## Guarantees, authority, and state

| Term | Meaning here |
| --- | --- |
| Invariant | A property that must hold throughout the relevant operations and observable states. Name its scope and enforcement point; a committed-state invariant need not hold between private steps inside an atomic transaction. |
| Contract | The obligations and expectations governing an interface: representation, meaning, accepted inputs, outcomes, effects, failure modes, timing, and conditions of use—not just its type signature. |
| Security guarantee | A protection requirement concerning confidentiality, integrity, availability, or permitted authority. State whose resource or action is protected, against whom, and under what assumptions. |
| Principal | The person, workload, or other identity to which actions and permissions are attributed. Distinguish the initiating principal from the effective identity used by an intermediary. |
| Authentication | Establishing an identity. It does not establish permission for a particular action or resource. |
| Authority | The ability or permission to cause an effect or access a resource. Include indirect use of credentials, execution identities, network access, and delegated capabilities—not only possession of a role or secret. |
| Authorization | Deciding whether this principal may perform this action on this resource under these conditions. Permission to invoke an intermediary is not necessarily permission for its downstream effect. |
| Baseline authority | What the caller could already legitimately do before using the path under review. A privilege-escalation claim needs an effect beyond that baseline. |
| Delegation | A grant allowing another principal or component to exercise authority. Identify grantor, recipient, permitted targets and actions, lifetime, and revocation semantics. Deliberate delegation is not inherently an escalation. |
| Trust boundary | A point where data, identity, or authority enters a different trust or enforcement domain and assumptions must be established again. It can occur inside a process; a network hop need not introduce a new one. This security term is distinct from a domain-modeling bounded context or a module interface. |
| Authoritative source | The component or record entitled to establish a particular fact, such as ownership, available balance, or permission. A caller's assertion or cached copy does not acquire that authority merely by being well-formed. |
| Data ownership | Responsibility for establishing, changing, protecting, and retiring a fact. Distinguish the system of record from the person or tenant entitled to control or access it. |
| Complete mediation | Every relevant access is subject to the applicable authorization policy, including alternate entry points and lifecycle paths. Cached decisions require explicit validity and revocation semantics; this does not prescribe one database query per access. |
| Least authority | Give a component only the resources, actions, scope, and duration needed for its task. Ambient authority is power available implicitly from the environment rather than deliberately passed to that operation. |
| State integrity | Valid states and authorized transitions are preserved. Types can prevent malformed combinations; mutable ownership, permissions, and concurrent updates still require enforcement at the authoritative transition. |
| Failure semantics | What callers can observe and what effects remain after denial, error, timeout, cancellation, or partial completion. An unknown outcome is not equivalent to no effect. |
| Idempotency | Repetition of the same logical operation preserves its specified effect. Define operation identity, scope, concurrency behavior, retention, and response semantics. Deduplication is one mechanism, not a proof of end-to-end idempotency. |
| Atomicity | A specified group of changes becomes observable together or not at all within its transaction domain. A database transaction does not automatically include a remote effect. |
| Compensation | A subsequent action intended to repair an earlier effect. It can fail or be observed and is not automatically equivalent to rollback. |
| Fail secure | On uncertainty or failure, preserve the specified protection guarantee. Deny protected actions when their authority cannot be established; define safe degraded behavior explicitly. Do not assume denial is the right safety or availability outcome for every system. |
| Blast radius | The resources, tenants, effects, or guarantees a defect or compromise can reach. Shared mechanisms and credentials can enlarge it even when the initiating operation is narrow. |

## Scope and evidence

**Security is not only escalation.** Confidentiality, integrity, and availability can fail without a caller gaining new privileges. Use baseline authority when claiming escalation; for other claims identify the protected property and actual effect.

**State versus transition:** Preventing invalid values does not prove legal transitions, fresh authority, or correct concurrent updates. Stable parsed values can remain valid while the facts needed for authorization change.

**Control versus guarantee:** A control is an enforcement mechanism; a guarantee is the property it is meant to establish. A control's presence, name, or checklist match does not prove it covers the relevant path.

**Static support versus runtime confirmation:** Source and configuration can establish a violating design path; an observed execution establishes what happened in that scenario and environment. State assumptions and uncertainty separately. Do not turn a plausible impact chain into a demonstrated exploit.

For correctness, coherence, evidence levels, and analysis-loop terminology, see [`scout-repository/vocabulary.md`](../scout-repository/vocabulary.md). For compatibility dimensions and explicit, implicit, or incidental obligations, see [`compatibility-review/vocabulary.md`](../compatibility-review/vocabulary.md). Those companions own these definitions; do not introduce parallel taxonomies here.

