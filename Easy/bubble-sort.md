# Bubble Sort

### Understanding the problem

Given an array of integers, I am asked to write a function that is going to sort the array in ascending order using the Bubble Sort and return the sorted array.

#

### Approach

The main idea of the Bubble Sort is repeatedly swap the adjacent elements if they are in wrong order.

Suppose we have the following array:

```
[8, 5, 2, 9, 6, 1]
```

We start at index `0`, compare `8` to its right neighbor, which is `5`.

```
[8, 5, 2, 9, 6, 1]
 ^
```

Since `8` is greater than `5`, we swap the two elements:

```
[5, 8, 2, 9, 6, 1]
 ^
```

We then move to index `1`, compare `8` to its right neighbor.

```
[5, 8, 2, 9, 6, 1]
    ^
```

Since `8` is greater than `2`, swap the two elements:

```
[5, 2, 8, 9, 6, 1]
    ^
```

We move to index `2`.

```
[5, 2, 8, 9, 6, 1]
       ^
```

`8` is smaller than `9`, so we do nothing and move to index `3`.

```
[5, 2, 8, 9, 6, 1]
          ^
```

`9` is larger than `6`, swap them:

```
[5, 2, 8, 6, 9, 1]
          ^
```

We then move to index `4`.

```
[5, 2, 8, 6, 9, 1]
             ^
```

`9` is greater than `1`, we swap the two elements:

```
[5, 2, 8, 6, 1, 9]
             ^
```

We can notice that the largest element in the array, which is `9`, is now in the correct position, so we don't need to compare it any more.

Then we start from index `0` again, compare `5` to its right neighbor.

```
[5, 2, 8, 6, 1, 9]
 ^
```

`5` is greater than `2`, we swap the two elements.

```
[2, 5, 8, 6, 1, 9]
 ^
```

Move to index `1`.

```
[2, 5, 8, 6, 1, 9]
    ^
```

`5` is smaller than `8`, do nothing and move to index `2`.

```
[2, 5, 8, 6, 1, 9]
       ^
```

`8` is greater than `6`, we swap the two elements:

```
[2, 5, 6, 8, 1, 9]
       ^
```

Move to index `3`.

```
[2, 5, 6, 8, 1, 9]
          ^
```

`8` is larger than `1`, swap the two elements:

```
[2, 5, 6, 1, 8, 9]
          ^
```

Now the second largest element in the array, which is `8`, is now in the correct place.

We start at index `0` again and keep doing the same process until the second smallest element in the array is in its correct position.

So I am going to use a while loop to keep track of the number of elements that we still need to sort, until there is only one element left. In the while loop, I am going to loop through the remaining unsorted elements, compare adjacent elements and swap them if they are in wrong order.

We can also optimize the bubble sort, if the input array is already sorted or nearly sorted, by initializing a Boolean flag, which is going to keep track of whether a swap has occurred. If I get out of the for loop and didn't swap any elements, then it means all the elements are already in the correct order and I can break out of the while loop.

### Time & Space Complexity

Best: O(n) time | O(1) space, where n is the length of the input array.

Average: O(n^2) time | O(1) space, where n is the length of the input array.

Worst: O(n^2) time | O(1) space, where n is the length of the input array.

### Solution

```js
function bubbleSort(array) {
  let numOfUnsortedElements = array.length;
  let isSorted = false;

  while (!isSorted) {
    isSorted = true;

    for (let i = 0; i < numOfUnsortedElements - 1; i++) {
      if (array[i] > array[i + 1]) {
        swap(array, i, i + 1);
        isSorted = false;
      }
    }

    numOfUnsortedElements--;
  }

  return array;
}

function swap(array, i, j) {
  [array[i], array[j]] = [array[j], array[i]];
}
```

### Solution in GO

```go
package main

func BubbleSort(array []int) []int {
	isSorted := false
	numOfUnsortedElements := len(array)

	for !isSorted {
		isSorted = true

		for i := 0; i < numOfUnsortedElements-1; i++ {
			if array[i] > array[i+1] {
				swap(array, i, i+1)
				isSorted = false
			}
		}

		numOfUnsortedElements--
	}

	return array
}

func swap(array []int, i, j int) {
	array[i], array[j] = array[j], array[i]
}
```
