# Three Number Sum

### Understanding the problem

We are given an array of distinct integers and an integer that represents a target sum. We are asked to write a function that is going to find all the triplets in the array that sum to the target value and return them in a 2-dimensional array. The triplets should be ordered in ascending order, and so are the numbers in each triplets. For instance, if the triplets we found are: `[5, -6, 1]`, `[2, -8, 6]` and `[3, 5, -8]`, then we should return `[[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]`. If there is no such triplet, return an empty array.

#

### Approach: Sorting + Two Pointers

We can kind of break this problem down into a smaller, easier to solve problem. The basic idea is, iterate through every integers in the array; at each integer, subtract it from the target sum, now it becomes a two number sum problem - find all pairs of numbers in the rest of the array that sum up to the difference between the integer and the target sum, which can be solved with the two pointers approach. In order to use the two pointers approach, and since the result should be ordered in ascending order, we need to sort the array in ascending order at the beginning. The two pointers approach works like this:

- Initialize two pointers, one called `left` and the other called `right`, set the `left` pointer to the beginning of the rest of the array, and the `right` pointer to the end of the rest of the array.

- Add up the values that the two pointers point to, if the sum is smaller than the target, here it is the difference between current integer and the target sum, move the `left` pointer to the right by one; if the sum is bigger than the target, move the `right` pointer to the left by one; if it is equal to the target, then we've found the pair, move the `left` pointer to right and the `right` pointer to left, if we move only one pointer, the next sum is definitely either smaller or larger than the target, since all integers in the array are distinct. Keep moving both pointers until they point to the same number.

### Implementation

```js
function threeNumberSum(array, targetSum) {
  array.sort((a, b) => a - b);
  const triplets = [];

  for (let i = 0; i < array.length - 2; i++) {
    const firstNum = array[i];
    const currentDifference = targetSum - firstNum;
    let left = i + 1;
    let right = array.length - 1;

    while (left < right) {
      const currentSum = array[left] + array[right];

      if (currentSum === currentDifference) {
        triplets.push([firstNum, array[left], array[right]]);
        left++;
        right--;
      } else if (currentSum > currentDifference) {
        right--;
      } else {
        left++;
      }
    }
  }

  return triplets;
}
```

### Complexity Analysis

Let N be the length of the input array.

- Time Complexity: O(N^2).

  The nested loops take O(N^2) time, so it is the dominant term in the total time complexity.

- Space Complexity: O(N).

  If we ignore the space required for the output array, the space complexity of the algorithm can be O(1) when the
  sorting algorithm we are using takes O(1) space, e.g. bubble sort. Since the overall time complexity of the algorithm is O(N^2), using an in-place sorting algorithm such as bubble sort (which has worst case time complexity of O(N^2)) won't change the upper bound.
