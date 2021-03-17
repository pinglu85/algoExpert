### Understanding the problem

Given an array of distinct integers and an integer representing a target sum, write a function that find all combinations of three numbers in the array that add up to the target sum. The function should return a 2-dimensional array of all these combinations. The numbers in each combination should be ordered in ascending order, and the combinations themselves should also be ordered in ascending order, for instance: `[[-8, 2, 6],[-8, 3, 5], [-6, 1, 5]]`. If there is no such combination, return an empty array.

### Approach 1

I can break this problem down into a smaller, easier to solve problem. Iterate through every integers; at each integer, subtract the integer from the target sum, now the problem becomes smaller and easier, it is a two number sum problem, I need to find all pairs of numbers in the rest of the array that sum up to the difference between the integer and the target sum, which can be solved with the two pointers approach. In order to use the two pointers approach, and since the result should be ordered in ascending order, I need to sort the array in ascending order at the beginning.

### Solution

```js
function threeNumberSum(array, targetSum) {
  array.sort((a, b) => a - b);
  const result = [];

  for (let i = 0; i < array.length - 2; i++) {
    const firstNum = array[i];
    const currentDifference = targetSum - firstNum;
    let left = i + 1;
    let right = array.length - 1;
    while (left < right) {
      const currentSum = array[left] + array[right];
      if (currentSum === currentDifference) {
        result.push([firstNum, array[left], array[right]]);
        left++;
        right--;
      } else if (currentSum > currentDifference) {
        right--;
      } else {
        left++;
      }
    }
  }

  return result;
}
```
