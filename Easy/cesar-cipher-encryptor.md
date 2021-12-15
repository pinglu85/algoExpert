# Cesar Cipher Encryptor

### Understanding the problem

Given a non-empty string that consists of lowercase English letters and a non-negative integer `key`, I am asked to write a function that is going to shift each letter in the given string `key` positions in the alphabet and return the new string.

#

### Approach

- Initialize an empty array `newChars` to store the new characters.

- Iterate through the input string.

  - For each character in the string, get its ASCII value. Then we can use the following formula to get the new ASCII value:

  ```
  (ASCII code + key - 19) % 26 + 97
  ```

  - Convert the new ASCII value back to character and push it into the `newChars` array.

- Join all the characters in the `newChars` array into a string and return the string.

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input string.

### Solution

```js
const LOWER_A_CODE = 97;
const ALPHABET_OFFSET = 19;

function caesarCipherEncryptor(string, key) {
  const newChars = [];

  for (const char of string) {
    newChars.push(getNewChar(char, key));
  }

  return newChars.join('');
}

function getNewChar(char, key) {
  const newCharCode =
    ((char.charCodeAt(0) + key - ALPHABET_OFFSET) % 26) + LOWER_A_CODE;
  return String.fromCharCode(newCharCode);
}
```
