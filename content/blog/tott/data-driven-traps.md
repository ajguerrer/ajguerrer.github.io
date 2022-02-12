+++
title = "Data Driven Traps!"
date = 2008-09-04
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

Data driven tests are efficient, but easy to abuse.

{% bad(header="Bad") %}
```cpp
struct TestData {
  const std::string word;
  const bool is_word;
};

const std::vector<TestData> test_data = {
    {"milk", true},
    {"centre", false},
    {"jklm", false},
};

TEST(IsWordTest, TestEverything) {
  for (const auto& entry : test_data) {
    EXPECT_EQ(IsWord(entry.word), entry.is_word);
  }
}
```
{% end %}

Data-driven tests make debugging and understanding failures, let alone false positives, more
difficult.

As the code grows in complexity, data tends to grow even faster. It quickly becomes impossible to
discern what behavior each piece of data is meant to test.

{% bad(header="Bad") %}
```cpp
const std::vector<Locale> locales = { Word::US, Word::UK, Word::France, ... };

struct TestData {
  std::string word;
  bool[kNumLocales] is_word;
};

const std::vector<TestData> test_data = {
    {"milk", {true, true, false, ...},
    {"centre", {false, true, true, ...}},
    {"jklm", {false, false, false, ...}},
};

TEST(IsWordTest, TestEverything) {
  for (const auto& entry : test_data) {
    for (const auto* locale: locales) {
      EXPECT_EQ(IsWord(entry.word, locale), entry.is_word);
    }
  }
}
```
{% end %}

Instead, think critically about what behaviors are worth testing.

{% good(header="Good") %}
```cpp
TEST(IsWordTest, ShouldExistInMultipleLocales) {
  EXPECT_TRUE(IsWord("milk", Word::US));
  EXPECT_TRUE(IsWord("milk", Word::UK));
  EXPECT_FALSE(IsWord("milk", Word::France));
}

TEST(IsWordTest, ShouldNotExist) {
  // "jklm" test not repeated as it uses the same code path
  EXPECT_FALSE(IsWord("jklm", Word::US));
}
```
{% end %}
