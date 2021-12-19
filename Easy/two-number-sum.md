# Two Number Sum

### Understanding the problem

Given an array of integers, no number in this array is repeated, and an integer representing the target sum, implement a function that find whether there's a pair of numbers in the array that adds up to the target sum. Return the pair in an array. If such pair does not exist, return an empty array.

#

### Approach 1: Hash Table

We can solve the problem by looping through each integer in the array and find if there is another integer in the rest of array that is equal to the difference between target sum and current integer. But that means we need a nested for loop, the outer for loop traverses the array and the inner for loop traverses the rest of the array to find the complement. Instead of the nested for loop, we can keep track of the complement of each integer (`target sum - integer`) in a hash table, which provides a constant-time lookup on average. In the hash table, the key is the complement and the value is the integer. At each iteration, look up the integer in the hash table, if it is already in the hash table, then the pair is found, return the pair, otherwise, calculate the complement and add the key/value pair into the hash table. If we get out of the for loop without returning the pair, return an empty array.

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input array.

### Solution 1

```js
function twoNumberSum(array, targetSum) {
  const hashTable = new Map();

  for (const num of array) {
    if (hashTable.has(num)) {
      return [hashTable.get(num), num];
    }

    const diff = targetSum - num;
    hashTable.set(diff, num);
  }

  return [];
}
```

### Solution 1 in Go

```go
package main

func TwoNumberSum(array []int, target int) []int {
	diffs := map[int]int{}

	for _, num := range array {
		if val, found := diffs[num]; found {
			return []int{val, num}
		}

		diff := target - num
		diffs[diff] = num
	}

	return []int{}
}
```

#

### Approach 2: Brute Force

Iterate through every number in the array. At each number, traverse through the rest of the array, if adding any number in the rest of the array to the number yields the target sum, return the pair.

### Time & Space Complexity

O(n^2) time | O(1) space, where n is the length of the input array.

### Solution 2

```js
function twoNumberSum(array, targetSum) {
  for (let i = 0; i < array.length; i++) {
    const firstNum = array[i];
    for (let j = i + 1; j < array.length; j++) {
      const secondNum = array[j];
      if (firstNum + secondNum === targetSum) {
        return [firstNum, secondNum];
      }
    }
  }

  return [];
}
```

### Solution 2 in Go

```go
package main

func TwoNumberSum(array []int, target int) []int {
	for i, firstNum := range array {
		for j := i + 1; j < len(array); j++ {
			secondNum := array[j]
			if firstNum+secondNum == target {
				return []int{firstNum, secondNum}
			}
		}
	}

	return []int{}
}
```

#

### Approach 3: Two Pointers

First sort the array in ascending order. Then initialize two pointers, one pointing to the first element in the sorted array, and the other pointing to the last element in the sorted array. Sum up the values that the two pointers point to. If their sum is smaller than the `targetSum`, shift the left pointer to right by one, because the array is sorted in ascending order, if we move the right pointer to left, the new sum is even smaller than the current sum, which is already smaller than the `targetSum`. If their sum is greater than the `targetSum`, shift the right pointer to left by one. Keep moving the pointers until we get to a sum that is equal to `targetSum` or the pointers overlap each other, in other words, the pointers are pointing to the same number.

### Time & Space Complexity

O(nlog(n)) time | O(1) space, where n is the length of the input array.

### Solution 3

```js
function twoNumberSum(array, targetSum) {
  array.sort((a, b) => a - b);
  let left = 0;
  let right = array.length - 1;
  while (left < right) {
    const currentSum = array[left] + array[right];
    if (currentSum === targetSum) {
      return [array[left], array[right]];
    }
    if (currentSum < targetSum) {
      left++;
    } else {
      right--;
    }
  }

  return [];
}
```

### Solution 3 in Go

```go
package main

import "sort"

func TwoNumberSum(array []int, target int) []int {
	sort.Ints(array)
	left, right := 0, len(array)-1

	for left < right {
		currSum := array[left] + array[right]

		if currSum == target {
			return []int{array[left], array[right]}
		} else if currSum > target {
			right--
		} else {
			left++
		}
	}

	return []int{}
}
```
