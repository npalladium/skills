# How to Document Architecture

## Output: Architecture README

Use when the user asks to document a codebase, package, or module.

Two jobs, in order: **analyze** code using the vocabulary in `vocabulary.md`, then **document** the result using the same vocabulary. Use terms exactly—the vocabulary is the whole point.

Focus on what's unlikely to change frequently—invariants, layer boundaries, and module relationships. These are invisible when reading code alone. Avoid documenting things derivable easily from the code itself. Target audience: someone who knows the problem domain but not this codebase. Begin with a bird's-eye problem overview, then a codemap showing how modules relate.

Document why unusual or surprising design choices exist (Chesterton's Fence). Without explicit rationale, future maintainers assume arbitrary choices and remove load-bearing structure.

```markdown
# [Codebase / Package / Module Name]

## Overview
[What this is, who calls it, what problem it solves.]

## Modules
### [Module name]
- **Interface:** [Full set of facts, not just types.]
- **Seam:** [Where the interface lives.]
- **Depth:** [Deep/shallow + one-line justification.]
- **Adapters:** [List if multiple; note if only one.]

## Seams
[Non-trivial ones. For each: what varies, which adapters exist, why the seam is here.]

## Design decisions
[Non-obvious choices, citing principles: "We accepted a shallow module because...", "Seam location chosen because the deletion test showed..."]

## Non-decisions
[Where a seam is deliberately absent. "One adapter for X; no seam introduced because nothing varies across it yet."]

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

- Design docs show *intended* design, not current system state. They're starting points for understanding, not authoritative documentation of how systems actually work. Always verify claims in design docs against current code before citing them in analysis.
