# Find Three Largest Numbers

### Understanding the problem

Given an array that contains at least three integers, I am asked to write a function that is going to find the three largest numbers in the input array without sorting and return them in an array sorted in ascending order.

#

### Brute-force Approach

I am going to solve the problem by searching for the three largest numbers in the input array and move them to the very end of the array. I would use a for loop starting at the very end of the array. The for loop is going to stop when the index of the last third number in the array is reached, because I only need to find the three largest numbers in the array. At each iteration, I am going to initialize a variable that is going to keep track of the index of the largest number in the current subarray, which is going to start at index `0` and end at the current index of the for loop; then I am going to start traversing the current subarray, searching for the largest number in it and updating the index of the largest number accordingly. When I get out of the inner loop, move the largest number to the end of the current subarray. Once I am done with the outer loop, return the last three numbers in the input array in an array.

### Time & Space Complexity

O(n) time | O(1) space, where n is the length of the input array.

The algorithm runs in 0(n) time, because the outer loop is always going to be executed 3 times, so the run time of the algorithm is approximately equal to O(3 \* n), which is equal to O(n).

### Brute-force Solution

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

### Better Approach

I am going to keep track of the three largest numbers as I traverse the entire input array. First I am going to initialize an array of length 3 filled with `-Infinity` values and it is going to keep track of the three largest numbers that I have currently seen. Then I would loop through the input array. For each number, I am going to compare it to the current third largest number, the number at index `0` in the array of the three largest numbers, if it is smaller than that number, move on to the next number in the input array; otherwise compare it to the current largest number, the number at index `2` in the array of the three largest numbers, and then depending on the comparison compare it to the current second largest number, the number at index `1` in the array of the three largest numbers:

- If the number is greater than the current largest number, shift the current largest number and the current second largest number to the left by one, then place the new largest number into index `2`.
- If the number is smaller than the current largest number but greater than the current second largest number, move the current second largest number to index `0`, and store the number in index `1`.
- Else set it as the current third largest number by placing it into index `0`.

Once I get out of the loop, return the array of the three largest numbers.

### Time & Space Complexity

O(n) time | O(1) space, where n is the length of the input array.

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
