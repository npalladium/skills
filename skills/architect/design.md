# Design Analysis

## Principles (reference — cite by name)

Form judgments with these and cite them by name in writeups.

- **Depth is a property of the interface, not the implementation.** A deep module can be internally composed of small parts — they just aren't part of the interface. Internal seams (private) vs external seam (the interface).
- **The deletion test.** Delete the module mentally. Complexity vanishes → pass-through. Reappears across N callers → earning its keep.
- **The interface is the test surface.** Callers and tests cross the same seam. Wanting to test *past* the interface → wrong module shape.
- **Design for evolution (accretion, not mutation).** Add new functions; never rename, never increase required parameters, never decrease returned data. Be consistent in parameter ordering across overloads. Provide at least one concrete type for each abstract interface you ship.
- **Triangle of Separation** — (1) Single level of abstraction, (2) Push conditionals to boundaries, (3) Separate branching from doing. Convergence → natural seam. All three violated → needs splitting.
- **Layered framework.** High-level interfaces optimised for productivity; low-level interfaces designed for power and expressiveness. If only one layer exists, ask which caller is underserved. Shorter overloads call through to longer ones. Separate layers by namespace when the low-level interface is complex; co-locate when easy fallback matters more.
- **The pit of success.** The Obvious Thing, the Easy Thing, and the Right Thing should overlap. Doing the Wrong Thing should be at least uncomfortable, at worst impossible. Good defaults, convenience overloads, best names reserved for the most common types.

## Procedure (follow in order)

1. **What is the module?** Name it. State its interface in a sentence or two — full set of facts a caller needs, not just the type signature.
2. **Where is the seam?** Where does the interface actually live (file, function, type)? Right place, or surprising spot?
3. **How deep?** What does a caller get per unit of interface they learn? If the interface is nearly as wide as the implementation, say so plainly: shallow. Consider whether the module's current shape reflects deliberate design or accumulated drift — well-designed systems degrade over time as modifications accumulate.
4. **Lifecycle context.** What stage is this system in? A product validating market fit may deliberately choose shallow, ugly-but-fast modules. A long-lived library must invest in deep interfaces. Don't recommend restructuring without considering lifecycle.
5. **Deletion test.** If the module vanished, where does the complexity go? "Nowhere, pass-through" is a finding. "Across N callers" is a finding — name N if you can.
6. **Count adapters.** One adapter = hypothetical seam. Two+ = real seam, and the interface had better genuinely abstract over what varies.
7. **Test surface.** What do the tests cross? If they reach past the interface — touching internals, mocking transitive dependencies, asserting on private state — the seam is wrong or the interface is too narrow.
8. **Pit of success.** Do callers naturally fall into correct usage? If misuse is easy or common, that's a finding — the interface shape is wrong regardless of depth.

Use the vocabulary terms verbatim. If you write "the API surface here is...", stop and rewrite as "the interface here is...".

## Output: Inline Review

Use this template for "review this" / "is this a good abstraction" requests.

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
