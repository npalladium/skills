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

Make it faster (cache setup, narrow scope), sharper (assert on specific symptom, not "didn't crash"), more deterministic (pin time, seed RNG, freeze network). A 2-second deterministic loop is a superpower; a 30-second flaky loop barely counts.

### Non-deterministic bugs

Goal: raise reproduction rate. Loop 100x, parallelise, add stress, narrow timing windows. A 50% flake is debuggable; 1% is not.

### If you cannot build a loop

Stop. List what you tried. Ask the user for: environment access, a captured artifact (HAR, logs, core dump), or permission to add production instrumentation. **Do not proceed without a loop.**

## Phase 2 — Reproduce

Run the loop. Confirm: (1) it produces the failure the **user** described, not a nearby one; (2) it's reproducible across runs; (3) you've captured the exact symptom for later verification. **Do not proceed until reproduced.**

## Phase 3 — Investigate root cause

**Complete at least one step below before hypothesising.**

- **Read error messages.** Full stack traces — line numbers, file paths, error codes. They often contain the answer.
- **Check recent changes.** Git diff, recent commits, new deps, config changes, env differences.
- **Trace data flow backward.** Don't fix where the error appears. Trace up the call chain: what called this with a bad value? Keep going until you find the **original trigger**. Fix at source.
- **Multi-component systems.** Instrument each component boundary (log data in/out, verify config propagation) and run once to locate WHERE it breaks, before proposing fixes.

## Phase 4 — Hypothesise

Generate **3–5 ranked, falsifiable hypotheses** before testing any. Each states a prediction: "If X is the cause, then changing Y makes the bug disappear / changing Z makes it worse." No prediction → discard it.

For complex bugs (multi-component, cross-cutting, unclear root cause): **show the list to the user** — they often re-rank or rule out instantly. For simple bugs, test directly and report.

**Do not proceed without falsifiable hypotheses.**

## Phase 5 — Instrument and test

One probe per prediction. **One variable at a time.** Prefer debugger/REPL over logs; prefer targeted logs over "log everything." Tag all debug artifacts with a unique prefix (`[DEBUG-a4f2]`) for easy cleanup.

**Perf bugs:** measure first (timing harness, profiler, query plan), then bisect. Logs mislead.

**Do not proceed until a hypothesis is confirmed** (or all ruled out → return to Phase 3).

## Phase 6 — Fix + regression test

**3-fix rule:** 3+ failed fixes → STOP. This is architectural, not a simple bug. Discuss with the user.

Otherwise: (1) write regression test at a **correct seam** before fixing (if no seam exists, document it); (2) watch it fail; (3) fix; (4) watch it pass; (5) re-run Phase 1 loop.

**Do not proceed until the test passes and the Phase 1 loop confirms the fix.**

## Phase 7 — Cleanup + post-mortem

- [ ] Phase 1 loop passes (no repro).
- [ ] Regression test passes (or absent seam documented).
- [ ] All `[DEBUG-...]` artifacts removed (grep the prefix).
- [ ] Throwaway harnesses deleted.
- [ ] Correct hypothesis stated in commit/PR message.
- [ ] **Defense in depth:** validation added at other layers the bad data passes through?

**Then ask: what would have prevented this bug?** Recommend architectural changes after the fix is in.

## Red flags — STOP, return to earliest incomplete phase

- Proposing fixes before tracing data flow.
- "Quick fix now, investigate later."
- "Just try changing X and see."
- Multiple changes at once.
- "It's probably X" without a falsifiable prediction.
- "I don't fully understand but this might work."
- "One more fix" after 2+ failed attempts.
