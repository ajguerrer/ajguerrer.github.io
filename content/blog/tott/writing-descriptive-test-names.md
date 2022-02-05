+++
title = "Writing Descriptive Test Names"
date = 2014-10-16
weight = 18
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Vague test names make it hard to keep track of what is tested.

{% bad(header="Bad") %}
```cpp
TEST_F(IsUserLockedOutTest, InvalidLogin) {
  authenticator_.Authenticate(username_, password_);
  EXPECT_FALSE(authenticator_.IsUserLockedOut(username_));

  authenticator_.Authenticate(username_, password_);
  EXPECT_FALSE(authenticator_.IsUserLockedOut(username_));

  authenticator_.Authenticate(username_, password_);
  EXPECT_TRUE(authenticator_.IsUserLockedOut(username_));
}
```
{% end %}

Descriptive test names make it easy to tell what behavior is broken without looking at code. Also,
the length of a good test name helps indicate when a test needs to be split apart.

{% good(header="Good") %}
```cpp
TEST_F(IsUserLockedOutTest, ShouldLockOutUserAfterThreeInvalidLoginAttempts) {
 // ...
}
```
{% end %}

A test's name should be all you need to know to understand the behavior being tested. Make sure
the name contains both the scenario being tested and the expected outcome.