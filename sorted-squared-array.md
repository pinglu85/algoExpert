### Understanding the problem

Given an array of integers that are sorted in ascending order, write a function that squares all the integers in the array and returns them in a new array, which should be also sorted in ascending order.

### Approach 1 - O(n) time | O(n) space

Create a new empty array to store the result. Iterate through each integer in the array, square it and push it to the result array. After the loop, return the result array. Or I can use the `.map()` function, which returns a new array populated with the results of calling a provided function on every element in the calling array. But this solution only works for positive numbers, because when we square negative numbers, we get a positive result, if we add them directly to the result array, the array is no longer sorted. I can sort the result array, then the solution is gonna run an O(nlog(n)) time. Instead of sorting, I can initialize two arrays, one storing the squares of non-negative integers, other storing the squares of negative numbers. At each iteration, if the integer is negative, push the square into the negative array. After squaring all the integers, I need to merge the two sorted arrays into one sorted array:

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

### Solution using `.sort()` - O(nlog(n)) time | O(n) space

```js
function sortedSquaredArray(array) {
  return array.map((value) => value * value).sort((a, b) => a - b);
}
```

### Approach 2 - O(n) time | O(n) space

To handle negative integers, because the array is sorted, I can use two pointers to keep track of the smaller number and the larger number, compare the absolute value of the two numbers, if the absolute value of the smaller number is larger than or equal to the absolute value of the larger number, it means the square of the smaller number will also be larger than or equal to the square of the larger number, so it should be put after the square of the larger number; if the absolute value of the smaller number is smaller than the value of the larger number, I need to continue the comparison until the correct position for the smaller number is found.

Consider we have array `[-5, -4, -3, 1, 3, 6]`, the absolute value of `-5` is `5`, and the value of `6` is `6`, `5` is smaller than `6`, but it is bigger than `3`, so the square of `5` should come after the square of `3`, it also means `6` is in its right place, so we can put the square of `6` at the index 5, then we move the pointer for large number to the left by one, `3` is smaller than `5`, so we put the square of `-5` at the index 4, and move the pointer for small number to right by one, now we compare `-4` with `3`, the absolute value of `-4` is still larger than `3`, put the square of `-4` at the index 3, so on so forth, until all the numbers are in the right place. Therefore we also need a variable to keep track of the correct index for inserting.

1. initialize an empty array to store the result.
2. initialize two pointers, one called `smallerValueIdx` and other called `largerValueIdx`, point `smallerValueIdx` to first number in the array, because it is the smallest number in the array, and `largerValueIdx` to the last number in the array.
3. initialize a variable that indexes the next location in the result array that we put the square into. Set it to the index of the last element in the array. Name the variable `i`.
4. Loop until `i` is less than 0, which means all numbers are squared and put into the result array.
   - compare the absolute value of the number `smallerValueIdx` points to with the absolute value of the number `largerValueIdx` points to, if the value of the smaller number is greater than or equal to the larger number, square the smaller number and put the square into the result array at index `i`; increase `smallerValueIdx` by 1, otherwise, square the larger number and put the result into the result array, decrease `largerValueIdx` by 1.
   - decrease `i` by 1.

### Solution 2

```js
function sortedSquaredArray(array) {
  const sortedSquares = new Array(array.length).fill(0);
  let smallerValueIdx = 0;
  let largerValueIdx = array.length - 1;

  for (let i = array.length - 1; i >= 0; i--) {
    const smallerValue = array[smallerValueIdx];
    const largerValue = array[largerValueIdx];
    if (Math.abs(smallerValue) >= Math.abs(largerValue)) {
      sortedSquares[i] = smallerValue * smallerValue;
      smallerValueIdx++;
    } else {
      sortedSquares[i] = largerValue * largerValue;
      largerValueIdx--;
    }
  }

  return sortedSquares;
}
```
