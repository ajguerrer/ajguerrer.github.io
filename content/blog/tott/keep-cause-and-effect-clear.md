+++
title = "Keep Cause and Effect Clear"
date = 2017-01-31
weight = 14
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

It's difficult to reason about a test when the cause is hidden far away from the effect.

{% bad(header="Bad") %}
```cpp
class TallyTest : public ::testing::Test {
protected:
  void SetUp() override {
    tally_.Increment("key1", 8);
    tally_.Increment("key2", 100);
    tally_.Increment("key1", 0);
    tally_.Increment("key1", 1);
  }

  Tally tally_;
}
// 200 lines of code

TEST_F(TallyTest, IncrementExistingKey) {
  EXPECT_EQ(9, tally_.Get("key1"));
}
```
{% end %}

Write tests where the effects immediately follow the causes.

{% good(header="Good") %}
```cpp
class TallyTest : public ::testing::Test {
protected:
  Tally tally_;
}

TEST_F(TallyTest, NewKey) {
  tally_.Increment("key", 100);
  EXPECT_EQ(100, tally_.Get("key"));
}

TEST_F(TallyTest, ExistingKey) {
  tally_.Increment("key", 8);
  tally_.Increment("key", 1);
  EXPECT_EQ(9, tally_.Get("key"));
}

TEST_F(TallyTest, IncrementByZeroDoesNothing) {
  tally_.Increment("key", 8);
  tally_.Increment("key", 0);
  EXPECT_EQ(8, tally_.Get("key"));
}
```
{% end %}

It may require a bit more code, but it's easier to read and maintain.