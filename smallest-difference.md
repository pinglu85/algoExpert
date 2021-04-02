### Understanding the problem

I am given two arrays of integers. I am asked to write a function that finds two numbers, one from each array, whose absolute difference is closest to zero, and returns them in an array where the number from the first array comes first.

For example,

```js
arrayOne = [-1, 5, 10, 20, 28, 3];
arrayTwo = [26, 134, 135, 15, 17];
```

The output should be `[28, 26]`.

### Approach 1 - O(n\*m) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

- initialize a variable that is going to keep track of the smallest absolute difference so far.
- initialize an empty array to store the current pair.
- iterate through every number in the first array. For each number, start iterating through the every number in the second array, compute the absolute difference between the number and each number in the second array, if I get a smaller absolute difference, update the current smallest difference and replace the previous pair in the array with the current pair.

### Solution 1

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

### Approach 2 - O(nlog(m) + mlog(m)) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

- initialize a variable that is going to keep track of the smallest absolute difference so far.
- initialize an empty array to store the current pair.
- sort the second input array.
- iterate through every number in the first array. For each number, use binary search to find the correct position in the sorted array that the number should be put into, compute the absolute difference between that number and the number in the sorted array that should be before it, and the absolute difference between that number and the number in the sorted array that should be after it.

For instance,

```js
arrayOne = [20, -1, 5, 10, 28, 3];
sortedArrayTwo = [15, 17, 26, 134, 135];
```

I am now at index 0 in the `arrayOne`, that is `20`. `20` should be inserted between `17` and `26` in the `sortedArrayTwo`. The absolute difference between `20` and `17` is `3`, which means the difference between `20` and every number before `17` in the `sortedArrayTwo` is gonna larger than `3`, since every number before `17` is smaller than `17` and that means they are farther away from `20` than `17`, so for `20` there is no need to visit them. The absolute difference between `20` and `26` is `6`, which means every number after `26` is farther away from `20` than `26`, so for `20` we can eliminate them.

Compare the two absolute differences to the current smallest difference; if one of the absolute differences is smaller than the current smallest difference, set it as the current smallest difference and replace the previous pair in the array with the current pair. In addition, if one of the absolute difference is equal to 0, return the current pair.

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

### Best Approach - O(nlog(n) + m(log(m))) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

Consider I am given the following two arrays:

```js
arrayOne = [-1, 5, 10, 20, 28, 3];
arrayTwo = [26, 134, 135, 15, 17];
```

After sorting both of them, I get:

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

It can be noticed that for each number in the range of `-1` to `10` in `arrayOne`, I only need to compute the absolute difference between the number and `15`, because they are all smaller than `15`, and every number in `arrayTwo` that is after `15` is larger than `15`, which means they are all farther away from `10` than `15`. For `15` and `17` in `arrayTwo`, I only need to compute the absolute difference between `15` and `20` and the absolute difference between `17` and `20`.

Based on the above observation, I can come up with the following solution:

- sort both arrays.
- initialize a variable that is going to keep track of the smallest absolute difference so far and initialize an empty array to store the current pair.
- initialize two pointers `idxOne` and `idxTwo` to `0`. `idxOne` is going to keep track of the index in the first array, and `idxTwo` the index in the second array.
- loop until both pointers out of bounds.
  - compute the absolute difference between the number that `idxOne` points to and the number that `idxTwo` points to. Compare the result to the current smallest difference; if it is smaller, set it as the current smallest difference and replace the previous pair in the array with the current pair.
  - compare the number that `idxOne` points to with the number that `idxTwo` points to. If the former is smaller, move `idxOne` to right by one; if the latter is smaller, move `idxTwo` to right by one; if they are equal to each other, break the loop.
- return the current pair.

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
