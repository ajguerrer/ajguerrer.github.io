+++
title = "Prefer Testing Public APIs Over Implementation-Detail Classes"
date = 2015-01-14
weight = 17
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Public APIs can be called by many users. Implementation details are only called by public APIs.
If the public APIs are well tested, as they should be, then the implementation details will get
tested by association.

Heavy testing against implementation details can cause a couple problems:

- Unlike public APIs, implementation details are vulnerable to refactoring. Tests for implementation
  details can fail even though the behavior from the public API is fine.
- Testing implementation details can give false confidence. Even if an implementation detail is well
  tested, that doesn't mean the pubic API behaves properly.