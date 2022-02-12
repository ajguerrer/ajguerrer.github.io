+++
title = "Tests Too DRY? Make Them DAMP!"
date = 2019-12-03
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Though the DRY ("Don't Repeat Yourself") principle is great for production code, tests don't test 
themselves. 

{% bad(header="Bad") %}
```cpp
class ForumTest : public ::testing::Test {
 protected:
  void SetUp() override {
    for (auto user : users_) {
      forum_.Register(user);
    }
  }

  Forum forum_;
  std::vector<User> users_ = {User("Alice"), User("Bob")};
}

TEST_F(ForumTest, CanRegisterMultipleUsers) {
  for (auto user : users_) {
    EXPECT_TRUE(forum_.HasRegisteredUser(user));
  }
}
```
{% end %}

Tests should optimize for readability, even at the expense of redundancy. Prefer the DAMP 
("Descriptive and Meaningful Phrases") principle. 

{% good(header="Good") %}
```cpp
TEST(ForumTest, CanRegisterMultipleUsers) {
  Forum forum;
  User user1("Alice");
  User user2("Bob");
  forum.Register(user1);
  forum.Register(user2);

  EXPECT_TRUE(forum.HasRegisteredUser(user1));
  EXPECT_TRUE(forum.HasRegisteredUser(user2));
}
```
{% end %}
