---
name: debug
description: Structured debugging discipline for hard bugs. Use when encountering any bug, test failure, or unexpected behavior before proposing fixes.
metadata:
  author: npalladium
  version: "1.0.0"
---

# Debug

Each phase gates the next — **do not proceed** until the current phase is complete.

**Iron law:** NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.

When a phase stalls—no hypotheses in Phase 4 or a fix that won't hold in Phase 6—see [`checklists.md`](checklists.md) for targeted debugging reminders.

## Phase 1 — Build a feedback loop

**This is the skill.** A fast, deterministic, agent-runnable pass/fail signal for the bug is everything. Without one, nothing else works. Spend disproportionate effort here.

### Approaches — try roughly in this order

1. **Failing test** — unit, integration, or e2e.
2. **Curl / HTTP script** against a dev server.
3. **CLI invocation** — fixture input, diff stdout against known-good.
4. **Headless browser** (Playwright / Puppeteer).
5. **Replay a captured trace** — saved request/payload/event log replayed in isolation.
6. **Throwaway harness** — minimal system subset, single call.
7. **Property / fuzz loop** — 1000 random inputs, find the failure mode.
8. **Bisection harness** — automate `git bisect run` between two known states.
9. **Differential loop** — old vs new version, diff outputs.
10. **HITL script** — last resort; structured script driving human actions.

### Refine the loop

Make it faster (cache setup, narrow scope), sharper (assert on the specific symptom, not just "didn't crash"), and more deterministic (pin time, seed RNG, freeze network).

Where available, check the diagnostic against known-good and known-bad controls; verify it distinguishes them for the expected reason.

**Reduce the reproducer.** Remove irrelevant input, setup, and steps; automate reduction when manual shrinking stalls. Accept a candidate only if it preserves the specific failure—not merely any error. Bound each trial and isolate parallel trials. A smaller case is not better if it makes the failure harder to reproduce.

### Intermittent bugs

Measure failures and trial counts under controlled conditions. Increase reproduction rate where possible with repetition, stress, or narrower timing windows. Compare before and after under the same conditions; a few passing runs do not establish a fix. Recheck after instrumentation changes, which may hide or alter the failure.

### If you cannot build a loop

Stop. List what you tried. Ask the user for: environment access, a captured artifact (HAR, logs, core dump), or permission to add production instrumentation. **Do not proceed without a loop.**

## Phase 2 — Reproduce

Run the loop. Confirm it produces the failure the **user** described, not a nearby one; capture the exact symptom and measure its frequency. Verify the request or action reaches the intended process, device, endpoint, and code path, and confirm the running artifact/version and active configuration. Check for stale builds, unrestarted processes, wrong URLs, and caches. **Do not proceed until the target and symptom are confirmed.**

## Phase 3 — Investigate and localize

Gather evidence before proposing a cause:

- **Read error messages carefully.** Inspect full stack traces, paths, codes, and surrounding normal behavior—not just the first apparent error.
- **Compare working and failing cases.** Check relevant code/artifact versions, runtime mode, configuration, data, dependencies, services, permissions, OS/architecture, and timing. Compare actual values and identities, not intended setup.
- **Trace data flow backward.** Follow the bad value or state toward its origin; distinguish where the error appears from where it was triggered.
- **Bracket the divergence.** Find a checkpoint where the relevant state is correct and a later one where it is wrong; probe between them to locate where it first changes. Use bisection when outcomes support it, preserving interacting conditions rather than assuming a single culprit.
- **For multi-component systems,** instrument boundaries (inputs, outputs, and configuration propagation) and run once to locate where behavior diverges.
- **Use two-ended reduction.** When a passing case exists, simplify the failure and extend the working case toward it while preserving their contrasting outcomes. The smallest contrasting pair can expose a missing condition.

Localization narrows where or under what conditions the failure occurs; it does not establish why. A reduced input, suspect component, or bisected location is evidence, not a causal explanation.

## Phase 4 — Hypothesise

Generate **3–5 ranked, falsifiable causal hypotheses** before testing causes. Each predicts an observation: "If X is the cause, changing Y makes the bug disappear; changing Z makes it worse." No prediction → discard it. Localization probes in Phase 3 may precede this phase; do not mistake localization for causal confirmation.

For complex bugs (multi-component, cross-cutting, unclear cause), **show the list to the user**—they often re-rank or rule out hypotheses. For simple bugs, test directly and report.

**Do not fix until a causal hypothesis is supported.**

## Phase 5 — Instrument and test

Test one causal prediction per probe; change one variable at a time. Prefer debugger/REPL over logs; prefer targeted logs over "log everything." Tag debug artifacts with a unique prefix (`[DEBUG-a4f2]`) for cleanup. After instrumentation, verify the original failure still reproduces.

Inspect the value actually consumed or produced; independently recomputing it in a log or probe can misrepresent the executed path.

**Perf bugs:** measure first (timing harness, profiler, query plan), then bisect. Logs mislead.

**Do not proceed until a causal hypothesis is confirmed** (or all are ruled out → return to Phase 3).

## Phase 6 — Fix + regression test

**3-fix rule:** 3+ failed fixes → STOP. This is architectural, not a simple bug. Discuss with the user.

Otherwise: (1) write a regression test at a **correct seam** before fixing and watch it fail; if no seam exists, document that exception; (2) fix; (3) if a regression test was added, watch it pass; (4) re-run the Phase 1 loop.

**Do not proceed until the regression test passes (or the absent seam is documented) and the Phase 1 loop confirms the fix.**

## Phase 7 — Cleanup + post-mortem

- [ ] Phase 1 loop passes (no repro).
- [ ] Regression test passes (or absent seam documented).
- [ ] All `[DEBUG-...]` artifacts removed (grep the prefix).
- [ ] Throwaway harnesses deleted.
- [ ] Correct hypothesis stated in commit/PR message.
- [ ] **Prevention gap:** identify whether a stronger design invariant, assertion/automated test, or diagnostic would have prevented, detected, or explained the defect sooner. Recommend the specific change after the fix.

## Red flags — STOP, return to earliest incomplete phase

- Proposing fixes before tracing data flow.
- "Quick fix now, investigate later."
- "Just try changing X and see."
- Multiple changes at once.
- "It's probably X" without a falsifiable prediction.
- "I don't fully understand but this might work."
- "One more fix" after 2+ failed attempts.
