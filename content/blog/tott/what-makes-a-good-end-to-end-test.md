+++
title = "What Makes a Good End-to-End Test?"
date = 2016-09-21
weight = 15
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

End-to-end tests give confidence about the health of the system when it is in a near production
state, but they tend to be more flaky and expensive to maintain.

To be cost effective, end-to-end tests should focus on aspects of the system that cannot be
evaluated by smaller tests. Minor and/or frequently changing details like error messages or visual
layouts should not effect the test.