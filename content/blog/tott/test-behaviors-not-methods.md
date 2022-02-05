+++
title = "Test Behaviors, Not Methods"
date = 2014-04-14
weight = 22
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

A single method can exhibit many behaviors. Likewise, a single behavior can span multiple methods.

It can be harmful to think that tests and public methods should have a 1:1 relationship.

{% bad(header="Bad") %}
``` cpp
TEST_F(TransactionProcessorTest, ProcessTransaction) {
  User user = NewUserWithBalance(kLowBalanceThreshold + Dollars(2));
  transaction_processor_.ProcessTransaction(
      user, Transaction("Pile of Beanie Babies", Dollars(3)));
  EXPECT_THAT(ui_.GetText(), HasSubstr("You bought a Pile of Beanie Babies"));
  EXPECT_EQ(user.GetEmails().size(), 1);
  EXPECT_STREQ(user.GetEmails().at(0).GetSubject(), "Your balance is low");
}
```
{% end %}

Each test should verify one behavior. Each method may take several tests to verify.

{% good(header="Good") %}
``` cpp
TEST_F(TransactionProcessorTest, ShouldDisplayNotification) {
  transaction_processor_.ProcessTransaction(
      User(), Transaction("Pile of Beanie Babies"));
  EXPECT_THAT(ui_.GetText(), HasSubstr("You bought a Pile of Beanie Babies"));
}

TEST_F(TransactionProcessorTest, ShouldSendEmailWhenBalanceIsLow) {
  User user = NewUserWithBalance(kLowBalanceThreshold + Dollars(2));
  transaction_processor_.ProcessTransaction(user, Transaction(Dollars(3)));
  EXPECT_EQ(user.GetEmails().size(), 1);
  EXPECT_STREQ(user.GetEmails().at(0).GetSubject(), "Your balance is low");
}
```
{% end %}