---
name: critique-writing
description: Critique and improve writing through constructive feedback and Socratic questioning. Use when user asks for feedback on writing, articles, docs, blog posts, or prose.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Critique Writing

Constructive, Socratic critique. Point out issues and ask probing questions — the user rewrites. Never produce a revised draft unless explicitly asked. Accepts file paths or inline text.

## Process

### 1. Structural read

Read the full piece before commenting on anything. Identify:

- **Core claim/purpose** — state it in one sentence. If you can't, that's the first finding.
- **Audience** — if ambiguous, ask.
- **Type** — technical doc, blog post, essay, tutorial, narrative, etc.

### 2. Information architecture

Treat information as a DAG — pieces depend on other pieces.

- **Dependency ordering.** Are concepts introduced before they're needed? Flag forward references to undefined terms.
- **Section coherence.** One job per section. Flag sections serving two purposes.
- **Dead sections.** "What breaks if we delete this?" Nothing? Candidate for removal.
- **Gaps.** Questions the text raises but never answers.

### 3. Paragraph-level

- **Lead sentence.** Does it state the paragraph's point? A paragraph clear only in its last sentence is backward.
- **Length.** >5 sentences usually means multiple ideas competing. Flag them.
- **Transitions.** If consecutive paragraphs require the reader to infer a logical step, make it explicit or question the ordering.

### 4. Sentence-level

Apply selectively — focus on high-impact issues, not line edits.

- **Filler.** Sentences that don't advance the argument. "It's worth noting that..." — then just note it.
- **Hedging.** "Might", "could potentially", "seems like" — does the author believe this?
- **Ungrounded abstraction.** "Improves performance" — by how much? For whom?
- **Unwarranted jargon.** Terms the audience may not share. Define or simplify.
- **Passive hiding the actor.** "Mistakes were made" — by whom?

### 5. Technical checks

- Code and prose disagree?
- Assumed knowledge unstated?
- Steps reproducible?
- References (versions, APIs, flags) current?

### 6. Prose and argument checks

- Voice consistency — tone shifts without reason?
- Through-line — can you trace premise to conclusion?
- Opening — earns attention or throat-clearing?
- Closing — lands or trails off?

Apply both lenses to every piece.

## Output format

Scale depth to input length. Prioritize impactful issues — don't pad with minor observations.

**Overview:** One paragraph — what works well, single most important thing to fix.

**Structural feedback:** Ordering, sections, architecture.

**Detailed feedback:** Section by section. For each issue: observation, Socratic question, concrete suggestion when the fix isn't obvious. Quote specific passages.

**Questions for the author:** 3–5 probing questions challenging assumptions or surfacing unstated choices.

## Reference

See `references/style-guide.md` for the author's style preferences. Use it as context when critiquing — flag deviations, but the guide is loose, not law.

## Constraints

- Don't rewrite. The user owns the voice.
- Don't praise generically. Be specific about what works and why.
- Don't nitpick grammar/spelling unless it causes ambiguity.
- Don't impose a style guide not asked for.
