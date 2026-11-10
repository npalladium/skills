# Repository Scout References

Sources for the shared operating contract and all seven scout skills. Local guidance is a synthesis, not wholesale adoption of any source's rules.

The scout concept is inspired in part by PostHog. Ryan Prior's Nyquist article also motivates keeping understanding current as software changes.

- [PostHog: What is a Scout?](https://posthog.com/blog/what-is-a-scout) and [What's in a scout](https://posthog.com/docs/self-driving/whats-in-a-scout)—skill plus thin harness, disqualifiers, stable deduplication, restrained signals, and durable scratchpads. The stateless/stateful separation here is a local design; analytics noise thresholds are not security-finding thresholds.
- [Cisco Foundry Constitution](https://github.com/CiscoDevNet/foundry-security-spec/blob/main/constitution.md), I, II, VII–X—evidence, triage, impact, stable identities, runtime isolation, and operator authority. Its fleet, queue, provider-rate, and autonomous-work policies are not adopted.
- [Trail of Bits: Your AI agent fixed the bug, but what else did it change?](https://blog.trailofbits.com/2026/10/21/your-ai-agent-fixed-the-bug-but-what-else-did-it-change/)—concise opinionated guidance, invariant-guided repair, regression checks, and independent replay. The [skills repository](https://github.com/trailofbits/skills), including [Differential Review](https://github.com/trailofbits/skills/tree/main/plugins/differential-review), informed change-path review. These scouts do not require their schemas or discard static findings for lack of a live proof.
- [Ryan Prior: Nyquist Limit](https://www.ryanprior.com/posts/nyquist-limit-frequency-of-changes-in-tech-invalidates-understanding/)—the warning that software change can outrun understanding; motivates reconstructing and revalidating knowledge rather than relying on stale interpretations. Used as inspiration, not a mathematical sampling law or fixed review cadence.

The six focused lenses and context/intent/loop/goal distinctions are local synthesis, not taxonomies prescribed by these sources.
