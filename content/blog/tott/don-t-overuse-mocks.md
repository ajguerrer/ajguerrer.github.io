+++
title = "Don't Overuse Mocks"
date = 2013-05-28
weight = 27
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Mocks are powerful, but easy to abuse.

{% bad(header="Bad") %}
```cpp
TEST_F(PaymentProcessorTest, ShouldChargeCreditCard) {
  PaymentProcessor payment_processor(mock_credit_card_server_);

  InSequence s;
  EXPECT_CALL(mock_credit_card_server_, IsServerAvailable())
      .WillOnce(Return(true));
  EXPECT_CALL(mock_credit_card_server_, BeginTransaction())
      .WillOnce(Return(mock_transaction_manager_));
  EXPECT_CALL(mock_transaction_manager_, GetTransaction())
      .WillOnce(Return(transaction_));
  EXPECT_CALL(mock_credit_card_server_, Pay(transaction_, credit_card_, 500))
      .WillOnce(Return(mock_payment_));
  EXPECT_CALL(mock_payment_, IsOverMaxBalance()).WillOnce(Return(false));

  payment_processor.ProcessPayment(credit_card_, Dollars(500));
}
```
{% end %}

Overusing mocks makes tests harder to understand, maintain, and provides less insurance that your
code is working properly.

If you don't need a mock, don't use one. Understanding when to use a mock comes from understanding
what you want to test.

{% good(header="Good") %}
```cpp
TEST_F(PaymentProcessorTest, ShouldChargeCreditCard) {
  PaymentProcessor payment_processor(credit_card_server_);
  payment_processor.ProcessPayment(credit_card_, Dollars(500));
  EXPECT_EQ(credit_card_server_.GetMostRecentCharge(credit_card_), 500);
}
```
{% end %}
