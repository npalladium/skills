# Compatibility Vocabulary

These are working review labels, not an externally mandated standard. Describe **what changed**, **the basis of the expectation**, **why it changed**, and **evidence of impact** independently.

## Independent questions

Compatibility asks whether supported consumers can continue relying on the system as it evolves. Consumers include stored data, jobs, events, integrations, operators, and independently deployed clients. A corrective change can still break them.

For the distinction between correctness across behavior, coherence across space, and compatibility across time, see [`scout-repository/vocabulary.md`](../scout-repository/vocabulary.md). This file owns the change dimensions and obligation attributes below.

## Dimensions: what changed?

Dimensions are independent; several can apply. A label never substitutes for an old/new behavior trace.

| Dimension | What changes | Examples |
| --- | --- | --- |
| `structural` | API surface and representation | Removed endpoint, renamed field, changed wire type, newly required parameter. |
| `semantic` | Meaning and interpretation | `amount` changes from gross to net; current available balance becomes historical deposits. |
| `behavioral` | Inputs, outcomes, effects, and state transitions | Changed default, validation rule, permitted transition, side effect, or error condition. |
| `temporal` | Ordering, timing, concurrency, and consistency | Changed read-after-write guarantee, event ordering, idempotency window, or pagination stability. |
| `operational` | Resource limits and service-use conditions | Changed rate limits, timeouts, retention periods, or authentication requirements. |
| `client` | SDK/source compatibility | Renamed generated method, moved Protobuf definition, changed client-library type despite unchanged wire format. |

## Attributes: expectation, cause, and impact

| Attribute | Values | Interpretation |
| --- | --- | --- |
| `basis` | `explicit` \| `implicit` \| `incidental` | Where the consumer expectation comes from. |
| `cause` | `deliberate` \| `corrective` \| `regression` \| `unknown` | Intentional redesign, restoration of intended behavior, accidental deviation, or insufficient evidence. |
| `impact` | `confirmed` \| `potential` \| `no-known-impact` | An identified consumer is demonstrably affected; a concrete consumer scenario could be affected but is unverified; or none is known within the inspected scope. |

### Basis and obligations

- **`explicit`:** An authoritative specification, documentation, RFC, or published policy offers the guarantee. Identify the applicable version and scope. Violating that guarantee is a contract violation even without an observed deployed failure.
- **`implicit`:** Domain meaning, established semantics, or a protocol supports a reasonable expectation without a direct promise. Explain the inference and evidence; a familiar but ambiguous name alone is insufficient. State unresolved intent rather than silently elevating an assumption into a contract.
- **`incidental`:** A consumer relies on observable behavior that was not offered as a guarantee. Demonstrated dependence is real compatibility evidence, but does not itself establish an obligation to preserve the detail. Resolve treatment through actual support and migration policy.

**Hyrum's Law dependence** describes reliance beyond the intended contract, not a seventh dimension. Incidental dependence can concern representation, timing, errors, or any other observable behavior. It neither excuses a demonstrated consumer break nor requires freezing all implementation details.

### Cause is not obligation; impact is not contract status

A corrective fix can restore an explicit guarantee and break a consumer's incidental dependence. Classify those expectations separately. Do not infer `regression` merely because a change is undesirable, or `corrective` merely because a commit calls itself a fix. Use `unknown` where intent or the intended baseline is unsupported.

`confirmed` requires evidence that an identified consumer is affected, not simply that the implementation changed. A supported contract can be violated with `potential` or `no-known-impact`; incidental reliance can have `confirmed` impact without a contractual violation. `no-known-impact` states bounded knowledge, not proof of compatibility. Identify the consumers and paths checked.

### One expectation per classification

Name the expectation first, then use the exact attribute values above; keep evidence and explanations outside the label fields. The existence of an explicit contract somewhere in the change does not make every consumer expectation explicit.

For a fix from incorrect gross 100 to contractually required net 90, an isolated replay shows an old client expecting gross 100 fails. The record about **that client's old gross expectation** is:

```yaml
expectation: The old client expects amount to contain gross value 100.
dimensions: [semantic, behavioral]
basis: incidental
cause: corrective
impact: confirmed
```

The published net-amount requirement is a different, explicit expectation restored by the fix. It does not change the old client's reliance to `explicit`, excuse its observed break, or require preserving the incorrect calculation. If the replay had not occurred and no other evidence established consumer impact, that reliance could instead have `potential` impact.

## Calibration examples

- **Current balance versus historical deposits:** A customer deposited 100, spent 40, and has 10 on hold. Under a policy subtracting holds, current available balance is 50; total historical deposits remain 100. Keeping a numeric `balance` field while switching between these calculations is `semantic`, and may also be `behavioral` if a withdrawal decision changes. Published balance semantics make the basis `explicit`; established account behavior may support `implicit`. The draft name alone does not settle it. Trace the spending consumer before claiming confirmed impact.
- **Sorting:** If `GET /users` promises ascending creation time, removing that ordering violates an `explicit` expectation in the `temporal` dimension despite identical fields. Tie-breaking and pagination can additionally change consumer outcomes. If ordering was never promised, inspect actual reliance rather than automatically declaring a contract violation.
- **Gross versus net:** Returning net 90 instead of gross 100 under the same `amount` type is `semantic`. If this corrects a published net-amount rule, its cause may be `corrective`; a reconciliation consumer summing the old gross values can still be affected. Test the consumer's calculation and evaluate each expectation separately.
- **JSON member order:** Reordering object members preserves JSON object meaning, but an identified client may incorrectly parse positionally or compare raw bytes. Such reliance is `incidental`; classify the changed representation as `structural` and any relevant changed outcome as `behavioral`. Do not infer `confirmed` impact without tracing that client, or introduce a universal order-preservation requirement. Array order, unlike object-member order, can carry protocol meaning and must be assessed separately.
