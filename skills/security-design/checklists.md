# Security Engineering Checklists

Apply relevant questions during implementation and review. Reason from intended behavior, security invariants, authoritative contracts, and complete affected paths—not only from known vulnerability patterns. Not every prompt applies. A checklist answer is not evidence by itself; do not invent requirements or perform speculative hardening. See [the workflow](SKILL.md) and [shared definitions](vocabulary.md).

## A. Before implementation

### Intent and scope

- What behavior is changing, and what must remain true for users, other components, persisted data, and operators?
- Which requirements are authoritative, and which assumptions or desired semantics remain unresolved?
- What protected resources, actions, and principals or tenants are in scope? What is explicitly out of scope?
- What does success mean, and what observable evidence could distinguish it from a plausible incorrect implementation?

### Boundaries, authority, and data

- Where do untrusted data, identity, derived data, or authority cross trust boundaries?
- Which component establishes each important fact—identity, ownership, permissions, tenant, destination, status, price, balance, or configuration? Is any caller claim accepted where the server or target owner must decide?
- Who may perform each action on which exact resource, under which conditions? Does permission to call an endpoint also permit its downstream effect?
- What identity acts at each hop? Who granted delegated authority, what does it cover, and how can it expire or be revoked?
- Which caches, indexes, events, exports, logs, callbacks, and scheduled jobs preserve or lose source permissions and tenant scope?
- Which alternate entry points, background workers, recovery flows, and internal callers can reach the same operation?

### State, dependencies, and lifecycle

- What states and transitions are valid? At which point must an invariant hold: during internal work, at commit, or to observers?
- What does each dependency promise, and what if it returns stale, malformed, contradictory, unauthorized, duplicated, or misleading data?
- What are the outcomes for denial, unavailable dependencies, timeout, retry, cancellation, crash, partial success, and unknown commit status?
- Can authority, ownership, configuration, credential validity, or resource existence change between scheduling, checking, executing, and serving?
- Which operations are concurrent or reordered? What coordination and recovery behavior does the existing system promise?

## B. During implementation

### Enforce guarantees at the right boundary

- Are untrusted inputs parsed into appropriate domain values at the boundary, with project-specific validation and canonicalization before security-sensitive use?
- Does the authoritative layer establish the facts required for a sensitive decision rather than trusting client-side checks or caller assertions?
- Is the relevant action authorized for the exact target and current principal at each enforcement domain, including alternate APIs, jobs, exports, and effects that can outlive a request?
- Are caches and remembered decisions valid under permission change, expiry, revocation, and tenant change according to the stated consistency guarantee?
- Are delegated capabilities no broader or longer-lived than the specified task, and are the grant and effective identity auditable?

### Preserve state and effect semantics

- Do types, constraints, state machines, or narrow interfaces make invalid state combinations and unauthorized transitions difficult to construct? Which invariants still require enforcement at a mutable authoritative boundary?
- Are checks and effects correctly ordered across transaction boundaries? Does a database transaction actually include the effects the guarantee depends on?
- Could a timeout, cancellation, retry, duplicate request/event, or concurrent operation cause repeated, reordered, or partially committed effects?
- Are idempotency identity, scope, retention, conflict behavior, and unknown-outcome recovery defined where they matter?
- Can a check/use race, stale read, lost update, lock lifetime, transaction propagation, or external effect violate an invariant? Does the isolation or concurrency strategy support the promised semantics?
- Are compensation and cleanup safe when they run twice, fail, race with later work, or encounter only some earlier effects?
- Are resource costs bounded where untrusted inputs or fan-out can influence memory, CPU, concurrency, data volume, deadlines, retries, or downstream calls?
- Are sensitive data minimized in responses, logs, caches, events, exceptions, exports, and third-party integrations? Can a changed destination or execution environment redirect use or disclosure of a secret?

### Keep the system understandable

- Does each security-critical rule have a clear owner and enforcement location? Are independently implemented paths consistent with the same domain meaning?
- Is there an existing abstraction, standard control, or safe sibling pattern worth reusing? Does a new wrapper, policy layer, or shared mechanism reduce or obscure the total reasoning burden?
- Are new dependencies, shared mechanisms, privileges, interfaces, configuration branches, or exceptions necessary to meet the stated requirement?
- Are errors useful for diagnosis without leaking credentials, sensitive payloads, or cross-tenant data?

## C. Before completing implementation

### Demonstrate changed behavior

- Do focused tests exercise the changed path through the boundary that enforces the guarantee—not only helper wiring or mocked return values?
- Do negative tests cover relevant unauthorized principals, wrong tenants/owners, stale or revoked authority, and invalid transitions?
- Do boundary tests address meaningful malformed, unexpected, empty, duplicate, or maximum inputs?
- Where transaction, retry, partial success, cancellation, or concurrency matters, is there evidence that a plausible interleaving preserves the invariant?
- Do dependency-failure tests distinguish failure-before-effect, effect-with-lost-response, and partial external success when relevant?
- Does the test actually fail for a plausible incorrect implementation, and do its assertions capture the contract rather than incidental structure?
- Are important guarantees verified at the enforcing layer, including alternate entry points or delayed execution where applicable?

### Inspect the change and operational behavior

- Are contracts, persisted representations, client/source surfaces, generated interfaces, migrations, configuration, deployment assumptions, and supported-version behavior accounted for where affected?
- Does secure behavior depend on undocumented assumptions or unsafe configuration defaults? Do supported deployment and recovery modes preserve the same protection guarantees?
- Could observability attribute sensitive decisions and effects to the initiating principal and effective intermediary without recording secrets?
- Can operators detect, diagnose, and repair the relevant failure or inconsistent state? Are rollback, replay, reconciliation, and recovery semantics truthful?
- Did the change introduce unrelated permissions, data exposure, dependencies, public surface, configuration exceptions, or architectural coupling?
- Are no-finding claims scoped to the paths and evidence actually checked?

## D. During review

- What changed in the system's observable behavior, not merely the changed lines?
- Can the affected path be traced through callers, identity transformations, authorization, state, storage, dependencies, and effects?
- Which invariant, source contract, or reasonable domain expectation supports each concern? Is a behavioral difference actually a violation, a justified change, an incidental dependency, or unknown?
- Has the meaning of a shared concept drifted across writers, readers, API surfaces, or independently deployed components? What establishes which meaning is intended?
- Did a new or weakened trust boundary appear? Could a privileged intermediary act on caller-selected targets without target-owner authorization or valid delegation?
- Are failure and exceptional paths, valid but unusual inputs, concurrent changes, and the surrounding transaction/lifecycle included?
- Do tests prove the claimed guarantee, including relevant denials and failure modes, or merely repeat the implementation's assumptions?
- Do configuration or dependency changes alter enforcement, defaults, failure behavior, or the effective authority of a component?
- Does additional complexity, duplicated control logic, abandoned mechanisms, or a one-off exception increase the probability or blast radius of later mistakes?
- For every suspected defect, is there a real affected path and a plausible violating scenario? What evidence or counterexample would falsify the concern?

## E. Report significant findings

For each significant finding, provide:

1. **Claim and evidence level:** a substantiated defect, supported design concern, or unresolved question; state whether support is static, runtime, or mixed. In scout mode use the shared signal kinds from `scout-repository`.
2. **Invariant or contract:** what should hold, its scope and authority source. If the concern also changes a consumer expectation, use `compatibility-review` for its independent classification.
3. **Affected path:** entry point, caller identity, relevant boundary/enforcement point, dependency or state transition, and affected resource/consumer.
4. **Evidence:** file/line or other locator, test or runtime evidence, counterevidence, assumptions, and what remains unverified.
5. **Counterexample:** a concrete request, state, interleaving, lifecycle, or consumer scenario that violates the claim; for compatibility claims, describe the old/new behavior and affected consumer.
6. **Consequence and extent:** confidentiality, integrity, availability, compatibility, or maintainability impact; prerequisites, baseline caller authority, and plausible blast radius.
7. **Narrowest sound next step:** remediation, verification, or owner decision. Do not prescribe a control broader than the demonstrated requirement.

Keep report severity distinct from evidence confidence. A powerful feature, suspicious name, absence of a comment, hypothetical chain, or stylistic difference alone is not a finding. Say what evidence is needed when the concern remains unresolved.

For focused read-only analysis, use the six scout entry points listed in [`scout-repository`](../scout-repository/SKILL.md). That shared skill owns stateless/stateful inputs, signal kinds, deduplication, and memory. For consumer obligations across a change, use [`compatibility-review`](../compatibility-review/SKILL.md).

