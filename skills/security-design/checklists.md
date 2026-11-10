# Security Engineering Checklists

Use with the [workflow](SKILL.md#workflow). Mark a condition complete only with supporting evidence.

## A. Before implementation

### Intent and scope

- What changes, who or what is protected, and where must the invariant hold?
- Which contract or owner decision establishes the requirement, and what remains unresolved?
- Which component failures or compromises would invalidate the guarantee? What evidence justifies reliance on those components, and how will the affected guarantee be verified?

### Boundaries, authority, and data

- Where do untrusted data, identity, derived data, or authority cross trust boundaries?
- Which facts must an authoritative component establish rather than accept from the caller?
- What identity acts at each hop? Who granted delegated authority, what does it cover, and how can it expire or be revoked?
- Which caches, indexes, events, exports, logs, callbacks, and scheduled jobs preserve or lose source permissions and tenant scope?
- Which alternate or delayed paths reach the operation, including internal or legacy protocols newly exposed to customer-controlled infrastructure?
- Who can change the enforcing code, policy, configuration, credentials, or authoritative records?
- Which capabilities, shared resources, and retained data are necessary, and for how long?
- Can authorized owners inspect, restrict, and disable risky capabilities and delegated grants? Which credentials or schedules survive account remediation, and is that consistent with the promised lifecycle?
- If multiple protections are claimed, which failures do they address independently, and which dependencies could defeat them together?

### State, dependencies, and lifecycle

- What states and transitions are valid? At which point must an invariant hold: during internal work, at commit, or to observers?
- What does each dependency promise, and what if it returns stale, malformed, contradictory, unauthorized, duplicated, or misleading data?
- What are the outcomes for denial, unavailable dependencies, timeout, retry, cancellation, crash, partial success, and unknown commit status?
- Can authority, ownership, configuration, credential validity, or resource existence change between scheduling, checking, executing, and serving?

## B. During implementation

### Enforce guarantees at the right boundary

- Do parsing, validation, authorization, routing, and eventual use agree on the representation? Could decoding, normalization, or resolution change the checked target? Is output encoded for its destination?
- [ ] The relevant action is authorized for the exact target and current principal at each enforcement domain, including alternate APIs, jobs, exports, and effects that can outlive a request.
- [ ] Where authorization results are cached, they bind every relevant decision input; their scope and validity preserve the stated permission-change, expiry, revocation, and tenant guarantees.
- [ ] Binding or changing a run-as identity checks the caller's authority to delegate it—not merely permission to invoke an already administrator-bound workload.
- Is a referenced target caller-selected, newly created, or resolved from an authorized owner? What permits selection of that target, including any intended delegation?
- Does the target-side decision receive a verified initiating identity or suitable delegation evidence, or only the intermediary's identity? Which component owns the cross-service decision?
- Does resolving a bucket, repository, package, or service name establish its expected owner? Can claiming or reassigning the name redirect trusted behavior?
- Can a caller occupy or induce traffic from a trusted network position? Does an allowlist admit a shared producer without binding the requesting caller to the particular connectivity resource?
- [ ] Derived-data ingestion and serving enforce applicable source restrictions or owner-authorized sharing policy, including alternate exports, queries, and generated answers.
- Can a parent-scope operational permission change a child's security posture? Is this intended governance, incidental escalation, or a cross-tenant effect requiring separate consent?
- Who authenticates the user and binds that identity to a caller-supplied user/session selector? Can changing only the selector reach another user's protected state?
- What can the credential actually authorize after audience, scope, and identity-policy checks? Can a caller redirect a stored credential to an attacker-controlled endpoint?

### Preserve state and effect semantics

- Do types, constraints, state machines, or narrow interfaces make invalid state combinations and unauthorized transitions difficult to construct? Which invariants still require enforcement at a mutable authoritative boundary?
- [ ] Checks and effects are ordered to preserve the required guarantee across transaction boundaries, accounting for effects outside the database transaction.
- Could a timeout, cancellation, retry, duplicate request/event, or concurrent operation cause repeated, reordered, or partially committed effects?
- [ ] Where idempotency is required, its identity, scope, retention, conflict behavior, and unknown-outcome recovery are defined.
- [ ] The isolation or concurrency strategy preserves required invariants under relevant check/use races, stale reads, lost updates, lock lifetimes, transaction propagation, and external effects.
- [ ] Where compensation or cleanup is used, it preserves required guarantees when repeated, interrupted, concurrent with later work, or applied to partial effects.
- [ ] Resource costs are bounded where untrusted inputs or fan-out influence memory, CPU, concurrency, data volume, deadlines, retries, or downstream calls.
- Can responses, errors, logs, caches, or integrations disclose protected data? Who can execute code where a credential is delivered and use it to sign, decrypt, authenticate, or connect without extracting it?

### Keep the system understandable

- Does each security-critical rule have a clear owner and enforcement location? Do writers, readers, APIs, and independently deployed components agree on shared concepts, and what establishes the intended meaning?
- Does a wrapper, shared mechanism, duplicate policy, or configuration exception obscure enforcement or increase concrete repair cost?

## C. Before completing implementation

### Demonstrate changed behavior

- [ ] Focused tests exercise the enforcing boundary with real policy decisions and relevant memberships, ownership, and settings, rather than stub the permission under test.
- [ ] Relevant negative tests cover unauthorized principals, wrong tenants/owners, stale or revoked authority, and invalid transitions; denial neither exposes protected data nor performs the protected effect.
- [ ] Caller-bound authorization tests hold target and operation constant while varying caller permission; where relationship/association gates or deputy permissions matter, tests vary them independently to establish what enforces the decision.
- Which boundary values or malformed representations could change authorization, target resolution, or resource cost?
- [ ] Where transaction, retry, partial success, cancellation, or concurrency matters, evidence covers plausible interleavings and demonstrates invariant preservation.
- [ ] Where relevant, dependency-failure tests distinguish failure-before-effect, effect-with-lost-response, and partial external success.
- [ ] Tests fail for plausible incorrect implementations and assert the contract rather than incidental structure.
- [ ] Guarantees are verified across affected alternate entry points and delayed execution, including authority changing between configuration, scheduling, execution, and serving where applicable.

### Inspect the change and operational behavior

- [ ] Affected contracts, persisted representations, client/source surfaces, generated interfaces, migrations, configuration, deployment assumptions, and supported-version behavior are accounted for.
- [ ] Fresh installations and supported configurations establish protection without undocumented corrective steps; initialization, maintenance, degraded operation, shutdown, and recovery preserve required guarantees.
- Could observability attribute sensitive decisions and effects to the initiating principal and effective intermediary without recording secrets?
- [ ] Restore, replay, failover, and reconciliation do not reintroduce revoked access, obsolete policy, or effects required to occur only once.
- Can an attacker or ordinary dependency failure trigger a defensive response whose side effects cause disproportionate disruption or obstruct recovery?
- [ ] The change introduces no unrelated permissions, data exposure, dependencies, public surface, configuration exceptions, or architectural coupling.

## D. During review

Apply A–C to the changed behavior and surrounding paths, not just edited lines.

- [ ] The fix preserves required guarantees through affected alternate paths and recovery without unrelated exposure, rather than merely blocking the demonstrated request.
- What observable behavior or trust assumption changed? Is the difference a violation, justified change, incidental dependency, or unresolved?
- Do configuration or dependency changes alter enforcement, defaults, failure behavior, or effective authority?
- For each concern, what concrete scenario supports it and what counterevidence would falsify it?

