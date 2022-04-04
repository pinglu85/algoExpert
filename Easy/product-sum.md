# Product Sum

### Understanding the problem

We are given a "special" array, which contains integers and optionally other "special" arrays. We are asked to write a function that is going to return the sum of the elements in the "special" array. If the element is a "special" array, we need to sum up the elements in the "special" array and then multiply the sum by the depth of the "special" array. For instance, if the input array is `[3, [4, 5]]`, then the result should be `3 + 2 * (4 + 5) = 21`; if the input array is `[6, [4, [5]]]`, then the result should be `6 + 2 * (4 + 3 * 5) = 44`.

#

### Approach

One might notice that the "special" array has a recursive structure. Therefore, the problem can be solved recursively.

We are going to have a recursive function which takes in a "special" array and the depth of the "special" array as arguments. The depth of the initial input array is going to be `1`. The recursive function is going to return the sum of all elements in the "special" array.

We iterate over the "special" array and use a variable to hold the sum of the elements in the current "special" array.

If we get to an integer, we can just add the element to the sum.

If we encounter an array, we call the recursive function on it, passing in the array and its depth, which is `the depth of the current array + 1`. We add the returned value of the recursive function to the sum.

After we get through all of the elements inside the array, we multiply the sum by the depth of the input array and return the result.

### Implementation

JavaScript:

```js
function productSum(array, multiplier = 1) {
  let sum = 0;

  for (const element of array) {
    if (Array.isArray(element)) {
      sum += productSum(element, multiplier + 1);
    } else {
      sum += element;
    }
  }

  return sum * multiplier;
}
```

Go:

```go
package main

type SpecialArray []interface{}

// Tip: Each element of `array` can either be cast
// to `SpecialArray` or `int`.
func ProductSum(array []interface{}) int {
	return calculateProductSum(array, 1)
}

func calculateProductSum(array []interface{}, multiplier int) int {
	sum := 0

	for _, el := range array {
		if specialArray, ok := el.(SpecialArray); ok {
			sum += calculateProductSum(specialArray, multiplier+1)
		} else if num, ok := el.(int); ok {
			sum += num
		}
	}

	return sum * multiplier
}
```

### Complexity Analysis

- Time Complexity: O(N), where N is the total number of elements in the array, including the sub-elements.

  For instance, if the input array is `[6, [4, [5]], 3]`, then N = 3 + 2 + 1 = 6. 3 is the number of the elements in the outermost array: `[6, [...], 3]`. 2 is the number of the elements in the array `[4, [...]]`. 1 is the number of the elements in the array `[5]`.

- Space Complexity: O(D), where D is the maximum depth of "special" arrays in the array.
