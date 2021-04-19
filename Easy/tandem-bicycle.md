# Tandem Bicycle

### Understanding the problem

A tandem bicycle is a bicycle ridden by two people. The speed of the bicycle is dictated by the person who pedals faster, for instance, if the speed of one person is `5` and the speed of the other person is `3`, then the speed of the bicycle is `5`.

I am given two arrays of positive integers, one containing the speeds of riders with red shirts and another one containing the speeds of riders with blue shirt. The number of red-shirt riders and the number of blue-shirt riders are equal. I will also receive a third input parameter, which is a Boolean. I am asked to write a function that is going to pair every red-shirt riders with a blue-shirt riders to operate a tandem bicycle, and if the Boolean is `true`, the function will return the maximum possible total speed, otherwise it will return the minimum possible total speed.

🙋‍♀️🙋‍♂️ In a coding interview, I can ask the interviewer what is the minimum possible total speed to eliminate ambiguity.

### Approach - O(nlog(n)) time | O(1) space, where n is the number of riders.

Suppose the input arrays are `[8, 5, 3]` and `[7, 2, 4]`. There are following ways to pair every integer in the first array with a integer from the second array:

- `[8, 7], [5, 2], [3, 4]`. The maximum total speed is going to be `8 + 5 + 4 = 17`.
- `[8, 7], [5, 4], [3, 2]`. The maximum total speed is going to be `8 + 5 + 3 = 16`.
- `[8, 2], [5, 7], [3, 4]`. The maximum total speed is going to be `8 + 7 + 4 = 19`.
- `[8, 2], [5, 4], [3, 7]`. The maximum total speed is going to be `8 + 5 + 7 = 20`.
- `[8, 4], [5, 7], [3, 2]`. The maximum total speed is going to be `8 + 7 + 3 = 18`.
- `[8, 4], [5, 2], [3, 7]`. The maximum total speed is going to be `8 + 5 + 7 = 20`.

So the maximum possible total speed is `8 + 5 + 7 = 20`. I can notice that `8`, `5` and `7` are the largest three integers in the two arrays; and `2`, `3` and `4` are the smallest three integers in the two arrays. I can get them by merging the two arrays into one and sorting the merged array in ascending order: `[2, 3, 4, 5, 7, 8]`. However, that means I need extra memory to story the merged array.

The maximum possible total speed is yielded when the pairs are `[8, 2], [5, 4], [3, 7]` or `[8, 4], [5, 2], [3, 7]`. It can be noticed `7`, the largest integer in the second array, is paired with `3`, the smallest integer in the first array. Pairing the smallest integer in the first array with the largest integer in the second array, I am maximizing the possibility of getting the largest number from the two arrays for each pair, without losing the possibility of getting the largest numbers thereafter, because:

1. `3` is the smallest integer in the first array, every number after it is for sure greater than `3`, which might be greater than `7`, the largest number in the second array. If the smallest number in the first array is greater than the largest number in the second array, then all the numbers in the first array are going to be greater than the numbers in the second array; I can get the maximum total speed by summing up all the integers in the first array.
2. Every number after `3` is going to be greater than `3`, they might be smaller than `7` but greater than the numbers after `7` which are for sure smaller than `7`.

So to get the maximum possible total speed, I would sort the first array in ascending order and the second array in descending order at the very beginning. Initialize a variable to 0 that is going to store the running sum of the total speed. Traverse through each integer in the first array. At each integer, get the integer at the current index in the second array and add the greater integer to the running sum.

From the example above, the minimum total speed is `8 + 5 + 3 = 16`, which is yielded by paring the largest numbers in the two arrays: `[8, 7], [5, 4], [3, 2]`. So to get the minimum total speed, I would sort the second array also in ascending order, then loop through each integer in the first array. At each integer, get the integer at the current index in the second array and add the greater integer to the running sum.

### Solution

```js
function tandemBicycle(redShirtSpeeds, blueShirtSpeeds, fastest) {
  redShirtSpeeds.sort((a, b) => a - b);
  if (fastest) {
    blueShirtSpeeds.sort((a, b) => b - a);
  } else {
    blueShirtSpeeds.sort((a, b) => a - b);
  }

  let totalSpeed = 0;
  for (let idx = 0; idx < redShirtSpeeds.length; idx++) {
    const redShirtSpeed = redShirtSpeeds[idx];
    const blueShirtSpeed = blueShirtSpeeds[idx];
    totalSpeed += Math.max(redShirtSpeed, blueShirtSpeed);
  }

  return totalSpeed;
}
```
