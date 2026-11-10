# Skills

<!-- Image: Vilkasss on Pixabay, https://pixabay.com/illustrations/ai-generated-bird-robot-mechanical-9431600/ — found via https://kureansiklopedi.com/en/detay/stochastic-parrot-9b0d7; license and attribution in references.md. -->
<img src="assets/stochastic-parrot.png" alt="A mechanical bird: the stochastic parrot" width="160" align="right">

Skills are how you personalize your coding agents. These are mine.

> “I predict the next token, therefore I reason.”
>
> — René Descartes, probably

> “Give me sustained next-token prediction, and I shall make machines reason about your codebase.”
>
> — Archimedes, probably

> “We choose to do these things not because they are easy, but because conventional tooling cannot economically or conveniently do them.”
>
> — John F. Kennedy, probably

<br clear="right">

## Purpose

Skills for recovering, operationalizing, and proposing software intent:

- **Recovering implied intent.** What does the software seem intended to do? Reconstruct intent from requirements, accepted decisions, code, tests, and operational evidence. Distinguish authoritative requirements from inferred intent; existing behavior is evidence, not automatically the specification.
- **Operationalizing intent.** What follows from these principles? Translate accepted goals, invariants, contracts, and design principles into concrete decisions, checks, and verification.
- **Proposing intent.** What would make the software more coherent? What change is most locally justified? Surface missing decisions, contradictions, and defensible improvements. Keep proposals distinct from accepted requirements.

The aim is to automate more of this reasoning—not to let an agent silently decide what the software ought to become.

## Agentic Engineering Model

Four complementary concerns shape this work. They are not sequential phases:

| Concern | Governing question | Practical obligation |
| --- | --- | --- |
| **Context engineering** | What does the agent know, and how trustworthy is that knowledge? | Supply scoped, current evidence with provenance. Keep observations, interpretations, institutional memory, and authoritative instructions distinct. |
| **Intent engineering** | What is supposed to be true? Which invariants, contracts, and design principles matter? | Identify applicable guarantees and distinguish accepted intent from inferred or proposed intent. |
| **Loop engineering** | How does the system discover mistakes, challenge conclusions, verify changes, and consolidate knowledge? | Separate fresh investigation from historical comparison, seek counterevidence, verify changed behavior, and curate knowledge with revalidation conditions. |
| **Goal engineering** | What are we actually optimizing, and how do we resolve competing objectives? | Make priorities and constraints explicit. Optimize justified improvements and maintained understanding—not change volume, finding counts, or agent agreement. |

### Work this enables

Use codebase reasoning for work that conventional tooling cannot economically or conveniently perform on its own:

- Reconstruct the implicit architecture and conceptual model of a codebase.
- Discover inconsistencies across independently developed components.
- Infer candidate invariants and challenge existing assumptions.
- Compare fresh observations against accumulated institutional knowledge.
- Critically examine the results of previous reasoning.
- Periodically consolidate that understanding into better code, tests, and engineering constraints.

Reasoning produces claims and proposals—not authority or proof. Candidate invariants are not automatically requirements; conclusions must be challenged against evidence and changes verified through observable behavior. Conventional tools remain essential for precise analysis, execution, and verification.

### Production and assimilation

- **Production:** Developers and coding agents introduce changes, new abstractions, and potential inconsistencies.
- **Assimilation:** Tests, reviews, architectural investigations, and engineers establish what those changes mean for the system.

Faster production is not sufficient if the capacity to investigate, integrate, and maintain understanding does not keep pace.

Measured in comparable units of work:

- $\lambda$ is the rate at which work requiring understanding is introduced.
- $\mu$ is the rate at which that work can be adequately investigated and assimilated.

In a simple queueing model, if $\lambda \geq \mu$ over a sustained period, the backlog of unassimilated work can grow without bound. Equality leaves no capacity margin for variability; even when average capacity exceeds arrivals, bursts and uneven workloads can create substantial delays.

This is a framing, not a claim that engineering work is uniform or its rates easily measured. Increase effective assimilation capacity, not merely review or agent-output volume. When understanding cannot keep pace, reducing production or narrowing concurrent work may be necessary.

Assimilation is not acceptance: understanding a change can justify rejecting, reverting, or redesigning it. Consolidation is not repetition: reconcile previous conclusions with fresh evidence rather than treating repeated claims as established knowledge.

## Governed automation

The guiding question:

> Can a human define the constitutions, goals, legitimacy boundaries, and override conditions under which machine systems are allowed to decide?

Here, a **constitution** means the governing commitments that constrain decisions; **legitimacy boundaries** specify which decisions the system is authorized to make.

Delegation should make explicit:

- Which guarantees must be preserved and which objectives may be traded off.
- What the agent may infer, propose, decide, or execute.
- When uncertainty or conflicting obligations require escalation.
- Who can approve exceptions, override decisions, or stop the system—and under what conditions.

Human oversight must remain practicable, not ceremonial: preserve the information, expertise, time, and recovery paths needed to intervene. Automation can move difficult work to exceptional situations and introduce new failure modes; a nominal approval step is not sufficient assurance.

Independent judgment must survive delegation. Intelligence does not protect a process that rewards deference and substitutes slogans or consensus for examining evidence. Preserve room for dissent and correction: authority to approve a decision does not establish that its premises are true.

Investigate interacting technical and organizational conditions, not just the last action or a convenient “human error” explanation. Successful operation can depend on adaptations that a proposed improvement would accidentally remove.

These skills guide reasoning. Permissions, approval gates, and execution limits require enforcement by the surrounding tools and runtime.

## Skill design goals

Roughly in descending order of importance:

1. **Agent consumability.** Make instructions actionable: clear triggers, decision rules, and explicit conditions for loading supporting files.
2. **Token economy.** Keep main files lean. Omit obvious advice, fluff, and repetition; split task-specific material into files loaded only when needed.
3. **Human understanding.** Keep guidance readable and easy to inspect, without turning it into a tutorial.
4. **Credit original sources and inspirations.** Always keep attribution in a separate `references.md`, linked from the relevant skill or README—not in the operational guidance.

**Caveat:** These rules are usually stricter than the guidance I give humans. It is up to the human reviewer to decide whether an agent has applied a rule too dogmatically and whether it should be relaxed for the situation.

## Dependency direction

Normal skills must not reference or require `scout-*` skills. They own reusable domain reasoning, verification, and domain-specific reporting.

Scouts may depend on normal skills and adapt their guidance to scout permissions, context modes, memory, deduplication, and signal reporting—not the reverse.

Keep normal skills as independent as possible; dependencies between them are allowed when genuinely needed. A normal skill must remain complete and usable when every `scout-*` directory is absent.

## Usage

```bash
npx skills@latest add npalladium/skills
```

## Structure

```
skills/
  <skill-name>/
    SKILL.md          # Required: frontmatter + instructions
    references.md     # Optional: source credits and inspirations
    references/       # Optional: supporting docs
    scripts/          # Optional: automation helpers
```

## References

See [references.md](references.md) for repository-wide inspirations.
