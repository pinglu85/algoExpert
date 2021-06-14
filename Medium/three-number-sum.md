# Three Number Sum

### Understanding the problem

Given an array of distinct integers and an integer representing a target sum, write a function that find all combinations of three numbers in the array that add up to the target sum. The function should return a 2-dimensional array of all these combinations. The numbers in each combination should be ordered in ascending order, and the combinations themselves should also be ordered in ascending order, for instance: `[[-8, 2, 6],[-8, 3, 5], [-6, 1, 5]]`. If there is no such combination, return an empty array.

#

### Approach

I can kind of break this problem down into a smaller, easier to solve problem. The basic idea is, iterate through every integers in the array; at each integer, subtract it from the target sum, now it becomes a two number sum problem - find all pairs of numbers in the rest of the array that sum up to the difference between the integer and the target sum, which can be solved with the two pointers approach. In order to use the two pointers approach, and since the result should be ordered in ascending order, I need to sort the array in ascending order at the beginning. The two pointers approach works like this:

- Initialize two pointers, one called `left` and the other called `right`, set the `left` pointer to the beginning of the rest of the array, and the `right` pointer to the end of the rest of the array.
- Add up the values that the two pointers point to, if the sum is smaller than the target, here it is the difference between current integer and the target sum, move the `left` pointer to the right by one; if the sum is bigger than the target, move the `right` pointer to the left by one; if it is equal to the target, then I found the pair, move the `left` pointer to right and the `right` pointer to left, if I move only one pointer, the next sum is definitely either smaller or larger than the target, since all integers in the array are distinct. Keep moving both pointers until they point to the same number.

### Time & Space Complexity

O(n^2) time | O(n) space, where n is the length of the input array.

The O(nlog(n)) runtime of the sorting step is not reflected in the final time complexity, because the nested loops run in O(n^2) time; this dwarfs the O(nlog(n)) runtime of the sorting step and allows us to remove it from the overall time complexity.

### Solution

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
