# Nth Fibonacci

### Understanding the problem

I am given an integer `n` and I am asked to write a function that is going to return the nth Fibonacci number in the Fibonacci sequence. Normally, the Fibonacci sequence uses zero based indexing, meaning the first two numbers are `F0 = 0` and `F1 = 1`. However, in this problem, one based indexing is used, for instance, `getNthFib(1)` should return `0`.

#

### Iterative Approach - O(n) time | O(1) space, where n is the input number.

I would initialize an array that is going to keep track of the last two Fibonacci numbers. Initially, the array contains the value of `F0`, which is `0`, and the value of `F1`, which is `1`. Then I would use a for loop that starts at `3`, because I already have `F0` and `F1`, and ends at `n`, to keep track of how many Fibonacci numbers I have calculated. At each iteration, I am going to add the last two Fibonacci numbers up and their sum will become my new last Fibonacci number; then I will update the array that stores the last two Fibonacci numbers. Lastly, use a ternary operator to take care of the edge case where `n` is equal to `1`, because when `n` is `1`, I should return the value of `F0`; otherwise return the last Fibonacci number.

### Iterative Solution

```js
function getNthFib(n) {
  const lastTwoFibs = [0, 1];

  for (let idx = 3; idx <= n; idx++) {
    const nextFib = lastTwoFibs[0] + lastTwoFibs[1];
    lastTwoFibs[0] = lastTwoFibs[1];
    lastTwoFibs[1] = nextFib;
  }

  return n <= 1 ? lastTwoFibs[0] : lastTwoFibs[1];
}
```

#

### Naive Recursive Approach - O(2^n) time | O(n) space, where n is the input number.

The math definition of a Fibonacci number is `F(n) = F(n - 1) + F(n - 2), for n > 1`. The naive recursive solution is going to be similar to this math definition.

Since the question here uses one based indexing, the base case of the recursive function is going to be the following:

- If `n` is equal to `1`, return `0`;
- If `n` is equal to `2`, return `1`.

The recursive part is going to be identical to the math equation. I am going to return `F(n - 1) + F(n - 2)`, where `F` is the recursive function.

**Note regarding the time space complexity**

The time complexity of this approach is O(2^n) or exponential, because in each step we are going to call the recursive function twice, which leads us to approximately `2 * 2 * 2 .... 2 = 2^n` operations(additions) for nth Fibonacci number.

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

So the total number of additions is going to be `2 + 2^2 + 2^3 + 2^4 + ... + 2^(n-1)`, which is approximately equal to `2^(n-1) + 2^(n-1) = 2 * 2^(n-1)`, thus the time complexity is O(2^n).

The space complexity is O(n), because we are going to have at most `n` function calls on the call stack.

### Naive Recursive Solution

```js
function getNthFib(n) {
  if (n === 1) return 0;
  if (n === 2) return 1;

  return getNthFib(n - 1) + getNthFib(n - 2);
}
```

#

### Recursive Approach with Dynamic Programming - O(n) time | O(n) space, where n is the input number.

Since the naive recursive solution has repeated calls for same inputs, I can memoize the results of function calls to avoid repeated computations. So to do that, at every recursive call I am going to pass down an object which is going to keep track of the results that I have computed. In this object, each key is going to be a input number and the values are going to be the result for each input. Initially, the object is going to hold the value of `F0` and the value of `F1`. At each recursion, I am going to lookup the input number in the object; if it is already a key in the object, return the result for that input; otherwise, compute the result, and store the input and the corresponding result in that object.

**Note regarding the time complexity**

The time complexity of this approach is going to be O(n), because we only calculate each Fibonacci number once:

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

### Recursive Solution with Dynamic Programming

```js
function getNthFib(n, memoized = { 1: 0, 2: 1 }) {
  if (n in memoized) return memoized[n];

  memoized[n] = getNthFib(n - 1, memoized) + getNthFib(n - 2, memoized);
  return memoized[n];
}
```
