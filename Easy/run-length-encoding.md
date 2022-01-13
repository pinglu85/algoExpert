# Run-Length Encoding

### Understanding the problem

Given an non-empty string, we are asked to write a function that is going to run-length-encode the input string and return the encoded string. Run-length encoding refers to replacing repetitive, consecutive data by a count and one copy of a repeated data. For instance, `AAABB` will be encoded as `3A2B`. If a sequence contains more than 9 consecutive, identical characters, we first encode 9 characters, then the remaining ones. For instance, `AAAAAAAAAA` (10 `A`s) will be encode as `9A1A`.

#

### Approach

- Initialize an empty array `encodedChars` that is going to store the encoded characters.

- Initialize a variable `start` to `0`, that is going to store the start index of a sequence of consecutive, identical characters.

- Loop through the string. We start at index `1` and end at `last index + 1` to handle the last sequence.

  - If the character at current index is different than `string[start]`, then we know we reach a new sequence. If `current index - start` is equal to `9`, that means we have processed 9 characters. If current index is equal to the length of the string, it means we are handling the last sequence. When one of these conditions is met, compute the count by `current index - start` and push the count as well as `string[start]` into `encodedChars` array, set `start` to `end`.

- Join all the characters in the `encodedChars` array into a string and return the string.

### Implementation

```js
function runLengthEncoding(string) {
  const encodedChars = [];
  let start = 0;

  for (let end = 1; end <= string.length; end++) {
    if (
      end === string.length ||
      string[start] !== string[end] ||
      end - start === 9
    ) {
      encodedChars.push(end - start);
      encodedChars.push(string[start]);
      start = end;
    }
  }

  return encodedChars.join('');
}
```

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input string.
