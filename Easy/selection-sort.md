# Selection Sort

### Understanding the problem

Given an array of integers, we are asked to sort the array in ascending order with the Selection Sort.

#

### Approach

The Selection Sort virtually divide the array into a sorted and an unsorted part. We keep finding the smallest value in the unsorted part and appending it to the end of the sorted part, until all of the values are in the correct order. At the very beginning, the sorted part is empty and the unsorted part is the entire array. To append the smallest value to the sorted part, we can just swap it with the first number in the unsorted part.

Suppose we have the following array of integers:

```
[40, 35, 90, 2, 10, 7]
```

First we iterate through the entire array and find the smallest number, which is `2`. We move it to the beginning of the array:

```
[2, 35, 90, 40, 10, 7]
 -
```

Now the sorted part contains `2` and the unsorted part becomes `35, 90, 40, 10, 7`. We loop through the unsorted part and find the smallest number, which is `7`, and place it at the beginning of the unsorted part.

```
[2, 7, 90, 40, 10, 35]
 ----
```

Find the smallest number in `90, 40, 10, 35` and move it to the beginning.

```
[2, 7, 10, 40, 90, 35]
 --------
```

Find the smallest number in `40, 90, 35` and put it to the beginning.

```
[2, 7, 10, 35, 90, 40]
 ------------
```

Find the smallest number in `90, 40` and move it to the beginning.

```
[2, 7, 10, 35, 40, 90]
 ----------------
```

Now the unsorted part consists just of `90`, so we are done.

### Implementation

```js
function selectionSort(array) {
  let startIdx = 0;
  while (startIdx < array.length - 1) {
    let smallestIdx = startIdx;
    for (let i = startIdx + 1; i < array.length; i++) {
      if (array[i] < array[smallestIdx]) {
        smallestIdx = i;
      }
    }

    if (smallestIdx !== startIdx) {
      swap(array, startIdx, smallestIdx);
    }

    startIdx++;
  }

  return array;
}

function swap(array, i, j) {
  [array[i], array[j]] = [array[j], array[i]];
}
```

### Complexity Analysis

- Time Complexity: O(N^2), where N is the length of the input array.

- Space Complexity: O(1).
