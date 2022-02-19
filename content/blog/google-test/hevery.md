+++
title = "Testable Code with Miško Hevery"
date = 2008-11-26
[taxonomies]
categories = ["Google Testing Blog"]
tags = ["testing", "google"]
[extra]
+++

In the early days of the [Google Testing Blog], Miško Hevery contributed a wealth of posts related
to making code more testable culminating into a [guide].

This guide hit me hard during the early parts of my career. I have spent plenty of time working on
server code rather that infamously is collection of [class-based threads]. It is a fine pattern to
use as long as other threads strictly pass messages to them. However, we turned them into Singletons
and had other threads summon them into existence at will to call their methods directly. This
pattern spread everywhere! As you can imagine, any testing that can be done to such an application
is superficial. We could only test the little bits of logic that leaked into our models.

Miško's guide was a simple, directly applicable cookbook to problems such as these. Even though it
was already too late for our server, it was nice to know there was a way out, given enough effort.
Primarily, his guide teaches you how proactively avoid untestable patterns or reactively refactor
them out.

Without further ado, here is a summary to the key concepts in the guide with examples translated
from Java to C++.

[Google Testing Blog]: https://testing.googleblog.com
[guide]: http://misko.hevery.com/code-reviewers-guide
[class-based threads]: https://docs.microsoft.com/en-us/cpp/parallel/multithreading-creating-user-interface-threads

## Dependency Injection

A testable class is one that can be constructed in isolation or with test double collaborators. Once
constructed, they have all the dependencies they need. This is known as Dependency Injection.

Dependencies do not need to be concrete classes. Abstract classes allow testers to leverage
inheritance for creating test double collaborators. This is the primary tool in a testers toolkit
and the primary benefit Dependency Injection brings.

Below are some examples of constructor flaws and how they can be fixed with Dependency Injection.

### Static method calls

{% bad(header="Untestable") %}
```cpp
class AccountView {
  public:
  // Hard-coded static method call
  AccountView() { user_ = RPCClient::GetInstance().GetUser(); }

  private:
  User user_;
}

TEST(AccountViewTest, SlowAndFlakyTest) {
  // Unit test is slow and requires working network connection.
  AccountView view;
};
```
{% end %}

{% good(header="Testable") %}
```cpp
class AccountView {
  public:
  explicit AccountView(const User& user): user_(user) {}

  private:
  const User& user_;
};

TEST(AccountViewTest, FastAndReliableTest) {
  // User does not need to be retrieved over the network.
  User user;
  AccountView view(user);
}
```
{% end %}

### Conditional logic

{% bad(header="Untestable") %}
```cpp
class Car {
 public:
  explicit Car(CarType type) {
    // Only two ways to make a car and none of them for testing
    switch (type) {
      case kCoupe: {
        engine_ = FastEngine();
        tires_ = SmoothTires();
        break;
      }
      case kTruck: {
        engine_ = StrongEngine();
        tires_ = KnobbyTires();
      }
      default:
        assert(false);
    }
  }

 private:
  Engine engine_;
  Tires tires_;
};

TEST(CarTest, HardToTest) {
  // Want a car with fake engine and tires but can only make real ones.
  Car car(kCoupe);
}
```
{% end %}
{% good(header="Testable") %}
```cpp
class Car {
 public:
  Car(std::unique_ptr<Engine> engine, std::unique_ptr<Tires> tires)
      : engine_(std::move(engine)), tires_(std::move(tires)) {}

 private:
  std::unique_ptr<Engine> engine_;
  std::unique_ptr<Tires> tires_;
};

TEST(CarTest, EasyToTest) {
  // Car is fully configurable
  Car car(std::make_unique<FakeEngine>(), std::make_unique<FakeTires>());
}
```
{% end %}

### Constructing dependencies

{% bad(header="Untestable") %}
```cpp
class House {
 public:
  House() : kitchen_(), bedroom_() {}

 private:
  ModernKitchen kitchen_;
  SpareBedroom bedroom_;
};

TEST(HouseTest, HardToTest) {
  // house is stuck with ModernKitchen and SpareBedroom objects
  House house;
}
```
{% end %}
{% good(header="Testable") %}
```cpp
class House {
 public:
  House(std::unique_ptr<Kitchen> kitchen,
        std::unique_ptr<Bedroom> bedroom)
      : kitchen_(std::move(kitchen)), bedroom_(std::move(bedroom)) {}

 private:
  std::unique_ptr<Kitchen> kitchen_;
  std::unique_ptr<Bedroom> bedroom_;
};

TEST(HouseTest, EasyToTest) {
  // house uses lightweight test doubles
  House house(std::make_unique<MockKitchen>(),
              std::make_unique<MockBedroom>());
}
```
{% end %}

### Partial construction

{% bad(header="Untestable") %}
```cpp
class VisualVoicemail {
  public:
  // Constructs a partially initialized VisualVoicemail.
  // Don't forget to call Initialize in production, or SetCalls in test!
  explicit VisualVoicemail(const User& user) : user_(user) {}

  void Initialize() {
    SetCalls(Server::GetCallsFor(user_));
  }

  private:
  // Hack to allow test access to private methods.
  // Making this function public is not a better idea.
  friend class VisualVoicemailTest_BrittleDesign_Test;
  void SetCalls(std::vector<Call> calls) { calls_ = std::move(calls); }

  User user_;
  std::vector<Call> calls_;
};

TEST(VisualVoicemailTest, BrittleTest) {
  DummyUser user;
  VisualVoicemail voicemail(user);
  std::vector<Call> calls = BuildListOfTestCalls();
  voicemail.SetCalls(calls);
}
```
{% end %}
{% good(header="Testable") %}
```cpp
class VisualVoicemail {
  public:
  explicit VisualVoicemail(const std::vector<Call>& calls) : calls_(calls) {}

  private:
  std::vector<Call> calls_;
};

TEST(VisualVoicemailTest, FlexibleTest) {
  VisualVoicemail voicemail(BuildListOfTestCalls());
}
```
{% end %}

## Law of Demeter

A class becomes difficult to test if it gets its dependencies from anywhere besides it's interface.
For example, a class may get a dependency by asking one of it dependencies, breaking the Law of
Demeter.

Violations of the Law of Demeter are a sign of both bad Dependency Injection, for passing in the
wrong dependency, and bad encapsulation, for exposing secondary dependencies in the first place.

Below are examples of Law of Demeter violations and how they can be fixed with accurate interfaces
and encapsulation.

### Indirect dependencies

{% bad(header="Untestable") %}
```cpp
class SalesTaxCalculator {
 public:
  SalesTaxCalculator(const TaxTable& taxTable) : taxTable_(taxTable) {}

  float ComputeSalesTax(const User& user, const Invoice& invoice) {
    // Get the correct dependencies first...
    Address address = user.GetAddress();
    float amount = invoice.GetSubTotal();
    // ..then do the calculation.
    return amount * taxTable_.GetTaxRate(address);
  }

 public:
  TaxTable taxTable_;
}

TEST(SalesTaxCalculatorTest, ComplexTest) {
  SalesTaxCalculator calc(TaxTable());
  User user(Address("1600 Amphitheater Parkway..."));
  Invoice invoice(1, ProductX(95.00));
  // ...
  EXPECT_EQ(0.09, calc.ComputeSalesTax(user, invoice));
}
```
{% end %}

{% good(header="Testable") %}
```cpp
class SalesTaxCalculator {
 public:
  SalesTaxCalculator(const TaxTable& taxTable) : taxTable_(taxTable) {}

  float ComputeSalesTax(const Address& address, const float amount) {
    // Already have the correct dependencies; do the calculation.
    return amount * taxTable_.GetTaxRate(address);
  }

 public:
  TaxTable taxTable_;
}

TEST(SalesTaxCalculatorTest, StraightforwardTest) {
  SalesTaxCalculator calc(TaxTable());
  Address address("1600 Amphitheater Parkway...")
  // ...
  EXPECT_EQ(0.09, calc.ComputeSalesTax(address, 95.00));
}
```
{% end %}

### Overusing getters/setters

{% bad(header="Untestable") %}
```cpp
class LoginPage {
 public:
  LoginPage(std::shared_ptr<RPCClient> client, const HttpRequest& request)
      : client_(client), request_(request) {}

  bool Login() {
    // Even using a getter once is one more time than it needs to be.
    std::string cookie = request_.GetCookie("g");
    // Using a member access operator twice in one statement is right out!
    return client_.GetAuthenticator().Authenticate();
  }

 private:
  std::shared_ptr<RPCClient> client_;
  HttpRequest request_;
};

TEST(LoginPageTest, ComplexBrittleTest) {
  MockRPCClient mock_client(make_shared<FakeAuthenticator>());
  std::vector<Cookie> cookies = {Cookie("g", "xyz123")};
  MockHttpRequest mock_request(cookies);
  LoginPage page(client, request);

  EXPECT_CALL(mock_client, GetAuthenticator());
  EXPECT_CALL(mock_request, GetCookie("g"));

  EXPECT_TRUE(page.Login());
}
```
{% end %}

{% good(header="Testable") %}
```cpp
class LoginPage {
 public:
  LoginPage(std::shared_ptr<Authenticator> authenticator, const Cookie& cookie)
      : authenticator_(std::move(authenticator)), cookie_(cookie) {}

  bool Login() { return authenticator_.Authenticate(cookie_); }

 private:
  std::shared_ptr<Authenticator> authenticator_;
  Cookie cookie_;
};

TEST(LoginPageTest, SimpleFlexibleTest) {
  LoginPage page(std::make_shared<FakeAuthenticator>(), Cookie("g", "xyz123"));
  EXPECT_TRUE(page.Login());
}
```
{% end %}

### Exposing dependencies

{% bad(header="Untestable") %}
```cpp
class UpdateBug {
 public:
  explicit UpdateBug(std::shared_ptr<DatabaseInterface> db) : db_(db) {}
  void Execute(const Bug& bug) {
    // Impose internal lock management on client. Don't do this!
    db_->GetLock().Lock();
    db_->Save(bug);
    db_->GetLock().Unlock();
  }

 private:
  std::shared_ptr<DatabaseInterface> db_;
};

// This test shouldn't be needed, but we must enforce Lock/Unlock is called
// around Save.
TEST(UpdateBugTest, HardToTest) {
  MockLock mock_lock;
  auto mock_db = std::make_shared<MockDatabase>();
  Bug bug("description");
  UpdateBug updateBug(mock_db);

  EXPECT_CALL(*mock_db, GetLock()).WillRepeatedly(ReturnRef(mock_lock));
  {
    InSequence dummy;
    EXPECT_CALL(mock_lock, Lock());
    EXPECT_CALL(*mock_db, Save(Ref(bug)));
    EXPECT_CALL(mock_lock, Unlock());
  }

  updateBug.Execute(bug);
}
```
{% end %}

{% good(header="Testable") %}
``` cpp
class UpdateBug {
 public:
  explicit UpdateBug(std::shared_ptr<DatabaseInterface> db) : db_(db) {}
  void Execute(const Bug& bug) {
    // Save calls Lock/Unlock internally
    db_->Save(bug);
  }

 private:
  std::shared_ptr<DatabaseInterface> db_;
};

// Go strait to testing state. No need for mock test since locking interaction
// is encapsulated in the database.
TEST(UpdateBugTest, EasyTest) {
  auto db = std::make_shared<FakeDatabase>();
  UpdateBug updateBug();
  Bug bug("description");
  updateBug.Execute(bug);

  EXPECT_EQ(bug, db.GetLastSaved());
}
```
{% end %}

### Context Objects

{% bad(header="Untestable") %}
```cpp
class MembershipPlan {
  // *snip*
 public:
  void ProcessOrder(UserContext& userContext) {
    User user = userContext.GetUser();
    PlanLevel level = userContext.GetLevel();
    Order order = userContext.GetOrder();

    // do some processing ...
  }
};

TEST(MembershipPlanTest, BrittleUnreadableTest) {
  MembershipPlan plan;
  UserContext userContext;
  userContext.SetUser(User("Kim"));
  userContext.SetLevel(PlanLevel(143, "yearly"));
  userContext.SetOrder(Order("SuperDeluxe", 100, true));
  // Hopefully this is all the setup the userContext needs to call ProcessOrder
  plan.ProcessOrder(userContext);

  // Make assertions against userContext and plan...
}
```
{% end %}

{% good(header="Testable") %}
```cpp
class MembershipPlan {
  // ...
 public:
  void ProcessOrder(User& user, Order& order, const PlanLevel& level) {
    // do some processing ...
  }
};

TEST(MembershipPlanTest, FlexibleSimpleTest) {
  MembershipPlan plan;
  User user("Kim");
  Order order("SuperDeluxe", 100, true);
  const PlanLevel level(143, "yearly");
  plan.ProcessOrder(user, order, level);

  // Make assertions against user, order and plan...
}
```
{% end %}

## Global State

Global state is difficult to understand. Ideally, the interface of an object should fully describe
it's dependencies. Global state ruins this ideal because it can be used anywhere without warning.
Such flexibility may seem convenient to the original developer, but to others it's confusing.
Especially in large code bases, it can be difficult to realize that some global state exists and
reason about the circumstances in which it should or shouldn't be used.

Global state is the enemy of testing. It strongly couples itself to the code that uses it. Global
scope encourages widespread usage, further compounding coupling issues. Coupling to concrete types
makes it impossible to write test doubles and global state must be concrete because it uses the
`static` keyword.

Below are some examples of global state and how they can be fixed with Dependency Injection.
However, Dependency Injection alone is often not enough to fix `static` methods. For those, an
adapter called the repository pattern can help remove the
[static cling](../tott/#defeat-static-cling).

Note, the solutions to the examples should not feel good and require a lot of work. It is really
tough to test global state. The best solution is to never use it or refactor away from it
altogether.

### Singletons

{% bad(header="Untestable") %}
```cpp
class LoginService {
 public:
  static LoginService& GetInstance() {
    static LoginService instance;
    return instance;
  }
  // *snip*
};

class AdminDashboard {
 public:
  bool IsAuthenticatedAdmin(const User& user) {
    return LoginService::GetInstance().IsAuthenticatedAdmin(user);
  }
  // *snip*
};

TEST_F(AdminDashboardTest, FlakeySlowAndBrittleTest) {
  // AdminDashboard is forced to use the real LoginService singleton.
  User user("username", "password");
  ASSERT_TRUE(adminDashboard_.IsAuthenticatedAdmin(user));
}
```
{% end %}

{% good(header="Testable") %}
```cpp
class LoginServiceRepository {
 public:
  virtual ~LoginServiceRepository() = default;
  virtual LoginService& GetInstance() = 0;
};

class AdminDashboard {
 public:
  explicit AdminDashboard(std::shared_ptr<LoginServiceRepository> loginService)
      : loginService_(loginService) {}
  bool IsAuthenticatedAdmin(const User& user) {
    return loginService_->GetInstance().IsAuthenticatedAdmin(user);
  }

 private:
  std::shared_ptr<LoginServiceRepository> loginService_;
};

TEST(AdminDashboardTest, ReliableFastAndFlexibleTest) {
  // Can now use test doubles for the LoginService singleton.
  AdminDashboard adminDashboard(std::make_shared<MockLoginService>());
  User user("username", "password");
  ASSERT_TRUE(adminDashboard.IsAuthenticatedAdmin(user));
}
```
{% end %}

### Flags

{% bad(header="Untestable") %}
```cpp
static bool kFlagUseRealBackend;

class RpcClient {
 public:
  static RpcClient& GetInstance() {
    static RpcClient instance;
    return instance;
  }

  bool IsReal() { return backend_->IsReal(); }

 private:
  RpcClient() {
    if (kFlagUseRealBackend) {
      backend_ = std::make_unique<RealBackend>();
    } else {
      backend_ = std::make_unique<DummyBackend>();
    }
  }
  // *snip*

  std::unique_ptr<Backend> backend_;
};

// The 2nd test to run could fail if both tests are run in parallel because
// the RpcClient is static and both tests are a part of the same test suite.
TEST(RpcClientTest, SmallComplexAndFlakeyTest) {
  kFlagUseRealBackend = false;
  RpcClient& client = RpcClient::GetInstance();
  EXPECT_FALSE(client.IsReal());
}

TEST(RpcClientTest, LargeComplexAndFlakeyTest) {
  kFlagUseRealBackend = true;
  RpcClient& client = RpcClient::GetInstance();
  EXPECT_TRUE(client.IsReal());
}
```
{% end %}

{% good(header="Testable") %}
```cpp
class RpcClient {
 public:
  explicit RpcClient(std::unique_ptr<Backend> backend)
      : backend_(std::move(backend)) {}

  bool IsReal() { return backend_->IsReal(); }

 private:
  std::unique_ptr<Backend> backend_;
};

// Now, both tests pass.

TEST(RpcClientTest, SmallSimpleAndReliableTest) {
  RpcClient client(std::make_unique<DummyBackend>());
  EXPECT_FALSE(client.IsReal());
}

TEST(RpcClientTest, LargeSimpleAndReliableTest) {
  RpcClient client(std::make_unique<RealBackend>());
  EXPECT_TRUE(client.IsReal());
}
```
{% end %}

### Static methods

{% bad(header="Untestable") %}
```cpp
class TrainSchedule {
  // *snip *
  std::shared_ptr<Schedule> FindNextTrain() {
    // Do some work...

    // Static method accessing slow third party service
    if (TrackStatus::IsClosed(track)) {
      // Do even more work...
    }

    return schedule;
  }
}

TEST_F(TrainScheduleTest, FlakeySlowAndBrittleTest) {
  // Forces third party TrackStatus to get called
  ASSERT_NE(trainSchedule_.FindNextTrain(), nullptr);
}
```
{% end %}

{% good(header="Testable") %}
```cpp
class TrackStatusRepository {
 public:
  virtual ~TrackStatusRepository() = default;
  virtual bool IsClosed(const Track& track) = 0;
};

class RealTrackStatusRepository : public TrackStatusRepository {
  public:
    // ... Wrap each of the third party library's methods.
    bool IsClosed(const Track& track) {
      return TrackStatus::IsClosed(track);
    }
};

class TrainSchedule {
 public:
  TrainSchedule(std::unique_ptr<TrackStatusRepository> trackStatus)
      : trackStatus_(std::move(trackStatus)) {}
  // *snip *
  std::shared_ptr<Schedule> FindNextTrain() {
    // Do some work...

    if (trackStatus_->IsClosed(track)) {
      // Do even more work...
    }

    return schedule;
  }

 private:
  std::unique_ptr<TrackStatusRepository> trackStatus_;
};

TEST(TrainScheduleTest, ReliableFastAndFlexibleTest) {
  // Now TrainSchedule can be tested in isolation.
  TrainSchedule trainSchedule(std::make_unique<StubTrackStatusRepository>());
  ASSERT_NE(trainSchedule.FindNextTrain(), nullptr);
}
```
{% end %}

## Single Responsibility Principle

If you have a class that feels like it:

- Contains hidden interactions that make you scratch your head.
- Is difficult to read and retain in memory.
- Is difficult to reason about its state or contains a bunch of conditional logic.
- Is difficult to describe what it does or the word 'and' is used.
- Requires too many dependencies.
- Requires too much work to write a test double.
- Requires too many tests for full coverage.
- Requires large tests with complex setup/teardown.

Then, that class is likely violating the Single Responsibility Principle. Such classes hide points
of flexibility and encapsulate interaction instead of encapsulating behavior. Poor flexibility and
encapsulation creates brittle design and strong coupling. The result is a class that's hard to test.

Opportunity to fix a violation of the Single Responsibility Principle is either proactive or
reactive, depending on whether the violation is caught before or after it's committed to the
codebase.

It should go without saying that the proactive approach is less work.

### Proactive

1. Identify the individual responsibilities.
1. Give each responsibility a crisp name.
1. Extract functionality into each class.
1. Celebrate how much easier the class is to test.

### Reactive

1. Extract a class in the place where behavior is being altered with new functionality.
1. Start to move chunks of behavior out of the legacy class and test each chunk in isolation.

{% tip(header="Tip") %}
`static` methods are a sign of a homeless method. It likely belongs to one of the parameters it
takes.
{% end %}