# Class Photos

### Understanding the problem

I am given two non-empty arrays of positive integers. The two arrays will have the same length. I am asked to write a function that is going to find out if for every integer in one array there is a greater integer in the other array. If the integers from the two arrays are equal to each other, return false.

🙋‍♀️🙋‍♂️ In a coding interview, I should ask the interviewer how to handle the case where the integers from the two arrays are equal to each other.

### Approach - O(nlog(n)) time | O(1) space, where n is the number of integers.

I would first sort both arrays in ascending order. Suppose the first array is `[4, 6, 2, 7]` and the second array is `[3, 5, 8, 9]`. After sorting them, I get `[2, 4, 6, 7]` and `[3, 5, 8, 9]`. `2` is the smallest integer in the first array and it is smaller than `3`, which is the smallest integer in the second array, that means I should find out whether for every integer in the first array there is a greater integer in the second array. I can then compare the second smallest integers in the two arrays, that are `4` and `5`. `4` is smaller than `5`, therefore I can compare the integers thereafter. If the second smallest integer in the first array was `6`, then I can return false, since every integer after `6` in the first array is for sure greater than `5`. If the integers from the two arrays are equal to each other, return false. When the end of both arrays are reached, that means for every integer in one array there is a greater integer in another array, I can then return true.

### Solution

```js
function classPhotos(redShirtHeights, blueShirtHeights) {
  redShirtHeights.sort((a, b) => a - b);
  blueShirtHeights.sort((a, b) => a - b);

  let areRedShirtHeightsTaller = redShirtHeights[0] > blueShirtHeights[0];
  for (let idx = 0; idx < redShirtHeights.length; idx++) {
    const redShirtHeight = redShirtHeights[idx];
    const blueShirtHeight = blueShirtHeights[idx];

    if (areRedShirtHeightsTaller && blueShirtHeight >= redShirtHeight) {
      return false;
    }

    if (!areRedShirtHeightsTaller && redShirtHeight >= blueShirtHeight) {
      return false;
    }
  }

  return true;
}
```
