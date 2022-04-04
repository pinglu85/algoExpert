# Run-Length Encoding

### Understanding the problem

Given a non-empty string, we are asked to write a function that is going to run-length-encode the input string and return the encoded string. Run-length encoding refers to replacing repetitive, consecutive data by a count and one copy of a repeated data. For instance, `AAABB` will be encoded as `3A2B`. If a sequence contains more than 9 consecutive, identical characters, we first encode 9 characters, then the remaining ones. For instance, `AAAAAAAAAA` (10 `A`s) will be encode as `9A1A`.

#

### Approach 1

We can iterate through the string and keep track of the starting index of a sequence. Whenever we get to a new sequence, either the character at current index doesn't match the first character of the current sequence or the current sequence is of length `9`, we append the length of the current sequence as well as the first character to our encoded string and set the current index as the starting index.

**Algorithm**

- Initialize an empty array `encodedChars` that is going to store the encoded characters.

- Initialize a variable `start` to `0`, that is going to keep track of the starting index of a sequence.

- Loop through the input string. We start at index `1` and end at `last index + 1` to handle the last sequence.

  - If the character at current index is not equal to the character that `start` is pointing at, then we know that we reach a new sequence. If `current index - start` is equal to `9`, it means we have processed 9 characters. If current index is equal to the length of the string, it means we are dealing with the last sequence. When one of the three conditions is satisfied, then push the count: ``current index - start` and the character that `start` is pointing at into the `encodedChars` array, and then set `start` to the current index.

- Join all the characters in the `encodedChars` array into a string and return the string.

### Implementation

```js
function runLengthEncoding(string) {
  const encodedChars = [];
  let start = 0;

  for (let i = 1; i <= string.length; i++) {
    if (i === string.length || string[start] !== string[i] || i - start === 9) {
      encodedChars.push(i - start);
      encodedChars.push(string[start]);
      start = i;
    }
  }

  return encodedChars.join('');
}
```

### Complexity Analysis

Let N be the length of the input string.

- Time Complexity: O(N).

- Space Complexity: O(N).

#

### Approach 2

Instead of tracking the starting index of the current sequence, we can also track its length.

### Implementation

```js
function runLengthEncoding(string) {
  const encodedChars = [];
  let currRunLength = 1;

  for (let i = 1; i <= string.length; i++) {
    const prevChar = string[i - 1];

    if (i === string.length || string[i] !== prevChar || currRunLength === 9) {
      encodedChars.push(String(currRunLength));
      encodedChars.push(prevChar);
      currRunLength = 0;
    }

    currRunLength++;
  }

  return encodedChars.join('');
}
```

### Complexity Analysis

Let N be the length of the input string.

- Time Complexity: O(N).

- Space Complexity: O(N).
