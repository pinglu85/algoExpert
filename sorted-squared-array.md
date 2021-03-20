### Understanding the problem

Given an array of integers that are sorted in ascending order, write a function that squares all the integers in the array and returns them in a new array, which should be also sorted in ascending order.

### Approach 1 - O(n) time | O(n) space

Create a new empty array to store the result. Iterate through each integer in the array, square it and push it to the result array. After the loop, return the result array. Or I can use the `.map()` function, which returns a new array populated with the results of calling a provided function on every element in the calling array. But this solution only works for positive numbers, because when we square negative numbers, we get a positive result, if we add them directly to the result array, the array is no longer sorted. Therefore I need to initialize two arrays, one storing the squares of non-negative integers, other storing the squares of negative numbers. At each iteration, if the integer is negative, push the square into the array for negative numbers. After squaring all the integers, I need to merge the two sorted arrays into a sorted array:

1. initialize a new empty array to store the result.
2. initialize two pointers to keep track of the position we are at in the both arrays. Point the pointer for the non-negative array to the beginning of it, and the one for the negative array to the last number in the array, because, for instance, if we have two negative numbers -5 and -1 and they are sorted in ascending order, after squaring, we get 25 and 1, and now they are in descending order - the smaller negative number becomes bigger after squaring.
3. Iterate until both pointers out of bound. At each iteration, compare the values the two pointers point to, push the smaller one into the result array, if it is from the non-negative array, increase the corresponding pointer, otherwise, decrease the corresponding pointer.
4. After the loop, if the pointer for non-negative array is not equal to the length of the array, push the remaining integers into the result array; if the pointer for negative array is not less than 0, push the remaining integers into the result array.

### Solution 1

```js
function sortedSquaredArray(array) {
  const positiveSquares = [];
  const negativeSquares = [];

  for (const value of array) {
    const square = value * value;
    if (value < 0) {
      negativeSquares.push(square);
    } else {
      positiveSquares.push(square);
    }
  }

  const squares = [];
  let positiveIdx = 0;
  let negativeIdx = negativeSquares.length - 1;

  while (positiveIdx < positiveSquares.length || negativeIdx >= 0) {
    const positiveSquare =
      positiveSquares[positiveIdx] !== undefined
        ? positiveSquares[positiveIdx]
        : Infinity;
    const negativeSquare = negativeSquares[negativeIdx] || Infinity;

    if (positiveSquare < negativeSquare) {
      squares.push(positiveSquare);
      positiveIdx++;
    } else {
      squares.push(negativeSquare);
      negativeIdx--;
    }
  }

  return squares;
}
```
