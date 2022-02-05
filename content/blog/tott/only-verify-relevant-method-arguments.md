+++
title = "Only Verify Relevant Method Arguments"
date = 2018-06-26
weight = 4
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Tests become fragile when they expect exact values on irrelevant arguments.

{% bad(header="Bad") %}
```cpp
TEST_F(DisplayGreetingTest, ShowSpecialGreetingOnNewYearsDay) {
  fake_clock_.SetTime(kNewYearsDay);
  fake_user_.SetName("Fake User");
  EXPECT_CALL(mock_user_prompter_,
              UpdatePrompt("Hi Fake User! Happy New Year!",
                           TitleBar("2018-01-01"), PromptStyle::kNormal));
  user_greeter_.DisplayGreeting();
}
```
{% end %}

- Only verify one behavior per test.
- Only verify arguments that affect the correctness of the specific behavior being tested.

{% good(header="Good") %}
```cpp
TEST_F(DisplayGreetingTest, ShowSpecialGreetingOnNewYearsDay) {
  fake_clock_.SetTime(kNewYearsDay);
  EXPECT_CALL(mock_user_prompter_,
              UpdatePrompt(HasSubstr("Happy New Year!"), _, _));
  user_greeter_.DisplayGreeting();
}

TEST_F(DisplayGreetingTest, RenderUserName) {
  fake_user_.SetName("Fake User");
  EXPECT_CALL(mock_user_prompter_, UpdatePrompt(HasSubstr("Fake User"), _, _));
  user_greeter_.DisplayGreeting();
}
```
{% end %}
