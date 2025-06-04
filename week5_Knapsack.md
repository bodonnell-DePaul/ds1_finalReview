### Knapsack Problem

The Knapsack Problem is a classic optimization problem in computer science and combinatorial optimization. It is named after the problem faced by someone who is constrained by a fixed-size knapsack and must fill it with the most valuable items.

#### Problem Definition

The Knapsack Problem can be described as follows:
- Given a set of items, each with a weight and a value, determine the number of each item to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible.

There are several variations of the Knapsack Problem, but the most common one is the **0/1 Knapsack Problem**, where each item can either be included or excluded from the knapsack.

#### Example

Imagine you have a knapsack with a weight capacity of 50 units, and you have the following items:

| Item | Weight | Value |
|------|--------|-------|
| 1    | 10     | 60    |
| 2    | 20     | 100   |
| 3    | 30     | 120   |

The goal is to maximize the total value of the items in the knapsack without exceeding the weight capacity.

#### Applications

The Knapsack Problem has many practical applications, including:
- **Resource allocation**: Allocating limited resources to maximize profit or efficiency.
- **Budget management**: Selecting projects or investments to maximize returns within a budget.
- **Cargo loading**: Optimizing the loading of cargo to maximize value while adhering to weight constraints.

#### Complexity

The Knapsack Problem is NP-complete, meaning that there is no known polynomial-time algorithm to solve it for all instances. The naive recursive approach to solve the 0/1 Knapsack Problem has exponential time complexity.

#### Naive Recursive Approach

A naive approach to solving the 0/1 Knapsack Problem is to use recursion to explore all possible combinations of items. Here is a Java implementation of the naive recursive approach:

```java
public class KnapsackProblem {
    public static void main(String[] args) {
        int[] weights = {10, 20, 30};
        int[] values = {60, 100, 120};
        int capacity = 50;
        int n = weights.length;
        System.out.println("Maximum value in Knapsack = " + knapsack(capacity, weights, values, n));
    }

    public static int knapsack(int capacity, int[] weights, int[] values, int n) {
        // Base Case
        if (n == 0 || capacity == 0) {
            return 0;
        }

        // If weight of the nth item is more than Knapsack capacity, then this item cannot be included
        if (weights[n - 1] > capacity) {
            return knapsack(capacity, weights, values, n - 1);
        } else {
            // Return the maximum of two cases:
            // (1) nth item included
            // (2) nth item not included
            return Math.max(
                values[n - 1] + knapsack(capacity - weights[n - 1], weights, values, n - 1),
                knapsack(capacity, weights, values, n - 1)
            );
        }
    }
}
```

#### Explanation

1. **Base Case**: The function `knapsack` checks if `n` (the number of items) is 0 or if `capacity` (the remaining capacity of the knapsack) is 0. If either is true, it returns 0 because no more items can be added.

2. **Recursive Case**:
   - If the weight of the nth item is greater than the remaining capacity, the item cannot be included, and the function calls itself with `n - 1` items.
   - Otherwise, the function considers two cases:
     - Including the nth item: The value of the nth item is added to the result of the recursive call with the remaining capacity reduced by the weight of the nth item and `n - 1` items.
     - Excluding the nth item: The function calls itself with `n - 1` items.
   - The function returns the maximum value of the two cases.

#### Time Complexity Analysis

- **Recursive Calls**: Each call to `knapsack` results in two more calls, leading to a binary tree of calls.
- **Number of Calls**: The number of calls grows exponentially with `n`, leading to a time complexity of O(2^n).

This exponential time complexity makes the naive recursive approach impractical for large numbers of items. More efficient algorithms, such as dynamic programming, are often used to solve the Knapsack Problem in practice.