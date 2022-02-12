+++
title = "What Makes a Good Test?"
date = 2014-03-18
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Tests provide more than verification. They also serve as documentation.

As a source of documentation, test should not be distracting or hide information.

{% bad(header="Bad") %}
``` cpp
TEST(CalculatorTest, ShouldPerformAddition) {
  Calculator calculator(RoundingStrategy(),
    "unused", kEnableCosinFeature, 0.01, kCalculusEngine, false);
  int result = calculator.DoComputation(MakeTestComputation());
  EXPECT_EQ(result, 5);
}
```
{% end %}

{% good(header="Good") %}
```cpp
TEST_(CalculatorTest, ShouldPerformAddition) {
  const int result = calculator_.DoComputation(MakeAdditionComputation(2, 3));
  EXPECT_EQ(result, 5);
}
```
{% end %}