### Understanding the problem

Given an array of integers, no number in this array is repeated, and an integer `targetSum`, I need to implement a function that find whether there's a pair of numbers in the array that adds up to the `targetSum`. If such pair does not exist, return an empty array.

### Approach 1: O(n) time, O(n) space.

Use a hash table to store the difference between each integer and the target sum as key and the integer as value for quick look up. Iterate through the array. At each iteration, look up the integer in the hash table, if it is in the hash table, then the pair is found, return it, otherwise, calculate the difference and add the key value pair into the hash table. If the loop finishes, it means no two numbers sum up to the target sum, return an empty array.

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

### Approach 2: O(n^2) time, O(1) space

Iterate through every number in the array. At each number, traverse through the rest of the array, if adding any number in the rest of the array to the number yields the target sum, return the pair. 

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