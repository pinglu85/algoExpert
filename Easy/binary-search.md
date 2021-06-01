# Binary Search

### Understanding the problem

Given a sorted array of integers and a target integer, I am asked to write a function that uses Binary Search to find out if the target integer is in the array. If the target is in the array, the function should return its index; otherwise return `-1`.

### Iterative Approach - O(log(n)) time | O(1) space, where n is the number of integers in the input array.

The overarching logic of Binary Search is:

1. Compare the target integer to the middle element in the array. If the target integer is smaller than the middle integer, then it means the target is going to be located in the left half of the array; otherwise it is going to be located in the right half of the array;
2. Eliminate half of the array that for sure doesn't contain the target, which means we don't need to explore the integers in that half;
3. Continue this process until we eventually find the target integer in the array or the target is not contained in the array.

I am going to create two pointers that are going to keep track of the start position and the end position of the search range in the array; initially, the start pointer is going to point to the start of the array and the end pointer to the end of the array. While the start pointer does not exceed the end pointer, keep getting the integer that is at the middle of the search range; if the target is equal to the integer, then it means I find the target, so break out of the while loop, return the index of the middle integer; otherwise figure out which half of the search range might contains the target, then eliminate the rest half and update the search range. Finally, if I get out of the while loop without returning the result, which means the target is not found in the array, return `-1`.

### Iterative Solution

```js
function binarySearch(array, target) {
  let startIdx = 0;
  let endIdx = array.length - 1;
  while (startIdx <= endIdx) {
    const middleIdx = Math.floor((startIdx + endIdx) / 2);
    const potentialMatch = array[middleIdx];

    if (target === potentialMatch) return middleIdx;

    if (target < potentialMatch) {
      endIdx = middleIdx - 1;
    } else {
      startIdx = middleIdx + 1;
    }
  }
  return -1;
}
```

### Recursive Approach - O(log(n)) time | O(log(n)) space, where the n is the length of the input array.

Instead of using a while loop, I am going to define a recursive function that is going to be called on each search range. I am going to pass in the start position and the end position of the search range to the recursive function, and also pass in the array and the target integer. The base case of the recursive function is going to be a start position that exceeds the end position.

### Recursive Solution

```js
function binarySearch(array, target) {
  return binarySearchImpl(array, target, 0, array.length - 1);
}

function binarySearchImpl(array, target, startIdx, endIdx) {
  if (startIdx > endIdx) return -1;

  const middleIdx = Math.floor((startIdx + endIdx) / 2);
  const potentialMatch = array[middleIdx];

  if (target === potentialMatch) return middleIdx;

  if (target < potentialMatch) {
    return binarySearchImpl(array, target, startIdx, middleIdx - 1);
  } else {
    return binarySearchImpl(array, target, middleIdx + 1, endIdx);
  }
}
```
