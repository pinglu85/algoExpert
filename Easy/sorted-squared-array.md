# Sorted Squared Array

### Understanding the problem

Given an array of integers that are sorted in increasing order, we are asked to write a function that squares all the integers in the array and returns them in a new array. The returned array must be sorted in increasing order as well.

#

### Approach 1

At first glance, we might think all we need to do is traverse the input array element by element, square each value and return the resultant array.

However, this approach only works for positive integers.

Negative numbers become positive when squared. If we put them directly into the resultant array, the array would no longer be sorted. We could sort the resultant array before returning it, but then the solution will take O(N · log(N)) time.

Instead of sorting, we can store the squares of non-negative and negative integers separately by using two arrays. After squaring all the integers, we merge the two arrays into one sorted array:

- Let `m` be the length of the first array and `n` be the length of the second array. Initialize an empty array `merged` of length `m + n`.

- Initialize two pointers that are going to remember the position we are at in the two arrays respectively. Point the pointer for the non-negative array to the first number of the array, and the one for the negative array to the last number of the array. The reason for that is because smaller negative numbers become bigger after squaring, and since the input array is sorted in ascending order, the squares of all the negative numbers will be in descending order.

- Iterate through the `merged` array. At each iteration, we compare the values that the two pointers are pointing to. Put the smaller value into `merged[i]` and move the corresponding pointer. To handle the situation where the end of one of the arrays is reached, we can set the corresponding value to a very large number.

### Implementation

```js
function sortedSquaredArray(array) {
  const positiveSquares = [];
  const negativeSquares = [];

  for (const value of array) {
    const square = value * value;

    if (value < 0) {
      negativeSquares.push(square);
    } else {
      positiveSquares.push(square);
    }
  }

  return mergeSortedArrays(positiveSquares, negativeSquares);
}

function mergeSortedArrays(ascendingArray, descendingArray) {
  const merged = new Array(ascendingArray.length + descendingArray.length);
  let ascendingIdx = 0;
  let descendingIdx = descendingArray.length - 1;

  for (let i = 0; i < merged.length; i++) {
    const ascendingItem = ascendingArray[ascendingIdx] ?? Infinity;
    const descendingItem = descendingArray[descendingIdx] ?? Infinity;

    if (ascendingItem < descendingItem) {
      merged[i] = ascendingItem;
      ascendingIdx++;
    } else {
      merged[i] = descendingItem;
      descendingIdx--;
    }
  }

  return merged;
}
```

### Complexity Analysis

Given N as the length of the input array.

- Time Complexity: O(N).

- Space Complexity: O(N).

#

### Approach 2: Two Pointers

Suppose the input array is `[-4, -2, 0, 1, 3]`. After being squared, we get `[16, 4, 0, 1, 9]`. We can notice two things:

1. The smallest possible squared value we get is `0`.

2. The farther the value is away from `0`, whether it is positive or negative, the larger the squared value will become.

```
       <--|---|---|---|---|---|---|---|-->
number:  -4  -3  -2  -1   0   1   2   3
square:  16   9   4   1   0   1   4   9
```

Therefore, the largest value in the output array comes either from the smallest element in the input array or from the largest element in the input array.

Since we know where to find the largest squared value in the input array, we can build the output array from the largest square value to the smallest squared value. We keep two pointers to track the smallest and the largest integers in the input array. Compare the absolute value of the two integers to find out which one is greater once squared. We put the larger squared value into the output array and move the corresponding pointer either to left or to right. Keep doing this until the output array is filled up.

### Implementation

```js
function sortedSquaredArray(array) {
  const sortedSquares = new Array(array.length);
  let smallerValueIdx = 0;
  let largerValueIdx = array.length - 1;

  for (let i = array.length - 1; i >= 0; i--) {
    const smallerValue = array[smallerValueIdx];
    const largerValue = array[largerValueIdx];

    if (Math.abs(smallerValue) >= Math.abs(largerValue)) {
      sortedSquares[i] = smallerValue * smallerValue;
      smallerValueIdx++;
    } else {
      sortedSquares[i] = largerValue * largerValue;
      largerValueIdx--;
    }
  }

  return sortedSquares;
}
```

### Complexity Analysis

Given N as the length of the input array.

- Time Complexity: O(N).

- Space Complexity: O(N).
