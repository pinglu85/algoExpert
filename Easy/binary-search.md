# Binary Search

### Understanding the problem

Given a sorted array of integers and a target integer, I am asked to write a function that uses Binary Search algorithm to find out if the target integer is in the array. If the target is in the array, the function should return its index, otherwise return `-1`.

#

### Approach 1: Iterative

The Binary Search algorithm works by dividing the search space into halves, and based on the value of the middle element, it eliminates half of the search space.

We maintain two pointers `left` and `right` that are going to define the current search space. Initially, the search space is the entire list. We find the middle value using the two pointers and compare it to the search target. If the middle value is equal to the target value, we've found the target. If the middle value is greater than the target, then it means the target value cannot lie in the right half of the search space. Because the list is sorted, all the values that come after the middle value must be greater than the middle value and even greater than the target, so we can remove the entire right half, narrowing down the search space to the left half. If the middle value is smaller than the target, then we can discard the entire left half and search for the target value in the right half. We repeat this process until the target value is found, or our search space is exhausted.

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

- Time Complexity: O(log(N)), where N is the number of integers in the input array.

- Space Complexity: O(1).

#

### Approach 2: Recursive

We are going to define a recursive function that is going to take in the input array, the search target and two pointers `left` and `right` as parameters. We find the middle value using the two pointers and compare it to the search target. We then recurse either on the left half or on the right half based on the result of comparison. The recursion stops when the target is found or the `left` pointer surpasses the `right` pointer.

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

- Time Complexity: O(log(N)), where N is the number of integers in the input array.

- Space Complexity: O(log(N)) to keep the recursion stack.

#

### Approach 3: Advanced Form of Binary Search

Instead of `right = mid - 1`, we set `right = mid`. This form of binary search is used when the middle number might be our candidate. An example of this is the problem [278. First Bad Version](https://leetcode.com/problems/first-bad-version/).

In order to make the algorithm work, we also need to replace the loop condition `left <= right` with `left < right`. This is because when our search space contains only one element and we should move `right`: `right = mid`, if the loop condition is `left <= right`, our algorithm will never terminate.

```
[ a,   b ]
      ^ ^
      l r
      ^
      m
```

Since the loop ends when we have one element left (`left == right`), we need to determine whether the remaining element satisfy our condition.

### Implementation

```js
function binarySearch(array, target) {
  let left = 0;
  let right = array.length - 1;

  while (left < right) {
    const mid = left + Math.floor((right - left) / 2);

    if (array[mid] === target) return mid;

    if (array[mid] < target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }

  return array[left] === target ? left : -1;
}
```

### Complexity Analysis

- Time Complexity: O(log(N)), where N is the number of integers in the input array.

- Space Complexity: O(1).
