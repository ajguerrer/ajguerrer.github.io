+++
title = "Change-Detector Tests Considered Harmful"
date = 2015-01-27
weight = 16
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Tests that break in response to any change to production code without verifying correct behavior
only add to maintenance costs without catching defects.

{% bad(header="Bad") %}
```cpp
void Processor::Process(Work w) {
  first_part_.Process(w);
  second_part_.Process(w);
}

TEST(ProcessorTest, ProcessWork) {
  MockFirstPart part1;
  MockSecondPart part2;
  Processor p(part1, part2);
  Work w;

  EXPECT_CALL(part1, Process(w));
  EXPECT_CALL(part2, Process(w));

  p.Process(w);
}
```
{% end %}

Tests like these should either be re-written or deleted.