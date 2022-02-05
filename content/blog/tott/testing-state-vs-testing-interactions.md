+++
title = "Testing State vs. Testing Interactions"
date = 2013-03-22
weight = 28
[taxonomies]
categories = ["Testing on the Toilet"]
tags = ["testing", "google", "tott"]
[extra]
+++

- **Testing State** - Verifying the code under test returns the correct results.

  ```cpp
  TEST(NumberSorterTest, ShouldSortIntegers) {
    NumberSorter number_sorter({quicksort, bubblesort});
    std::vector<int> numbers = {3, 1, 2};
    EXPECT_EQ({1, 2, 3}, number_sorter.SortNumbers(numbers));
  }
  ```

- **Testing Interaction** - Verifying the code under test calls methods correctly.

  ```cpp
  TEST(NumberSorterTest, ShouldUseQuicksort) {
    NumberSorter number_sorter({mock_quicksort, mock_bubblesort});
    std::vector<int> numbers = {3, 1, 2};
    EXPECT_CALL(mock_quicksort, Sort(numbers));
    number_sorter.SortNumbers(numbers);
  }
  ```

Most of the time you want to test state. Occasionally interactions need to be tested when the number
of calls or order of calls matter.
