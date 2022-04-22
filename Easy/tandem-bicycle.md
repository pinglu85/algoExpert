# Tandem Bicycle

### Understanding the problem

We are given two arrays of positive integers: the first one represents the speeds of riders with red shirts and the second one represents the speeds of riders with blue shirt. Both array have the same length. We will also receive a Boolean `fastest`.

We are asked to write a function that is going to pair every red-shirt rider with a blue-shirt rider to operate a tandem bicycle. A tandem bicycle is a bicycle operated by two people. The speed of the bicycle is dictated by the rider who pedals faster, for instance, if the speed of one rider is `5` and the speed of the other rider is `3`, then the speed of the bicycle is `5`. If the Boolean `fastest` is `true`, the function should return the total maximum speed of all tandem bicycles being ridden; otherwise return the total minimum speed.

Example 1:

<pre>
<b>Input:</b>
redShirtSpeeds = [3, 9, 2]
blueShirtSpeeds = [7, 2, 1]
fastest = true

<b>Output:</b> 19
</pre>

Example 2:

<pre>
<b>Input:</b>
redShirtSpeeds = [3, 9, 2]
blueShirtSpeeds = [7, 2, 1]
fastest = false

<b>Output:</b> 14
</pre>

#

### Approach 1

Suppose the input arrays are `[8, 5, 3]` and `[7, 2, 4]`. There are following ways to pair every integer in the first array with a integer from the second array:

1. `[8, 7], [5, 2], [3, 4]`. The maximum total speed is going to be `8 + 5 + 4 = 17`.

2. `[8, 7], [5, 4], [3, 2]`. The maximum total speed is going to be `8 + 5 + 3 = 16`.

3. `[8, 2], [5, 7], [3, 4]`. The maximum total speed is going to be `8 + 7 + 4 = 19`.

4. `[8, 2], [5, 4], [3, 7]`. The maximum total speed is going to be `8 + 5 + 7 = 20`.

5. `[8, 4], [5, 7], [3, 2]`. The maximum total speed is going to be `8 + 7 + 3 = 18`.

6. `[8, 4], [5, 2], [3, 7]`. The maximum total speed is going to be `8 + 5 + 7 = 20`.

So the maximum total speed is `8 + 5 + 7 = 20`. We can notice that `8`, `5` and `7` are the largest three integers in the two arrays, and `2`, `3` and `4` are the smallest three integers in the two arrays. We can get them by merging the two arrays into one and sorting the merged array in ascending order: `[2, 3, 4, 5, 7, 8]`. However, that means we need extra memory to store the merged array.

The maximum total speed is yielded when the pairs are `[8, 2], [5, 4], [3, 7]` or `[8, 4], [5, 2], [3, 7]`. It can be noticed that `8`, the largest integer in the first array, is paired with `2`, the smallest integer in the second array, or `4`, but it cannot be paired with `7`, the largest integer in the second array. `7` is paired with `3`, the smallest integer in the first array. `8` can not be paired with `7`, because if `8` is paired with `7`, `7` is going to be eliminated and we lose a very large speed. Pairing the smallest integer in one array with the largest integer in another array allows us to maximize the possibility of getting the largest number from the two arrays for each pair and save the larger numbers that we could probably use in other pairings.

So to get the maximum total speed, we would sort the first array in ascending order and the second array in descending order at the very beginning. Initialize a variable to `0` that is going to store the running sum of the total speed. Traverse through each integer in the first array. At each integer, get the integer at the current index in the second array and add the larger one from these two integers to the running sum.

From the above pairings, the minimum possible total speed is `8 + 5 + 3 = 16`, which is yielded by pairing the largest numbers from both of these arrays together: `[8, 7], [5, 4], [3, 2]`, because that way we can eliminate two large numbers. So to get the minimum total speed, we could sort the second array in ascending order as well, and then we perform the same process.

### Implementation

JavaScript:

```js
function tandemBicycle(redShirtSpeeds, blueShirtSpeeds, fastest) {
  redShirtSpeeds.sort((a, b) => a - b);
  if (fastest) {
    blueShirtSpeeds.sort((a, b) => b - a);
  } else {
    blueShirtSpeeds.sort((a, b) => a - b);
  }

  let totalSpeed = 0;
  for (let i = 0; i < redShirtSpeeds.length; i++) {
    const redShirtSpeed = redShirtSpeeds[i];
    const blueShirtSpeed = blueShirtSpeeds[i];
    totalSpeed += Math.max(redShirtSpeed, blueShirtSpeed);
  }

  return totalSpeed;
}
```

Go:

```go
package main

import (
	"sort"
)

func TandemBicycle(redShirtSpeeds []int, blueShirtSpeeds []int, fastest bool) int {
	sort.Ints(redShirtSpeeds)
	if !fastest {
		sort.Ints(blueShirtSpeeds)
	} else {
		sort.Slice(blueShirtSpeeds, func(i, j int) bool {
			return blueShirtSpeeds[i] > blueShirtSpeeds[j]
		})
	}

	totalSpeed := 0

	for i, redShirtSpeed := range redShirtSpeeds {
		blueShirtSpeed := blueShirtSpeeds[i]
		totalSpeed += max(redShirtSpeed, blueShirtSpeed)
	}

	return totalSpeed
}

func max(x, y int) int {
	if x < y {
		return y
	}

	return x
}
```

**With reverse array function**

JavaScript:

```js
function tandemBicycle(redShirtSpeeds, blueShirtSpeeds, fastest) {
  redShirtSpeeds.sort((a, b) => a - b);
  blueShirtSpeeds.sort((a, b) => a - b);

  if (fastest) reverseArrayInPlace(blueShirtSpeeds);

  let totalSpeed = 0;
  for (let i = 0; i < redShirtSpeeds.length; i++) {
    const redShirtSpeed = redShirtSpeeds[i];
    const blueShirtSpeed = blueShirtSpeeds[i];
    totalSpeed += Math.max(redShirtSpeed, blueShirtSpeed);
  }

  return totalSpeed;
}

function reverseArrayInPlace(array) {
  let start = 0;
  let end = array.length - 1;
  while (start < end) {
    [array[start], array[end]] = [array[end], array[start]];
    start++;
    end--;
  }
}
```

Go:

```go
package main

import (
	"sort"
)

func TandemBicycle(redShirtSpeeds []int, blueShirtSpeeds []int, fastest bool) int {
	sort.Ints(redShirtSpeeds)
	sort.Ints(blueShirtSpeeds)

	if fastest {
		reverseSlice(blueShirtSpeeds)
	}

	totalSpeed := 0

	for i, redShirtSpeed := range redShirtSpeeds {
		blueShirtSpeed := blueShirtSpeeds[i]
		totalSpeed += max(redShirtSpeed, blueShirtSpeed)
	}

	return totalSpeed
}

func reverseSlice(s []int) {
	for start, end := 0, len(s)-1; start < end; start, end = start+1, end-1 {
		s[start], s[end] = s[end], s[start]
	}
}

func max(x, y int) int {
	if x < y {
		return y
	}

	return x
}
```

### Complexity Analysis

- Time Complexity: O(N · log(N)), where N is the number of riders.

- Space Complexity: O(log(N)) or O(N), depending on the implementation of the sorting algorithm.
