+++
title = "Don't Put Logic in Tests"
date = 2014-07-31
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Tests should be simple by stating I/O directly rather than computing them.

{% bad(header="Bad") %}
```cpp
TEST(NavigatorTest, ShouldNavigateToPhotosPage) {
  const std::string baseUrl = "http://plus.google.com/";
  Navigator nav(baseUrl);
  nav.GoToPhotosPage();
  EXPECT_EQ(baseUrl + "/u/0/photos", nav.GetCurrentUrl());
}
```
{% end %}

Even a simple string concatenation can lead to bugs.

{% good(header="Good") %}
```cpp
TEST(NavigatorTest, ShouldNavigateToPhotosPage) {
  Navigator nav("http://plug.google.com/");
  nav.GoToPhotosPage();
  EXPECT_EQ("http://plus.google.com//u/0/photos", nav.GetCurrentUrl());
}
```
{% end %}

If a test requires logic, move that logic out of the test body into utilities and helper functions
and write tests for them too.
