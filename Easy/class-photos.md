# Class Photos

### Understanding the problem

I am given two non-empty arrays of positive integers. The first array is going to represent the heights of students wearing red shirts and the second array is going to represent the heights of students wearing blue shirts. The two arrays will always have the same length. I am asked to write a function that is going to find out if we can take a photo of these students that adheres to the following constraints:

1. All the students that are wearing red shirts must be in the same row;
2. All of the students that are wearing blue shirts must be in the same row;
3. The photo must have exactly two rows and the two rows must have the same number of students in them.
4. Every student in the front row must be shorter than the student directly behind them in the back row.

The function is going to arrange the students and return true if we can take a photo that follows these constraints; otherwise return false.

### Approach - O(nlog(n)) time | O(1) space, where n is the number of integers.

The problem could be summarized as the following: for every integer in one array whether there is a greater integer in the other array. the problem first sort both arrays in ascending order. Suppose the first array is `[4, 6, 2, 7]` and the second array is `[3, 5, 8, 9]`. After sorting them, I get `[2, 4, 6, 7]` and `[3, 5, 8, 9]`. `2` is the smallest integer in the first array and it is smaller than `3`, which is the smallest integer in the second array, that means I should find out whether for every integer in the first array there is a greater integer in the second array. I can then compare the second smallest integers in the two arrays, that are `4` and `5`. `4` is smaller than `5`, therefore I can compare the integers thereafter. If the second smallest integer in the first array was `6`, then I can return false, since every integer after `6` in the first array is for sure greater than `5`. If the integers from the two arrays are equal to each other, return false. When the end of both arrays are reached, that means for every integer in one array there is a greater integer in another array, I can then return true.

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
