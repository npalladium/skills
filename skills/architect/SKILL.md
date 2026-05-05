---
name: architecture-documentation
description: Analyze, review, or document codebase architecture. Triggers on requests about design quality, module structure, abstractions, seams, or architecture documentation (READMEs, ADRs). Not for general coding, bug fixes, or feature implementation.
---

# Request shape

Identify which mode(s) the request involves:

- **Review** — "is this well-designed?", "is this a good abstraction?"
- **Document** — "write a README/ADR for this"
- **Decide** — "should I extract this?", "where should the seam go?"

Most requests mix these. Be explicit about which mode each section of your response is in.

# Load order

Always load `vocabulary.md` first. Every architectural claim must use these terms.

| Mode | Also load | Produce |
|------|-----------|---------|
| Review | `design.md` | Inline review (template in design.md §Output) |
| Document | `documentation.md` | Architecture README or ADR (templates in documentation.md) |
| Decide | `design.md` | Recommendation grounded in named principles |

# Constraints (all modes)

- Do not substitute vocabulary terms (*API, boundary, service, component, signature*) unless genuinely using their reserved sense.
- Do not make claims without citing a named principle (deletion test, adapter count, depth, test surface, etc.).
- Do not describe depth as a property of implementations — it is a property of the interface.
- Do not recommend new seams unless two adapters exist or are imminent. If only one adapter exists, soften: "if a second adapter appears, this is where the seam should go."

# When the user pushes back on the vocabulary

If they want a translation, do it — note the analysis was done in this vocabulary. If they reject it entirely, drop it. The vocabulary is a tool for thinking clearly, not a loyalty test.
