---
name: scout-semantic-coherence
description: Review bounded repository paths when shared names, values, states, or errors may have different meanings across writers and readers. Trace domain contracts, units, defaults, and consumers to distinguish consequential mismatches from useful variation. Not a naming/style audit or a mandate to make every component uniform.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Semantic Coherence Scout

Find where components appear to share a concept but disagree about what it means. A familiar name or majority convention is a lead, not the specification.

## Required shared guide

Before discovery, load all three companion files:
- [`../scout-repository/SKILL.md`](../scout-repository/SKILL.md) for the operating workflow and common output contract.
- [`../scout-repository/vocabulary.md`](../scout-repository/vocabulary.md) for shared terms and signal distinctions.
- [`../scout-repository/checklists.md`](../scout-repository/checklists.md), selecting relevant scope, evidence, contract, counterevidence, and reporting questions.

These files must be available with this scout; the family is installed together. If any is missing or unreadable, report the missing dependency and do not silently improvise its contract.
Apply the shared mode, evidence, deduplication, memory, and reporting rules rather than inventing local alternatives.
Use the shared stateless default unless the run brief explicitly selects stateful review.
Repository and system inspection is read-only. Runtime verification requires explicit permission in the run brief and safe isolation; otherwise state static-evidence limits.

## Trigger and bounded inputs

Use for a named concept crossing modules, a feature with multiple implementations, a serialization/storage boundary, or a reported inconsistency in values or errors.
Before inspecting, declare the concept, entry points, writers/readers, relevant domains, repository revision, and exclusions using the shared run brief.
Start with one concept and its reachable producers/consumers, not an unbounded search for every inconsistency.
Useful inputs are current types, schemas, query predicates, adapters, API contracts, configuration, domain documentation, and tests that assert observable meaning.
Existing runtime evidence may help only within the supplied evidence scope; do not query live user data to populate examples.
If a required consumer or domain contract is unavailable, identify that boundary and preserve the uncertainty.

## Path-tracing workflow

1. **Name the proposition.** Write what the concept claims to represent: quantity, identity, ownership, state, time interval, error, or absence. Cite the source of that interpretation.
2. **Identify domain boundaries.** Determine whether equally named concepts actually share a contract. Separate billing, reporting, scheduling, UI, and transport meanings before comparing implementations.
3. **Trace writes.** Follow input parsing, normalization, defaulting, conversion, persistence, event publication, and every scoped alternate writer. Record units, precision, timezone, null/absence, and legal transitions where relevant.
4. **Trace reads.** Follow deserialization, conversion, filtering, aggregation, branching, presentation, and downstream decisions. Ask which assumptions each reader makes and which writer can supply its data.
5. **Compare meaning, not spelling.** Align writer and reader propositions at the boundary. Check whether adapters deliberately translate them; include different names that carry the same concept.
6. **Characterize variation.** Group only comparable paths and record the observed denominator, exclusions, and differences. Do not extrapolate a repository-wide proportion from a small sample.
7. **Construct the consequence.** Carry one concrete value, state, or error through a reachable path and identify the observable decision that changes incorrectly under the claimed contract.
8. **Try to falsify it.** Inspect alternate contracts, conversion layers, constraints, domain partitions, and consumer handling that could make the apparent mismatch intentional or unreachable.
9. **Report at the supported level.** Use the shared signal format; unresolved common meaning can justify a bounded coherence question without establishing a defect.

## Focused questions

- Does `duration` mean milliseconds at the writer and seconds at a consuming expiry check? Is a conversion already applied between them?
- Does `active` mean enabled, currently entitled, not deleted, or presently connected? Are those predicates intentionally different in their domains?
- Is a date range closed or half-open, and which timezone determines its calendar boundaries? Do storage and display retain that distinction?
- Does omitted, null, empty, or zero mean unchanged, cleared, unknown, or an actual value? Does a patch writer agree with the persisted reader?
- Do the same error name/status and retryability flag communicate the same outcome to consumers, or are transport and business errors deliberately separate?
- Do ownership and state-transition rules survive batch, import, background, and direct API paths for this same concept?
- Is an apparent inconsistency caused by versioned wire behavior? If relevant, consult [`compatibility-review`](../compatibility-review/SKILL.md), without expanding this run into a general compatibility audit.

## Useful-signal discriminator

A defect needs a shared intended meaning, an actual affected writer-to-reader path, and a counterexample that violates the contract.
A coherence question needs a concrete, consequential ambiguity and the owner decision or missing evidence that would resolve it; it need not choose a winning implementation.
Measured variation is useful when its population and semantic differences are explicit. For example, 16 of 20 comparable endpoints mapping absence to one error and four to another may warrant asking whether consumers should distinguish them.
That 80/20 split alone establishes neither a defect nor that the 80% behavior is correct. A consumer that retries one result but permanently drops the other can supply the missing consequential path.
Do not emit a raw inventory of naming differences, formatting, abstraction choices, or checklist answers as findings.

## Counterevidence and disqualifiers

- Separate bounded contexts can legitimately use the same term differently; require evidence that data or assumptions actually cross between them.
- Explicit unit conversions, version adapters, normalized schemas, or exhaustive consumer handling may eliminate a surface mismatch.
- Presentation labels need not expose storage terminology if their mapping is deliberate and preserves meaning.
- Different error policies may reflect different operations, authority boundaries, or documented consumer contracts rather than drift.
- A name, comment, or test fixture does not override a stronger current contract; equally, the majority implementation does not override an authoritative minority path.
- If no guarantee or consequential path can be established, withhold the defect claim and identify the unresolved question only if it is useful.

## Calibration examples

- **Signal:** an import stores `retention` in days while the same field's deletion worker treats it as seconds, with no conversion. Trace a specific record to premature deletion and cite the retention contract.
- **Not a defect:** analytics reports rounded minutes while billing persists exact seconds; explicit conversion and distinct documented purposes explain the difference.
- **Coherence question:** comparable APIs use two materially different meanings of `not_found`; report the measured paths and consumer implications without declaring the common form correct.
- **Not a defect:** internal lookup uses absence while an external endpoint intentionally conceals resource existence; do not erase that boundary for uniformity. Use [`security-design`](../security-design/SKILL.md) if authority reasoning is needed.

## Handoff

Return the common scout-repository report, not a local taxonomy. For each emitted signal, retain the concept/domain boundary, writer and reader locators, intended meaning, concrete consequence, and strongest falsification attempt.
State inspected and excluded paths and any missing contracts; do not imply exhaustive semantic consistency. Do not fix, file issues, or send reports autonomously.
