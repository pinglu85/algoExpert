# Smallest Difference

### Understanding the problem

We are given two arrays of integers. Both of them contain at least one element. We are asked to write a function that is going to find two numbers, one in each array, with the smallest absolute difference, and return them in an array. In the resultant array, the number from the first array should come first.

Example:

<pre>
<b>Input:</b>
arrayOne = [-1, 5, 10, 20, 28, 3]
arrayTwo = [26, 134, 135, 15, 17]

<b>Output:</b> [28, 26]
</pre>

#

### Approach 1: Brute force

We generate all the possible pairs and compare their absolute difference.

**Algorithm**

- Initialize a variable to keep track of the smallest absolute difference so far.

- Initialize an empty array to store the current smallest pair.

- Iterate through the first array. For each number in the first array, start iterating through the second array.

  - Compute the absolute difference between the current number from the first array and the one from the second array.

  - Compare the result to the smallest absolute difference we have so far. If we get a smaller difference, update the current smallest difference and the current smallest pair.

- Return the smallest pair.

### Implementation

```js
function smallestDifference(arrayOne, arrayTwo) {
  let smallestDiff = Infinity;
  const smallestPair = [];

  for (const firstNum of arrayOne) {
    for (const secondNum of arrayTwo) {
      const currentDiff = Math.abs(firstNum - secondNum);

      if (currentDiff < smallestDiff) {
        smallestDiff = currentDiff;
        smallestPair[0] = firstNum;
        smallestPair[1] = secondNum;
      }
    }
  }

  return smallestPair;
}
```

### Complexity Analysis

- Time Complexity: O(N · M), where N and M are the lengths of the input arrays.

- Space Complexity: O(1).

#

### Approach 2: Sorting + Two Pointers

Suppose we want to find the pair of numbers from the following two arrays:

```js
arrayOne = [-1, 5, 10, 20, 28, 3];
arrayTwo = [26, 134, 135, 15, 17];
```

After sorting both arrays in ascending order, we get:

```js
arrayOne = [-1, 3, 5, 10, 20, 28];
arrayTwo = [15, 17, 26, 134, 135];
```

Map them onto a number line:

```
         <--|---|---|---|---|---|---|---|---|---|---|-->
arrayOne:  -1   3   5   10          20      28
arrayTwo:                  15  17      26     134  135
```

It can be noticed that for each number in the range of `-1` to `10` in `arrayOne`, we only need to compute their absolute difference with regards to `15`, because they are all smaller than `15`; since every number in `arrayTwo` that is after `15` is greater than `15`, they are all farther apart than `10` and `15`. For `15` and `17` in `arrayTwo`, we only need to compute the absolute difference between `15` and `20` and the absolute difference between `17` and `20`, and for `15` and `17` we can eliminate all the numbers after `20`. In other words, if a number from one array is smaller than the number from the other array, for instance, `5` from `arrayOne` and `15` from `arrayTwo`, we compute their absolute difference, then move to the number that is after `5` and compute the difference between that number and `15`. The reason is that since our arrays are sorted, the number that is after `5` is definitely greater than `5` and it might be closer to `15` than `5`. In our `arrayOne` the number that is after `5` is `10` and it is indeed closer to `15` than `5`. We don't need to compute the difference between the numbers that are after `15` and `5`, since they are all farther away from `5` than `15`, so for `5`, we don't need to look at the numbers that are after `15`.

Based on the above observation, we can come up with the following solution:

- Sort both arrays.

- Initialize a variable that is going to keep track of the smallest absolute difference so far and initialize an empty array to store the current smallest pair.

- Initialize two pointers `idxOne` and `idxTwo` to `0`. `idxOne` is going to keep track of the index in the first array, and `idxTwo` the index in the second array.

- Loop until both pointers reach the end of the arrays.

  - Compute the absolute difference between the number that `idxOne` points to and the number that `idxTwo` points to. Compare the result to the current smallest difference; if it is smaller, set it as the current smallest difference and update the current smallest pair with these two numbers.

  - Compare the number that `idxOne` points to with the number that `idxTwo` points to. If the former is smaller, move `idxOne` to right by one; if the latter is smaller, move `idxTwo` to right by one; if they are equal to each other, break the loop.

- Return the current smallest pair.

### Implementation

```js
function smallestDifference(arrayOne, arrayTwo) {
  arrayOne.sort((a, b) => a - b);
  arrayTwo.sort((a, b) => a - b);

  let smallestAbsDiff = Infinity;
  const smallestPair = [];
  let idxOne = 0;
  let idxTwo = 0;

  while (idxOne < arrayOne.length && idxTwo < arrayTwo.length) {
    const firstNum = arrayOne[idxOne];
    const secondNum = arrayTwo[idxTwo];
    const currAbsDiff = Math.abs(firstNum - secondNum);

    if (currAbsDiff < smallestAbsDiff) {
      smallestAbsDiff = currAbsDiff;
      smallestPair[0] = firstNum;
      smallestPair[1] = secondNum;
    }

    if (firstNum === secondNum) return smallestPair;

    if (firstNum < secondNum) {
      idxOne++;
    } else {
      idxTwo++;
    }
  }

  return smallestPair;
}
```

### Complexity Analysis

Let N be the length of the first input array and M be the length of the second input array.

- Time Complexity: O(N · log(N) + M · log(M)).

- Space Complexity: O(log(N) + log(M)) or O(N + M), depending on the implementation of the sorting algorithm.
