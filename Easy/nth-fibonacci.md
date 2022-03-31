# Nth Fibonacci

### Understanding the problem

Given an integer `n`, we are asked to write a function that is going to return the `n`th Fibonacci number in the Fibonacci sequence. Normally, the Fibonacci sequence uses zero based indexing, which means the first two numbers of the sequence are `F0 = 0` and `F1 = 1`. However, in this problem, we are going to use one based indexing. For instance, `getNthFib(1)` should return `0` instead of `1`.

#

### Approach 1: Iterative

We keep track of the last two Fibonacci numbers in the Fibonacci sequence and use a counter to remember how many Fibonacci numbers we have calculated. While the counter is less than or equal to `n`, we keep calculating the next Fibonacci number.

**Algorithm**

- Use an array `lastTwoFibs` to keep track of the last two Fibonacci numbers. Initially, the array is going to contain the first two numbers of the Fibonacci sequence, which are `0` and `1`.

- Initialize `counter` to `3`, because we already have the first two Fibonacci numbers.

- While the `counter` is less than or equal to `n`:

  - Calculate the next Fibonacci number by adding up the last two Fibonacci numbers.

  - Updating the last two Fibonacci numbers as well as the `counter`.

- Return the last Fibonacci number or the first number of the Fibonacci sequence if `n` is less than or equal to `1`.

### Implementation

```js
function getNthFib(n) {
  const lastTwoFibs = [0, 1];
  let counter = 3;

  while (counter <= n) {
    const nextFib = lastTwoFibs[0] + lastTwoFibs[1];
    lastTwoFibs[0] = lastTwoFibs[1];
    lastTwoFibs[1] = nextFib;
    counter++;
  }

  return n <= 1 ? lastTwoFibs[0] : lastTwoFibs[1];
}
```

### Complexity Analysis

- Time Complexity: O(n), where n is the input number.

- Space Complexity: O(1).

#

### Approach 2: Naive Recursive Approach

The math definition of a Fibonacci number is `F(n) = F(n - 1) + F(n - 2), for n > 1`. The naive recursive solution is going to be similar to this math definition.

Since the question here uses one based indexing, the base case of the recursive function is going to be the following:

- If `n` is equal to `1`, return `0`.

- If `n` is equal to `2`, return `1`.

The recursive part is going to be identical to the math equation. We are going to just return `F(n - 1) + F(n - 2)`, where `F` is our recursive function.

### Implementation

```js
function getNthFib(n) {
  if (n === 1) return 0;
  if (n === 2) return 1;

  return getNthFib(n - 1) + getNthFib(n - 2);
}
```

### Complexity Analysis

Let n be the input number.

- Time Complexity: O(2^n).

  In each step we are going to call the recursive function twice, which leads us to approximately 2 \* 2 \* 2 .... 2 = 2^n operations(additions) for nth Fibonacci number.

  The time complexity can also be estimated by drawing the recursion tree:

  ```
                              F(n)
                            /      \
  ^                   F(n-1)      F(n-2)       -------- maximum 2^1 = 2 additions
  |                   /    \      /    \
  |               F(n-2) F(n-3) F(n-3) F(n-4)  -------- maximum 2^2 = 4 additions
  n-1 levels       /    \
  |            F(n-3) F(n-4)                   -------- maximum 2^3 = 8 additions
  |                                                      ........
  v                                            -------- maximum 2^(n-1) additions
  ```

  So the total number of additions is going to be 2 + 2^2 + 2^3 + 2^4 + ... + 2^(n-1), which is approximately equal to 2^(n-1) + 2^(n-1) = 2 \* 2^(n-1), thus the time complexity is O(2^n).

- Space Complexity: O(n), because we are going to have at most n function calls on the call stack.

#

### Approach 3: Recursive Approach with Memoization

The naive recursive approach has repeated calls for same inputs. We can optimize it by memoizing the results of function calls. At every recursive call we are going to pass down an object which is going to store the Fibonacci numbers we have calculated. In this object, each key is going to be a input number and the values are going to be the corresponding Fibonacci number. Initially, the object is going to hold the first two numbers of the Fibonacci sequence. At each recursion, we are going to lookup the input number in the object. If it is already a key in the object, we can just return the corresponding Fibonacci number. Otherwise, we compute the Fibonacci number for that input number, and store them in the object.

### Implementation

```js
function getNthFib(n, memoized = { 1: 0, 2: 1 }) {
  if (n in memoized) return memoized[n];

  memoized[n] = getNthFib(n - 1, memoized) + getNthFib(n - 2, memoized);
  return memoized[n];
}
```

### Complexity Analysis

Let n be the input number.

- Time Complexity: O(n), because we only calculate each Fibonacci number once:

  ```
                F(5)
              /     \
            F(4)     F(3)    -------- F(3)'s result is memoized.
          /    \
        F(3)   F(2)         -------- F(2)'s result is memoized.
        /    \
      F(2)   F(1)
    /    \
  F(0)    F(1)
  ```

- Space Complexity: O(n).
