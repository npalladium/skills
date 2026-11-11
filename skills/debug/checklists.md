# Debugging reminders

A reference list for when the investigation stalls. It complements the phase discipline in [`SKILL.md`](SKILL.md); apply only the techniques relevant to the failure.

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

## Specialized probes

- For failures involving filesystems, permissions, processes, or sockets, consider tracing OS interactions. Follow the failing system call back to the application behavior that caused it; protect traces, which may contain sensitive data, and recheck reproduction because tracing can alter timing.
- For a large candidate set, use automated bisection or reduction only when each trial has a reliable, specific failure predicate. Detect flaky outcomes; a failing candidate may require interacting locations, so do not discard them independently without evidence.
- Treat labels such as race, compiler, or dependency bug as hypotheses, not diagnoses. Test a discriminating mechanism; a setting that makes the symptom disappear is not proof of cause.
- For inexplicable text or parsing differences, inspect escaped or byte-level input: Unicode whitespace, line endings, trailing characters, and the final newline.
- For filesystem discrepancies, verify the resolved path, object type, symlink target, mount, and permissions—not just the displayed filename.
- For recurring failures, record timestamps and time since startup or the preceding event; compare intervals with expirations, scheduled jobs, and protocol timers. Treat alignment as a hypothesis to test.
