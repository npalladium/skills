---
name: scout-transaction-lifecycle
description: Review bounded repository operations whose atomicity, resource ownership, transaction propagation, locks, remote effects, or recovery may diverge from their guarantees. Trace concrete crash points and interleavings across lifecycle boundaries. Not an annotation/style audit or a requirement to wrap every effect in one transaction.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Transaction and Lifecycle Scout

Determine what an operation actually owns, commits, releases, and recovers—not what a transaction name or happy-path sequence appears to promise.

## Required shared guide

Before discovery, load all three companion files:
- [`../scout-repository/SKILL.md`](../scout-repository/SKILL.md) for the operating workflow and common output contract.
- [`../scout-repository/vocabulary.md`](../scout-repository/vocabulary.md) for shared terms and signal distinctions.
- [`../scout-repository/checklists.md`](../scout-repository/checklists.md), selecting relevant scope, invariants, lifecycle, evidence, counterevidence, and reporting questions.

These files must be available with this scout; the family is installed together. If any is missing or unreadable, report the missing dependency and do not silently improvise its contract.
Apply the shared mode, evidence, deduplication, memory, and reporting rules rather than inventing local alternatives.
Use the shared stateless default unless the run brief explicitly selects stateful review.
Repository and system inspection is read-only. Runtime verification requires explicit permission in the run brief and safe isolation; otherwise state static-evidence limits.

## Trigger and bounded inputs

Use for an operation spanning multiple writes, asynchronous work, transaction wrappers, locks, resource lifetimes, or database and remote effects.
Declare one operation or invariant, its entry points, affected stores/resources, lifecycle end, and exclusions using the shared run brief.
Follow the operation through its recovery owner; do not expand into every transaction in the repository.
Useful inputs are call sites, transaction/session configuration, framework propagation rules, queries, lock acquisition, callbacks, task ownership, migrations, queue handling, and recovery code.
Use current contracts and relevant existing tests to establish what callers or observers may rely on; annotations alone do not establish actual transaction participation.
If framework behavior or a remote participant's contract cannot be established from permitted evidence, mark that exact assumption unresolved.

## Path-tracing workflow

1. **State the invariant and observation point.** Identify which facts must hold together and whether the guarantee applies during work, at commit, after acknowledgment, or after recovery.
2. **Walk the real entry path.** Follow wrappers, proxies, direct/self calls, callbacks, thread/task hops, and nested calls. Determine where transaction scope starts, joins, suspends, commits, rolls back, and ends.
3. **Track resource identity.** Record which connection/session each read or write uses, when it is checked out and returned, and which owner closes it. Verify propagation rather than assuming similarly named repositories share a transaction.
4. **Track locks and visibility.** Identify lock target, acquisition, release, isolation level, read snapshot, and observer. Check whether the protected read and mutation remain in the same effective scope.
5. **Order every effect.** Distinguish database changes, callbacks, message acknowledgment/publication, files, cache updates, and remote operations. State which can be rolled back and which cannot.
6. **Follow ownership transfers.** Trace spawned work, retained handles, leases, cleanup, cancellation, timeout, and deletion/revocation. Determine who must finish or recover work after the initiating scope ends.
7. **Place a concrete interruption.** Select a reachable process-death point, failed commit, cancellation, or two-actor interleaving; enumerate durable and externally visible state immediately afterward.
8. **Trace recovery to its observer.** Follow restart, retry, reconciliation, compensation, or lease expiry, including repeat execution and compensation failure. Determine whether recovery restores the promised state within its actual contract.
9. **Seek counterevidence and report.** Inspect constraints, ownership fences, outbox/commit coupling, idempotent effects, or documented partial-success semantics that could invalidate the concern; use the common report format.

## Focused questions

- Does nested work join the caller's transaction, obtain another connection, suspend it, or run after commit? Does that match the atomicity claim?
- Does a connection or lock remain held during a remote call or await? What concrete wait cycle, resource exhaustion, or invalid lifetime follows on the scoped path?
- Can a lock be released before the check-dependent mutation, or can another actor change the relevant fact before commit?
- Does an after-commit hook run before acknowledgment, survive process death, and have a durable recovery owner where the contract requires one?
- Can one sub-operation commit while its caller later rolls back? Which observer can see the partial state, and is that allowed?
- Does cancellation release local resources while leaving remote work active? Who owns any subsequent result or cleanup?
- Can deletion or revocation race with in-flight work to recreate state or retain access contrary to the lifecycle contract?
- Can compensation undo the intended effect safely if invoked twice or after later valid work? Is compensation actually promised, or is partial success explicit?

## Useful-signal discriminator

A useful defect claim identifies the promised invariant, actual scope/resource ownership, a reachable interruption or interleaving, the resulting state, and the observer or recovery failure that matters.
A retained connection, long transaction, or remote call is not inherently wrong; connect it to an evidenced lifetime/atomicity violation or concrete resource dependency.
Do not prescribe a distributed transaction, outbox, lock, or retry as a generic remedy. First show the existing mechanism cannot uphold the actual guarantee.
A missing propagation or recovery contract can support a bounded question, but cannot establish a broken guarantee by itself.
If security-sensitive authority or revocation is the invariant, consult [`security-design`](../security-design/SKILL.md) for that specialized reasoning without broadening the run.

## Counterevidence and disqualifiers

- Database constraints, compare-and-set updates, serializable isolation, or a correctly scoped lock may prevent the proposed interleaving; inspect their actual applicability.
- An explicitly non-atomic batch or saga can expose intermediate state legitimately if its consumer/recovery contract permits it.
- A separate connection may be intentional for an independent audit record; first establish whether its survival violates or fulfills the contract.
- Durable work ownership and replay may close a crash window even when immediate completion is not guaranteed.
- A process-local cleanup handler does not prove crash recovery; conversely, absence of one does not prove leakage when the resource owner releases it automatically.
- Do not infer a transaction defect from method names, annotations, nesting, or lack of a preferred abstraction without tracing effective behavior.

## Calibration examples

- **Signal:** two balance writes promise one transfer, but a nested call commits on a separate connection. A crash after that commit leaves one balance changed; trace the unrepaired state to a balance reader.
- **Not a defect:** an independent audit transaction deliberately survives a rejected business operation and does not participate in its balance invariant.
- **Signal:** a row commit is followed by non-durable task scheduling, but success promises eventual delivery. Crash between them leaves no recoverable work record; identify the delivery consumer and missing recovery path.
- **Not a defect:** a durable outbox commits with the row, and a restartable worker owns eventual delivery under the documented contract.
- **Signal:** a parent awaits a child that needs the same single available connection while retaining it; trace the actual ownership cycle rather than merely objecting to an await in a transaction.

## Handoff

Return the common scout-repository report, not a local taxonomy. Retain the effect timeline, scope/resource locators, interruption or actor ordering, durable outcome, recovery owner, and strongest falsification attempt for each signal.
State uninspected participants and unknown runtime assumptions; do not claim crash safety from static inspection alone. Do not fix, file issues, or send reports autonomously.
