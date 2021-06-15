# Bubble Sort

### Understanding the problem

Given an array of integers, I am asked to write a function that is going to sort the array in ascending order using the Bubble Sort and return the sorted array.

#

### Approach

The main idea of the Bubble Sort is repeatedly swap the adjacent elements if they are in wrong order. So I am going to use a nested for loop, where the outer loop is going to start at the very end of the input array, and the inner loop is going to start at the very beginning of the input array and stops at the current index of the outer loop, current index of the outer loop is exclusive. In the inner loop I am going to compare the integer to its right neighbor, if it is greater than the neighbor, which means they are in wrong order, swap them. The reason that the outer loop starts at the very end of the array and the inner loop stops at the current index of the outer loop is that after bubbling the larger numbers up, they are already in correct place. If the input array is already sorted or nearly sorted, I can optimize the Bubble Sort by initializing a Boolean flag in the outer loop that is going to keep track of whether a swap has occurred, if I get out of the inner loop, and didn't swap any numbers, which means all the numbers are already in correct place, I can break out of the outer loop.

### Time & Space Complexity

Best: O(n) time | O(1) space, where n is the length of the input array.

Average: O(n^2) time | O(1) space, where n is the length of the input array.

Worst: O(n^2) time | O(1) space, where n is the length of the input array.

### Solution

```js
function bubbleSort(array) {
  for (let idx = array.length - 1; idx >= 0; idx--) {
    let isSorted = true;

    for (let j = 0; j < idx; j++) {
      if (array[j] > array[j + 1]) {
        [array[j], array[j + 1]] = [array[j + 1], array[j]];
        isSorted = false;
      }
    }

    if (isSorted) break;
  }

  return array;
}
```
