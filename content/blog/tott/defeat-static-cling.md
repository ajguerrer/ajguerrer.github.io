+++
title = "Defeat \"Static Cling\""
date = 2008-06-26
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Static functions, like this singleton `GetInstance` method, are a sign of tight coupling.

{% bad(header="Bad") %}
```cpp
class MyObject {
 public:
  int DoSomething(int id) {
    return TheirEntity::GetInstance().GetSomething(id);
  }
};
```
{% end %}

There is a way around this using the Repository Pattern.

{% good(header="Good") %}
```cpp
class TheirEntityRepository {
 public:
  virtual ~TheirEntityRepository() = default;
  virtual TheirEntity& GetInstance() = 0;
  // Other static methods here
};

class TheirEntityStaticRepository : public TheirEntityRepository {
 public:
  TheirEntity& GetInstance() { return TheirEntity::GetInstance(); }
};

class MyObject {
 public:
  explicit MyObject(std::unique_ptr<TheirEntityRepository> repository)
      : repository_(std::move(repository)) {}
  int DoSomething(int id) { return repository_->GetInstance().GetSomething(); }

 private:
  std::unique_ptr<TheirEntityRepository> repository_;
};
```
{% end %}

All thats left is to derive a `MockTheirEntityRepository` suitable for your testing needs.