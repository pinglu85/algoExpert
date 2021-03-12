### Understanding the problem

Given an array of distinct integers and an integer `targetSum`, we need to implement a function that find out any two numbers in the array that add up to the `targetSum`. If this two numbers do not exist, return an empty array.

### Approach

Use a hash table to store the difference between each integer and the target sum as key and the integer as value for quick look up. Iterate through the array. At each iteration, look up the integer in the hash table, if it is in the hash table, then the two numbers are found, return them, otherwise, calculate the difference and add the key value pair into the hash table. If the loop finishes, it means no two numbers add up to the target sum, return an empty array.

### Solution

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
