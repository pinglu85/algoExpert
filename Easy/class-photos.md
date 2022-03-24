# Class Photos

### Understanding the problem

We are given two non-empty arrays of positive integers: the first is going to represent the heights of students wearing red shirts and the second is going to represent the heights of students wearing blue shirts. The two arrays will always have the same length. We are asked to write a function that is going to find out if we can take a photo of these students that satisfies the following constraints:

1. All the students that are wearing red shirts must be in the same row;
2. All of the students that are wearing blue shirts must be in the same row;
3. The photo must have exactly two rows and the two rows must have the same number of students in them.
4. Every student in the front row must be shorter than the student directly behind them in the back row.

The function is going to arrange the students and return `true` if we can take a photo that follows these constraints; otherwise return `false`.

#

### Approach

Suppose the first array is `[4, 6, 2, 7]` and the second array is `[9, 5, 8, 3]`. To arrange the students under the aforementioned constraints, we can first find out the shortest student in the two arrays, since that student is shorter than all of the other students and hence cannot stand behind any of them. The height of the shortest student is `2` from the first array. Since all the students in red shirts must be in the same row, all the students from the first array should be in the front row.

```
First array (red shirt): [4, 6, 7]
Second array (blue shirt): [9, 5, 8, 3]

Front row(red shirt): 2
Back row(blue shirt):
```

Then we are going to find out who is going to stand behind the student with height of `2`. The student must come from the second array. We are going to select the shortest student in the second array, because the shortest student from the second array might be shorter than all of the remaining students in the first array. The shortest student in the second array is `3`.

```
First array (red shirt): [4, 6, 7]
Second array (blue shirt): [9, 5, 8]

Front row(red shirt): 2
Back row(blue shirt): 3
```

Now we are going to find out who is going to be the next student that stands in the front row. We are going to pick the shortest student in the rest of the first array, and compare it to the shortest student in the rest of the second array. If the shortest student from the first array is taller than the shortest student from the second array, then the photo can not be taken, because all the student in the rest of the first array are going to be taller than the shortest student from the second array and hence the shortest student from the second array cannot stand behind anyone. The shortest student in the rest of the first array is `4`, and the shortest student in the rest of the second array is `5`. `4` is smaller than `5`, so the photo can be taken.

```
First array (red shirt): [6, 7]
Second array (blue shirt): [9, 8]

Front row(red shirt): 2  4
Back row(blue shirt): 3  5
```

Repeat that process until we reach the end of the both arrays or we find out such a photo cannot be taken.

**Algorithm**

- Sort both arrays in ascending order at the very beginning.

- Find the shortest student in the two arrays by comparing the shortest student of the first array to the shortest student of the second array to find out whether or not the students wearing red shirts are going to be in the front row. Use a Boolean `redShirtsInFrontRow` to store the comparison result.

- Iterate through the two sorted arrays. We start at index `0`, because we need to handle the case where the two student have the same height. Suppose the first array is `[4, 8]` and second array is `[4, 6]`. `redShirtsInFrontRow` is going to be `false`, since the shortest student in red shirt is not shorter than the student in blue shirt. If we start from index `1`, then we'll return `true`, since `8` is greater than `6` and the students wearing red shirts are going to be in the back row. However, since every student in the front row must be shorter than the student directly behind them in the back row and `4 == 4`, the photo can't actually be taken.

  - At each iteration, compare the heights at current index in the two arrays. If the students im red shirts are going to be in the front row, then the height from the second array should be greater than the height from the first array; otherwise it should be smaller. If that is not the case, return `false`.

- When we get out of the loop without returning the result, return `true`.

### Implementation

```js
function classPhotos(redShirtHeights, blueShirtHeights) {
  redShirtHeights.sort((a, b) => a - b);
  blueShirtHeights.sort((a, b) => a - b);

  const redShirtsInFrontRow = redShirtHeights[0] < blueShirtHeights[0];

  for (let i = 0; i < redShirtHeights.length; i++) {
    const redShirtHeight = redShirtHeights[i];
    const blueShirtHeight = blueShirtHeights[i];

    if (redShirtsInFrontRow && redShirtHeight >= blueShirtHeight) {
      return false;
    }

    if (!redShirtsInFrontRow && blueShirtHeight >= redShirtHeight) {
      return false;
    }
  }

  return true;
}
```

### Complexity Analysis

- Time Complexity: O(N · log(N)), where N is the number of integers(students).

- Space Complexity: O(log(N)) or O(N), depending on the implementation of the sorting algorithm.

#

### Alternative Approach

We can also start with the tallest students in the two arrays and the logic is the same.

### Implementation

JavaScript:

```js
function classPhotos(redShirtHeights, blueShirtHeights) {
  redShirtHeights.sort((a, b) => b - a);
  blueShirtHeights.sort((a, b) => b - a);

  const redShirtsInFrontRow = redShirtHeights[0] < blueShirtHeights[0];
  for (let i = 0; i < redShirtHeights.length; i++) {
    const redShirtHeight = redShirtHeights[i];
    const blueShirtHeight = blueShirtHeights[i];

    if (redShirtsInFrontRow && redShirtHeight >= blueShirtHeight) {
      return false;
    }
    if (!redShirtsInFrontRow && redShirtHeight <= blueShirtHeight) {
      return false;
    }
  }

  return true;
}
```

Go:

```go
package main

import (
	"sort"
)

func ClassPhotos(redShirtHeights []int, blueShirtHeights []int) bool {
	sort.Slice(redShirtHeights, func(i, j int) bool {
		return redShirtHeights[i] > redShirtHeights[j]
	})
	sort.Slice(blueShirtHeights, func(i, j int) bool {
		return blueShirtHeights[i] > blueShirtHeights[j]
	})

	redShirtsInFrontRow := redShirtHeights[0] < blueShirtHeights[0]
	for i, redShirtHeight := range redShirtHeights {
		blueShirtHeight := blueShirtHeights[i]

		if redShirtsInFrontRow && redShirtHeight >= blueShirtHeight {
			return false
		}

		if !redShirtsInFrontRow && blueShirtHeight >= redShirtHeight {
			return false
		}
	}

	return true
}
```

### Complexity Analysis

- Time Complexity: O(N · log(N)), where N is the number of integers(students).

- Space Complexity: O(log(N)) or O(N), depending on the implementation of the sorting algorithm.
