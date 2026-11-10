# Pre-construction Architecture Review

Use when reviewing an architecture document before construction. Apply questions relevant to the system; do not invent unnecessary structure to satisfy a checklist.

## Architecture review

Use before construction starts when reviewing an architecture document.

**Specific architectural elements:**
- Is the overall program organization clear, with an architectural overview and justification?
- Are major building blocks well defined—responsibility and interfaces to others?
- Are all functions in the requirements covered sensibly, with neither too many nor too few building blocks?
- Are the most critical classes described and justified?
- Is the data design described and justified?
- Is the database organization and content specified?
- Are key business rules identified, with their impact on the system described?
- Is a strategy for user-interface design described?
- Is the user interface modularized so changes to it won't ripple into the rest of the program?
- Is a strategy for handling I/O described and justified?
- Are resource-use estimates and a management strategy described for scarce resources?
- Are the architecture's security requirements described?
- Does the architecture set space and speed budgets per class, subsystem, or functionality area?
- Does it describe how scalability will be achieved?
- Does it address interoperability?
- Is a strategy for internationalization and localization described?
- Is a coherent error-handling strategy provided?
- Is the approach to fault tolerance defined, where any is needed?
- Has technical feasibility of all parts been established?
- Is an approach to overengineering specified?
- Are necessary buy-vs.-build decisions included?
- Does it describe how reused code will be made to conform to the other architectural objectives?
- Is the architecture designed to accommodate likely changes?

**General architectural quality:**
- Does the architecture account for all the requirements?
- Is any part over- or under-architected, and are expectations set explicitly?
- Does the whole hang together conceptually?
- Is the top-level design independent of machine and language?
- Are motivations for all major decisions provided?
- Would you, as the programmer who implements it, be comfortable with this architecture?
