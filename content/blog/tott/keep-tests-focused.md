+++
title = "Keep Tests Focused"
date = 2018-06-11
weight = 5
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Testing too many scenarios at once can make it difficult to understand test and reason about the
failure.

{% bad(header="Bad") %}
```cpp
TEST_F(BankAccountTest, WithdrawFromAccount) {
  Transaction transaction = account_.Deposit(Usd(5));
  clock_.AdvanceTime(kMinTimeToSettle);
  account_.Settle(transaction);

  EXPECT_THAT(account_.Withdraw(Usd(5)), IsOk());
  EXPECT_THAT(account_.Withdraw(USd(1)), IsRejected());
  account_.SetOverdraftLimit(Usd(1));
  EXPECT_THAT(account_.Withdraw(Usd(1)), IsOk());
}
```
{% end %}

Break up each scenario into its own test.

{% good(header="Good") %}
```cpp
TEST_F(BankAccountTest, CanWithdrawWithinBalance) {
  DepositAndSettle(Usd(5));
  EXPECT_THAT(account_.Withdraw(Usd(5)), IsOk());
}

TEST_F(BankAccountTest, CannotOverdraw) {
  DepositAndSettle(Usd(5));
  EXPECT_THAT(account_.Withdraw(Usd(6)), IsRejected());
}

TEST_F(BankAccountTest, CanOverdrawUpToOverdraftLimit) {
  DepositAndSettle(Usd(5));
  account_.SetOverdraftLimit(Usd(1));
  EXPECT_THAT(account_.Withdraw(Usd(6)), IsOk());
}
```
{% end %}

Notice how each test only verifies the output of one call per test.