# Pre-construction Module Review

Use for class/module design before construction. For implementation tactics, see `code-design`.

## Design in construction

Use before construction for class/module-level design. This overlaps `code-design`; consult only when that review scope is requested.

**Design process:**
- Have you iterated, picking the best of several attempts rather than the first?
- Have you tried decomposing the system in several different ways?
- Have you approached the design both top-down and bottom-up?
- Have you prototyped risky or unfamiliar parts with the minimum throwaway code?
- Has the design been reviewed, formally or informally?
- Have you driven the design to where the implementation seems obvious?
- Have you captured the design work in an appropriate medium (wiki, UML, CRC cards, photos, or comments in code)?

**Design goals:**
- Does the design address issues identified and deferred at the architectural level?
- Is the design stratified into layers?
- Are you satisfied with how the program decomposes into subsystems, packages, and classes?
- Are you satisfied with how classes decompose into routines?
- Are classes designed for minimal interaction with each other?
- Are classes and subsystems reusable in other systems?
- Will the program be easy to maintain?
- Is the design lean—every part strictly necessary?
- Does it use standard techniques and avoid exotic, hard-to-understand elements?
- Overall, does it minimize both accidental and essential complexity?
