# Insertion Sort

### Understanding the problem

Given an array of integers, I am asked to write a function that is going to sort the array using the Insertion Sort and return the sorted array.

#

### Approach

To sort an array in ascending order using the Insertion Sort:

1. First, we loop through the array starting at index `1`, because in the Insertion Sort, we assume the first element in the array is already sorted.
2. At each iteration, we compare the element at the current index to its predecessor. If the element is smaller than its predecessor, swap the element with its predecessor, and then compare it to the element that comes before it again. Keep performing the comparison and the swap, until a predecessor is smaller than the element or there is no more predecessor.

### Time & Space Complexity

Best: O(n) time | O(1) space, where n is the length of the input array.

Average: O(n^2) time | O(1) space, where n is the length of the input array.

Worst: O(n^2) time | O(1) space, where n is the length of the input array.

### Solution

```js
function insertionSort(array) {
  for (let i = 1; i < array.length; i++) {
    let j = i;
    while (j > 0 && array[j - 1] > array[j]) {
      swap(array, j, j - 1);
      j--;
    }
  }

  return array;
}

function swap(array, i, j) {
  [array[i], array[j]] = [array[j], array[i]];
}
```
