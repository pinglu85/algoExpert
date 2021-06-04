# Find Three Largest Numbers

### Understanding the problem

Given an array that contains at least three integers, I am asked to write a function that is going to find the three largest numbers in the input array without sorting and return them in an array sorted in ascending order.

#

### Approach - O(n) time | O(1) space, where n is the length of the input array.

I am going to use a for loop starting at the end of the array. The for loop is going to stop when I reach the index of the last third number in the input array, since I only need to find the three largest numbers. At each iteration, I am going to initialize a variable that is going to keep track of the index of the largest number in the current subarray, then use another for loop starting from the start of the input array to search for the largest number in the current subarray and update the index of the largest number. The range of the current subarray is going to be specified by the inner loop index and the outer loop index, where the former indexes the start position of the current subarray and the latter the end position. When I get out of the inner loop, move the largest number to the end of the current subarray. Finally, when I get out of the outer loop, return the last three numbers in the input array in an array.

The algorithm runs in 0(n) time, because the outer loop is always going to be executed 3 times, so the run time of the algorithm is approximately equal to O(3 \* n), which is equal to O(n).

### Solution

```js
function findThreeLargestNumbers(array) {
  for (let i = array.length - 1; i >= array.length - 3; i--) {
    let largestNumIdx = 0;
    for (let j = 0; j <= i; j++) {
      if (array[j] > array[largestNumIdx]) {
        largestNumIdx = j;
      }
    }
    [array[largestNumIdx], array[i]] = [array[i], array[largestNumIdx]];
  }

  return array.slice(-3);
}
```

#

### Better Approach - O(n) time | O(1) space, where n is the length of the input array.

I am going to initialize an array populated with three `-Infinity`s and the array is going to keep track of the three largest numbers so far. Then I would loop through the input array. For each number in the input array, first I am going to compare the number to the first number in the array of the three largest numbers, in other words, compare the number to the smallest number in the array of the three largest numbers, if it is smaller than that number, move on to the next number in the input array; otherwise compare it to the other two numbers in the array of the three largest numbers, starting from the last number in that array:

- If the number is greater than the last number, the largest number, in the array of the three largest numbers, shift the current last number and the current second number to the left by one, then place the new largest number in index `2`.
- If the number is greater than the second number in the array of the three largest numbers, move the current second number to index `0`, and store the new number in index `1`.
- Else replace the current first number with the new number.

Lastly, return the array of the three largest numbers.

### Better Solution

```js
function findThreeLargestNumbers(array) {
  const threeLargestNums = [-Infinity, -Infinity, -Infinity];

  for (const num of array) {
    const thirdLargestNum = threeLargestNums[0];
    if (num > thirdLargestNum) {
      updateThreeLargestNums(threeLargestNums, num);
    }
  }

  return threeLargestNums;
}

function updateThreeLargestNums(threeLargestNums, num) {
  const [, secondLargestNum, largestNum] = threeLargestNums;
  if (num > largestNum) {
    threeLargestNums[2] = num;
    threeLargestNums[1] = largestNum;
    threeLargestNums[0] = secondLargestNum;
  } else if (num > secondLargestNum) {
    threeLargestNums[1] = num;
    threeLargestNums[0] = secondLargestNum;
  } else {
    threeLargestNums[0] = num;
  }
}
```

### Refactored Solution

```js
function findThreeLargestNumbers(array) {
  const threeLargestNums = [-Infinity, -Infinity, -Infinity];
  for (const num of array) {
    updateThreeLargestNums(threeLargestNums, num);
  }
  return threeLargestNums;
}

function updateThreeLargestNums(threeLargestNums, num) {
  const [thirdLargestNum, secondLargestNum, largestNum] = threeLargestNums;
  if (num > largestNum) {
    shiftAndUpdate(threeLargestNums, num, 2);
  } else if (num > secondLargestNum) {
    shiftAndUpdate(threeLargestNums, num, 1);
  } else if (num > thirdLargestNum) {
    shiftAndUpdate(threeLargestNums, num, 0);
  }
}

function shiftAndUpdate(array, num, idx) {
  for (let i = 0; i <= idx; i++) {
    if (i === idx) {
      array[i] = num;
    } else {
      array[i] = array[i + 1];
    }
  }
}
```
