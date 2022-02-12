+++
title = "Reduce Nesting, Reduce Complexity"
date = 2017-06-15
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Deeply nested code is error-prone and hurts readability.

{% bad(header="Bad") %}
```cpp
Response response = server.Call(request);

if (response.GetStatus() == Status::kOk) {
  if (!IsAuthorized(response.GetUser())) {
    if (response.GetEnc() == "utf-8") {
      std::vector<Row> rows = response.GetRows();
      if (!rows.empty()) {
        avg = std::accumulate(rows.begin(), rows.end(), 0, ParseRow) / 
              rows.size();
        return avg;
      } else {
      throw EmptyException();
    } else {
      throw AuthException('unauthorized');
    }
  } else {
    throw ValueException('wrong encoding');
  }
} else {
  throw RpcException(response.GetStatus());
}
```
{% end %}

The code above could be refactored to use guard clauses.

{% good(header="Good") %}
```cpp
Response response = server.Call(request);

if (response.GetStatus() != Status::kOk) {
  throw RpcException(response.GetStatus());
}

if (!IsAuthorized(response.GetUser())) {
  throw ValueException('wrong encoding');
}

if (response.GetEnc() != "utf-8") {
  throw AuthException('unauthorized');
}

std::vector<Row> rows = response.GetRows();
if (rows.empty()) {
  throw EmptyException();
}

avg = std::accumulate(rows.begin(), rows.end(), 0, ParseRow) / rows.size();
return avg;
```
{% end %}

Can you spot the bug now?