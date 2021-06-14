# Tandem Bicycle

### Understanding the problem

I am given two arrays of positive integers, one containing the speeds of riders with red shirts and another one containing the speeds of riders with blue shirt. The number of red-shirt riders and the number of blue-shirt riders are equal. I will also receive a third input parameter called `fastest` and it is going to be a Boolean value. I am asked to write a function that is going to pair every red-shirt rider with a blue-shirt rider to ride a tandem bicycle; and if the Boolean is `true`, the function will return the total maximum speed of all tandem bicycles being ridden; otherwise it will return the total minimum speed.

A tandem bicycle is a bicycle operated by two people. The speed of the bicycle is dictated by the rider who pedals faster, for instance, if the speed of one rider is `5` and the speed of the other rider is `3`, then the speed of the bicycle is `5`.

🙋‍♀️🙋‍♂️ In a coding interview, I can ask the interviewer what does the minimum possible total speed mean to eliminate ambiguity.

#

### Approach

Suppose the input arrays are `[8, 5, 3]` and `[7, 2, 4]`. There are following ways to pair every integer in the first array with a integer from the second array:

- `[8, 7], [5, 2], [3, 4]`. The maximum total speed is going to be `8 + 5 + 4 = 17`.
- `[8, 7], [5, 4], [3, 2]`. The maximum total speed is going to be `8 + 5 + 3 = 16`.
- `[8, 2], [5, 7], [3, 4]`. The maximum total speed is going to be `8 + 7 + 4 = 19`.
- `[8, 2], [5, 4], [3, 7]`. The maximum total speed is going to be `8 + 5 + 7 = 20`.
- `[8, 4], [5, 7], [3, 2]`. The maximum total speed is going to be `8 + 7 + 3 = 18`.
- `[8, 4], [5, 2], [3, 7]`. The maximum total speed is going to be `8 + 5 + 7 = 20`.

So the maximum total speed is `8 + 5 + 7 = 20`. I can notice that `8`, `5` and `7` are the largest three integers in the two arrays; and `2`, `3` and `4` are the smallest three integers in the two arrays. I can get them by merging the two arrays into one and sorting the merged array in ascending order: `[2, 3, 4, 5, 7, 8]`. However, that means I need extra memory to story the merged array.

The maximum total speed is yielded when the pairs are `[8, 2], [5, 4], [3, 7]` or `[8, 4], [5, 2], [3, 7]`. It can be noticed that `8`, the largest integer in the first array, is paired with `2`, the smallest integer in the second array, or `4`, but it cannot be paired with `7`, the largest integer in the second array, and `7` is paired with `3`, the smallest integer in the first array. `8` can not be paired with `7`, because if `8` is paired with `7`, `7` is going to be eliminated and I lose a very large speed. Pairing the smallest integer in one array with the largest integer in another array allows me to maximize the possibility of getting the largest number from the two arrays for each pair and save the larger numbers that I could probably use in other pairings.

So to get the maximum total speed, I would sort the first array in ascending order and the second array in descending order at the very beginning. Initialize a variable to 0 that is going to store the running sum of the total speed. Traverse through each integer in the first array. At each integer, get the integer at the current index in the second array and add the larger one from these two integers to the running sum.

From the above pairings, the minimum possible total speed is `8 + 5 + 3 = 16`, which is yielded by pairing the largest numbers from both of these arrays together: `[8, 7], [5, 4], [3, 2]`, because that way I can eliminate two large numbers. So to get the minimum total speed, I would sort the second array also in ascending order, then loop through each number in the first array; for each number, get the number at the current index in the second array and add the larger one from these two numbers to the running sum.

### Time & Space Complexity

O(nlog(n)) time | O(1) space, where n is the number of riders.

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

### Solution with reverse array function

```js
function tandemBicycle(redShirtSpeeds, blueShirtSpeeds, fastest) {
  redShirtSpeeds.sort((a, b) => a - b);
  blueShirtSpeeds.sort((a, b) => a - b);

  if (fastest) reverseArrayInPlace(blueShirtSpeeds);

  let totalSpeed = 0;
  for (let idx = 0; idx < redShirtSpeeds.length; idx++) {
    const redShirtSpeed = redShirtSpeeds[idx];
    const blueShirtSpeed = blueShirtSpeeds[idx];
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
