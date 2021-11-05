# Insertion Sort

### Understanding the problem

Given an array of integers, I am asked to write a function that is going to sort the array using the Insertion Sort and return the sorted array.

#

### Approach

To sort an array in ascending order using the Insertion Sort:

1. First, we loop through the array starting at index `1`, because in the Insertion Sort, we assume the first element in the array is already sorted.
2. At each iteration, we compare the element at the current index to its predecessor. If the element is smaller than its predecessor, compare it to the elements before, until we get to an element that is smaller than the element at the current index or there is no more element to compare to. Move the greater elements one position up and place the element into its correct position.

### Time & Space Complexity

Best: O(n) time | O(1) space, where n is the length of the input array.

Average: O(n^2) time | O(1) space, where n is the length of the input array.

Worst: O(n^2) time | O(1) space, where n is the length of the input array.

### Solution

```js
function insertionSort(array) {
  for (let i = 1; i < array.length; i++) {
    const currNum = array[i];
    let j = i - 1;
    while (j >= 0 && array[j] > currNum) {
      array[j + 1] = array[j];
      j--;
    }
    array[j + 1] = currNum;
  }

  return array;
}
```
