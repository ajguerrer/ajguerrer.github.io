+++
title = "Make Interfaces Hard to Misuse"
date = 2018-07-25
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Don't push the responsibility of maintaining invariants on the caller.

{% bad(header="Bad") %}
```cpp
class Vector {
  explicit Vector(int num_slots);
  int RemainingSlots() const;
  void AddSlots(int num_slots);
  void Insert(int value);
};
```
{% end %}

In the code above, the caller needs to check `RemainingSlots`, and if `0`, `AddSlots` in order for
`Insert` to work properly.

Instead, `Insert` could automatically manage slots.

{% good(header="Good") %}
```cpp
class Vector {
  explicit Vector(int num_slots);
  void Insert(int value);
};
```
{% end %}

Other signs an interface is hard to use:

- `Initialize` / `Deitialize` functions.
- Allowing partially created objects.
- Parameters that can have invalid values.

Sometimes it's not practical to have a foolproof interface. In those cases rely on static analysis
or documentation.