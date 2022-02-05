+++
title = "Cleanly Create Test Data"
date = 2018-02-20
weight = 7
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Helper methods make it easier to create test data, but they don't age well.

{% bad(header="Bad") %}
```cpp
// Starts simple
Company company = NewCompany(kPublic);

// But soon acquires more parameters
Company small = NewCompany(2, 2, nullptr, kPublic);
Company privately_owned = NewCompany(0, 0, nullptr, kPrivate);
Company bankrupt = NewCompany(0 , 0, kPastDate, kPublic);

// Or more methods
Company small = NewCompanyWithEmployeesAndBoardMembers(2, 2, kPublic);
Company privately_owned = NewCompanyWithType(kPrivate);
Company bankrupt = NewCompanyWithBankruptcyDate(kPastDate, kPublic);
```
{% end %}

Try the builder pattern.

{% good(header="Good") %}
```cpp
Company small = Company::Builder{}.SetEmployees(2).SetBoardMembers(2).Build();
Company privately_owned = Company::Builder{}.SetType(kPrivate).Build();
Company bankrupt = Company::Builder{}.SetBankruptcyDate(kPastDate).Build();
Company default_company = Company::Builder{}.Build();

class Company::Builder {
 public:
  Builder& SetEmployees(int n) {
    employees_ = n;
    return *this;
  }
  Builder& SetBoardMembers(int n) {
    board_members_ = n;
    return *this;
  }
  Builder& SetBankruptcyDate(BankruptcyDate d) {
    date_ = d;
    return *this;
  }
  Builder& SetType(Type t) {
    type_ = t;
    return *this;
  }

  Company Build() const {
    return Company(employees_, board_members_, date_, type_);
  }

 private:
  int employees_ = 0;
  int board_members_ = 0;
  BankruptcyDate date_ = kBeforeDate;
  Type type_ = kPublic;
};
```
{% end %}
