o---
name: architecture-documentation
description: Use this skill when the user explicitly asks to review, analyze, document, or improve the architecture of a codebase, module, or system. Triggers include phrases like "review my architecture", "document this module", "is this a good abstraction", "where should the seam go", "write an architecture doc/README/ADR for this", "improve the codebase architecture", or "is this module too shallow/deep". The skill enforces a precise shared vocabulary (Module, Interface, Implementation, Depth, Seam, Adapter, Leverage, Locality) for analyzing structure and produces written documentation that uses these terms consistently. Do NOT use for general coding help, bug fixes, or feature implementation that don't ask about structural quality.
---

# Architecture Documentation

Two jobs, in order: **analyze** code or a described system using a precise vocabulary, then **document** the result — inline review, architecture README, or ADR — using the same vocabulary.

The vocabulary is the whole point. Use these terms exactly. When "interface" sometimes means a TypeScript keyword and sometimes a REST endpoint, every discussion has to re-establish what the words mean before it can go anywhere.

## The Vocabulary

Every architectural claim should land in one of these terms. Each entry says what the term means here, and — for the often-confused ones — what its avoided substitutes should mean instead, so they're available when the topic genuinely calls for them.

**Module** — Anything with an interface and an implementation. Scale-agnostic on purpose: a function, class, package, or tier-spanning slice are all modules.
- *component* → reserve for UI-framework components (a React `<Button>`).
- *service* → reserve for a process running over a network (an HTTP service, a systemd service). A class isn't a "service" just because it does work.

**Interface** — Everything a caller must know to use the module correctly: type signature, but also invariants, ordering constraints, error modes, required configuration, performance characteristics, threading rules. If a caller can be wrong about it, it's part of the interface.
- *signature* → reserve for the strictly type-level shape (parameter types, return type, generics). The signature is *part of* the interface, not the whole thing.
- *API* → reserve for a published HTTP/RPC surface ("the GitHub API") or established terms of art ("public API"). Don't use it to mean "the public methods of this class" — say *interface*.

**Implementation** — What's inside a module. Distinct from *adapter*: a Postgres repo is a small adapter with a large implementation; an in-memory fake is a large adapter with a small implementation. Reach for *adapter* when the seam is the topic, *implementation* otherwise.

**Depth** — Leverage at the interface: how much behaviour a caller (or test) can exercise per unit of interface they have to learn. **Deep** = a lot of behaviour behind a small interface. **Shallow** = the interface is nearly as wide as the implementation.

**Seam** *(Michael Feathers, Working Effectively with Legacy Code)* — A place where you can alter behaviour without editing in that place. The *location* at which a module's interface lives.
- *boundary* → reserve for DDD's bounded context (a domain-modeling concept, distinct from where an interface sits).

**Adapter** — A concrete thing that satisfies an interface at a seam. Describes *role* (what slot it fills), not substance (what's inside).

**Leverage** — What callers get from depth. More capability per unit of interface they learn. One implementation pays back across N call sites and M tests.

**Locality** — What maintainers get from depth. Change, bugs, knowledge, and verification concentrate at one place. Fix once, fixed everywhere.

## Principles

Form judgments with these and cite them by name in writeups.

- **Depth is a property of the interface, not the implementation.** A deep module can be internally composed of small, mockable, swappable parts — they just aren't part of the interface. A module can have **internal seams** (private, used by its own tests) as well as the **external seam** at its interface.
- **The deletion test.** Imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **The interface is the test surface.** Callers and tests cross the same seam. Wanting to test *past* the interface means the module is the wrong shape — wrong seam location, or too narrow an interface.
- **One adapter means a hypothetical seam. Two adapters means a real one.** Don't introduce a seam unless something actually varies across it.

## Relationships

A **Module** has exactly one **Interface**. **Depth** is a property of a **Module**, measured against its **Interface**. A **Seam** is where the **Interface** lives. An **Adapter** sits at a **Seam** and satisfies the **Interface**. **Depth** produces **Leverage** for callers and **Locality** for maintainers.

## Framings to reject

- **Depth as ratio of implementation-lines to interface-lines** (Ousterhout's original): rewards padding the implementation. Use depth-as-leverage.
- **"Interface" as the TypeScript `interface` keyword or a class's public methods**: too narrow. Interface here includes every fact a caller must know.
- **"Boundary"** as a synonym for seam or interface: keep it for DDD's bounded context.

## Workflow

### 1. Identify the request shape

- **Review** ("is this well-designed?") → analyze, produce inline findings.
- **Document** ("write a README/ADR for this") → analyze, produce a written artifact.
- **Decide** ("should I extract this?", "where should the seam go?") → analyze, recommend, ground in the principles.

Most requests mix these. Be clear which mode each section is in.

### 2. Analyze (in this order)

1. **What is the module?** Name it. State its interface in a sentence or two — full set of facts a caller needs, not just the type signature.
2. **Where is the seam?** Where does the interface actually live (file, function, type)? Right place, or surprising spot?
3. **How deep?** What does a caller get per unit of interface they learn? If the interface is nearly as wide as the implementation, say so plainly: shallow.
4. **Deletion test.** If the module vanished, where does the complexity go? "Nowhere, pass-through" is a finding. "Across N callers" is a finding — name N if you can.
5. **Count adapters.** One adapter = hypothetical seam. Two+ = real seam, and the interface had better genuinely abstract over what varies.
6. **Test surface.** What do the tests cross? If they reach past the interface — touching internals, mocking transitive dependencies, asserting on private state — the seam is wrong or the interface is too narrow.

Use the vocabulary terms verbatim. If you write "the API surface here is...", stop and rewrite as "the interface here is...".

### 3. Produce the artifact

#### Inline review — for "review this" / "is this a good abstraction"

```
## [Module name]
**Interface.** [What callers must know.]
**Seam.** [Where it lives. Right place?]
**Depth.** [Deep/shallow/mixed + one-line why.]
**Findings.**
- [Each cites a principle: "Fails the deletion test — ...", "One adapter, so the seam is hypothetical", etc.]
**Recommendations.** [Concrete moves, each tied to a principle.]
```

Keep findings specific. Not "this module is too shallow" but: "Shallow — the interface is six methods, the implementation is six methods each delegating one-to-one to a database call. By the deletion test, complexity wouldn't reappear at callers; they'd call the database directly with the same surface area."

#### Architecture README — for "document this codebase/package"

Adapt depth to size; keep section names — they map to the vocabulary.

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
```

#### ADR — for "write an ADR for [decision]"

```markdown
# ADR-NNN: [Title — name the decision, not the outcome]

## Status
[Proposed / Accepted / Superseded by ADR-MMM]

## Context
[Forces, in vocabulary terms: which module, interface, seam is in question.]

## Decision
[What was decided. Be specific about which module/interface/seam changes.]

## Consequences
- **Leverage:** [What callers gain.]
- **Locality:** [What maintainers gain.]
- **Costs:** [What this makes harder. A seam added with one adapter is a known cost — name it.]

## Alternatives considered
[Each with one-two sentences grounded in principles: "Rejected: would have failed the deletion test."]
```

### 4. Self-check

Before handing back, scan for:

- **Substituted vocabulary.** Grep mentally for *API, boundary, service, component, signature*. Replace unless the term genuinely means its reserved sense (a literal `Service` class, an actual HTTP API, a type signature being discussed *as* a signature).
- **Claims without principles.** Every "this is bad" or "this should change" cites a principle (deletion test, adapter count, depth, test surface). Otherwise add the citation or soften the claim.
- **Depth claims about implementations.** Depth is a property of the *interface*. "This implementation is deep" → "This module is deep — the interface is small relative to the behaviour it exposes."
- **Speculative seams.** Recommended a new seam? Two adapters must actually exist or be imminent. Otherwise soften: "if a second adapter appears, this is where the seam should go."

## When the user pushes back on the vocabulary

If they want a one-off translation ("rephrase for a team that uses 'service'"), do it — note once that the analysis was done in this vocabulary and translated for output. If they reject the vocabulary entirely, drop the discipline and answer in their terms. The vocabulary is a tool for thinking clearly, not a loyalty test.

## Examples

**Shallow module finding:**

> Input: "Review this `UserRepository` — it has `getUser`, `saveUser`, `deleteUser`, `findUsersByEmail`, `findUsersByRole`, each a one-line ORM call."
>
> Output: The `UserRepository` is a **shallow module**. Its **interface** is five methods; its **implementation** is five methods, each delegating one-to-one. By the **deletion test**, removing it would not cause complexity to reappear at callers — they would call the ORM directly with the same surface area. There is **one adapter** at this **seam**, so the seam is hypothetical. Recommendation: delete the repository and call the ORM directly until a second adapter (e.g., a test fake) is genuinely needed.

**Vocabulary caught in self-check:**

> Draft: "The auth boundary exposes a clean API for token validation."
> Revised: "The auth **module** exposes a deep **interface** for token validation — callers learn one method (`validate(token)`) and get token parsing, signature checking, expiry handling, and revocation lookup."

The revision swaps *boundary* and *API* for the right terms and makes the depth claim explicit. ("Signature checking" stays — that's literally about cryptographic signatures, not the avoided sense.)
