+++
title = "Understanding Code in Review"
date = 2018-05-01
weight = 6
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

If you find yourself reviewing code that is difficult to understand, don't waste time reviewing it.

{% bad(header="Bad") %}
```cpp
bool IsOkay(int n) {
  bool f = false;
  for (int i = 2; i <= n; ++i) {
    if (n % i == 0) f = true;
  }
  return !f;
}
```
{% end %}

Ask for it to be clarified.

{% good(header="Good") %}
```cpp
bool IsPrime(int n) {
  for (int divisor = 2; divisor <= n / 2; ++divisor) {
    if (n % divisor == 0) return false;
  }
  return true;
}
```
{% end %}

Clarifying code often results in noticing improvements.