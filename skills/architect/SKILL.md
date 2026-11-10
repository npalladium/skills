---
name: architect
description: Analyze, review, or document codebase architecture. Triggers on requests about design quality, module structure, abstractions, seams, or architecture documentation (READMEs, ADRs, design docs). Not for general coding, bug fixes, or feature implementation.
---

# Load on demand

Read only the files needed for the request:

| Need | Load |
|------|------|
| Review design quality or decide where a seam belongs | [design.md](design.md) |
| Write an architecture README, ADR, or Design Doc | Relevant template in [documentation.md](documentation.md) |
| Define architecture terms, distinguish reserved meanings, or compare named patterns | Relevant entries in [vocabulary.md](vocabulary.md) |
| Calibrate requirement detail or frame vision and scope | [requirements.md](requirements.md) |
| Choose a diagram | [diagrams.md](diagrams.md) |
| Review an architecture document before construction | [architecture-review.md](architecture-review.md) |
| Review class/module design before construction | [module-review.md](module-review.md) |

Load only relevant files or sections, not every guide or source credits.

A Design Doc is forward-looking—design before building; the README and ADR cover existing structure.

# Constraints

- Do not substitute vocabulary terms (*API, boundary, service, signature*) unless genuinely using their reserved sense. Before using these terms, consult their definitions in `vocabulary.md`.
- Do not measure depth by implementation size—judge it as leverage at the interface.
- Introduce a substitution interface only when multiple adapters exist or are imminent. A single implementation can justify module extraction through depth, invariant ownership, or a useful test surface; it does not by itself justify interchangeable implementations.

# When the user pushes back on terminology

Translate it. If they reject the vocabulary entirely, drop it; terminology is a tool, not a loyalty test.

# Related skills

Use `security-design` for security guarantees and authorization; `code-design` for intra-module tactics; `commit` for staging and history hygiene.

Source attribution: [references.md](references.md) (load only when sources or provenance matter).
