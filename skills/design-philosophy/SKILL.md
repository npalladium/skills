---
name: design-philosophy
description: Mindset and dispositions for software design — what good design optimizes for, the economics of complexity and change, when (and when not) to abstract, and how to reason about boundaries and your own fallibility. Use when making a design judgment call, weighing a tradeoff, doing architecture review, or deciding whether/how much to invest in structure. For concrete tactics (naming, control flow, state modeling) see the `code-design` skill.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Design Philosophy

These are dispositions, not rules — the stance you bring to a design decision before any specific tactic applies. They mostly trade off against each other; judgment is choosing which dominates *here*. Expect competent designers to disagree based on which they weight most.

## The prime directive: understandability

You cannot reliably make code correct, secure, or fast if you can't understand it. So optimize first for the next person — often future-you, exhausted, paged at 2am — being able to read, change, and *delete* it.

- In review, "how does this work?" is a defect signal. Refactor to self-explanatory code instead of explaining it.
- Re-read your own code after a few days away to surface confusion you couldn't see while writing.
- If experts can't follow it, blame the design, not the reader.

## Code is a liability, not an asset

Less code means less to read, test, and break. The product is the asset; the code is the cost of having it.

- **Design for deletion.** Minimize dependencies, keep clear boundaries, so any piece is easy to remove or replace. A good design lets you change your mind without changing much code.
- **Make yourself deletable.** Don't become the irreplaceable expert; structure and document so others can own it.

## Design = ease of change

Judge a design by how cheaply you can change your mind later, not by how clever or complete it is.

- No current pain → don't refactor. Ease-of-change is the only durable measure; everything else is proxy.
- Manage state deliberately and early — poor state handling is a primary driver of change-cost.

## Complexity decays, and has no ceiling

- **Good design is an unstable equilibrium.** Entropy is the default; easy-to-change systems degrade into hard-to-change ones unless you spend energy resisting it. Budget for continuous maintenance, not one-time correctness.
- **You mostly work inside already-degraded systems** — that's the normal case, not a failure. Most successful systems are badly designed systems.
- **There is no natural ceiling on complexity** in a multi-developer system. Complexity breeds and hides bugs. Include only as many moving parts as the requirements force; fewer interacting components means fewer failure modes.

## No surprises

Every defect is an unmet expectation. Align behavior with what users (and callers) expect; prefer consistent over clever.

When qualities conflict, this priority order resolves it — never sacrifice a higher tier for a lower one:

> **data integrity > reliability > security > usability > performance**

- Protect data integrity above all — loss/corruption is irreversible trust damage.
- Make it reliable *before* you make it fast; speed on an unstable base just surfaces failures sooner.

## Code for humans, not machines

- "Theoretically faster" and "more elegant" lose to "obvious." Favor explicit over terse — a plain loop beats a dense functional chain for most readers.
- Measure performance empirically before optimizing; "should be faster" is frequently slower in practice.
- Optimize code for *use* (callers grasp and extend it without studying internals), not for admiring.

## On abstraction — the disposition

The concrete "when to extract" tactics live in `code-design`; the mindset:

- **Duplication is cheaper than the wrong abstraction.** A premature abstraction couples things that will later diverge, and the cost of unwinding it exceeds the cost of the duplication it removed.
- **DRY is about knowledge, not text.** Two pieces of code that look alike but encode *different* decisions are not duplication — keep them separate.
- **Premature design is not design.** Let usage drive structure; co-evolve design with implementation. Defer decisions until evidence demands them.
- **Match the game you're playing.** Shipping a product to validate with users rewards "the dumbest thing that works" + a conscious plan to refactor on success. A library/framework meant to last rewards investment in elegance. Decide which; don't straddle.
- **Justify every abstraction by ROI** (error prevention, real reuse), never by elegance or "just in case." Expect what you unify to diverge — design for divergence rather than scattering conditionals later.

## On boundaries and interfaces — the disposition

- **Do one thing well.** "Don't generalize — generalizations are generally wrong. If in doubt, leave it out." An interface should capture an abstraction's minimum essentials.
- **Don't hide power.** A low-level abstraction must never conceal capability its clients legitimately need.
- **Think in M+N, not M×N.** When many producers meet many consumers, the design win is a shared interface in the middle so integration cost grows additively, not multiplicatively. (Concrete form: narrow waists, in `code-design`.)
- **Get it right** — neither abstraction nor simplicity substitutes for correctness. But "plan to throw one away; you will anyhow."

## Trust nothing, including yourself

- **Assume every non-trivial abstraction leaks.** Learn at least one layer beneath the tools you rely on.
- **Verify assumptions by trying to disprove them**, not by watching tests pass. Hunt unknown-unknowns deliberately when adopting new tech.
- **Heuristics are fallible aids, not laws.** When one fails, regroup and try another; always leave yourself a retreat option — don't paint yourself into a corner. Learn *why and when* a heuristic works, not just the rule.

---

## Sources

Distilled from links in the user's `software-engineering.org` notes.

[Three Laws of Software Complexity](https://maheshba.bitbucket.io/blog/2024/05/08/2024-ThreeLaws.html) · [Four principles of software engineering](https://drewdevault.com/2020/10/09/Four-principles-of-software-engineering.html) · [No Surprises: a framework for Software Quality](https://abdulapopoola.com/2021/09/22/no-surprises-a-framework-for-software-quality/) · [The most important goal is understandability](https://ntietz.com/blog/the-most-important-goal-in-designing-software-is-understandability/) · [My Engineering Axioms](https://martinrue.com/my-engineering-axioms/) · [My guiding principles after 20 years](https://alexewerlof.medium.com/my-guiding-principles-after-20-years-of-programming-a087dc55596c) · [Good code is rarely read](https://www.alexmolas.com/2024/06/06/good-code.html) · [Write code you can understand when paged at 2am](https://www.pcloadletter.dev/blog/clever-code/) · [Premature Design Is Not Design](https://articles.pragdave.me/p/premature-design-is-not-design) · [Abstractions Are The Best, Abstractions Are The Worst](https://mbuffett.com/posts/abstractions-best-worst/) · [The 80% abstraction](https://engineering.gusto.com/the-80-abstraction-d8ab95ab22f3) · [Ugly Code and Dumb Things](https://lucumr.pocoo.org/2025/2/20/ugly-code/) · [Everything is an X](https://lukeplant.me.uk/blog/posts/everything-is-an-x-pattern/) · [Hints for Computer System Design (Lampson)](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-17.pdf) · [Programmers Should Never Trust Anyone, Not Even Themselves](https://carbon-steel.github.io/jekyll/update/2024/06/19/abstractions.html) · [Growing Your Personal Design Heuristics Toolkit](https://rebecca.wirfs-brock.com/blog/2019/03/21/growing-your-personal-design-heuristics/) · [When Is a Detail Just a Detail?](https://oblac.rs/when-is-a-details-just-a-detail/)
