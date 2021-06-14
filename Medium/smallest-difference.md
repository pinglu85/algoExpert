# Smallest Difference

### Understanding the problem

Given two arrays of integers, I am asked to write a function that finds two numbers, where one number comes from the first array and another number comes from the second array, with smallest absolute difference, and returns them in an array. In the resulting array, the number from the first array should come first.

For example,

```js
arrayOne = [-1, 5, 10, 20, 28, 3];
arrayTwo = [26, 134, 135, 15, 17];
```

The output is `[28, 26]`.

#

### Brute-force Approach

- Initialize a variable that is going to keep track of the smallest absolute difference so far.
- Initialize an empty array to store the current smallest pair.
- Iterate through every number in the first array. For each number, start iterating through the every number in the second array, compute the absolute difference between the number and each number in the second array, if I get a smaller absolute difference, update the current smallest difference and the current smallest pair.

### Time & Space Complexity

O(n\*m) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

### Brute-force Solution

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

#

### Approach 2

- Initialize a variable that is going to keep track of the smallest absolute difference so far.
- Initialize an empty array to store the current smallest pair.
- Sort the second input array in ascending order.
- Iterate through every number in the first array. For each number, use binary search to find the correct position in the sorted array that the number should be put into, compute the absolute difference between that number and the number in the sorted array that should be before it, and the absolute difference between that number and the number in the sorted array that should be after it.

Suppose we have the following two arrays:

```js
arrayOne = [20, -1, 5, 10, 28, 3];
sortedArrayTwo = [15, 17, 26, 134, 135];
```

We are now at index 0 in the `arrayOne` and the number is `20`. `20` should be inserted between `17` and `26` in the `sortedArrayTwo`. The absolute difference between `20` and `17` is `3`, which means the difference between `20` and every number before `17` in the `sortedArrayTwo` is going to be greater than `3`, since every number before `17` is smaller than `17` and that means they are farther away from `20` than `17`, so for `20` there is no need to look at them. The absolute difference between `20` and `26` is `6`, which means every number after `26` is farther away from `20` than `26`, so for `20` we can eliminate them.

Compare the two absolute differences to the current smallest difference; if one of the absolute differences is smaller than the current smallest difference, set it as the current smallest difference and update the current smallest pair. If one of the absolute difference is equal to 0, return the current smallest pair.

- Return the current smallest pair after we reach the very end of the first array.

### Time & Space Complexity

O(nlog(m) + mlog(m)) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

### Solution 2

```js
function smallestDifference(arrayOne, arrayTwo) {
  arrayTwo.sort((a, b) => a - b);

  let smallestDiff = Infinity;
  const smallestPair = [];

  for (const firstNum of arrayOne) {
    const [numBefore, numAfter] = findNumBeforeAndAfter(arrayTwo, firstNum);
    let currentDiff = Math.abs(firstNum - numBefore);
    if (currentDiff < smallestDiff) {
      smallestDiff = currentDiff;
      smallestPair[0] = firstNum;
      smallestPair[1] = numBefore;
    }

    currentDiff = Math.abs(firstNum - numAfter);
    if (currentDiff < smallestDiff) {
      smallestDiff = currentDiff;
      smallestPair[0] = firstNum;
      smallestPair[1] = numAfter;
    }

    if (currentDiff === 0) break;
  }

  return smallestPair;
}

function findNumBeforeAndAfter(array, target) {
  let startIdx = 0;
  if (target <= array[startIdx]) {
    return [-Infinity, array[startIdx]];
  }

  let endIdx = array.length - 1;
  if (target >= array[endIdx]) {
    return [array[endIdx], Infinity];
  }

  while (startIdx <= endIdx) {
    const midIdx = Math.floor((startIdx + endIdx) / 2);
    if (array[midIdx] === target) {
      return [array[midIdx], Infinity];
    }

    if (array[midIdx] > target) {
      endIdx = midIdx - 1;
    } else {
      startIdx = midIdx + 1;
    }
  }

  return [array[endIdx], array[startIdx]];
}
```

#

### Best Approach

Suppose we want to find the pair of numbers from the following two arrays:

```js
arrayOne = [-1, 5, 10, 20, 28, 3];
arrayTwo = [26, 134, 135, 15, 17];
```

After sorting both arrays in ascending order, I get:

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

### Time & Space Complexity

O(nlog(n) + m(log(m))) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

### Best Solution

```js
function smallestDifference(arrayOne, arrayTwo) {
  arrayOne.sort((a, b) => a - b);
  arrayTwo.sort((a, b) => a - b);

  let smallestDiff = Infinity;
  const smallestPair = [];
  let idxOne = 0;
  let idxTwo = 0;

  while (idxOne < arrayOne.length && idxTwo < arrayTwo.length) {
    const firstNum = arrayOne[idxOne];
    const secondNum = arrayTwo[idxTwo];
    const currentDiff = Math.abs(firstNum - secondNum);
    if (currentDiff < smallestDiff) {
      smallestDiff = currentDiff;
      smallestPair[0] = firstNum;
      smallestPair[1] = secondNum;
    }

    if (firstNum < secondNum) {
      idxOne++;
    } else if (firstNum > secondNum) {
      idxTwo++;
    } else {
      break;
    }
  }

  return smallestPair;
}
```
