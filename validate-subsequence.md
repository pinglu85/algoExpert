### Understanding the problem

Implement a function that takes two arrays of integers as input and finds out whether all the numbers in the second array are in the first array. They don't need to be adjacent but have to be in the same order as they are in the first array.

### Approach

First compare the length of both arrays. If the size of the second array is larger than the first one, then the second array can not be a subsequence of the first one. Then use two pointers i and j to keep track of the indices of the two arrays. Iterate through the first array. At each iteration, compare the ith integer in the first array with the jth integer in the second array, if they are equal, increase j by 1. After the loop finishes, if the pointer j is equal to the length of the second array, then it means all the numbers in the second array are found in the first array and they are in the same order, thus the second array is a subsequence of the first one. Otherwise it is not.

### Solution

```js
function isValidSubsequence(array, sequence) {
  if (sequence.length > array.length) {
    return false;
  }

  let j = 0;
  for (let i = 0; i < array.length; i++) {
    if (array[i] === sequence[j]) {
      j++;
    }
  }

  return j === sequence.length ? true : false;
}
```
