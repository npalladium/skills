# Design Analysis

Judge design by how cheaply you can change your mind. Cite relevant named principles in findings and recommendations.

- **Judge depth at the interface, not by implementation size.** A deep module can be internally composed of small parts; that internal structure (internal seams) isn't part of the interface and doesn't make it shallow.
- **The deletion test.** Delete the module mentally. Complexity vanishes → pass-through. Reappears across N callers → earning its keep.
- **The interface is the test surface.** Callers and tests cross the same seam. Wanting to test *past* the interface → wrong module shape.
- **Design for evolution.** Prefer additive changes for published, persisted, or independently deployed contracts; preserve supported consumers through explicit migration and retirement. For internal interfaces whose callers migrate together, rename, change parameters or results, and remove obsolete entry points as one coordinated change.
- **Triangle of Separation**—(1) Single level of abstraction, (2) Push conditionals to edges, (3) Separate branching from doing. Convergence → natural seam. All three violated → needs splitting.
- **Layered framework.** High-level interfaces optimised for productivity; low-level interfaces designed for power and expressiveness. If only one layer exists, ask which caller is underserved.
- **Layering**—horizontal system layers, distinct from *Layered framework* above (that's interface tiers inside one module; this is the system-wide stack).
  - *Value*—each layer uses the one below and the lower is unaware of the higher; isolating domain logic from UI, persistence, and messaging (DDD's UI/Application/Domain/Infrastructure) lets the model evolve independently and keeps layers substitutable.
  - *Cost (the smell)*—strict layers fragment feature cohesion: one feature ripples through controller, service, and repository. High change-coupling across layers is the signal; when features consistently cochange across all of them, reach for a feature/use-case-centric (vertical) structure.
  - *Calibrate*—layer to contain cross-cutting concerns (auth, persistence), not feature code. Hexagonal/ports-and-adapters and CQRS are valid evolutions that preserve domain isolation. Trivial CRUD scripts need none.
- **The pit of success.** The Obvious Thing, the Easy Thing, and the Right Thing should overlap. Doing the Wrong Thing should be at least uncomfortable, at worst impossible. Good defaults, convenience overloads, best names reserved for the most common types.
- **Quality ordering.** Required data-integrity and security guarantees constrain every tradeoff; never bypass them for availability. Among designs satisfying those guarantees, prefer reliability > usability > performance. Fail closed when a protected action cannot be authorized.

Named patterns and their decision rules are in `vocabulary.md`; load it when a pattern is relevant.

## Procedure (follow in order)

1. **What is the module?** Name it. State its interface in a sentence or two—full set of facts a caller needs, not just the type signature.
2. **Where is the seam?** Where does the interface actually live (file, function, type)? Right place, or surprising spot?
3. **How deep?** What does a caller get per unit of interface they learn? If the interface is nearly as wide as the implementation, say so plainly: shallow. Note whether the shape is deliberate or accumulated drift.
4. **Lifecycle context.** What stage is this system in? A product validating market fit may deliberately choose shallow, ugly-but-fast modules. A long-lived library must invest in deep interfaces. Don't recommend restructuring without considering lifecycle.
5. **Deletion test.** If the module vanished, where does the complexity go? "Nowhere, pass-through" is a finding. "Across N callers" is a finding—name N if you can.
6. **Count adapters for substitution.** Require multiple existing or imminent adapters before introducing a substitution interface, and identify what genuinely varies. Judge single-implementation module extraction by depth, invariant ownership, and test surface—not adapter count.
7. **Test surface.** What do the tests cross? If they reach past the interface—touching internals, mocking transitive dependencies, asserting on private state—the seam is wrong or the interface is too narrow.
8. **Pit of success.** Do callers naturally fall into correct usage? If misuse is easy or common, that's a finding—the interface shape is wrong regardless of depth.

## Output: Inline Review

Use this template for "review this" / "is this a good abstraction" requests.

```
## [Module name]
**Interface.** [What callers must know.]
**Seam.** [Where it lives. Right place?]
**Depth.** [Deep/shallow/mixed + one-line why.]
**Findings.**
- [Each cites a principle: "Fails the deletion test—...", "Only one adapter and no second is imminent; substitution is speculative", etc.]
**Recommendations.** [Concrete moves, each tied to a principle.]
```

Keep findings specific. Not "this module is too shallow" but: "Shallow—the interface is six methods, the implementation is six methods each delegating one-to-one to a database call. By the deletion test, complexity wouldn't reappear at callers; they'd call the database directly with the same surface area."
