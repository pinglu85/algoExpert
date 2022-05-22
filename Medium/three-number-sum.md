# Three Number Sum

### Understanding the problem

We are given an array of distinct integers and an integer that represents a target sum. We are asked to write a function that is going to find all the triplets in the array that sum to the target value and return them in a 2-dimensional array. The triplets should be ordered in ascending order, and so are the numbers in each triplets. For instance, if the triplets we found are: `[5, -6, 1]`, `[2, -8, 6]` and `[3, 5, -8]`, then we should return `[[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]`. If there is no such triplet, return an empty array.

#

### Approach: Sorting + Two Pointers

Since the triplets must sums to the target value, `a + b + c = targetSum`, where `a`, `b` and `c` denote the values in a triplet. Rewriting the equation, we can get:

```
a + b + c   = targetSum
a + (b + c) = targetSum
b + c       = targetSum - a
```

We can notice that it becomes a Two Number Sum problem: Find the pair in the rest of the array that adds up to `targetSum - a`. Hence, we can solve the problem by iterating through every integers in the array and for each integer apply one of the Two Number Sum problem's solutions to find the other two numbers in the rest of the array. There are two ways to solve the Two Number Sum problem, one uses a hashmap to find complement values and the other one uses the two pointers approach and requires the array to be sorted. Since we are asked to return the triplets in ascending order and the values in each triplet need to be ordered in ascending order as well, it would be sensible to use the two pointers approach.

### Implementation

```js
function threeNumberSum(array, targetSum) {
  array.sort((a, b) => a - b);

  const triplets = [];

  for (let i = 0; i < array.length - 2; i++) {
    twoNumberSum(array, i, targetSum, triplets);
  }

  return triplets;
}

function twoNumberSum(array, i, targetSum, triplets) {
  let left = i + 1;
  let right = array.length - 1;

  while (left < right) {
    const currSum = array[i] + array[left] + array[right];

    if (currSum === targetSum) {
      triplets.push([array[i], array[left], array[right]]);
      left++;
      right--;
    } else if (currSum > targetSum) {
      right--;
    } else {
      left++;
    }
  }
}
```

### Complexity Analysis

Let N be the length of the input array.

- Time Complexity: O(N^2).

  The nested loops take O(N^2) time, so it is the dominant term in the total time complexity.

- Space Complexity: O(N).

  If we ignore the space required for the output array, the space complexity of the algorithm can be O(1) when the
  sorting algorithm we are using takes O(1) space, e.g. bubble sort. Since the overall time complexity of the algorithm is O(N^2), using an in-place sorting algorithm such as bubble sort (which has worst case time complexity of O(N^2)) won't change the upper bound.

#

### Follow-up

What if the input array contains duplicates and the output array must not include duplicate triplets?

[3Sum](https://leetcode.com/problems/3sum/)
