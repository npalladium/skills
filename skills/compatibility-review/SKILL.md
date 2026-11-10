---
name: compatibility-review
description: Design, implement, or review changes to contracts and consumer-visible behavior across APIs, SDKs, persisted data, messages, deployment, and rollback. Use when an evolving system may change explicit, implicit, or incidental consumer expectations, including corrective bug fixes. Not a mandate for universal backward compatibility, a security review, or authorization to modify code during a repository scout.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Compatibility Review

Assess whether supported consumers can continue relying on a system as it evolves. Trace meaning and outcomes, not only interface shape. A correct bug fix can break a consumer; a contract violation can exist without a demonstrated consumer failure. Neither statement alone decides whether the change should ship.

## Load the relevant guidance

- Read [`vocabulary.md`](vocabulary.md) for the six independent change dimensions and the independent `basis`, `cause`, and `impact` attributes.
- Select the relevant sections of [`checklists.md`](checklists.md) for implementation, review, and concrete consumer tests.
- For repository scouts, explicitly load [`../scout-repository/SKILL.md`](../scout-repository/SKILL.md), [`../scout-repository/vocabulary.md`](../scout-repository/vocabulary.md), and relevant sections of [`../scout-repository/checklists.md`](../scout-repository/checklists.md). These companion files must be available with this skill family. If any are missing, report the missing dependency rather than improvise scout operation. That shared skill owns the operating and reporting contract; this skill adds compatibility reasoning, not another scout protocol. Scout mode makes no automatic code changes.
- Use [`../security-design/SKILL.md`](../security-design/SKILL.md) for security requirements and authority boundaries. Compatibility does not justify retaining a security defect.

## Workflow

1. **Bound the change and support policy.** Identify the baseline and proposed behavior, supported versions and consumers, deprecation promises, rollout constraints, and exclusions. Locate the authoritative contract and policy sources. Unknown policy is an open decision, not permission to assume either perpetual support or immediate breakage.
2. **Trace old and new behavior end to end.** Follow inputs, defaults, validation, calculations, state transitions, effects, errors, serialization, storage, messages, and consumer interpretation. Include relevant configuration and dependency changes. Write a concrete old/new example; an unchanged type or schema is not proof of unchanged meaning.
3. **Identify supported consumer paths.** Inspect API callers, generated and handwritten SDKs, integrations, batch jobs, persisted artifacts, readers/writers, message producers/consumers, and operators. Identify independently deployed versions, rolling-deployment combinations, and rollback after new writes. State the boundary of visibility when external consumers cannot be inspected.
4. **Assess each expectation separately.** Name the expectation being classified before assigning its source, scope, dimensions, and `basis`, `cause`, and `impact` from the vocabulary. Use the exact attribute labels; put explanations separately. Distinguish a violated obligation from an observable change or actual reliance. One change can restore an explicit contract while disrupting incidental dependence on the previous behavior; never assign the restored contract's `basis` to that incidental expectation.
5. **Try to falsify the concern.** Inspect adapters, feature negotiation, version gates, documented non-guarantees, migrations, tolerant readers, and actual consumer logic. Construct the input, state, or version pairing that could distinguish old and new outcomes. Do not call a consumer affected merely because a field or implementation changed.
6. **Test the known consumer path where authorized.** Use the checklist to choose focused contract and integration tests through the real serialization, persistence, or protocol boundary. Assert consumer decisions and effects, not just successful parsing. In scout mode, runtime permission and static-evidence limits come from the run brief and shared operating contract. Propose unexecuted tests explicitly; do not report them as evidence.
7. **Choose the narrow policy-consistent response.** Depending on actual obligations and impact, preserve behavior, migrate callers, negotiate/version a representation, stage a data migration, deprecate, or document an accepted intentional break. Establish the supported version window and removal conditions before introducing compatibility machinery. Do not add shims or freeze incidental behavior automatically. An authorized implementation must update affected callers, tests, contracts, and deployment instructions together; review alone does not authorize edits.

## Compatibility conclusions

For each material compatibility signal, add to the applicable review/scout format: the old/new behavior; expectation source and scope; dimensions and attributes; the supported consumer/version scenario; evidence of contract violation separately from impact; counterevidence and untested paths; and the narrow policy decision or remediation needed. These are compatibility-specific facts, not a replacement reporting taxonomy.

A passing schema check, green suite, corrective intent, or lack of known failures cannot alone establish compatibility. Keep conclusions scoped to the consumers, versions, artifacts, and outcomes actually inspected.
