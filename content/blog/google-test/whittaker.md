+++
title = "Musings from James Whittaker"
date = 2020-02-29
weight = 3
[taxonomies]
categories = ["Google Testing Blog"]
tags = ["testing", "google"]
[extra]
+++

James A. Whittaker wrote three thought-provoking series in the [Google Testing
Blog](https://testing.googleblog.com/). I get the feeling he likes the number seven. Every one of
his posts provided me with a unique and colorful take on what can sometimes be a dry subject. The
abstract concepts shared in his posts shaped my foundational beliefs on testing software. 

Below you will find a few sentences per post which I think capture the essence of what Whittaker is
conveying in each of his posts. The final series is a bit different than the first two; it's an 
inside view of the structure and process inside the software shop known as Google.

## The Seven Plagues of Software Testing

1. **Aimlessness** - Do not test for the sake of testing. Every test should have a goal. Document
   what works and analyze what doesn't. Then, share with your colleagues.[^1]
1. **Repetitiveness** - Running the same test suite over again without finding new bugs does not
   mean that there are no bugs. Variation is healthy.[^2]
1. **Amnesia** - Chances are the problem your are trying to solve has been solved before. If the
   same issue keeps stinging you, or you had to answer a question the hard way, document it and put
   it in a place others will find it.[^3]
1. **Boredom** - A bored tester rushes through the tactical aspects of testing without considering
   the interesting strategic aspects. The day testing gets "figured out" is the day it gets
   completely automated away.[^4]
1. **Homelessness** - Testers are homeless. They don't actually live in the software like users do.
   Some bugs are only found with the hands of users doing their work in their environment.[^5]
1. **Blindness** - Testers require tools to provide helpful feedback from software. It's tempting to
   settle down with a trusty set of tools, but doing so causes self-inflicted blindness to a growing
   ecosystem of useful feedback.[^6]
1. **Entropy** - Testers increase entropy by giving developers things to do. This is unavoidable,
   but preventative. As developers do more *during* development, testers add less work, and entropy
   tends towards a minimum.[^7]

## An Ingredients List for Testing

1. **Product expertise** - A good developer knows how the product works; a good tester knows how to
   use it.[^8]
1. **Bill of materials** - Testers should be able to reference a complete list of features that can
   be tested.[^9] 
1. **Risk analysis** - Features are not equally important, or equally time consuming to test. Have a
   model to quantitatively analyze the risk of each feature.[^10] 
1. **Domain expertise** - It is not enough to be good at testing. Testers also need expertise with
   the technologies of the domain the product operates in.[^11]
1. **Test guidance** - Whether it be technique, nomenclature, or history, testers need a way to
   identify and store tribal knowledge of the team.[^12]
1. **Variation** - Tests often get stale. Wasting time running stale tests is also a form of risk.
   Adding variation can breathe new life into stale tests.[^13]
1. **Completeness analysis** - Teams need a model to measure how well their testing efforts have
   covered the risk landscape of their product.[^14]

## How Google Tests Software

Engineers are loaned out to product teams on an as-needed basis. Engineers are free to change
product teams at their own cadence.[^15]

Developers own quality while testers support developers with tools and feedback. As developers get
better at testing, less testers are needed. Successful teams have higher developer-to-tester ratios.
[^15]

Development and test are not treated as separate disciplines. Developers test and testers code.[^17]
Instead, each of the three roles look at the product from different angles:

- **SWE (Software Engineer)** - Feature creators responsible for their work. SWEs design and write
  features, and then prove they work by writing and running tests.
- **SET (Software Engineer in Test)** - Codebase caretakers who enable SWEs to write tests. SETs
  refactor code for testability, and write test features including test doubles and test framework.
- **TE (Test Engineer)** - Product experts who analyze quality and risk from the perspective of the
  user. TEs write large tests and automation scrips as well as drive test execution and interpret
  their results.[^16]

SETs and TEs are usually not involved early in the design phase of a product. Only when the product
gains traction do they begin to exert their influence.[^20] [^22]

SETs and SWEs have similar skill sets. Conversions from one role to another are common.[^21]

Quality is a work in progress that relies on getting product out to users and receiving feedback as
quickly as possible. As its being developed, a release is pushed through several channels in order
of increasing confidence in quality:

- **Canary** - Only fit for ultra tolerant users running experiments.
- **Dev** - Used by developers for day-to-day work.
- **Test** - Used internally for day-to-day work.
- **Beta/Release** - Fit for external exposure.[^18]

Tests are classified by scope, falling under three categories:

- **Small** - Covers a single function, focusing on logic.
- **Medium** - Covers a function and its nearest neighbors, focusing on interoperability.
- **Large** -  Covers an entire user scenario, focusing on business requirements.[^19]

If a test doesn't require human cleverness or intuition, it is automated. Bug reporting is automated
too.[^19]

[^1]:  [The 7 Plagues of Software Testing](https://testing.googleblog.com/2009/06/7-plagues-of-software-testing.html)

[^2]:  [The plague of repetitiveness](https://testing.googleblog.com/2009/06/by-james.html)

[^3]:  [The Plague of Amnesia](https://testing.googleblog.com/2009/07/plague-of-amnesia.html)

[^4]:  [The Plague of Boredom](https://testing.googleblog.com/2009/07/plague-of-boredom.html)

[^5]:  [The Plague of Homelessness](https://testing.googleblog.com/2009/07/plague-of-homelessness.html)

[^6]:  [The Plague of Blindness](https://testing.googleblog.com/2009/07/plague-of-blindness.html)

[^7]:  [The Plague of Entropy](https://testing.googleblog.com/2009/09/plague-of-entropy.html)

[^8]:  [An Ingredients List for Testing - Part One](https://testing.googleblog.com/2010/08/ingredients-list-for-testing-part-one.html)

[^9]:  [An Ingredients List for Testing - Part Two](https://testing.googleblog.com/2010/08/ingredients-list-for-testing-part-two.html)

[^10]: [An Ingredients List for Testing - Part Three](https://testing.googleblog.com/2010/09/ingredients-list-for-testing-part-three.html)

[^11]: [An Ingredients List for Testing - Part Four](https://testing.googleblog.com/2010/09/ingredients-list-for-testing-part-four.html)

[^12]: [An Ingredients List for Testing - Part Five](https://testing.googleblog.com/2010/10/ingredients-list-for-testing-part-five.html)

[^13]: [An Ingredients List for Testing - Part Six](https://testing.googleblog.com/2010/11/ingredients-list-for-testing-part-six.html)

[^14]: [An Ingredients List for Testing - Part Seven (of Seven)](https://testing.googleblog.com/2010/11/ingredients-list-for-testing-part-seven.html)

[^15]: [How Google Tests Software - Part One](https://testing.googleblog.com/2011/01/how-google-tests-software.html)

[^17]: [How Google Tests Software - Part Three](https://testing.googleblog.com/2011/02/how-google-tests-software-part-three.html)

[^16]: [How Google Tests Software - Part Two](https://testing.googleblog.com/2011/02/how-google-tests-software-part-two.html)

[^20]: [How Google Tests Software - Part Six](https://testing.googleblog.com/2011/05/how-google-tests-software-part-six.html)

[^22]: [How Google Tests Software - Part Seven](https://testing.googleblog.com/2011/05/how-google-tests-software-part-seven.html)

[^21]: [How Google Tests Software - A Break for Q&A](https://testing.googleblog.com/2011/05/how-google-tests-software-break-for-q.html)

[^18]: [How Google Tests Software - Part Four](https://testing.googleblog.com/2011/03/how-google-tests-software-part-four.html)

[^19]: [How Google Tests Software - Part Five](https://testing.googleblog.com/2011/03/how-google-tests-software-part-five.html)