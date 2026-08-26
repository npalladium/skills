---
name: code-design
description: Direct heuristics and tips for structuring code — naming, state/data modeling, control flow, coupling, abstraction tactics, readability, defensiveness, and project hygiene. Use when writing non-trivial new code, refactoring, reviewing a diff, or deciding how to structure a module/API/data model. For the underlying mindset and tradeoff reasoning, see the `design-philosophy` skill.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Code Design

Concrete, lookup-style tactics. Each is "do X **when** …, **unless** …" — apply with judgment, not dogma. The *why* behind them lives in the `design-philosophy` skill; this is the *how*.

## Model data and state precisely

- **Make illegal states unrepresentable.** N booleans imply 2^N states but few are legal — use an enum / tagged union so only valid states compile. Adding a state to a boolean is an error-prone retrofit; an enum extends cleanly.
- **That boolean is probably something else.** A flag that flips once on an event → store the **timestamp** (`deleted_at`, not `deleted`); keep *when*, not just *whether*. Mutually-exclusive flags → one status enum. Reserve bare booleans for short-lived local predicates, not persisted data.
- **Avoid boolean blindness.** A bare `bool` discards which proposition was true; boolean *parameters* are opaque at the call site (`create(true, false)`). Prefer named enums/variants.
- **Zero-one-infinity.** Reject "exactly one" assumptions — allow none or unbounded many; avoid arbitrary cardinality caps. When something goes from one to two, jump straight to arbitrary-many. Attach roles/labels/primary-flags to the multiples rather than a fixed slot.
- **Prefer meaningless IDs.** Synthetic opaque keys over IDs that encode facts — encoded meaning eventually lies. Keep mutable attributes as separate, updatable fields; never overload the identifier.
- **Decide symmetry on purpose.** For relations between same-typed entities, ask whether `F(x,y) ⇒ F(y,x)`. Model symmetric many-to-many in a join table, not array fields (which make violations silent). Treat antisymmetric predicates (invites) as fragile — enumerate simultaneous/duplicate/merge edge cases up front.
- **Parse, don't validate.** Convert untrusted input into trusted *typed* values once at the boundary, so the core never re-checks. Prefer total functions and explicit error values over partial functions + exceptions.

## Naming

- **Name by intent / what a thing *is*** — not how it works, not what it might hold. `filename`, not `file`, for a string. `MAX_ATTEMPTS`, not `MAX_RETRIES` (off-by-one trap). Avoid generic names that hide behavior (`close()`), and metasyntactic/vague names (`foo`, bare `id`).
- **Define a name by what it excludes.** Test borderline cases against the boundary. Ask "what if there's more than one?"
- **Consistency across boundaries beats local cleverness.** Use the *same* name for the same concept up and down the stack; don't translate at boundaries (`street_name` stays `street_name`, not `streetName`). Minimize the "name stack." Name files after their primary export.
- **Greppability is a real metric.** Never construct identifiers dynamically (`obj["set"+name]`, `method_missing`, dotted-key concatenation) — write the full literal so grep finds every use. Un-findable references get wrongly assumed dead and break on edit. Dynamic *declaration* is occasionally OK; dynamic *invocation* almost never.
- **Pick descriptiveness by blast radius.** Cheap-to-rename + internal (variables, functions) → descriptive. Hard-to-change + externally-facing (services, products) → a distinctive/memorable name is fine. Let a name growing long be a smell that the unit wants splitting.
- **Noun-first for variants** (`ButtonPrimary`, not `PrimaryButton`) so related items cluster alphabetically.

## Control flow

- **Guard clauses over `else`.** Handle exceptional/short cases first and return early; de-indent the main path. Each removed `else` removes a nesting level.
- **Beware `if/else if` with no final `else`** — it silently falls through when a new case appears. Add an explicit final `else`/`default`/assert so unhandled states are caught.
- **Anti-if patterns:** split a behavior-altering boolean param into two named methods; replace type-switches with polymorphism (so new types *must* implement the branch); design nulls out (return empty collections / `Optional` / Null Object).
- **Push *decision* ifs up; push *data-validation* ifs down.** These two famous heuristics only seem to conflict:
  - *Business-logic / "which thing to do" branches* → hoist toward the caller. Concentrates control flow in one place, exposes dead/redundant branches, keeps inner functions doing one unconditional thing.
  - *Data-shape / error-handling branches (null/not-found/parse/status-code)* → push down to where data **enters** the system, so every caller doesn't re-implement defensive checks.
  - **Reconciliation:** the data source converts ambiguous raw input into a precise type *once* (push down + parse-don't-validate); everyone above then branches on clean *meaning*, not raw mess (push up). Distinguish failure modes by type (`FetchById` throws vs `FetchByIdOptional` returns null) rather than collapsing into one ambiguous `null`.
- **Push `for`s down.** Operate on whole batches/slices inside a function rather than one element per call — amortizes per-call cost, enables vectorization. Hoist loop-invariant conditions out of hot loops.

## Coupling, boundaries, locality

- **Minimize coupling along the severity ladder** (worst → best): Pathological (reaching into internals / monkey-patching) → Global (shared mutable singleton) → Control (passing a flag that drives the callee's branching) → Data (params that don't alter control flow) → Message (call with no args). Slide each call site as far down as you can; fewer arguments = looser coupling. Kill control coupling by splitting into intent-named methods (`save` / `save_without_validations`, not `save(false)`).
- **Connascence = Strength × Degree / Locality.** Reduce strength (magic values → named constants → named params), reduce degree (fewer entities that must change together), increase locality (keep things that change together close). Trading high-strength coupling for more low-strength connascence-of-name is a net win.
- **Locality of Behaviour.** A unit's behavior should be understandable from inspecting that unit alone. Keep related code physically close; surface call sites (visible invocation), hide only implementation. Avoid action-at-a-distance. LoB trades against DRY/SoC — cross-file violations hurt more than local ones.
- **Narrow waist.** When M producers meet N consumers, define one shared interface/protocol in the middle so glue is O(M+N), not O(M×N). Prefer universal substrates (byte streams, plain text) — you get an ecosystem of tools for free. Reimplementing cross-cutting concerns (auth, metrics, deploy) per-service/per-language is the O(M×N) smell of a missing waist.
- **Separate decision from action** ("triangle of separation"): compute *what* to do (calculation/branching) apart from *how/doing* it; push branching to the edges so the core runs straight-line and is independently testable.
- **One purpose per concept.** Keep concepts independent (their types/actions don't reference each other); coordinate only through explicit synchronizations at the boundary. An overloaded concept serving multiple purposes is the smell.

## Abstraction tactics

(Mindset in `design-philosophy`; the mechanics:)

- **Wait for ~3 instances** before extracting. Two aren't a pattern.
- **To find the right abstraction, inline the wrong one** back into its call sites and let the recurring shape reveal itself.
- **A good abstraction has fewer conditionals**, groups similarities, separates differences. A bad one is a condition-laden procedure interleaving vaguely related ideas — that's the signal to break it up.
- **Don't wrap working third-party code "just in case."** An abstraction with no test coverage behind it (e.g. DI) is pure friction.

## Readability

- **Keep functions doing one thing at one level of abstraction.** Don't impose arbitrary line caps; instead manage the *distribution* — many short functions, a bounded longest one. Size by intention, not by rule.
- **Lower per-function load:** fewer variables/operators, shorter scopes (declare near use), no name shadowing, shallow nesting (extract deep blocks). Break long chains/comprehensions into named intermediates.
- **Avoid novelty.** Reuse the codebase's established patterns and idioms rather than inventing new syntactic forms. Don't mix `&&` and `||` in one conditional.
- **The grep test & the 2am test:** can you find every use by grepping the literal token? Can exhausted-you understand it during an incident? If not, simplify.

## Defensiveness and operability

- **Scale defensiveness to context.** Confident, testable, internal code → minimal guards. Critical systems, third-party/untrusted data, hard-to-patch deployments → heavy boundary validation. Defensive code complements but never replaces a test suite.
- **Never silently swallow anomalies.** The worst thing you can do is hide a real problem — log/surface unexpected conditions to monitoring. Choose the failure mode (return nil / default / correct) deliberately and consistently.
- **Build debuggable systems.** Every error carries operation context: what was attempted, with which inputs, what result — plus a referenceable id pointing to logs/docs. Production needs toggleable diagnostics, not dev-only logging. Start with test-failure messages rich enough to diagnose without rerunning.
- **Bound everything.** `LIMIT` on queries; cap updates/deletes/queue sizes; rate-limit user actions; set aggressive connect/read timeouts on every client; circuit-breakers/bulkheads so downstream failure degrades gracefully. Give data a retention policy.

## Build-it-early exceptions (PAGNIs)

YAGNI is the default, but a few things cost vastly more to add later — build them from day one:

- **Versioning** into protocols/APIs/formats, especially across boundaries you don't control; and a forced-upgrade/kill-switch for deployed clients.
- **Observability:** logging, request/response capture, `created_at` + state-change timestamps on every entity.
- **Pagination** on every list endpoint (even single-result ones).
- **CI, automated deploys, a test framework** from project start — they set the quality norms.
- Default to a **relational DB** (Postgres, incl. JSON columns) unless you have a concrete reason not to.

## Project hygiene (starting/scaling a codebase)

- Keep the main branch **always green**: a well-defined set of checks that pass at all times; zero tolerance for flaky tests (delete checks you can't trust).
- Split tests **fast (seconds, local + CI) vs slow (minutes, CI only)**; make benchmarks runnable as tests so they don't rot.
- One-command, reproducible build with a fixed number of entry points (a lint is a kind of test).
- **Inverse triangle inequality:** split big PRs/refactors into smaller steps each safer than the whole.
- Codify recurring review feedback into a style guide owned by one person; assign (don't just request) reviewers.

## Red flags — stop and reconsider

- Abstracting on the second occurrence (or before any).
- A persisted boolean that will gain a third state.
- A function with a boolean/flag parameter that switches its behavior.
- `null` overloaded to mean both "absent" and "error."
- Dynamically constructed identifiers; renamed concepts across the stack.
- `if/else if` chains with no final `else`.
- Swallowed exceptions / empty catch blocks.
- Reaching into another object's internals; passing a big object to use one field.
- "Let's just…" framing (hides complexity); wrapping working third-party code "just in case."
- Reimplementing auth/metrics/deploy per service (missing narrow waist).

---

## Sources

Distilled from links in the user's `software-engineering.org` notes.

**Data & state modeling:** [Make illegal states unrepresentable](https://www.pathsensitive.com/2018/02/making-bugs-impossible-illustrating.html) · [That boolean should probably be something else](https://ntietz.com/blog/that-boolean-should-probably-be-something-else/) · [Boolean Blindness](https://runtimeverification.com/blog/code-smell-boolean-blindness) · [Don't use booleans](https://www.luu.io/posts/dont-use-booleans) · [Never, Sometimes, Always](https://lukeplant.me.uk/blog/posts/never-sometimes-always/) · [There can't be only one](https://www.b-list.org/weblog/2024/aug/27/highlander-problem/) · [Power of Meaningless IDs](https://utcc.utoronto.ca/~cks/space/blog/tech/PowerOfMeaninglessIDs) · [Symmetric Properties](https://buttondown.email/hillelwayne/archive/symmetric-properties/) · [Practices of Reliable Software Design](https://two-wrongs.com/practices-of-reliable-software-design)

**Naming:** [How To Name Things](https://www.swyx.io/how-to-name-things) · ["Naming Things" is a Poor Name](https://buttondown.email/hillelwayne/archive/naming-things-is-a-poor-name-for-naming-things/) · [Giving Names to Things](https://buttondown.email/hillelwayne/archive/giving-names-to-things/) · [Cute vs descriptive names](https://ntietz.com/blog/when-to-use-cute-names-or-descriptive-names/) · [Prefer Noun-Adjective Naming](https://kyleshevlin.com/prefer-noun-adjective-naming/) · [Greppability is an underrated code metric](https://morizbuesing.com/blog/greppability-code-metric/)

**Control flow:** [Anti-If: The missing patterns](https://code.joejag.com/2016/anti-if-the-missing-patterns.html) · [Else Nuances](https://testing.googleblog.com/2023/09/else-nuances.html) · [When `else`s Make Your Code Worse](https://kyleshevlin.com/when-elses-make-your-code-worse/) · [Don't push ifs up, put them close to the data](https://gieseanw.wordpress.com/2024/06/24/dont-push-ifs-up-put-them-as-close-to-the-source-of-data-as-possible/) · [Push Ifs Up And Fors Down](https://matklad.github.io/2023/11/15/push-ifs-up-and-fors-down.html)

**Coupling & boundaries:** [Types of Coupling](https://thoughtbot.com/blog/types-of-coupling) · [Connascence as a vocabulary for coupling](https://thoughtbot.com/blog/connascence-as-a-vocabulary-to-discuss-coupling) · [Locality of Behaviour](https://htmx.org/essays/locality-of-behaviour/) · [Narrow Waists](https://www.oilshell.org/blog/2022/02/diagrams.html) · [Triangle of Separation](https://thoughtbot.com/blog/triangle-of-separation) · [Concept design in three easy steps](https://newsletter.squishy.computer/p/concept-design-in-three-easy-steps)

**Abstraction tactics:** [Abstractions Are The Best, Abstractions Are The Worst](https://mbuffett.com/posts/abstractions-best-worst/) · [The 80% abstraction](https://engineering.gusto.com/the-80-abstraction-d8ab95ab22f3)

**Readability & heuristics craft:** [What Makes Code Hard To Read](https://seeinglogic.com/posts/visual-readability-patterns/) · [How Long Should Functions Be?](https://tidyfirst.substack.com/p/how-long-should-functions-be) · [Too DRY — The Grep Test](https://jamie-wong.com/2013/07/12/grep-test/) · [Attractive nuisances in software design](https://blog.ganssle.io/articles/2023/01/attractive-nuisances.html) · [Avoid the Long Parameter List](https://testing.googleblog.com/2024/05/avoid-long-parameter-list.html) · [The Aristotelian Approach to Writing Good Programs](https://alonso.network/aristotelian-logic-as-the-foundation-of-code/) · [What do typical design heuristics look like?](https://rebecca.wirfs-brock.com/blog/2019/04/04/what-do-typical-design-heuristics-look/)

**Defensiveness & operability:** [Levels of Defensiveness](https://marcgg.com/blog/2025/01/06/levels-of-defensiveness/) · [Please Create Debuggable Systems](https://naildrivin5.com/blog/2025/08/06/please-create-debuggable-systems.html) · [Anti-Decay Programming](https://www.johnnunemaker.com/anti-decay-programming/) · [Elements of Backend Quality](https://github.com/fpereiro/backendqual) · [How I write backends](https://github.com/fpereiro/backendlore)

**Build-early / hygiene:** [YAGNI exceptions](https://lukeplant.me.uk/blog/posts/yagni-exceptions/) · [PAGNIs](https://simonwillison.net/2021/Jul/1/pagnis/) · [Basic Things](https://matklad.github.io/2024/03/22/basic-things.html) · [Things I Learnt from a Senior Software Engineer](https://neilkakkar.com/things-I-learnt-from-a-senior-dev.html)
