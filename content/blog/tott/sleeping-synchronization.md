+++
title = "Sleeping != Synchronization"
date = 2008-08-21
weight = 31
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Beware of `sleep`. `sleep` should never be used for synchronization, or in test.

{% bad(header="Bad") %}
```cpp
class CoffeeMaker {
 public:
  virtual ~CoffeeMaker() = default;
  virtual void MakeCoffee(const std::function<void()> callback) = 0;
};

class Intern : public CoffeeMaker {
 public:
  void MakeCoffee(const std::function<void()> callback) {
    // make coffee, hopefully within 60 seconds.
    callback();
  }
};

class Employee {
 public:
  void DrinkCoffee() { caffeinated_ = true; }
  bool IsCaffeinated() { return caffeinated_; }

  void DemandCoffee(CoffeeMaker& cm) {
    std::thread t(&CoffeeMaker::MakeCoffee, &cm,
                  std::bind(&Employee::DrinkCoffee, this));
    t.detach();
  }

 private:
  bool caffeinated_ = false;
};

TEST(EmployeeTest, ShouldBeCaffeinatedOnlyAfterDrinkingCoffee) {
  Employee e;
  Intern i;
  e.DemandCoffee(i);
  EXPECT_FALSE(e.IsCaffeinated());
  std::this_thread::sleep_for(60s);
  EXPECT_TRUE(e.IsCaffeinated());
}
```
{% end %}

Code that sleeps can be improved by waiting on a `std::future` or a `std::condition_variable`.
As always, if your waiting on a non-trivial operation, like `Intern::MakeCoffee`, use a fake.

{% good(header="Good") %}
```cpp
class FakeIntern : public CoffeeMaker {
 public:
  void MakeCoffee(const std::function<void()> callback) {
    std::unique_lock<std::mutex> lock(mut_);
    cv_.wait(lock, [this] { return ready_; });

    callback();
    done_ = true;

    lock.unlock();
    cv_.notify_one();
  }

  void SignalAndWait() {
    std::unique_lock<std::mutex> lock(mut_);
    ready_ = true;
    cv_.notify_one();

    cv_.wait(lock, [this] { return done_; });
  }

 private:
  bool ready_ = false;
  bool done_ = false;
  std::condition_variable cv_;
  std::mutex mut_;
};

TEST(EmployeeTest, ShouldBeCaffeinatedOnlyAfterDrinkingCoffee) {
  Employee e;
  FakeIntern i;
  e.DemandCoffee(i);
  EXPECT_FALSE(e.IsCaffeinated());
  i.SignalAndWait();
  EXPECT_TRUE(e.IsCaffeinated());
}
```
{% end %}