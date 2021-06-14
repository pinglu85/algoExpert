# Validate Subsequence

### Understanding the problem

Implement a function that takes two arrays of integers as input and finds whether all the numbers in the `sequence` array appear in the first array and they appear in the same order. In other words, the function need to find out if we can get the `sequence` array, when we delete some or no elements in the first array without changing the order of the remaining elements.

For example:

```js
array = [5, 1, 22, 25, 6, -1, 8, 10];
sequence = [1, 6, -1, 10];
```

The output should be `true`.

#

### Approach

Use a pointer to keep track of the position we are at in the `sequence` array. Iterate through every integer in the first array. At each iteration, compare the integer in the first array with the value in the `sequence` array that the pointer currently points to, if they are equal, then we found the value in the first array, move the pointer forward by 1. If the pointer is equal to the length of the `sequence` array, then it means all the numbers in the `sequence` array are found in the first array and they are in the same order, return `true`. After the loop finishes, if the pointer is not equal to the length of the `sequence` array, return `false`.

### Time & Space Complexity

O(n) time | O(1) space, where n is the length of the array.

### Solution 1

```js
function isValidSubsequence(array, sequence) {
  let seqIdx = 0;
  for (const value of array) {
    if (value === sequence[seqIdx]) seqIdx++;
    if (seqIdx === sequence.length) return true;
  }

  return false;
}
```

### Solution 2

```js
function isValidSubsequence(array, sequence) {
  let arrIdx = 0;
  let seqIdx = 0;
  while (arrIdx < array.length && seqIdx < sequence.length) {
    if (array[arrIdx] === sequence[seqIdx]) {
      seqIdx++;
    }
    arrIdx++;
  }

  return seqIdx === sequence.length;
}
```
