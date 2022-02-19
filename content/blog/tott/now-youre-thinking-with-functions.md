+++
title = "Now You're Thinking With Functions"
date = 2022-02-07
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Creating `for` loops with the same repeated pattern contributes to maintenance burden.  

{% bad(header="Bad") %}
```cpp
  bool every_request_valid = true;
  for (const Request request : requests)
  {
    if (!IsValid(request))
    {
      every_request_valid = false;
      break;
    }
  }

  if (every_request_valid)
  {
    // do something
  }
```

```cpp
  bool every_user_valid = true;
  for (const User user : users)
  {
    if (!IsValid(user))
    {
      every_user_valid = false;
      break;
    }
  }

  if (every_user_valid)
  {
    // do something
  }
```
{% end %}

Using higher-order functions can help reduce duplication and make the code easier to read.

{% good(header="Good") %}
```cpp
  if (std::all_of(requests.begin(), requests.end(), IsValid))
  {
    // do something
  }
```

```cpp
  if (std::all_of(users.begin(), users.end(), IsValid))
  {
    // do something
  }
```
{% end %}

{% info(header="Note") %} 
This is not to say that `for` loops are considered harmful and should never be used. In each case,
consider the right tool for the job.
{% end %}