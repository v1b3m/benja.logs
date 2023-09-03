---
title: "Identical Eggs"
categories:
  - dsa
  - problem
tags:
  - dynamic programming
toc: true
header:
  # image: /assets/images/dsa/00117-2661402125.jpg
mathjax: true
---

## PROBLEM

You are given `N` identical eggs and access to a building with `k` floors. Your task is to find the lowest floor that will cause an egg to break, if dropped from that floor. Once an egg breaks, it cannot be dropped again. If an egg breaks when dropped from the `xth` floor, you can assume it will also break when dropped from any floor greater than `x`.

Write an algorithm that finds the minimum number of trial drops it will take, in the worst case, to identify this floor.

For example, if `N = 1` and `k = 5`, we will need to try dropping the egg at every floor, beginning with the first, until we reach the fifth floor, so our solution will be `5`.

## SOLUTION

To solve this problem optimally with multiple eggs, we employ a dynamic programming approach. The main idea behind the approach is to minimize the number of drops in the worst-case scenario.

For a given number of eggs `N` and number of floors `k`, we'll attempt to find the minimum number of attempts needed in the worst case.

**Algorithm**:

1. Create a table `dp` with dimensions `(N+1) x (k+1)`.
2. Initialize `dp[i][j] = 0` for all `i` and `j`.
3. For a single egg, the worst-case number of attempts for `j` floors is `j` (i.e., trying from the 1st floor to the jth floor sequentially). So, initialize `dp[1][j] = j` for all `j`.
4. For `i` eggs and `j` floors, consider dropping an egg from the `x`th floor. There are two possibilities:
   a. The egg breaks: The problem reduces to `i-1` eggs and `x-1` floors.
   b. The egg does not break: The problem reduces to `i` eggs and `j-x` floors.
5. We are looking for the worst-case scenario, so we take the maximum of the two cases mentioned above and add 1 to it (for the current drop). We iterate over all floors to find the minimum number of drops in the worst case.
6. Return `dp[N][k]`.

Here's the code implementing the algorithm:

```typescript
function minTries(eggs: number, floors: number): number {
  // dp (dynamic programming)
  let dp: number[][] = Array.from({ length: eggs + 1 }, () =>
    Array(floors + 1).fill(0)
  );

  // Base case: 0 trials for 0 floors and j trials for j floors with 1 egg
  for (let j = 1; j <= floors; j++) {
    dp[1][j] = j;
  }

  // Calculate for the rest
  for (let i = 2; i <= eggs; i++) {
    for (let j = 1; j <= floors; j++) {
      let minVal = Number.MAX_SAFE_INTEGER;
      for (let x = 1; x <= j; x++) {
        // 1 + max(egg breaks, egg doesn't break)
        let temp = 1 + Math.max(dp[i - 1][x - 1], dp[i][j - x]);
        minVal = Math.min(minVal, temp);
      }
      dp[i][j] = minVal;
    }
  }

  return dp[eggs][floors];
}

// Test the function
let N = 1;
let k = 5;
console.log(minTries(N, k)); // Output: 5
```

This algorithm will find the minimum number of trial drops it will take, in the worst case, to identify the critical floor.

### SKETCHBOOK

```txt
                 j(floors)
            0    1    2    3
          +----+----+----+---+
        0 | 0  | 0  | 0  | 0 |
          +----+----+----+---+
        1 | 0  | 1  | 2  | 3 |
i (eggs)  +----+----+----+---+
        2 | 0  | 0  |    |   |
          +----+----+----+---+
        3 | 0  | 0  |    |   |
          +----+----+----+---+
```

- For one egg, worst case is you have to drop the egg  on the last floor

  ```js
  dp[1][j] = j;
  ```

- For the other floors, assuming you've dropped an egg. The worst case is the chance you've already taken (1) plus the worst case of if the egg breaks or not

  ```txt
  1 + max(egg breaks, egg doesn't break)
  1 + Math.max(dp[i - 1][x - 1], dp[i][j - x]);
  ```

## AN INTRODUCTION TO DYNAMIC PROGRAMMING

Dynamic programming is a method for solving complex problems by breaking them down into simpler overlapping subproblems. It is a technique for solving optimization problems by caching or storing solutions to overlapping subproblems to avoid redundant computations. The approach is generally used to solve problems that have two key attributes:

1. **Overlapping Subproblems**: The problem can be broken down into smaller, simpler subproblems which can be solved independently. The solution to the main problem is constructed from the solutions to its subproblems.

2. **Optimal Substructure**: An optimal solution to the main problem is built from the optimal solutions of its subproblems.

Dynamic programming is typically implemented using two main techniques:

1. **Top-down approach (Memoization)**: This approach starts with the original problem and breaks it down into its subproblems. If a subproblem has already been computed, its result is stored in a data structure (typically an array or a map), and the stored result is reused, saving unnecessary computation.

2. **Bottom-up approach (Tabulation)**: This approach solves the problem by solving all the related subproblems first, typically by filling up an n-dimensional table. Entries in the table are usually filled in a systematic order (e.g., left-to-right, bottom-to-top) such that the solution to each subproblem assumes the solutions to smaller subproblems have already been computed and stored.

Dynamic programming is widely used in computer science and mathematics due to its ability to optimize certain types of algorithms, particularly those used in combinatorial problems, from exponential time complexities to polynomial time complexities.

**Example**:
One classic example of a problem that can be solved with dynamic programming is the Fibonacci sequence. Instead of using a naive recursive approach, which leads to redundant calculations, a dynamic programming approach can be used to store already calculated Fibonacci numbers and thus reduce the number of computations drastically.

### `fibonacci` with dynamic programming

#### Intro to the `fibonacci` sequence

The Fibonacci sequence is a series of numbers where each number is the sum of the two preceding ones. It starts from 0 and 1. Mathematically, the sequence $$F(n)$$ is defined by the recurrence relation:

$$F(n) = F(n-1) + F(n-2)$$

With seed values:

$$F(0) = 0, \quad F(1) = 1$$

The Fibonacci sequence begins as follows:

$$0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ... $$

So, for instance:


<!-- $$
\begin{itemize}
    \item $F(0)$ is 0
    \item $F(1)$ is 1
    \item $F(2)$ is $F(1) + F(0)$ = 1
    \item $F(3)$ is $F(2) + F(1)$ = 2
    \item $F(4)$ is $F(3) + F(2)$ = 3
    \item ... and so on.
\end{itemize}
$$ -->

$$-  F(0) is 0 $$

$$- F(1) \, is \, 1$$

$$- F(2) \, is \, F(1) + F(0) = 1$$

$$- F(3) \, is \, F(2) + F(1) = 2$$

$$- F(4) \, is \, F(3) + F(2) = 3$$

$$- ... and \, so \, on.$$

This sequence has a vast array of applications and interesting properties. It appears in various areas such as mathematics, computer science, and even in nature, where the pattern of leaves, flowers, and fruits often follows the Fibonacci sequence. The sequence is also closely associated with the "Golden Ratio" ($$ \phi $$), which has unique mathematical properties and appears in various natural phenomena and works of art.

1. **Memoization (Top-down approach)**:

```typescript
function fibonacciMemo(
  n: number,
  memo: Map<number, number> = new Map()
): number {
  if (n <= 1) return n;

  // If value is in memo, return it
  if (memo.has(n)) {
    return memo.get(n)!;
  }

  // Otherwise, compute and add to memo
  const value = fibonacciMemo(n - 1, memo) + fibonacciMemo(n - 2, memo);
  memo.set(n, value);
  return value;
}


// Test the function
console.log(fibonacciMemo(10));  // Output: 55
```

1. **Tabulation (Bottom-up approach)**:

```typescript
function fibonacciTab(n: number): number {
  if (n <= 1) return n;

  let fib: number[] = Array(n + 1).fill(0);
  fib[1] = 1;

  for (let i = 2; i <= n; i++) {
    fib[i] = fib[i - 1] + fib[i - 2];
  }

  return fib[n];
}

// Test the function
console.log(fibonacciTab(10));  // Output: 55
```

Both methods will compute the nth Fibonacci number efficiently, but they differ in the way they approach the problem. Memoization is a more intuitive approach for many people since it's closer to the problem's recursive structure, while tabulation usually offers slightly better space efficiency and is more straightforward to convert to an iterative algorithm.
