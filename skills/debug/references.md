# References

- Steve McConnell, _Code Complete_, 2nd ed., ch. 23—debugging reminders and defect-finding techniques.
- Laurence Tratt, [Test-case Reducers Are Underappreciated Debugging Tools](https://tratt.net/laurie/blog/2026/test_case_reducers_are_underappreciated_debugging_tools.html)—input reduction and precise interestingness tests.
- Nicole Tietz, [A systematic approach to debugging](https://ntietz.com/blog/how-i-debug-2023/)—symptom definition, reproduction, system understanding, localization, and iterative hypotheses.
- Ryan Bigg, [Debugging Checklist](https://ryanbigg.com/2024/07/debugging-checklist)—sanity-checking the actual target, runtime, and environment.
- Scraggo, [Debugging Strategies Checklist](https://www.scraggo.com/debugging-strategies-checklist/)—systematic narrowing, isolation, and observing actual values rather than duplicated calculations.
- Henrik Warne, [Lessons From 9 More Years of Tricky Bugs](https://henrikwarne.com/2025/06/15/lessons-from-9-more-years-of-tricky-bugs/)—two-ended comparison and stale-target mistakes.
- thoughtbot, [It works on my machine. Why?](https://thoughtbot.com/blog/it-works-on-my-machine-why)—comparing relevant working and failing environment dimensions.
- The Art of Machinery, [Debugging Software Deployments with strace](https://theartofmachinery.com/2019/11/14/deployment_debugging_strace.html)—OS-level tracing for deployment failures.
- Russ Cox, [Hash-Based Bisect Debugging in Compilers and Runtimes](https://research.swtch.com/bisect)—bisection and reduction over interacting candidate locations.
- Gray, [A Taxonomy of Bugs](https://ruby0x1.github.io/machinery_blog_archive/post/a-taxonomy-of-bugs/index.html)—testing diagnostic hypotheses rather than relying on bug labels.
- Keunwoo Lee, [On rebooting: the unreasonable effectiveness of turning computers off and on again](https://keunwoo.com/notes/rebooting/)—restart-based recovery, reset scope, cascading restart failures, and preserving forensic evidence before recovery.
- Tom Limoncelli, [A list of dumb (and not so dumb) things to check](https://everythingsysadmin.com/dumb-things-to-check.html)—diagnostic controls, text and filesystem representation checks, and failure periodicity.
