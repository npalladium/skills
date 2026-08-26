# Checklists

Review-only checklists for construction-level code, drawn from McConnell, _Code Complete_, 2nd ed. Use after implementation or while reviewing a diff, function, or method; select only relevant sections. `SKILL.md` guides design decisions; this file catches construction concerns before merging. Run the repository's formatter, linter, compiler, and static analysis first; these checklists cover concerns those tools cannot decide.

| Change | Checklist |
| --- | --- |
| Extracting a function or method | Function extraction |
| Branches or loops | Control flow; Loops |
| Errors or input boundaries | Defensiveness |
| Primitive data or variables | Primitive data types; General considerations in using data |
| Language or runtime semantics | Language-specific checks |

## Function extraction

*Source: ch. 7.* Use when deciding whether to extract code into a function or method, or verifying that an existing one earns its place. A function or method should serve at least one of these:

- Reduce complexity—hide deep nesting or a complicated algorithm.
- Introduce an intermediate, understandable abstraction.
- Avoid duplicate code.
- Support subclassing—smaller overrides, less duplication.
- Hide a sequence—the order in which events must be processed.
- Improve portability—wrap nonportable operations.
- Simplify a complicated boolean test behind a well-named function.
- Improve performance—one place to tune, cache, or optimize.
- Isolate complexity.
- Hide implementation details.
- Limit the effects of changes.
- Hide global data.
- Make a central point of control.
- Facilitate reusable code.
- Accomplish a specific refactoring.

## Control flow

*Source: ch. 19.* For any function or method with non-trivial control flow.

- Are boolean values tested implicitly, not compared explicitly to `true`/`false`?
- Are expressions simplified by adding new boolean variables, functions, or decision tables?
- Are boolean expressions stated positively?
- Are tests written in number-line order?
- Are nested statements simplified—by retesting, `if`/`else`, `case`, extracting a function or method, or polymorphism?
- If a function or method's decision count exceeds ~10, is there a good reason not to redesign it?

## Loops

*Source: ch. 16.* For any function or method containing loops.

- Is `while` used instead of `for` where flexibility was needed?
- Was the loop created from the inside out?
- Is the loop entered only from the top?
- Is initialization placed directly before the loop?
- Is housekeeping grouped at the start or end of the loop?
- Does the loop perform only one function?
- Is the loop short enough to view at once?
- Is the loop nested no more than three levels deep?
- Has a long loop body been extracted into a function or method?
- Is a long loop especially clear?
- Is the loop index an ordinal or enum, never floating-point?
- Does the loop index have a meaningful name?
- Does the loop avoid index cross-talk between nested loops?
- Does the loop end under all possible conditions?
- Are safety counters used, if standard for the project?
- Is the termination condition obvious?
- Are `break` and `continue` used correctly?

## Defensiveness

*Source: ch. 8.* For a function, method, class, or subsystem before merging or releasing.

**General:**
- Does the function or method protect itself from bad input data?
- Do assertions document assumptions, including pre- and postconditions?
- Are assertions used only for conditions that should never occur—not for expected errors?
- Does the architecture or high-level design specify a set of error-handling techniques?
- Does it specify whether error handling favors robustness or correctness?
- Have barricades been created to contain the damage from errors?
- Are debugging aids used, and installed so they can be activated or deactivated without fuss?
- Is the amount of defensive code appropriate—neither too much nor too little?
- Have offensive-programming techniques been used to make errors hard to overlook during development?

**Security:**
- Does input-checking guard against buffer overflows, SQL injection, HTML injection, integer overflows, and the like?
- Do error messages avoid revealing information that would help an attacker?

## Primitive data types

*Source: ch. 12.* For any function or method that manipulates primitive data.

**Numbers:**
- Does the code avoid magic numbers?
- Does it anticipate divide-by-zero?
- Are type conversions obvious?
- Does it avoid mixed-type comparisons?

**Integers:**
- Does integer division behave as intended, and is overflow avoided?

**Floating point:**
- Does it avoid mixing values of very different magnitudes?
- Are rounding errors handled systematically?
- Does it avoid equality comparison (`==`) on floats?

**Strings:**
- Does it avoid magic strings and characters, and is it free of off-by-one errors?

**Booleans:**
- Are extra boolean variables used to document and simplify tests?

**Enums:**
- Are enums used instead of named constants or booleans where they're richer?

**Constants:**
- Are named constants used in data declarations and loop limits rather than literals, and used consistently—never mixed with bare literals?

**Arrays:**
- Are indexes within bounds and free of off-by-one errors?
- Is subscript order correct in multidimensional arrays, with the right index variable in each nested loop?

**Custom types:**
- Is a custom type used for each kind of data that might change?
- Are type names oriented to real-world entities rather than redefining predefined types?

## General considerations in using data

*Source: ch. 10.* For variable hygiene in a function, method, or class.

**Initialization:**
- Does each function or method check input parameters for validity?
- Are variables declared close to where they're first used?
- Are variables initialized as they're declared, where possible?
- Where declare-and-initialize isn't possible, are variables initialized close to first use?
- Are counters and accumulators initialized properly and reset each time they're reused?
- Are variables reinitialized properly in code that runs repeatedly?

**Other usage:**
- Do all variables have the smallest scope possible?
- Are references to a variable kept as close together as possible?
- Do control structures match the data types?
- Are variables bound at appropriate times—balancing the flexibility of late binding against its complexity?
- Does each variable have one and only one purpose?
- Is each variable's meaning explicit, with no hidden meanings?

## Language-specific checks

Apply every matching group.

### Exception-based languages (Java, Python, Ruby, C#, etc.)

*Source: ch. 8.*

- Are exceptions handled or translated where recovery or context is available?
- Do exceptions match the API's abstraction and carry safe diagnostic context?
- Are ignored exceptions explicitly justified?

### Explicit result/error languages (C, Go, Rust, etc.)

*Source: ch. 8.*

- Is each error or result handled, propagated, or explicitly discarded?

### Identity/value equality languages (Java, Python, JavaScript, etc.)

*Source: ch. 19.*

- Does each comparison use the intended identity or value semantics?

### C-style imperative loop languages (C, C++, Java, C#, etc.)

*Source: ch. 16.*

- Are intentional infinite loops idiomatic rather than fake-bounded?
- Does each `for` header contain only iteration control, without body mutation of its variable?
- Are post-loop values captured explicitly rather than inferred from the final index?
