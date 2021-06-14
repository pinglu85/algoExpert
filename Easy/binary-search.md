# Binary Search

### Understanding the problem

Given a sorted array of integers and a target integer, I am asked to write a function that uses Binary Search to find out if the target integer is in the array. If the target is in the array, the function should return its index; otherwise return `-1`.

#

### Iterative Approach

The overarching logic of Binary Search is:

1. Find the middle number in the sorted array.
2. Compare the middle number to the target. Eliminate half of the array, depending on where the target would be located compared to the middle number:

   - If the target is equal to the middle number, then we have found the target.

   - If the target is smaller than the middle number, then it means the target would be located in the left half of the array, because all the integers to the right of the middle number are for sure greater than the middle number, and so even greater than the target.

   - If the target is greater than the middle number, then the target would be located in the right half of the array, because all the numbers to the left of the middle number must be smaller than the middle number, and thus even smaller than the target.

3. Find the middle number in the remaining half of the array and continue as in step 2. Eventually, we will find the target or there is no more array to explore, which means the target is not found in the array.

I am going to initialize two pointers `left` and `right` to keep track of the current subarray that remains to be explored. Initially, the `left` pointer is going to point at the start of the array and the `right` pointer at the end of the array. While the `left` pointer doesn't surpass the `right` pointer, keep finding the middle number in the current subarray; if the target is equal to the middle number, then I have found the target, so return the index of the middle number; otherwise update either the left pointer or the right pointer based on the comparison, eliminating half of the current subarray. Finally, if I get out of the while loop without returning the result, then it means the target has not been found and it can not be found, return `-1`.

### Time & Space Complexity

O(log(n)) time | O(1) space, where n is the number of integers in the input array.

### Iterative Solution

```js
function binarySearch(array, target) {
  let left = 0;
  let right = array.length - 1;
  while (left <= right) {
    const middleIdx = Math.floor((left + right) / 2);
    const potentialMatch = array[middleIdx];

    if (target === potentialMatch) return middleIdx;

    if (target < potentialMatch) {
      right = middleIdx - 1;
    } else {
      left = middleIdx + 1;
    }
  }
  return -1;
}
```

#

### Recursive Approach

Instead of using a while loop, I am going to define a recursive function that is going to be called on each subarray that remains to be explored. The recursive function is going to take in the input array, the target integer and two pointers, the `left` pointer and the `right` pointer, which specify the starting index and the ending index of the subarray to be explored. The base case of the recursive function is the `left` pointer is greater than the `right` pointer.

### Time & Space Complexity

O(log(n)) time | O(log(n)) space, where the n is the length of the input array.

### Recursive Solution

```js
function binarySearch(array, target) {
  return binarySearchImpl(array, target, 0, array.length - 1);
}

function binarySearchImpl(array, target, left, right) {
  if (left > right) return -1;

  const middleIdx = Math.floor((left + right) / 2);
  const potentialMatch = array[middleIdx];

  if (target === potentialMatch) return middleIdx;

  if (target < potentialMatch) {
    return binarySearchImpl(array, target, left, middleIdx - 1);
  } else {
    return binarySearchImpl(array, target, middleIdx + 1, right);
  }
}
```
