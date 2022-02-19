+++
title = "A Lightning Tour of the Google Testing Blog"
date = 2017-04-17
[taxonomies]
categories = ["Google Testing Blog"]
tags = ["testing", "google"]
[extra]
+++

[Google Testing Blog](https://testing.googleblog.com/) has a huge wealth of information for software
engineers looking for resources on testing. Testing culture at Google is one that I really look up
to and I am grateful to the engineers who shared these resources. With that said, keep in mind that
some of the content can be outdated and/or opinionated. Testing is a widespread topic and there can
be different, sometimes conflicting, answers to any given problem.

Below you will find a a categorized list of key concepts I found in the blog with links to the
particular post in which it was found. The idea here is brevity. If an item catches your eye, take a
look at the linked blog post(s) for more info.

I plan to keep this updated as more posts are added.

## Test Reliability

- Smaller tests run faster, are less flakey, and isolate failures. As a general rule of thumb, favor
  a pyramid-shaped composition of 70% small, 20% medium, and 10% large tests.[^11]
- Test binary size and memory usage, including third-party testing tools, have a strong correlation
  on whether a test is flaky.[^18]
- Beware of using UI testing to verify underlying functionality. In these cases, it is cheaper and
  more reliable to have smaller tests that break closer to the source of the problem.[^3] [^4]
- Hermetic servers add speed and reliability into end-to-end tests. A environment is considered
  Hermetic if it can run an entire system under test on a single machine with fake network
  connections and database implementations.[^9]
- Filter out flakey tests by rerunning failing tests. If a test fails three times in a row, consider
  it a real failure.[^13]
- Build testability into the product. For example, a real-time system can rely on a fake clock
  instead of a hardware clock. Processes can spawn other processes attached to a debugger with
  debugging flags.[^15]

## Code Quality

- Releasing often gives teams an incentive to automate testing and reduce coupling with the rest of
  the system.[^17]
- When a team provides [fakes](../tott/#know-your-test-doubles) and writes tests for them, they
  become clients of their own software. Experiencing the perspective of the client gives the team an
  incentive to make their API easier to use.[^17]
- While writing the header first encourages consideration for the interface, writing tests first
  encourages consideration for how the interface will be *used*.[^5]
- Good code quality is taught, not enforced. Create a culture that teaches code quality through code
  review, pair programming, and mentoring.[^15]
- Don't obsess over coverage numbers. Use code coverage reports to identify areas that are not
  covered and human judgement over whether to cover it. For example, frequently changed code should
  be covered.[^19]
- Consider investing in mutation testing, a automated tool that "mutates" code and expects tests to
  fail. Tuned properly, it can help find oversights in tests that are worth fixing. [^20]

## Productivity

- Automation is costly. Automate only the tests that you find yourself running often to reliably
  catch regressions on features with business value.[^3]
- Speed up the feedback loop between test engineers and development engineers. Share the same space,
  tools, daily stand-ups, and design discussions.[^3]
- Effective automation depends on test design. Good test design is built from a solid foundation of
  manual tests.[^6]
- If a test plan isn't worth bothering to update, it isn't worth creating in the first place. A
  quick brainstorming session will suffice.[^8]
- Use formatting tools, like clang-format, to improve readability.[^14]

## Infrastructure
- Remove the detective work of tracking down bad changes by investing in a pre-submit system that
  runs automated tests against the commit before it reaches the depot.[^7] [^13]
- Don't fall behind on updating third party dependencies. Update them quickly by setting up CI
  system with dependencies pinned at head.[^12]
- Avoid making more than one branch by putting risky new changes behind feature flags.[^14]
- Constantly look for opportunities to make the build system faster. Reduce the amount of code being
  compiled, replace tools with faster counterparts, and use distributed build systems.[^16]
- Release early and release often. Services and websites can deploy rapidly. A good target for
  client projects is Chromes six week cycle.[^16]


## Metrics & Logging

- *Pre- vs post-production defect ratio* and a breakdown of *defects by component or functional
  area* help identify holes in test.[^1]
- Premature performance optimization makes bad code. Develop in a clean, maintainable and
  extensible manner *first*, and then let benchmarks drive performance optimizations.[^2]
- Remove unwanted noise by logging with conditional verbosity. Log all levels to a logging queue.
  If a transaction completes successfully, discard the unimportant levels.[^10]
- Use two sets of logging levels, one for production builds and one for development builds.[^10]
- Trace the time spent on every significant processing step. Measuring is the only way to detect
  performance issues or make claims about performance.[^10]
- Write automated performance tests for performance sensitive parts of your product.[^16]

[^11]: [Just Say No to More End-to-End Tests](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html)

[^18]: [Where do our flaky tests come from?](https://testing.googleblog.com/2017/04/where-do-our-flaky-tests-come-from.html)

[^3]:  [Automating tests vs. test-automation](https://testing.googleblog.com/2007/10/automating-tests-vs-test-automation.html)

[^4]:  [Taming the Beast (a.k.a. how to test AJAX applications) : Part 1](https://testing.googleblog.com/2008/06/taming-beast-aka-how-to-test-ajax.html)

[^9]:  [Hermetic Servers](https://testing.googleblog.com/2012/10/hermetic-servers.html)

[^13]: [Flaky Tests at Google and How We Mitigate Them](https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html)

[^15]: [Hackable Projects - Pillar 2: Debuggability](https://testing.googleblog.com/2016/10/hackable-projects-pillar-2-debuggability.html)

[^17]: [Discomfort as a Tool for Change](https://testing.googleblog.com/2017/02/discomfort-as-tool-for-change.html)

[^5]:  [Test first is fun!](https://testing.googleblog.com/2008/09/test-first-is-fun_08.html)

[^19]: [Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html)

[^20]: [Mutation Testing](https://testing.googleblog.com/2021/04/mutation-testing.html)

[^6]:  [Presubmit And Performance](https://testing.googleblog.com/2008/09/presubmit-and-performance.html)

[^8]:  [The 10 Minute Test Plan](https://testing.googleblog.com/2011/09/10-minute-test-plan.html)

[^14]: [Hackable Projects - Pillar 1: Code Health](https://testing.googleblog.com/2016/08/hackable-projects.html)

[^7]:  [Burning Test Questions at Google](https://testing.googleblog.com/2009/06/burning-test-questions-at-google.html)

[^12]: [Multi-Repository Development](https://testing.googleblog.com/2015/05/multi-repository-development.html)

[^16]: [Hackable Projects - Pillar 3: Infrastructure](https://testing.googleblog.com/2016/11/hackable-projects-pillar-3.html)

[^1]:  [Post Release: Closing the loop](https://testing.googleblog.com/2007/10/post-release-closing-loop_02.html)

[^2]:  [Performance Testing](https://testing.googleblog.com/2007/10/performance-testing.html)

[^10]: [Optimal Logging](https://testing.googleblog.com/2013/06/optimal-logging.html)
