# Checklists

Review checklists for construction-level code, drawn from McConnell, _Code Complete_, 2nd ed. Use them when reviewing a diff or routine—the tactics in `SKILL.md` say how to write code well; these say what to check before merging.

## Valid reasons to create a routine

*Source: ch. 7.* Use when deciding whether to extract code into its own routine, or verifying that an existing one earns its place. A routine should serve at least one of these:

- Reduce complexity—hide deep nesting or a complicated algorithm.
- Introduce an intermediate, understandable abstraction.
- Avoid duplicate code.
- Support subclassing—smaller overrides, less duplication.
- Hide a sequence—the order in which events must be processed.
- Hide pointer operations.
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

## Control-structure issues

*Source: ch. 19.* For any routine with non-trivial control flow.

- Do expressions use `true`/`false` rather than `1`/`0`?
- Are boolean values tested implicitly, not compared explicitly to `true`/`false`?
- Are numeric values compared to test values explicitly?
- Are expressions simplified by adding new boolean variables, functions, or decision tables?
- Are boolean expressions stated positively?
- Do pairs of braces balance?
- Are braces used everywhere they aid clarity?
- Are logical expressions fully parenthesized?
- Are tests written in number-line order?
- In languages with reference equality (e.g. Java), do tests use `a.equals(b)` rather than `a == b` where appropriate?
- Are null statements made obvious?
- Are nested statements simplified—by retesting, `if`/`else`, `case`, extracting a routine, or polymorphism?
- If a routine's decision count exceeds ~10, is there a good reason not to redesign it?

## Loops

*Source: ch. 16.* For any routine containing loops.

- Is `while` used instead of `for` where flexibility was needed?
- Was the loop created from the inside out?
- Is the loop entered only from the top?
- Is initialization placed directly before the loop?
- Is an infinite loop constructed cleanly (`while (true)`) rather than via `for i = 1 to 9999`?
- Is the `for`-loop header reserved for loop-control code only?
- Is the loop body wrapped in braces?
- Is the loop body non-empty?
- Is housekeeping grouped at the start or end of the loop?
- Does the loop perform only one function?
- Is the loop short enough to view at once?
- Is the loop nested no more than three levels deep?
- Has a long loop body been extracted into a routine?
- Is a long loop especially clear?
- Does the code refrain from monkeying with the `for`-loop index?
- Are important loop-index values saved to a variable rather than read after the loop?
- Is the loop index an ordinal or enum, never floating-point?
- Does the loop index have a meaningful name?
- Does the loop avoid index cross-talk between nested loops?
- Does the loop end under all possible conditions?
- Are safety counters used, if standard for the project?
- Is the termination condition obvious?
- Are `break` and `continue` used correctly?

## Defensive programming

*Source: ch. 8.* For a routine, class, or subsystem before merging or releasing.

**General:**
- Does the routine protect itself from bad input data?
- Do assertions document assumptions, including pre- and postconditions?
- Are assertions used only for conditions that should never occur—not for expected errors?
- Does the architecture or high-level design specify a set of error-handling techniques?
- Does it specify whether error handling favors robustness or correctness?
- Have barricades been created to contain the damage from errors?
- Are debugging aids used, and installed so they can be activated or deactivated without fuss?
- Is the amount of defensive code appropriate—neither too much nor too little?
- Have offensive-programming techniques been used to make errors hard to overlook during development?

**Exceptions:**
- Has the project defined a standardized approach to exception handling?
- Have alternatives to exceptions been considered?
- Is the error handled locally rather than thrown nonlocally, where possible?
- Does the code avoid throwing exceptions in constructors and destructors?
- Are exceptions at the right level of abstraction for the routines that throw them?
- Does each exception carry the relevant background information?
- Is the code free of empty catch blocks—or, where empty truly fits, is that documented?

**Security:**
- Does input-checking guard against buffer overflows, SQL injection, HTML injection, integer overflows, and the like?
- Are all error-return codes checked?
- Are all exceptions caught?
- Do error messages avoid revealing information that would help an attacker?

## Fundamental data types

*Source: ch. 12.* For any routine that manipulates primitive data.

**Numbers:**
- Does the code avoid magic numbers?
- Does it anticipate divide-by-zero?
- Are type conversions obvious?
- Does it avoid mixed-type comparisons, and compile with no warnings?

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
- Do tests check for invalid values, with the first entry reserved as invalid?

**Constants:**
- Are named constants used in data declarations and loop limits rather than literals, and used consistently—never mixed with bare literals?

**Arrays:**
- Are indexes within bounds and free of off-by-one errors?
- Is subscript order correct in multidimensional arrays, with the right index variable in each nested loop?

**Custom types:**
- Is a custom type used for each kind of data that might change?
- Are type names oriented to real-world entities rather than redefining predefined types?
- Has a class been considered instead?

## General considerations in using data

*Source: ch. 10.* For variable hygiene in a routine or class.

**Initialization:**
- Does each routine check input parameters for validity?
- Are variables declared close to where they're first used?
- Are variables initialized as they're declared, where possible?
- Where declare-and-initialize isn't possible, are variables initialized close to first use?
- Are counters and accumulators initialized properly and reset each time they're reused?
- Are variables reinitialized properly in code that runs repeatedly?
- Does the code compile with no warnings, with all available warnings turned on?
- If the language allows implicit declarations, have their hazards been compensated for?

**Other usage:**
- Do all variables have the smallest scope possible?
- Are references to a variable kept as close together as possible?
- Do control structures match the data types?
- Are all declared variables actually used?
- Are variables bound at appropriate times—balancing the flexibility of late binding against its complexity?
- Does each variable have one and only one purpose?
- Is each variable's meaning explicit, with no hidden meanings?
