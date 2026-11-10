# How to Document Architecture

## Output: Architecture README

Analyze the code before documenting it. Use the vocabulary's terms and reserved meanings consistently; consult relevant definitions when needed.

Focus on invariants, layer relationships, and module seams that are unlikely to change frequently; omit facts easily derived from code. Write for someone who knows the problem domain but not this codebase. Start with a bird's-eye overview and codemap. Record why unusual or surprising choices exist (Chesterton's Fence).

```markdown
# [Codebase / Package / Module Name]

## Overview
[What this is, who calls it, what problem it solves.]

## Modules
### [Module name]
- **Interface:** [Full set of facts, not just types.]
- **Seam:** [Where the interface lives.]
- **Depth:** [Deep/shallow + one-line justification.]
- **Adapters:** [For substitution interfaces, list existing or imminent adapters and what varies.]

## Seams
[Non-trivial ones. For each: what complexity or invariants it owns, what callers or tests exercise, and why it lives here. For substitution interfaces, also identify variation and adapters.]

## Design decisions
[Non-obvious choices, citing principles: "We accepted a shallow module because...", "Seam location chosen because the deletion test showed..."]

## Non-decisions
[Deliberately omitted structure. "One adapter for X and no second is imminent; no substitution interface introduced. The concrete module owns the invariant directly."]

## Lenses
Include whichever lenses are relevant to the system. Omit those that don't apply.

### Infra and network
[Deployment topology, environments, network boundaries, load balancers, DNS, CDNs, regions. Where do processes run and how do they reach each other?]

### Code organization
[Repo layout, package/directory structure, build targets, dependency graph between packages. How does the source map to deployable units?]

### Call graph
[Runtime flow between modules. Entry points, request paths, async boundaries, queue consumers, scheduled jobs. How does a request move through the system?]

### Data model
[Persistent entities, their relationships, storage engines, migration strategy. Where is authoritative state and how does it flow?]

### Identity and auth model
[Principals (users, services, API keys), authentication mechanisms, authorization model (RBAC, ABAC, scopes), token lifecycle, trust boundaries.]
```

## Output: ADR

Use when the user asks to write an ADR for a decision.

```markdown
# ADR-NNN: [Title—name the decision, not the outcome]

## Status
[Proposed / Accepted / Superseded by ADR-MMM]

## Context
[Forces, in vocabulary terms: which module, interface, seam is in question.]

## Decision
[What was decided. Be specific about which module/interface/seam changes.]

## Consequences
[Expected outcomes.]

## Alternatives considered
[Each with one-two sentences grounded in principles: "Rejected: would have failed the deletion test."]
```

## Output: Design Doc

Use when the user asks to design a system or feature *before* building it—distinct from the README (documents what exists) and the ADR (records one decision).

Three layers, each a branch of possibilities; designing is choosing among them. Lay out the choices made, the notable ones rejected, and why.

```markdown
# [System / Feature Name]

## Problem
[Statement, goals, non-goals, and requirements—functional and non-functional.]

## Functional specification
[Precisely how the system behaves from the outside, independent of internals.]

## Technical specification
[The internals—modules, interfaces, seams, data model—in vocabulary terms.
Cite principles for the non-obvious choices; note alternatives rejected.]
```

## Caveats

- Design docs show intended design, not current system state. Verify claims against current code before treating them as facts.
