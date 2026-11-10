# Security Engineering Checklists

Use with the [workflow](SKILL.md#workflow). A checklist answer is not evidence of enforcement.

## A. Before implementation

### Intent and scope

- What changes, who or what is protected, and where must the invariant hold?
- Which contract or owner decision establishes the requirement, and what remains unresolved?
- Which component failures or compromises would invalidate the guarantee? What evidence supports that reliance, and is verification rigor proportionate to the consequence?

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
- Is the relevant action authorized for the exact target and current principal at each enforcement domain, including alternate APIs, jobs, exports, and effects that can outlive a request?
- Does a cached authorization result bind every relevant decision input? Do its scope and validity preserve the stated permission-change, expiry, revocation, and tenant guarantees?
- Before binding or changing a run-as identity, is the caller authorized to delegate it? Distinguish this from invoking a workload whose identity was already bound by an authorized administrator.
- Is a referenced target caller-selected, newly created, or resolved from an authorized owner? What permits selection of that target, including any intended delegation?
- Does the target-side decision receive a verified initiating identity or suitable delegation evidence, or only the intermediary's identity? Which component owns the cross-service decision?
- Does resolving a bucket, repository, package, or service name establish its expected owner? Can claiming or reassigning the name redirect trusted behavior?
- Can a caller occupy or induce traffic from a trusted network position? Does an allowlist admit a shared producer without binding the requesting caller to the particular connectivity resource?
- Where are source restrictions enforced during both ingestion and serving of derived data? Can alternate exports, queries, or generated answers bypass them?
- Can a parent-scope operational permission change a child's security posture? Is this intended governance, incidental escalation, or a cross-tenant effect requiring separate consent?
- Who authenticates the user and binds that identity to a caller-supplied user/session selector? Can changing only the selector reach another user's protected state?
- What can the credential actually authorize after audience, scope, and identity-policy checks? Can a caller redirect a stored credential to an attacker-controlled endpoint?

### Preserve state and effect semantics

- Do types, constraints, state machines, or narrow interfaces make invalid state combinations and unauthorized transitions difficult to construct? Which invariants still require enforcement at a mutable authoritative boundary?
- Are checks and effects correctly ordered across transaction boundaries? Does a database transaction actually include the effects the guarantee depends on?
- Could a timeout, cancellation, retry, duplicate request/event, or concurrent operation cause repeated, reordered, or partially committed effects?
- Are idempotency identity, scope, retention, conflict behavior, and unknown-outcome recovery defined where they matter?
- Can a check/use race, stale read, lost update, lock lifetime, transaction propagation, or external effect violate an invariant? Does the isolation or concurrency strategy support the promised semantics?
- Are compensation and cleanup safe when they run twice, fail, race with later work, or encounter only some earlier effects?
- Are resource costs bounded where untrusted inputs or fan-out can influence memory, CPU, concurrency, data volume, deadlines, retries, or downstream calls?
- Can responses, errors, logs, caches, or integrations disclose protected data? Who can execute code where a credential is delivered and use it to sign, decrypt, authenticate, or connect without extracting it?

### Keep the system understandable

- Does each security-critical rule have a clear owner and enforcement location? Do writers, readers, APIs, and independently deployed components agree on shared concepts, and what establishes the intended meaning?
- Does a wrapper, shared mechanism, duplicate policy, or configuration exception obscure enforcement or increase concrete repair cost?

## C. Before completing implementation

### Demonstrate changed behavior

- Do focused tests exercise the enforcing boundary with real policy decisions and relevant memberships, ownership, and settings, rather than stub the permission under test?
- Do negative tests cover unauthorized principals, wrong tenants/owners, stale or revoked authority, and invalid transitions—and establish that the denied operation neither exposed protected data nor performed the protected effect?
- Holding target and operation constant, does changing caller permission change the decision? Separately vary the purported relationship/association gate and deputy permissions to identify what actually enforced a denial.
- Which boundary values or malformed representations could change authorization, target resolution, or resource cost?
- Where transaction, retry, partial success, cancellation, or concurrency matters, is there evidence that a plausible interleaving preserves the invariant?
- Do dependency-failure tests distinguish failure-before-effect, effect-with-lost-response, and partial external success when relevant?
- Does the test actually fail for a plausible incorrect implementation, and do its assertions capture the contract rather than incidental structure?
- Are guarantees verified across alternate entry points and delayed execution, including authority changing between configuration, scheduling, execution, and serving where applicable?

### Inspect the change and operational behavior

- Are contracts, persisted representations, client/source surfaces, generated interfaces, migrations, configuration, deployment assumptions, and supported-version behavior accounted for where affected?
- Does a fresh installation or supported configuration establish protection without undocumented corrective steps? Do initialization, maintenance, degraded operation, shutdown, and recovery preserve the required guarantees?
- Could observability attribute sensitive decisions and effects to the initiating principal and effective intermediary without recording secrets?
- Can restore, replay, failover, or reconciliation reintroduce revoked access, obsolete policy, or an already-completed effect?
- Can an attacker or ordinary dependency failure trigger a defensive response whose side effects cause disproportionate disruption or obstruct recovery?
- Did the change introduce unrelated permissions, data exposure, dependencies, public surface, configuration exceptions, or architectural coupling?

## D. During review

Apply the relevant questions in A–C to the changed behavior and surrounding paths, not just edited lines.

- What observable behavior or trust assumption changed? Is the difference a violation, justified change, incidental dependency, or unresolved?
- Do configuration or dependency changes alter enforcement, defaults, failure behavior, or effective authority?
- Does the fix preserve guarantees through alternate paths and recovery, without unrelated exposure, or merely block the demonstrated request?
- For each concern, what concrete scenario supports it and what counterevidence would falsify it?

## E. Report significant findings

For each significant finding, provide:

1. **Claim and evidence level:** a substantiated defect, supported design concern, or unresolved question; state whether support is static, runtime, or mixed. In scout mode use the shared signal kinds from `scout-repository`.
2. **Invariant or contract:** what should hold, its scope and authority source. If the concern also changes a consumer expectation, use `compatibility-review` for its independent classification.
3. **Affected path:** entry point, caller identity, relevant boundary/enforcement point, dependency or state transition, and affected resource/consumer.
4. **Evidence:** file/line or other locator, test or runtime evidence, counterevidence, assumptions, and what remains unverified.
5. **Counterexample:** the request, state, interleaving, or lifecycle that violates the guarantee; for escalation, the consumer of any staged artifact and capability beyond baseline. For compatibility, give old/new behavior.
6. **Consequence and extent:** confidentiality, integrity, availability, compatibility, or maintainability impact; prerequisites, baseline caller authority, and plausible blast radius.
7. **Narrowest sound next step:** remediation, verification, or owner decision. Do not prescribe a control broader than the demonstrated requirement.

Keep severity separate from confidence. Label unverified links and the evidence needed to resolve them; do not inflate impact with hypothetical consumers or unrelated failures.

