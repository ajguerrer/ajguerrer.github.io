+++
title = "Refactoring Tests in the Red"
date = 2007-04-26
weight = 37
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

As your test suite grows, you will find yourself needing to refactor your tests. However,
your tests don't have tests!

One thing you can do is intentionally break the test, refactor the test, and make sure the test
still fails as expected.

Just remember to revert your code under test!
