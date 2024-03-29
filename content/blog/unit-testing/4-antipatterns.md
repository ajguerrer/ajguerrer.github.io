+++
title = "Part 4: Unit Testing Anti-patterns"
date = 2020-01-06
[taxonomies]
categories = ["Unit Testing Principles, Practices, and Patterns"]
tags = ["testing", "unit-testing"]
[extra]
+++

### Don't test private methods directly

Only test observable behavior. Coupling a test to implementation details will reduce the tests 
resistance to refactoring.

{% bad(header="Bad") %}
```cpp
class Order {
 public:
  std::string GenerateDescription();

 private:
  float GetPrice();

  Customer customer_;
  std::vector<Product> products_;
};

std::string Order::GenerateDescription() {
  return absl::StrCat("Customer name: ", customer_.name,
                      ", total number of products: ", products_.size(),
                      ", total price: ", GetPrice());
}

float Order::GetPrice() {
  float basePrice =
      std::accumulate(products_.begin(), products_.end(), 0.0, sum_base_prices);
  float discount = customer_.discount;
  float taxes =
      std::accumulate(products_.begin(), products_.end(), 0.0, sum_taxes);

  return basePrice * discount + taxes;
}
```
{% end %}

If a private method is hard to test indirectly, then it either has dead code or is in need of an 
abstraction.

{% good(header="Good") %}
```cpp
class Order {
 public:
  std::string GenerateDescription();

 private:
  Customer customer_;
  std::vector<Product> products_;
};

class PriceCalculator {
 public:
  float Calculate(Customer customer, std::vector<Product> products);
};

std::string Order::GenerateDescription() {
  PriceCalculator calc;
  return absl::StrCat("Customer name: ", customer_.name,
                      ", total number of products: ", products_.size(),
                      ", total price: ", calc.Calculate(customer_, products_));
}

float PriceCalculator::Calculate(Customer customer,
                                 std::vector<Product> products) {
  float basePrice =
      std::accumulate(products.begin(), products.end(), 0.0, sum_base_prices);
  float discount = customer.discount;
  float taxes =
      std::accumulate(products.begin(), products.end(), 0.0, sum_taxes);

  return basePrice * discount + taxes;
}
```
{% end %}

### Don't expose private state

Its tempting to add a way to access internal state for the sake of testing.

{% bad(header="Bad") %}
```cpp
enum class CustomerStatus {
  kRegular,
  kPreferred,
};

class Customer {
 public:
  void Promote() { status_ = CustomerStatus::kPreferred; }
  float GetDiscount() {
    return (status_ == CustomerStatus::kPreferred) ? 0.95 : 1;
  }

 private:
  CustomerStatus status_;
};
```
{% end %}

However exposing state will leak implementation details. Instead, look at how the production code 
uses the class and test accordingly.

{% good(header="Good") %}
```cpp
class Customer {
 public:
  Customer(double starting_)
  void Promote() { status_ = CustomerStatus::kPreferred; }

 private:
  CustomerStatus status_;
};

TEST(Customer, PromotedCustomerReceivesDiscount) {
  Customer customer;
  customer.Promote();
  Product<kFujiApple> apple;
  customer.Purchase(apple);
  EXPECT_LT(customer.Balance(), apple.Price());
}
```
{% end %}

### Don't leak domain knowledge to tests

Don't base expectations off of implementation details.

{% bad(header="Bad") %}
```cpp
class Calculator {
 public:
  int Add(int a, int b) { return a + b; }
};

TEST(Calculator, Add) {
  Calculator calc;
  int a = 1;
  int b = 3;
  int expected = a + b;
  int actual = calc.Add(a, b);
  EXPECT_EQ(actual, expected);
}
```
{% end %}

While it may sound counterintuitive at first, using hardcoded values in your tests is a good thing.

{% good(header="Good") %}
```cpp
TEST(Calculator, Add) {
  Calculator calc;
  EXPECT_EQ(calc.Add(1, 3), 4);
}
```
{% end %}

### Don't add production code for tests only

Mixing test code with production code only adds to maintenance costs.

{% bad(header="Bad") %}
```cpp
class Logger {
 public:
  explicit Logger(bool is_test_environment)
      : is_test_environment_(is_test_environment) {}

  void Log(std::string text) const {
    if (is_test_environment_) return;
    std::cout << text << '\n';
  }

 private:
  bool is_test_environment_;
};
```
{% end %}

Instead, write two distinct implementations that share a common interface.

{% good(header="Good") %}
```cpp
class Logger {
 public:
  virtual void Log(std::string text) const = 0;
};

class ProductionLogger : public Logger {
 public:
  void Log(std::string text) const { std::cout << text << '\n'; }
};

class FakeLogger : public Logger {
 public:
  void Log(std::string text) const {}
};

class Controller {
 public:
  void SomeMethod(const Logger& logger) { logger.Log("SomeMethod was called"); }
};
```
{% end %}

### Don't mock concrete classes

Treat urges to mock a concrete class as a warning flag for violating the Single Responsibility 
Principle.

{% bad(header="Bad") %}
```cpp
class StatisticsCalculator {
 public:
  virtual std::vector<DeliveryRecord> GetDeliveries(int customer_id);
  Statistic Calculate(int customer_id);
};

class CustomerController {
 public:
  CustomerController(const StatisticsCalculator& calculator)
      : calculator_(calculator) {}

  std::string GetStatistics(int customer_id);

 private:
  StatisticsCalculator calculator_;
};
```
{% end %}

{% bad(header="Bad") %}
```cpp
std::vector<DeliveryRecord> StatisticsCalculator::GetDeliveries(
    int customer_id) {
  /* Call an external dependency to get the list of deliveries */
  return std::vector<DeliveryRecord>{};
}

std::string CustomerController::GetStatistics(int customer_id) {
  Statistic stat = calculator_.Calculate(customer_id);

  return absl::StrCat("Total weight delivered: ", stat.totalWeight,
                      ". Total cost: ", stat.totalCost);
}
```
{% end %}

{% bad(header="Bad") %}
```cpp
class MockStatisticsCalculator : public StatisticsCalculator {
  MOCK_METHOD(std::vector<DeliveryRecord>, GetDeliveries, (int customer_id),
              (override));
};

TEST(CustomerController, CustomerWithNoDeliveries) {
  MockStatisticsCalculator calc;
  CustomerController controller(calc);
  std::string result = controller.GetStatistics(1);

  EXPECT_EQ("Total weight delivered: 0. Total cost: 0", result);
}
```
{% end %}

Separate unrelated responsibilities into two different classes, plus an interface for the shared 
dependencies.

{% good(header="Good") %}
```cpp
class DeliveryGateway {
 public:
  virtual ~DeliveryGateway(){};
  virtual std::vector<DeliveryRecord> GetDeliveries(int customer_id) = 0;
};

class ConcreteDeliveryGateway : public DeliveryGateway {
 public:
  std::vector<DeliveryRecord> GetDeliveries(int customer_id);
};

class StatisticsCalculator {
 public:
  Statistic Calculate(const std::vector<DeliveryRecord>& records);
};

class CustomerController {
 public:
  CustomerController(const StatisticsCalculator& calculator,
                     std::unique_ptr<DeliveryGateway> gateway)
      : calculator_(calculator), gateway_(std::move(gateway)) {}

  std::string GetStatistics(int customer_id);

 private:
  StatisticsCalculator calculator_;
  std::unique_ptr<DeliveryGateway> gateway_;
};
```
{% end %}

{% good(header="Good") %}
```cpp
std::string CustomerController::GetStatistics(int customer_id) {
  const std::vector<DeliveryRecord> records =
      gateway_->GetDeliveries(customer_id);
  const Statistic stat = calculator_.Calculate(records);

  return absl::StrCat("Total weight delivered: ", stat.totalWeight,
                      ". Total cost: ", stat.totalCost);
}
```
{% end %}

{% good(header="Good") %}
```cpp
TEST(CustomerController, CustomerWithNoDeliveries) {
  StatisticsCalculator calc;
  CustomerController controller(calc, std::make_unique<MockDeliveryGateway>());

  std::string result = controller.GetStatistics(1);

  EXPECT_EQ("Total weight delivered: 0. Total cost: 0", result);
}
```
{% end %}

### Don't use time as an ambient context

Using time as an ambient context adds unnecessary shared dependencies, making testing difficult.

```cpp
using std::chrono::system_clock;
using time_point = std::chrono::time_point<system_clock>;
```

{% bad(header="Bad") %}
```cpp
class DateTimeServer {
 public:
  DateTimeServer() : now_func_(&system_clock::now) {}
  void Init(std::function<time_point()> now_func) { now_func_ = now_func; }
  time_point Now() { return now_func_(); }

 private:
  std::function<time_point()> now_func_;
};
```
{% end %}

{% bad(header="Bad") %}
```cpp
DateTimeServer server;
std::time_t time = system_clock::to_time_t(server.Now());
std::cout << std::ctime(&time) << '\n';

std::function<time_point()> now([] {
  std::tm time{tm_mday : 1, tm_mon : 0, tm_year : 120};
  return system_clock::from_time_t(std::mktime(&time));
});
server.Init(now);

time = system_clock::to_time_t(server.Now());
std::cout << std::ctime(&time) << '\n';
```
{% end %}

Instead, inject the time dependency explicitly either as a value or a service. 

{% good(header="Good") %}
```cpp
class DateTimeServer {
 public:
  virtual ~DateTimeServer(){};
  virtual time_point Now() = 0;
};

class ConcreteDateTimeServer : public DateTimeServer {
 public:
  time_point Now() { return system_clock::now(); }
};

class InquiryController {
 public:
  InquiryController(std::unique_ptr<DateTimeServer> server)
      : date_time_server_(std::move(server)) {}

  void ApproveInquiry(int id);

 private:
  std::unique_ptr<DateTimeServer> date_time_server_;
};
```
{% end %}

{% good(header="Good") %}
```cpp
void InquiryController::ApproveInquiry(int id) {
  Inquiry inquiry = GetById(id);
  inquiry.Approve(date_time_server_->Now());
  SaveInquiry(inquiry);
}
```
{% end %}

Prefer injecting values over injecting services.

{% good(header="Good") %}
```cpp
void InquiryController::ApproveInquiry(int id, time_point date_time) {
  Inquiry inquiry = GetById(id);
  inquiry.Approve(date_time);
  SaveInquiry(inquiry);
}
```
{% end %}
