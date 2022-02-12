+++
title = "Test Behavior, Not Implementation"
date = 2013-08-05
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Except where explicitly intended, tests should work independent of the implementation details being
tested.

```cpp
class Calculator {
 public:
  int Add(int a, int b)  {
    return a + b;
  }
};

class Calculator {
 public:
  int Add(int a, int b) {
    Adder adder = adder_factory_.CreateAdder();
    ReturnValue return_value = adder.Compute(Number(a), Number(b));
    return return_value.ConvertToInteger();
  }

 private:
  AdderFactory adder_factory_;
};

TEST_F(CalculatorTest, ShouldAddIntegers) {
  EXPECT_EQ(3, calculator_.Add(2, 1));
  EXPECT_EQ(2, calculator_.Add(2, 0));
  EXPECT_EQ(1, calculator_.Add(2, -1));
}
```