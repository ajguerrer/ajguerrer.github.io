+++
title = "Testable Contracts Make Exceptional Neighbors"
date = 2008-05-28
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Modify external visible state only after completing all operations which could possibly fail.

{% bad(header="Bad") %}
```cpp
bool SomeCollection::GetObjects(std::vector<Object>& objects) const {
  objects.clear();
  for (const auto& object : collection_) {
    if (object.IsFubarred()) return false;
    objects.push_back(object);
  }
  return true;
}
```
{% end %}

In these situations, the `swap` trick comes in handy.

{% good(header="Good") %}
```cpp
bool SomeCollection::GetObjects(std::vector<Object>& objects) const {
  std::vector<Object> known_good_objects;
  for (const auto& object : collection_) {
    if (object.IsFubarred()) return false;
    known_good_objects.push_back(object);
  }
  objects.swap(known_good_objects);
  return true;
}
```
{% end %}

Now, the caller has good objects on success, or unchanged objects on failure.