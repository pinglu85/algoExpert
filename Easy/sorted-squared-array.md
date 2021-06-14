# Sorted Squared Array

### Understanding the problem

Given an array of integers that are sorted in increasing order, write a function that squares all the integers in the array and returns them in a new array, also sorted in increasing order.

#

### Approach

At first glance, I think all I need to do is traverse the input array element by element, square each value and then return the resulting array, or I can use the `.map()` method, which returns a new array populated with the results of calling a provided function on every element in the calling array. But this approach only works for positive numbers, because when we square negative numbers, we get a positive result, if we add them directly to the resulting array, the array is no longer sorted. I can sort the resulting array, then the solution is gonna run an O(nlog(n)) time. Instead of sorting, I can initialize two arrays, one storing the squares of non-negative integers, other storing the squares of negative numbers. At each iteration, if the integer is negative, push the square into the negative array, otherwise push the square into the positive array. After squaring all the integers, I need to merge the two sorted arrays into one sorted array:

- Initialize a new empty array to store the result.
- Initialize two pointers to keep track of the position we are at in the both arrays. Point the pointer for the non-negative array to the beginning of it, and the one for the negative array to the last number in the array, because, for instance, if we have two negative numbers -5 and -1 and they are sorted in ascending order, after squaring, we get 25 and 1, and now they are in descending order - the smaller negative number becomes bigger after squaring.
- Iterate until both pointers out of bound. At each iteration, compare the values the two pointers point to, push the smaller one into the resulting array, if it is from the non-negative array, increase the corresponding pointer, otherwise, decrease the corresponding pointer.
- After the loop, if the pointer for non-negative array is not equal to the length of the array, push the remaining integers into the resulting array; if the pointer for negative array is not less than 0, push the remaining integers into the resulting array.

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input array.

### Solution

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

  return mergeSortedArrays(positiveSquares, negativeSquares);
}

function mergeSortedArrays(ascendingArray, descendingArray) {
  const mergedArray = [];
  let ascendingIdx = 0;
  let descendingIdx = descendingArray.length - 1;

  while (ascendingIdx < ascendingArray.length || descendingIdx >= 0) {
    const ascendingItem = getItemByIdx(ascendingArray, ascendingIdx);
    const descendingItem = getItemByIdx(descendingArray, descendingIdx);

    if (ascendingItem < descendingItem) {
      mergedArray.push(ascendingItem);
      ascendingIdx++;
    } else {
      mergedArray.push(descendingItem);
      descendingIdx--;
    }
  }

  return mergedArray;
}

function getItemByIdx(array, idx) {
  return array[idx] === undefined ? Infinity : array[idx];
}
```

#

### Better Approach (based on the video explanation of AlgoExpert)

Consider we have the input array `[-4, -2, 0, 1, 3]`, after being squared, we got `[16, 4, 0, 1, 9]`, we can realize the smallest possible squared value which could be end up in the output array is `0`. The farther the value is away from 0, regardless of whether it is positive or negative, the larger the squared value will become. And the closer the value is to 0, the smaller the squared value.
The number line below illustrates this.

```
<--|---|---|---|---|---|---|---|---|-->
  -5  -4  -3  -2  -1   0   1   2   3
  25  16   9   4   1   0   1   4   9
```

What the number line also tells us is the largest value in the output array is either going to come from the smallest element in the input array or the largest element in the input array.
Since we know where to find the largest squared value in the input array, we can build the sorted squared values array from largest square value to smallest squared value. We can use two pointers to keep track of the smallest integer and the largest integer in the input array; compare the absolute value of the two integers to find out which integer is larger once squared; square the larger value and put it into the resulting array; move the pointer that points to the larger value either to left if the largest squared value comes from the largest integer or to right if it comes from the smallest integer, so we can find the next largest squared value. Continue until the resulting array is filled up.

- Initialize an empty array to store the squared values. The size of the array is going to be the same as the input array.
- Initialize two pointers, one called `smallerValueIdx` and other called `largerValueIdx`, point `smallerValueIdx` to first number in the array and `largerValueIdx` to the last number in the array.
- Initialize a variable that indexes the correct location in the resulting array that we should put the square into. Name the variable `idx`. Initially, `idx` equals the index of the last element in the array.
- Loop until `idx` is less than 0, which means all numbers are squared and put into the resulting array.
  - Compare the absolute value of the integer that `smallerValueIdx` points to with the absolute value of the integer that `largerValueIdx` points to, if the value of the smaller number is greater than or equal to the larger number, square the smaller number and place the result into index `idx` in the resulting array; increase `smallerValueIdx` by 1, otherwise, square the larger number and put the square into index `idx` of the resulting array, decrease `largerValueIdx` by 1.
  - Decrease `idx` by 1.

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input array.

### Better Solution

```js
function sortedSquaredArray(array) {
  const sortedSquares = new Array(array.length).fill(0);
  let smallerValueIdx = 0;
  let largerValueIdx = array.length - 1;

  for (let idx = array.length - 1; idx >= 0; idx--) {
    const smallerValue = array[smallerValueIdx];
    const largerValue = array[largerValueIdx];
    if (Math.abs(smallerValue) >= Math.abs(largerValue)) {
      sortedSquares[idx] = smallerValue * smallerValue;
      smallerValueIdx++;
    } else {
      sortedSquares[idx] = largerValue * largerValue;
      largerValueIdx--;
    }
  }

  return sortedSquares;
}
```
