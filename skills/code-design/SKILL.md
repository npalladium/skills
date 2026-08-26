---
name: code-design
description: Direct heuristics and tips for structuring code—naming, state/data modeling, control flow, coupling, abstraction tactics, readability, defensiveness, and project hygiene. Use when writing non-trivial new code, refactoring, reviewing a diff, or deciding how to structure a module/API/data model. Not for throwaway scripts, prototypes, or one-off code that won't be maintained.
metadata:
  author: npalladium
  version: "2.7.0"
---

# Code Design

Lookup-style tactics, ordered broad→narrow: project → module → state → function → naming → documentation. Jump to the scope of the decision; ⭐ marks the highest-leverage heuristics. Apply with judgment, not dogma.

For reviewing existing code against construction-level checklists (function or method extraction, control flow, loops, defensiveness, primitive data types, variable use), see `checklists.md`.

*Related: the `architect` skill for system/architecture-level design and design docs; the `commit` skill for staging, commit messages, and history hygiene.*

## Default workflow

1. Define constraints and state invariants.
2. Set module boundaries and isolate effects.
3. Shape functions; then name and document the design.
4. Review relevant construction concerns in `checklists.md`.

## Project-level tactics

### Hygiene

- ⭐ **Always-green main:** a fixed check set that always passes; zero flaky tests (delete ones you can't trust).
- ⭐ **Make every rule deterministic.** Express rules as automated checks—tests, linters, architecture/dependency tests (fitness functions)—not prose. If your taste can be a test, make it one.
- **Split tests fast vs slow:** seconds (local+CI) vs minutes (CI only); run benchmarks as tests so they don't rot.
- **One-command reproducible build**, few entry points (a lint is a test).
- **What you can't automate, codify** into a style guide.

### Build it early

- **CI, automated deploys, test framework** from day one.
- **Plan compatibility from day one for protocols/APIs/formats that are durable or independently evolving.** Version—or otherwise make compatible—serialized data that crosses a deployment boundary or outlives a process, especially at boundaries you don't control.
- **Observability:** wide canonical logs, state-change logs, "critical" location logs at the minimum.
- **Paginate every list endpoint from day one**—even if it currently returns one item—for consistent response shapes across endpoints and so future growth does not change the API.

### Operability & bounds

- **Debuggable systems.**
  - Every error carries safe context (operation, relevant state) and a reference ID; exclude sensitive data (credentials, PII, regulated data, etc.).
  - Ship toggleable production diagnostics, not dev-only logging.
  - Make test failures diagnosable without a rerun.
  - Write error messages for the reader—minimize cognitive load and provide context: show an example, a likely fix, or a hint, not just what failed.
- **Bound everything.** Anything that can grow, wait, or consume resources needs a deliberate limit. Examples: `LIMIT` queries; cap updates/deletes/queues; rate-limit actions. For wire data, bound bytes or cardinality and provide pagination, streaming, or chunking when valid data can exceed the limit. Isolate downstream failure with timeouts, circuit breakers, and bulkheads.

## Module-level tactics

### Coupling & cohesion

Make module connections small, direct, visible, and flexible (easy to substitute).

- ⭐ **Coupling severity ladder** (worst→best). Slide each call site down toward Message; fewer args = looser coupling, and pass only the fields needed, not a whole object. Kill control coupling with intent-named methods (`save`/`save_without_validations`, not `save(false)`).
  - **Pathological**—reach into another module's internals / monkey-patch.
  - **Global**—shared mutable singleton.
  - **Control**—a flag drives the callee's branching.
  - **Data**—params, no control flow.
  - **Message**—no args; relies only on the interface name.
- **Trade strong coupling for connascence-of-name.** Swapping shared execution-order / value / algorithm coupling for agreement on a name (magic value → named constant, positional → named args) wins; also cut how many parts change together, and keep them close.
- **Locality of Behaviour.** Understand a unit by reading it alone: keep related code close, surface call sites, hide only implementation. Avoid action-at-a-distance; trades against DRY/SoC—cross-file violations hurt most.
  - **No train-wreck chains** (`a.getX().getY().doZ()`)—they couple the caller to a navigation path.
  - **Keep logic with its data**—same method if possible, else same object, at least same package. *Feature envy* (a method using another object's data more than its own) means the logic belongs on that other object.
- **One purpose per concept.** Keep concepts independent (types/actions don't cross-reference); coordinate via explicit boundary syncs. A misplaced boundary shows two ways:
  - *Divergent change*—one unit changes for two unrelated reasons → split it.
  - *Shotgun surgery*—one change forces little edits across many units → group them.
- **Let the declared type state the contract.** Return the interface that expresses exactly what you promise, no more—the declared type tells the reader as much as the implementation does.
  - `Iterable`/`Collection`—you only guarantee elements (hides implementation; discourages order/mutation assumptions).
  - `List`—order is part of the contract.
  - `Set`—uniqueness.
  - `Map`—keyed access.
  - `Optional`—at most one; may be absent.
  - `Stream`/lazy iterator—produced lazily, possibly large; consume once.

### Boundaries & effect isolation

- ⭐ **Functional core, imperative shell.** Push I/O, clock, randomness to a thin shell; keep the core pure—deterministic, tests with plain values.
  - **Examples:** for a hard-to-test class (UI renderer, event-loop handler, broker callback), extract logic into a tested object and leave a thin untested wiring shell.
- **Separate decision from action** (triangle of separation, *the same split within one function*): compute *what* to do apart from doing it; push branching to the edges so the core runs straight-line and tests alone.
- **Prefer pure functions, then small impure ones, then objects.** Maximize pure functions; next, small (1–4 param) functions that touch the outside world; only then domain objects wrapping them.
- **Testability through seams, not ceremony.** Reserve DI for real I/O boundaries (clock, network, DB) where a fake swaps in; the pure core needs none.
  - **Mock only at the unmanaged edge.** Asserting on mock calls (communication-based testing) is the least-preferred style: reserve it for outgoing commands to unmanaged out-of-process dependencies (message bus, third-party API). Don't mock in-process collaborators—test them through observable behaviour.

### Abstraction

- **Proximity over premature abstraction.** Small duplication, or copies modeling *different* concepts → place them side by side, not behind a flag-laden shared abstraction.
- **A good abstraction has few conditionals**—groups similarities, separates differences. A condition-laden procedure interleaving unrelated ideas wants breaking up.
- **Don't wrap working third-party code "just in case"**—an untested wrapper is pure friction.
- **Layer convenience over power.** Convenience overloads call through to one canonical (fullest) implementation. Separate the layers by namespace when the low-level interface is complex; co-locate when easy fallback matters more.

## State & data tactics

- ⭐ **Make illegal states unrepresentable (MISU).** N booleans = 2^N states, few legal—use an enum/tagged union so only valid states compile (and extend cleanly). The general move: push correctness into types so the compiler rejects bad states, not runtime checks—
  - **Domain types over primitives.** `Money`/`EmailAddress`, not `double`/`string`—a validated type can't hold garbage; "stringly-typed" fields scatter validation. (Skip universal primitives: counts, indices.)
  - ⭐ **Parse, don't validate.** Convert untrusted input to trusted *typed* values once at the boundary; the core never re-checks. Prefer total functions + explicit errors over partial functions + exceptions.
  - **Programmatic, not semantic, interfaces.** What the compiler can't enforce gets misused—encode assumptions ("array must be sorted", "call within a transaction") as types/asserts/builders, not comments.
- **Multi-status entities → explicit state machine.** Enumerate states *and* legal transitions so illegal moves can't be invoked—MISU for the moves, not just the states. The transition table is the spec.
- **That boolean is probably something else.** Flips once on an event → store the timestamp (`deleted_at`, not `deleted`: keep *when*, not just *whether*); mutually-exclusive flags → one status enum; bare booleans only for short-lived local predicates, not persisted data.
- **Avoid boolean blindness.** A bare `bool` loses which proposition was true—prefer named enums/variants.
- **Zero-one-infinity.** Avoid arbitrary small maxima; preserve exactly-one domain invariants and otherwise model zero or many, with operational bounds.
- **Prefer meaningless IDs.** Synthetic opaque keys over fact-encoding IDs—encoded meaning eventually lies. Keep mutable attributes as separate fields; never overload the identifier.
- **Decide symmetry on purpose.** For same-typed relations, ask if `F(x,y) ⇒ F(y,x)`. Symmetric many-to-many → join table, not array fields (silent violations). Antisymmetric predicates (invites) are fragile—enumerate simultaneous/duplicate/merge cases up front.
- **Prefer immutability.** `const`/`final`/frozen for values stable after construction; shared mutable state changes under other callers. Limit mutability to where you need it.
- **Construct fully-formed objects.** A constructor returns a ready-to-use, valid object—no separate `init()` to forget. Provide one constructor per coherent input set, each funnelling to a single primary constructor that does the initialization.

## Function tactics

### Control flow

Control flow is a tree of possible states; each condition prunes branches—prune as early as possible so less code runs on fewer paths.

- ⭐ **Four-block function shape:**
  - **Declare**—gather inputs, state the contract.
  - **Validate**—preconditions; fail before acting.
  - **Process**—core logic.
  - **Commit & respond.**
- ⭐ **Guard clauses for special cases; `else` for core branches.** Bail early on exceptional/short cases to de-indent the main path (each dropped `else` removes a nesting level); keep `else` when both arms are core responsibility—don't scatter related conditional logic.
- ⭐ **Command-Query Separation.** A function returns a value *or* changes state, not both—unless the dual role *is* the contract, then name it so (`get_or_create`, `ensure_*`).
- **`if/else if` with no final `else`** silently falls through on a new case—add an explicit final `else`/`default`/assert.
- **Prefer declarative over imperative.** For parts that are simple facts with no sequence or conditionals, state them declaratively.
  - **Branching:** turn a hardcoded `if`/`switch` cascade into a lookup table / transition map the code interprets—easier to extend, audit, test; pairs with the state-machine table.
- **Anti-if patterns:**
  - **Boolean params** (`create(true, false)` is opaque at the call site) → two named methods, or a named enum/variant.
  - **Type switches:** use exhaustive matching for closed variants; use polymorphism or dispatch for open ones.
  - **Model absence explicitly.** Translate external nulls at boundaries; use `Option`/`Optional`, empty collections, or domain results only when their meanings fit.
- **Push *decision* ifs up; *data-validation* ifs down.** They only seem to conflict:
  - **Decision branches** ("which thing to do") → hoist to the caller: control flow in one place, dead branches exposed, inner functions do one unconditional thing.
  - **Data/error branches** (null/parse/status-code) → push down to where data enters, so callers don't re-implement checks.
  - **Reconciliation:** the source parses raw input to a precise type *once* (push down); everyone above branches on clean *meaning* (push up). Distinguish failure modes by type (`FetchById` throws vs `FetchByIdOptional` returns null), not one ambiguous `null`.
- **Push `for`s down.** Operate on whole batches inside a function, not one element per call—amortizes per-call cost, enables vectorization. Hoist loop-invariant conditions out of hot loops.

### Readability

- **One level of abstraction per function.** Keep all statements at one altitude—don't mix high-level orchestration with low-level detail; extract the detail behind a named call.
- **Lower per-function load:** fewer variables/operators, scopes declared near use, no shadowing, shallow nesting. Break long chains into named intermediates.
- **Name complex conditionals.** Pull a multi-clause condition behind a name—a **variable** for a one-off (local clarity), a **predicate function** when it recurs or deserves its own test. Don't mix `&&` and `||` in one conditional—parenthesize or extract.
- **No arbitrary line caps.** Manage the *distribution*: many short functions, a few long ones.

### Defensiveness

Reliable software has three separately-improvable parts: the functionality, assertions that it works, and introspection to debug it.

- **Scale defensiveness to context.** Internal, testable code → minimal guards; critical systems / untrusted data / hard-to-patch deploys → heavy boundary validation. Complements, never replaces, tests.
- **Never silently swallow anomalies.** Surface unexpected conditions to monitoring; choose the failure mode (nil / default / correct) deliberately and consistently.
- **Make failures visible; choose production behavior by risk.** Fail fast in development. In production, degrade only when correctness and security remain intact; otherwise fail closed and surface the error.
- **Guarantee cleanup (RAII/`defer`/try-finally).** Bind release to scope exit, not manual close on every path—an early return, exception, or new branch can't skip it.

## Naming tactics

Naming is lossy compression: you squeeze precise behavior into imprecise language. Most issues are one of two kinds: the name isn't *quite right* for the intent, or it drops something *important*.

### Intent

- ⭐ **Name by intent**—what a thing *is* or does, ideally *why* it's done, never *how*. `filename` not `file`; match a count's name to what it counts—`MAX_ATTEMPTS` and `MAX_RETRIES` differ by one.
- ⭐ **Use the domain's ubiquitous language.** One agreed term per concept, used identically in code, schema, UI, docs, and conversation—mirror the word domain experts actually say. No programmer-coined synonyms or aliases (if the business says "policy," don't model it as `agreement`); when the domain term shifts, rename everywhere.
- **A good name has clear boundaries**—you can say what it *excludes*. Stress-test edges: is a draft a `post`? a soft-deleted one? "What if there's more than one?"

### Consistency

- **Consistency across boundaries beats local cleverness.** Same concept, same name up and down the stack—don't translate at boundaries (`street_name` stays `street_name`). Minimize the "name stack".
- **Related operations get parallel names; opposites complementary ones.** `add`/`remove`, `encrypt`/`decrypt`, `open`/`close`.

### Form

- **Descriptiveness by blast radius.** Cheap-to-rename internals (vars, functions) → descriptive; hard-to-change external names (services, products) → distinctive/memorable is fine. A name growing long is a split smell.
- **Greppability is a real metric.** Never build identifiers dynamically (`obj["set"+name]`, `method_missing`)—write the literal so grep finds every use; un-findable refs get assumed dead and break on edit. Dynamic *declaration* sometimes OK; dynamic *invocation* almost never.
- **Encode role and units.** Role: `sourcePort`, `retryCount`. Units: `timeoutMs`, `distanceMeters`.
- **Name booleans positively.** `is`/`has` prefixes; `found`, not `notFound` (`if (!notFound)` is unreadable).
- **Noun-first for variants** (`ButtonPrimary`, not `PrimaryButton`) so related items cluster alphabetically.
- **Generic buckets, precisely.** `utils`/`common` are fine for domain-agnostic code (http/date/string wrappers); they're low-cohesion magnets only once *domain* logic lands there. `helpers` = codebase-specific orphans; growth signals a missing home.

## Documentation

### Comments

A comment must earn its cognitive cost: if reading it is as much work as reading the code, drop it. Comment what the code *can't* communicate easily.

- **Comment the why and the context, not the what.** Explain *why* the code does this, plus the global context local code can't show (what a called service does, the tradeoff behind an algorithm). Don't restate the code.
- **Kinds worth writing:**
  - **Why**—the reason behind code whose mechanics are already clear.
    - *Chesterton's Fence*—when something looks removable or "fixable" but is load-bearing, say why it must stay; especially for third-party code or action-at-a-distance.
  - **Design**—why this algorithm/technique/tradeoff over another.
  - **Teacher**—teach the domain the reader may lack (math, networking, graphics), or point to where to learn it.
  - **Checklist**—"change this → also update X over there," when a concept can't be centralized in one place.
  - **Debt**—record a known shortcut/limitation inline.
  - (Plus ordinary API/function docs. Skip *trivial* comments—if they cost more to read than the code, they're noise.)
- **Comment hygiene.** Don't duplicate the code; don't use a comment to excuse unclear code (fix the code first); if you can't write a clear comment, the code may be wrong; dispel confusion, don't add it.
- **Capture what code can't encode:** negative info ("we do NOT do X, because Y"), warnings ("delicate—talk to X before changing"), non-guarantees ("idempotent today, but don't rely on it"), and unidiomatic/optimized stretches.
- **For a very complex function, sketch the idealized happy path** in a comment above the error/edge/logging overhead, so the intent reads fast.
- **Standardize placard markers:** `FIXME` (broken, deferred), `TODO` (maybe-later), `HACK`/`WORKAROUND`.
- **Explain every inline lint-disable**—say why the rule doesn't apply here, not just that it's silenced.
