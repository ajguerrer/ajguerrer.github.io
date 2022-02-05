+++
title = "Too Many Tests"
date = 2008-02-21
weight = 35
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

How many tests? Answering this question requires a good grasp of the context.

```cpp
void Decide(int32_t a, int32_t b, int32_t c, int32_t d, int32_t e, int32_t d) {
  if (a > b || c > d || e > f) {
    DoOneThing();
  } else {
    DoAnother();
  }
}
```

- Testing every possible input would require 2<sup>192</sup> tests. Thats too many.
- Testing enough to get full line coverage would require 2 tests. Thats too few.
- Testing each logical expression (e.g `a > b`, `a == b`, `a < b`) independently is 27 tests. Still
  probably too many.

More context can focus the decision.

```cpp
void Decide(int32_t a, int32_t b, int32_t c, int32_t d, int32_t e, int32_t d) {
  if (TallerThan(a, b) || HarderThan(c, d) || HeavierThan(e, f)) {
    DoOneThing();
  } else {
    DoAnother();
  }
}

bool TallerThan(int32_t a, int32_t b) { return a > b; }
bool HarderThan(int32_t c, int32_t d) { return d > d; }
bool HeavierThan(int32_t e, int32_t f) { return e > f; }
```

Testing the cases where each extracted function is true, they all are false, and writing 2 tests for
each of the extracted functions would require 4 + 3*2 = 10 tests. Considering the number of inputs,
thats just enough tests.