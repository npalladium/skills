# Security Design References

Sources for security design and compatibility review. Local guidance is a synthesis, not wholesale adoption of any source's rules.

## Primary foundations

- [Kat Traxler: Security Constitution](https://github.com/KatTraxler/security-constitution/blob/main/phase%202%20-%20security%20constitution/security-constitution.md)—primary influence on authority-path analysis and false-positive calibration:
  - Principles 1–12: boundary enforcement, caller-attributed audit, human/workload privilege, customer governance, tenant isolation, exposed internal protocols, credential lifecycle, cross-domain authorization, secret custody, connectivity, and enforcement ownership.
  - Anchors A1–A13: caller/target/delegation bindings; deputy tenancy; network position and owner allowlists; credential authority; namespace, hierarchy, and user-selector provenance; derived data, delayed authority, and identity loss across service hops.
  - Lessons 1–11 and §4: correct principal/domain checks, intended orchestration, recommended configurations as preconditions, expectation-versus-effect contradictions, tracing staged artifacts to usable capability, consent, evidence-chain limits, and demonstrated rather than assumed cross-tenant impact.
  - Adaptation limits: the provider-owned-deputy gate, six-condition graduation framework, cloud severity classes, and execution-only finding threshold are specialized. This skill retains useful static findings with explicit limits, does not exempt all customer-owned identity chains, and does not prescribe fresh authorization where the contract permits durable grants. Generative anchors guide inquiry; they are not demonstrated universal flaw patterns.
- [Saltzer–Schroeder: The Protection of Information in Computer Systems](https://www.cs.virginia.edu/~evans/cs551/saltzer/), I.A.3—economy of mechanism, fail-safe defaults, complete mediation, open design, separation of privilege, least privilege, least common mechanism, and psychological acceptability. Informs enforcement, narrow authority, sharing, and usable controls; departures prompt investigation rather than establish defects.
- [NIST SP 800-160 Vol. 1 Rev. 1: Engineering Trustworthy Secure Systems](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-160v1r1.pdf), Appendix E—commensurate protection and rigor (E.3, E.5–E.6); composition and independent protection (E.7, E.9–E.10); bounded trust and exposure (E.13–E.17, E.20, E.27, E.29); continuous protection, failure, and recovery (E.8, E.23–E.24); detection and response (E.1, E.4); mediated access and trustworthy control (E.19, E.30); abstractions, defaults, and complexity (E.2, E.22, E.25, E.28). Section 2.3 and Appendix F distinguish trust from justified trustworthiness. The principles are reasoning aids, not a compliance checklist.
- [OWASP Developer Guide: Security Principles](https://github.com/OWASP/DevGuide/blob/main/docs/en/02-foundations/03-security-principles.md)—application-security design, protective defaults, least privilege, defense in depth, and manageable controls. Its [Secure Product Design Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Product_Design_Cheat_Sheet.html) expands the practical design guidance.

## Supporting guidance

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)—authorization design and enforcement guidance.
- [OWASP ASVS](https://asvs.owasp.org/)—select applicable, versioned application-verification requirements; this skill does not replace them.
- [OWASP FIASSE: Securability](https://github.com/OWASP/FIASSE/blob/main/skills/securability/SKILL.md)—canonical input handling, server-authoritative integrity, boundary control, and maintainability. Informs representation agreement and authoritative-data questions.
- [GitLab: Authorization Code Review Guidelines](https://docs.gitlab.com/development/permissions/review_guidelines/)—resource-specific enforcement, policy decision dependencies, and testing real authorization rather than stubbing the permission under test. Its role definitions, Ruby helpers, and framework conventions are not general requirements.
- [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org/)—iterative, fit-for-purpose analysis and useful outcomes rather than a mandatory notation or methodology.
- [Plumatic: A Few Testing Principles](https://github.com/plumatic/eng-practices/blob/master/clojure/20130821-testing-principles.md)—earlier influence on regression protection, refactoring confidence, and testing at meaningful boundaries; not a requirement to test trivial forwarding or adopt its mocking examples.

## Review influences

- [Trail of Bits: Your AI agent fixed the bug, but what else did it change?](https://blog.trailofbits.com/2026/10/21/your-ai-agent-fixed-the-bug-but-what-else-did-it-change/)—concise opinionated guidance, invariant-guided repair, regression checks, and independent replay. The [skills repository](https://github.com/trailofbits/skills), including [Differential Review](https://github.com/trailofbits/skills/tree/main/plugins/differential-review), informed change-path review. This skill does not require their schemas or discard static findings for lack of a live proof.

The compatibility dimensions and attributes are local synthesis, not a taxonomy prescribed by these sources.
