# Vocabulary

Every architectural claim should land in one of these terms. Each entry says what the term means here, and—for the often-confused ones—what its avoided substitutes should mean instead, so they're available when the topic genuinely calls for them.

**Module**—Anything with an interface and an implementation. Scale-agnostic on purpose: a function, class, package, or tier-spanning slice are all modules.
- *service* → reserve for a process running over a network (an HTTP service, a systemd service). A class isn't a "service" just because it does work.

**Interface**—Everything a caller must know to use the module correctly: type signature, but also invariants, ordering constraints, error modes, required configuration, performance characteristics, threading rules. If a caller can be wrong about it, it's part of the interface.
- *signature* → reserve for the strictly type-level shape (parameter types, return type, generics). The signature is *part of* the interface, not the whole thing.
- *API* → reserve for a published HTTP/RPC surface ("the GitHub API") or established terms of art ("public API"). Don't use it to mean "the public methods of this class"—say *interface*.

**Invariant**—A condition that always holds: over a module's data, between its calls, across its lifetime. The implementation must preserve it. When a caller can break it through their own use—must call `init()` first, must not mutate a returned list—it's a fact they must know, so it belongs in the interface. Deeper modules uphold more invariants on the caller's behalf.

**Implementation**—What's inside a module. Distinct from *adapter*: a Postgres repo is a small adapter with a large implementation; an in-memory fake is a large adapter with a small implementation. Reach for *adapter* when the seam is the topic, *implementation* otherwise.

**Depth**—Leverage at the interface: how much behaviour a caller (or test) can exercise per unit of interface they have to learn. **Deep** = a lot of behaviour behind a small interface. **Shallow** = the interface is nearly as wide as the implementation.
- Interfaces that use domain vocabulary rather than invented abstractions are deeper—callers learn the domain once and can predict behavior system-wide.

**Seam** *(Michael Feathers, Working Effectively with Legacy Code)*—A place where you can alter behaviour without editing in that place. The *location* at which a module's interface lives.
- *boundary* → reserve for DDD's bounded context (a domain-modeling concept, distinct from where an interface sits).

**Adapter**—A concrete thing that satisfies an interface at a seam. Describes *role* (what slot it fills), not substance (what's inside).

**Leverage**—What callers get from depth. More capability per unit of interface they learn. One implementation pays back across N call sites and M tests.

**Locality**—What maintainers get from depth. Change, bugs, knowledge, and verification concentrate at one place. Fix once, fixed everywhere.

**Coupling**—The degree to which modules rely on each other. Ranked from worst to best:
1. *Pathological*—directly accessing another module's internals (private state, internal data structures).
2. *Global*—multiple modules depend on shared mutable state (singletons, global variables).
3. *Control*—passing flags/modes that change a module's behavior; the caller knows implementation details.
4. *Data*—passing parameters that affect functionality; necessary but still coupling.
5. *Message*—calling with no parameters; the loosest form, relying only on the interface name.

Control coupling is a sign the seam is in the wrong place—split into separate modules or push the decision into the caller.

**Connascence**—A precise vocabulary for discussing coupling. Two pieces share connascence when *a change in one requires a corresponding change in the other*. Measured on three axes:
- *Strength*—how difficult to discover and refactor (weaker is better).
- *Degree*—how many parts are coupled (fewer is better).
- *Locality*—how close the coupled parts are (closer is more tolerable).

Use connascence to make coupling discussions specific and measurable rather than vague.

## Relationships

A **Module** has exactly one **Interface**. An **Interface** promises **Invariants** the **Implementation** must preserve. **Depth** is a property of a **Module**, measured against its **Interface**. A **Seam** is where the **Interface** lives. An **Adapter** sits at a **Seam** and satisfies the **Interface**. **Depth** produces **Leverage** for callers and **Locality** for maintainers.

## Named patterns

Reach for these by name; each lands in the core terms above.

**Service Layer** *(Fowler, PoEAA)*—a use-case-oriented interface over a Domain Model: a deep module whose seam is the app's transaction/security boundary.
- *Add when* multiple presentations or external clients share the domain, or transactions/security/messaging cross-cut.
- *Stay thin*—a facade over domain objects, thickened only on demonstrated need. Behaviour stays on the objects; an anemic "controller-entity" holding all the logic is the anti-pattern. A pure Transaction Script app needs none.

**Domain Model** *(Fowler, PoEAA)*—the domain as a network of behaviour-bearing objects around its nouns; validation and calculation live on the object they concern, forwarded between collaborators.
- Reach for it as business rules grow complex and evolving, or product/contract types vary richly—payoff scales with rule complexity.
- A thin Service Layer over a rich Domain Model is the canonical pairing.

**Gateway** *(Fowler, PoEAA)*—an adapter wrapping an external system (third-party API, messaging, legacy package, even a DB) behind one domain-shaped interface.
- A test-time Service Stub is the second adapter that makes the seam real, so tests cross it instead of the live system.
- Wrap for that reason, not "just in case".

**Pipes and Filters** *(POSA)*—a stream-processing system split into independent filters (modules) joined by pipes.
- Each filter consumes input incrementally, transforms it, and emits output incrementally; the pipe is the only connection—data flows through, nothing else shared (Data coupling, near the loose end of the ladder).
- A uniform stream interface lets you recombine filters into new pipelines without changing them.

## Framings to reject

- **Depth as ratio of implementation-lines to interface-lines** (Ousterhout's original): rewards padding the implementation. Use depth-as-leverage.
- **"Interface" as the TypeScript `interface` keyword or a class's public methods**: too narrow. Interface here includes every fact a caller must know.
- **"Boundary"** as a synonym for seam or interface: keep it for DDD's bounded context.
