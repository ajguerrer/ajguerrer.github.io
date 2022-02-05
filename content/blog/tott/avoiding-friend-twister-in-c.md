+++
title = "Avoiding Friend Twister in C++"
date = 2007-10-30
weight = 36
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

"Testing private members requires more `friend` contortions than a game of Twister®."

If you find yourself saying that, theres a better way.

{% bad(header="Bad") %}
```cpp
// include/my_project/dashboard.h

class Dashboard {
 private:
  // Declaration of functions getResults(), GetResultsFromCache(),
  // GetResultsFromDatabase(), and CountPassFail()

  std::unique_ptr<Database> database_; // instantiated in constructor

  friend class DashboardTest; // one friend declaration per test fixture
};
```
{% end %}

Instead, make a helper class by extracting a helper class (a variant of the Pimple idiom).

To preserve privacy, the helper class is tucked away in a private implementation directory separate
from the public API.

{% good(header="Good") %}
```cpp
// include/my_project/dashboard.h

class ResultsLog; // Foreword declare extracted helper interface

class Dashboard {
 public:
  explicit Dashboard(std::unique_ptr<ResultsLog> results)
      : results_(std::move(results)) {}

 private:
   std::unique_ptr<ResultsLog> results_;
};

// src/results_log.h

class ResultsLog {
 public:
  // Declaration of functions getResults(), GetResultsFromCache(),
  // GetResultsFromDatabase(), and CountPassFail()
};

// src/live_results_log.h

class LiveResultsLog : public ResultsLog {
 public:
  explicit LiveResultsLog(std::unique<Database> database)
      : database_(std::move(database)) {}
};
```
{% end %}

As an added bonus, now you can inject a `MockResultsLog` or a `FakeDatabase` for testing the
`Dashboard` class.