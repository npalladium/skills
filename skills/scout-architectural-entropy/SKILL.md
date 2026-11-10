---
name: scout-architectural-entropy
description: Examine a bounded repository surface for competing meanings or controls, evidenced coupling and repair costs, and obsolete mechanisms that impair correctness or maintenance. Historical trend claims require curated dated evidence. Not a style audit, uniformity drive, or presumption that duplication is debt.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Architectural Entropy Scout

Look for structure that makes a real domain guarantee harder to implement, understand, audit, or repair. Variation is a lead to investigate, not proof of decay: different domains can legitimately use different abstractions or controls.

## Required shared guides

Before discovery, explicitly load these companion files:
- [scout-repository/SKILL.md](../scout-repository/SKILL.md) for the run brief and operating contract.
- [scout-repository/vocabulary.md](../scout-repository/vocabulary.md) for shared terms and signal distinctions.
- [scout-repository/checklists.md](../scout-repository/checklists.md), selecting questions about concept boundaries, control ownership, coupling, and consequences.

These files must be available with this entry point; install the skill family together. If a companion is unavailable, report the missing dependency and stop rather than silently improvise its contract. Use the shared mode, evidence, output, deduplication, and memory rules; do not invent a local substitute.

For security-specific control reasoning, optionally load [security-design](../security-design/SKILL.md). This lens remains independently runnable with the required shared guides.

## Bound the inquiry

Use the shared run brief to name a domain concept, control mechanism, or change surface; identify relevant owners, consumers, and exclusions. Follow only connections whose meaning or maintenance consequence can be demonstrated.

Relevant current evidence may include contracts, data models, writers/readers, configuration, dependency boundaries, call paths, ownership and lifecycle, migration/deprecation state, and tests. Claims about drift, accumulation, or prior repair burden additionally need curated, dated historical evidence supplied in stateful mode. Current duplication alone establishes neither historical trend nor debt.

Read repository/system evidence only. Do not change structures, create cleanup plans detached from an evidenced problem, or run runtime probes unless the shared contract and run brief explicitly permit isolated verification.

## Practical tracing workflow

1. **Name the concept and intended boundary.** Find the authoritative domain contract and identify which components are supposed to share a meaning. If two similarly named concepts serve different domains, keep them distinct unless evidence establishes shared semantics.
2. **Map implementations and ownership.** Trace producers, consumers, policy/control owners, adapters, configuration branches, and lifecycle. Record where meanings, units, null rules, authority, state transitions, or defaults differ.
3. **Find the contradiction or coupling.** Determine whether variation changes observable behavior or forces changes to coordinate distant modules. Trace actual dependencies and control flow; a directory layout or repeated text is not itself coupling.
4. **Check legitimate explanations.** Look for explicit contracts, domain distinctions, compatibility requirements, intentional isolation, and owner decisions. A majority pattern is not authority. An undocumented difference remains a question until its intended semantics and consequence are known.
5. **Test the repair-cost claim.** Identify a concrete change, incident, or invariant repair and the parts that must be coordinated, or show how duplicate controls can drift and affect a real path. Separate observed coordination cost from hypothetical future inconvenience.
6. **Assess obsolescence.** For a seemingly abandoned mechanism, trace whether it is still invoked, configured, persisted, exposed, used for recovery, or required by supported consumers. Names and lack of nearby references do not prove safe removal or deadness.
7. **Use historical context only when supplied.** In stateful mode, compare current evidence with curated dated observations and scope. Revalidate memory against present code; historical notes do not establish current defect or trend. Stateless findings may identify present parallel structures but must not claim they accumulated, diverged over time, or resisted prior repair.
8. **Construct a counterexample and classify.** Describe the concrete maintenance or correctness event affected by the structure. Distinguish verified semantic contradiction, supported coupling/repair concern, contained legacy path, and unresolved intent. Identify counterevidence and narrowest owner decision or verification.

## Signal discriminator

A substantive signal connects an evidenced structure to a consequential path or repair cost. For competing meanings, establish the shared contract and a consumer harmed by the mismatch. For duplicated controls, establish a common guarantee and a concrete way divergence can bypass or obscure it. For coupling, show actual dependencies or coordination required by a realistic change. For obsolete mechanisms, establish that they are truly superseded and identify remaining cost or risk.

Report a present structural observation without claiming historical deterioration unless dated evidence supports the trend. Similarity, novelty, abstraction count, and local inconsistency are not consequences. Unknown intent or missing historical records can be reported as questions, not verified debt.

## Disqualifiers

- Preference for one language, pattern, module shape, abstraction level, or naming scheme.
- “Most call sites do it this way” absent authoritative shared semantics and a consequential mismatch.
- Duplicate code with no evidence that it represents the same concept or creates divergent behavior/repair burden.
- A speculative future change that identifies no realistic coordination path.
- An “obsolete” flag based only on age, naming, or an incomplete reference search.
- A claim that inconsistency has accumulated, worsened, or repeatedly resisted repair based only on today's code.
- A recommendation to consolidate that erases useful domain or compatibility variation without resolving a demonstrated contradiction.

## Calibrated examples

**Meaningful contradiction:** Two consumers of the same documented amount field interpret its units differently; a real API response is consequently off by a factor, and the contract identifies the intended unit. Report the mismatch and affected consumer, not a generalized demand for naming uniformity.

**Potential coupling signal:** Updating one domain rule requires synchronized edits to three independently owned policy implementations; trace their shared contract and show a realistic partial update can leave one reachable path inconsistent. Without the contract or consequence, report an ownership question rather than established entropy.

**Not historical evidence:** Two similar validators exist today. That observation may prompt a current semantic comparison, but it does not show they were introduced separately over time, are obsolete, or have increased repair cost. Use dated curated evidence for any such trend claim.

## Handoff

Use the shared signal format and operating rules. State the concept boundary, components/consumers traced, evidence for consequence, counterevidence, and limits. Keep present observations distinct from historical trend claims, and do not treat a proposed consolidation as the finding itself.
