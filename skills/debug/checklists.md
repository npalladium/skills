# Debugging reminders

*Source: McConnell, _Code Complete_, 2nd ed., ch. 23.*

A reference list for when you're stuck on a defect, or before claiming a fix is done. It complements the phase discipline in `SKILL.md`—reach for it when a phase stalls (no hypotheses in Phase 4, a fix that won't hold in Phase 6) rather than as a substitute for the phases.

## Finding the defect

- Use all the available data to form your hypothesis.
- Refine the test cases that produce the error.
- Exercise the code in your unit test suite.
- Use the available tools.
- Reproduce the error several different ways.
- Generate more data to generate more hypotheses.
- Use the results of negative tests.
- Brainstorm for possible hypotheses.
- Keep a notepad by your desk, and list things to try.
- Narrow the suspicious region of the code.
- Be suspicious of classes and routines that have had defects before.
- Check code that's changed recently.
- Expand the suspicious region of the code when narrowing fails.
- Integrate incrementally.
- Check for common defects.
- Talk to someone else about the problem.
- Take a break from the problem.
- Set a maximum time for quick-and-dirty debugging before switching to a systematic approach.
- Make a list of brute-force techniques, and use them.

## Fixing the defect

- Understand the problem before you fix it.
- Understand the program, not just the problem.
- Confirm the defect diagnosis.
- Relax.
- Save the original source code.
- Fix the problem, not the symptom.
- Change the code only for good reason.
- Make one change at a time.
- Check your fix.
- Add a unit test that exposes the defect.
- Look for similar defects elsewhere.

## General approach

- Do you use debugging as an opportunity to learn?
- Do you avoid the trial-and-error, superstitious approach?
- Do you assume errors are your fault?
- Do you use the scientific method to stabilize intermittent errors?
- Do you use the scientific method to find defects?
- Do you use several different techniques rather than relying on one?
- Do you verify that the fix is correct?
- Do you use compiler warnings, profiling, a test framework, scaffolding, and a debugger?
