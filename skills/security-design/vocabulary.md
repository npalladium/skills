# Security Engineering Vocabulary

Use these distinctions to make claims precise. Project-specific meanings take precedence; surface conflicts rather than silently resolving them.

| Term | Meaning |
| --- | --- |
| Invariant | A property required to hold over specified operations or observable states. State where and when it must hold; a committed-state invariant need not hold between private transaction steps. |
| Security guarantee | A required protection of confidentiality, integrity, availability, or permitted authority, under stated assumptions. |
| Principal | The identity to which actions and permissions are attributed. Distinguish the initiating principal from an intermediary's effective identity. |
| Authentication / authorization | Authentication establishes identity; authorization determines whether that principal may perform this action on this resource under these conditions. |
| Authority | The ability or permission to access a resource or cause an effect, including through credentials, execution environments, network position, or delegation. |
| Baseline authority | What the caller could already legitimately do without the path under review. An escalation claim must establish authority or effects beyond that baseline. |
| Delegation | A grant allowing another principal or component to exercise authority within specified actions, targets, conditions, and lifetime, including revocation semantics. |
| Trust boundary | A point where assumptions about data, identity, or authority change and must be established for the receiving context. It need not coincide with a process, network, or domain-model boundary. |
| Authoritative source / resource owner | The authoritative source establishes a fact; the resource owner is the principal or domain entitled to control the resource. They need not be the same entity. |
| Trusted / trustworthy | A component is trusted when the guarantee relies on it; it is trustworthy to the extent evidence justifies that reliance for the stated purpose. |

For evidence and claim calibration, see [Calibrate security claims](SKILL.md#calibrate-security-claims); for consumer obligations and change dimensions, see [compatibility vocabulary](../compatibility-review/vocabulary.md).
