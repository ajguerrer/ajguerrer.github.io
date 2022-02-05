+++
title = "To Comment or Not to Comment"
date = 2017-07-17
weight = 11
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Comments are not always helpful.

{% bad(header="Bad") %}
```cpp
// Subtract discount from final price.
final_price = (num_items * item_price) -
              std::min(5, num_items) * item_price *  0.1;

// Filter offensive words.
for (std::string word : words) { ... }

int width = ...; // Width in pixels.

// Safe since height is always > 0.
return width / height;
```
{% end %}

It's often better to make your code self-explanatory.

{% good(header="Good") %}
```cpp
price = num_items * item_price;
discount = std::min(5, num_items) * item_price * 0.1;
final_price = price - discount;

FilterOffensiveWords(words);

Pixels width = ...;

CheckArgument(height > 0);
return width / height;
```
{% end %}

Avoid using comments to explain *what* code does. Use comments to explain *why* code does something.