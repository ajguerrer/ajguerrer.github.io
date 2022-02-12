+++
title = "IdentifierNamingPostForWorldWideWebBlog"
date = 2017-10-23
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Names should be clear and precise.

Don't mention the type in the variable name. It's OK for the name and the type match.

{% bad(header="Bad") %}
```cpp
std::string name_string;
std::list<std::time_t> holiday_date_list;
```
{% end %}

{% good(header="Good") %}
```cpp
std::string name;
std::list<std::time_t> holidays;
Payments payments;
```
{% end %}

Don't use overly specific names. Get more specific if there is a need for disambiguation.

{% bad(header="Bad") %}
```cpp
Monster final_battle_most_dangerous_boss_monster;
Payments non_typical_monthly_payments;
```
{% end %}

{% good(header="Good") %}
```cpp
Monster boss;
Payments payments;
```
{% end %}

Do not repeat context.

{% bad(header="Bad") %}
```cpp
class AnnualHolidaySale {
  bool PromoteHolidaySale();
  int annual_sale_rebate_;
};
```
{% end %}

{% good(header="Good") %}
```cpp
class AnnualHolidaySale {
  bool Promote();
  int rebate_;
}
```
{% end %}