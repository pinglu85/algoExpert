# Find Three Largest Numbers

### Understanding the problem

Given an array that contains at least three integers, we are asked to write a function that is going to find the three largest numbers in the input array and return them in an array sorted in ascending order. We are not allowed to sort the input array.

#

### Approach 1: Brute-force

We can solve the problem by searching for the three largest numbers in the input array and move them to the very end of the array. The logic is similar to the Selection Sort, where we keep searching for the smallest number in the current subarray and moving it to the very beginning of the current subarray.

We use a for loop that starts from the very end of the array and stops when the index of the last third number in the array is reached, because we only need to find the three largest numbers in the array. In each iteration,

- Initialize a variable `largestNumIdx` to `0` that is going to keep track of the index of the largest number in the current subarray.

- Traverse the current subarray, starting from index `0` and ending at the current index of the outer for loop. When we get to a number that is greater than the number that `largestNumIdx` points to, set current index as the `largestNumIdx`.

- When the traversal finishes, move the largest number to the end of the current subarray.

Once we are done with the outer loop, return the last three numbers in the input array in an array.

### Implementation

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

### Complexity Analysis

- Time Complexity: O(N), where N is the length of the input array. The algorithm takes linear time, because the outer loop is always going to be executed 3 times - we only search for the three largest numbers in the array, so the run time of the algorithm is approximately equal to O(3 \* N), which is equal to O(N).

- Space Complexity: O(1).

#

### Approach 2: Using Array to Track the Three Largest Number

We are going to keep track of the three largest numbers as we traverse the entire input array.

- First we are going to initialize an array of length `3` filled with `-Infinity` values. The array is going to keep track of the three largest numbers that we've seen so far.

- Then we traverse the input array. For each number, we are going to compare it to the current third largest number, the number at index `0` in the array of the three largest numbers. If it is smaller than that number, move on to the next number in the input array; otherwise compare it to the current largest number, the number at index `2` in the array of the three largest numbers, then depending on the comparison compare it to the current second largest number, the number at index `1` in the array of the three largest numbers:

  - If the number is greater than the current largest number, shift the current largest number and the current second largest number to the left by one, then place the new largest number into index `2`.

  - If the number is smaller than the current largest number but greater than the current second largest number, move the current second largest number to index `0`, and store the number in index `1`.

  - Else set it as the current third largest number by placing it into index `0`.

- Once we get out of the loop, return the array of the three largest numbers.

### Implementation

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

**Alternative Implementation**

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

### Complexity Analysis

- Time Complexity: O(N), where N is the length of the input array.

- Space Complexity: O(1).
