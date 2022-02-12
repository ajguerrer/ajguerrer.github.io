+++
title = "Obsessed With Primitives?"
date = 2017-11-14
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Code that relies too heavily on primitive types instead of custom abstractions can be hard to
understand and maintain.

{% bad(header="Bad") %}
```cpp
std::vector<std::pair<int, int>> polygon = {
    std::make_pair(0, 0), std::make_pair(0, 4), std::make_pair(4, 0)};
std::pair<std::pair<int, int>, std::pair<int, int>> bounding_box =
    GetBoundingBox(polygon);
int area = (bounding_box.second.first - bounding_box.first.first) *
           (bounding_box.second.second - bounding_box.first.second);

```
{% end %}

Make higher-level abstractions.

{% good(header="Good") %}
```cpp
Polygon polygon = RightTriangle(4, 4);
int area = polygon.GetBoundingBox().GetArea();
```
{% end %}

This advice doesn't just apply to primitives and The STL. It's possible for any type to be too
primitive for the job.

{% good(header="Good") %}
```cpp
Polygon polygon = IsoscelesRightTriangle(4);
int area = polygon.GetBoundingBox().GetArea();
```
{% end %}
