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
- use two variables to keep track of the current pair.
- iterate through every number in the first array. For each number, start iterating through the every number in the second array, compute the absolute difference between the number and each number in the second array, if I get a smaller absolute difference, update the current smallest difference and the variables that store the current pair.

### Solution 1

```js
function smallestDifference(arrayOne, arrayTwo) {
  let smallestDifference = Infinity;
  let firstNum = 0;
  let secondNum = 0;

  for (const num1 of arrayOne) {
    for (const num2 of arrayTwo) {
      const currentDifference = Math.abs(num1 - num2);
      if (currentDifference < smallestDifference) {
        smallestDifference = currentDifference;
        firstNum = num1;
        secondNum = num2;
      }
    }
  }

  return [firstNum, secondNum];
}
```

### Approach 2 - O(nlog(m) + mlog(m)) time | O(1) space, where n is the length of the first input array, and m is the length of the second input array.

- initialize a variable that is going to keep track of the smallest absolute difference so far.
- use two variables to keep track of the current pair.
- sort the second input array.
- Iterate through every number in the first array. For each number, use binary search to find the correct position in the sorted array that the number should be put into, compute the absolute difference between that number and the number in the sorted array that should be before it, and the absolute difference between that number and the number in the sorted array that should be after it.

For instance,

```js
arrayOne = [20, -1, 5, 10, 28, 3];
sortedArrayTwo = [15, 17, 26, 134, 135];
```

I am now at index 0 in the `arrayOne`, that is `20`. `20` should be inserted between `17` and `26` in the `sortedArrayTwo`. The absolute difference between `20` and `17` is `3`, which means the difference between `20` and every number before `17` in the `sortedArrayTwo` is gonna larger than `3`, since every number before `17` is smaller than `17` and that means they are farther away from `20` than `17`, so for `20` there is no need to visit them. The absolute difference between `20` and `26` is `6`, which means every number after `26` is farther away from `20` than `26`, so for `20` we can eliminate them.

Compare the two absolute differences to the current smallest difference; if one of the absolute differences is smaller than the current smallest difference, update the current smallest difference and the variables that store the current pair. In addition, if one of the absolute difference is equal to 0, then I can return the pair.

### Solution 2

```js
function smallestDifference(arrayOne, arrayTwo) {
  arrayTwo.sort((a, b) => a - b);

  let currentSmallestDifference = Infinity;
  let firstNum = 0;
  let secondNum = 0;

  for (const num1 of arrayOne) {
    const [numBefore, numAfter] = findNumBeforeAndAfter(arrayTwo, num1);
    let currentDifference = Math.abs(num1 - numBefore);
    if (currentDifference < currentSmallestDifference) {
      currentSmallestDifference = currentDifference;
      firstNum = num1;
      secondNum = numBefore;
    }

    currentDifference = Math.abs(num1 - numAfter);
    if (currentDifference < currentSmallestDifference) {
      currentSmallestDifference = currentDifference;
      firstNum = num1;
      secondNum = numAfter;
    }

    if (currentDifference === 0) break;
  }

  return [firstNum, secondNum];
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
