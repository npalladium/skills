# Checklists

Quick calibration aids for documentation and design work. Each names its source so the rationale is auditable.

## Detail level per requirement

*Source: Wiegers, _Software Requirements_, 3rd ed., ch. 11.*

When writing a requirement, spec, or design doc, calibrate how much detail to commit to paper.

**Write more detail when:**
- The customer is an external client.
- Development or testing is outsourced.
- The team is distributed.
- System testing will be driven from the requirements.
- Accurate estimates are needed.
- Traceability is required.

**Write less detail when:**
- The customer is internal.
- The customer is involved throughout.
- Developers are domain experts.
- Precedents exist to follow.
- A package solution supplies the behavior.

**The calibration test:** if a developer can think of several acceptable ways to satisfy a requirement and you'd be content with any of them, the detail level is right. If only one implementation would satisfy you, you've under-specified; if every plausible implementation would be wrong, you've over-specified.

## Pick the right diagram for the information

*Source: Wiegers, _Software Requirements_, 3rd ed., ch. 12—modernised for current tooling (C4, BPMN, sequence/journey diagrams).*

Match the modeling technique to what you're trying to convey. Each kind of information has a notation that shows it most cheaply; reaching for the wrong one buries the point.

- **System external interfaces** → context diagram (C4 system-context), use case diagram, ecosystem map.
- **System decomposition** → C4 container/component diagram.
- **Business process flow** → swimlane or BPMN diagram (detailed), activity diagram, flowchart.
- **Runtime interaction across components** → sequence diagram.
- **Data relationships** → entity-relationship diagram or UML class diagram, plus a data dictionary or schema definition.
- **System or object states** → state-transition diagram with a state table; event-response table.
- **Complex logic** → decision table or decision tree.
- **User interfaces** → screen-flow map (high-level), wireframe or interactive prototype (detailed).
- **User tasks** → user story, use case, journey map, or swimlane—whichever the audience reads fastest.
- **Quality attributes and constraints** → Planguage-style structured text with explicit fit criteria.

## Vision-and-scope document sections

*Source: Wiegers, _Software Requirements_, 3rd ed., ch. 5.*

A vision-and-scope document frames *why* and *how much* before any requirements are written—it's the forward-looking complement to a Design Doc. Cover these sections; omit one only deliberately, noting why.

**1. Business requirements**
- **Background**—the history that led to building this product.
- **Business opportunity**—the problem solved or market addressed.
- **Business objectives**—quantitative, measurable benefits.
- **Success metrics**—indicators tracked during and after release.
- **Vision statement**—long-term purpose and intent.
- **Business risks**—what could go wrong (market, timing, adoption).
- **Business assumptions and dependencies.**

**2. Scope and limitations**
- **Major features**—top-level capabilities, each uniquely labeled.
- **Scope of the initial release.**
- **Scope of subsequent releases.**
- **Limitations and exclusions**—what is explicitly out.

**3. Business context**
- **Stakeholder profiles**—categories, the value each gets, attitudes, constraints.
- **Project priorities**—the five dimensions held in tension: features, quality, schedule, cost, staff.
- **Deployment considerations.**

## Architecture review

*Source: McConnell, _Code Complete_, 2nd ed., ch. 3.*

Run before construction starts, against an architecture document.

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

## Design in construction

*Source: McConnell, _Code Complete_, 2nd ed., ch. 5. Overlaps `code-design`—use this for the class/module level before construction.*

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
