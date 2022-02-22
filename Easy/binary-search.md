# Binary Search

### Understanding the problem

Given a sorted array of integers and a target integer, I am asked to write a function that uses Binary Search algorithm to find out if the target integer is in the array. If the target is in the array, the function should return its index, otherwise return `-1`.

#

### Approach 1: Iterative

The Binary Search algorithm divides a sorted list of values into halves, and keeps narrowing down the search space until the target value is found. We maintain two pointers `left` and `right` that are going to keep track of the current search space. Initially, the search space is the entire list. We use the two pointers to find the middle value and then compare it to the search target. If the middle value is equal to the target value, then we've found the target value. If the middle value is greater than the target, then it means the target value cannot lie in the right half of the search space, since the list is sorted and all the values that come after the middle value must be greater than the middle value and even greater than the target, so we can remove the entire right half, narrowing down the search space to the left half. If the middle value is smaller than the target, then we can eliminate the entire left half and search for the target value in the right half. We repeat the process until the target value is found, or our search space is exhausted.

### Implementation

```js
function binarySearch(array, target) {
  let left = 0;
  let right = array.length - 1;

  while (left <= right) {
    // Avoid overflow
    const mid = left + Math.floor((right - left) / 2);

    if (target === array[mid]) return mid;

    if (target < array[mid]) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }
  return -1;
}
```

### Complexity Analysis

- Time Complexity: O(log(n)), where n is the number of integers in the input array.

- Space Complexity: O(1).

#

### Approach 3: Recursive

We are going to define a recursive function that is going to take in the input array, the search target and two pointers `left` and `right` as parameters. We find the middle value using the two pointers, and call the recursive function on the remaining half. The recursion stops when the target is found or the `left` pointer surpasses the `right` pointer.

### Implementation

```js
function binarySearch(array, target) {
  return binarySearchImpl(array, target, 0, array.length - 1);
}

function binarySearchImpl(array, target, left, right) {
  if (left > right) return -1;

  // Avoid overflow
  const mid = left + Math.floor((right - left) / 2);

  if (target === array[mid]) return mid;

  if (target < array[mid]) {
    return binarySearchImpl(array, target, left, mid - 1);
  } else {
    return binarySearchImpl(array, target, mid + 1, right);
  }
}
```

### Complexity Analysis

- Time Complexity: O(log(n)), where n is the number of integers in the input array.

- Space Complexity: O(log(n)) to keep the recursion stack.
