# Insertion Sort

### Understanding the problem

Given an array of integers, we are asked to sort the input array using the Insertion Sort and return the sorted array.

#

### Approach

In the Insertion Sort, we virtually split the array into a sorted and an unsorted part. We keep inserting values from the unsorted part into the sorted part until all the elements in the array are in the correct order . At the beginning, the sorted part consists just of the first value in the array.

Suppose we have the following array of integers:

```
[5, 4, 8, 2, 6]
```

We start at index `1`. The sorted part consists just of `5`.

```
[5, 4, 8, 2, 6]
 -
    i
```

We want to insert `4` into the sorted part. We compare `4` to `5`, the very last number in the sorted part. `4` is smaller than `5`, so we swap `4` with `5`.

```
[4, 5, 8, 2, 6]
    i
```

We reach the very beginning of the sorted part, so we are done. Now the sorted part is `4, 5`. We move on to index `2`.

```
[4, 5, 8, 2, 6]
 ----
       i
```

Compare `8` to `5`. They are in the correct order, so we just expand the sorted part and move on to index `3`.

```
[4, 5, 8, 2, 6]
 -------
          i
```

Compare `2`, to the last number in the sorted part, which is `8`. `2` is smaller than `8`, so we swap them:

```
[4, 5, 2, 8, 6]
          i
```

We then compare `2` to `5`. Since they are out of order, we swap `2` and `5`:

```
[4, 2, 5, 8, 6]
          i
```

Then compare `2` to `4`. `2` is smaller than `4`, so we swap them again.

```
[2, 4, 5, 8, 6]
          i
```

We reach the very beginning of the sorted part, so we move on to index `4` and the sorted part becomes `2, 4, 5, 8`.

```
[2, 4, 5, 8, 6]
 ----------
             i
```

Compare `6` to `8`. `6` is smaller than `8`, so we swap these two numbers.

```
[2, 4, 5, 6, 8]
             i
```

Then we compare `6` to `5`, they are in the correct order, so we don't need to touch them. Since we are already at the end of the array, the array is now sorted.

**Algorithm**

- Use a for loop to iterate through the array starting at index `1`.

- At each iteration,

  - initialize a variable `j` which is going to keep track of the position of the value we are trying to insert. Initially, set it to the index of the for loop, since the value we're trying to insert is the value at the current index of the for loop.

  - while the value is smaller than its predecessor and we haven't reach the very beginning of the sorted part, swap the value with its predecessor and decrement `j` by 1.

### Implementation

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

### Complexity Analysis

- Time Complexity:

  Best Case: O(N), when the array is already sorted.

  Average & Worst Case: O(N^2).

- Space Complexity: O(1).
