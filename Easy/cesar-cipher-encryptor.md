# Cesar Cipher Encryptor

### Understanding the problem

Given a non-empty string that consists of lowercase English letters and a non-negative integer `key`, I am asked to write a function that is going to shift each letter in the given string `key` positions in the alphabet and return the new string.

#

### Approach

To shift a English letter `k` places down in the alphabet, we can convert it to ASCII value, add `k` to it,then convert the new ASCII value back to the corresponding letter. We can use the formula `newPos = (oldPos + k) % 26` (zero-based position) to handle the circular shifting, for instance, `(25 + 1) % 26 = 0`, `(25 + 26) % 26 = 25`, `(25 + 27) % 26 = 0`. However, we should notice, the result of `122 % 26` is `18`, where `122` is the ASCII value of letter `z`, and `97 % 26 = 19`, where `97` is the ASCII value of letter `a`. If we do `(97 - 19) % 26`, then we get `0`; `(122 - 19) % 26 = 25`, `(122 + 1 - 19) % 26 = 0`. So to get the position of the new letter in the alphabet, we can use the formula `newPos = (oldAsciiValue + k - 19) % 26`. Then we can add `97` to the position to get the ASCII value.

- Initialize an empty array `newChars` to store the new characters.

- Iterate through the input string.

  - For each character in the string, get its ASCII value. Then use the formula `(ASCII code + key - 19) % 26 + 97` to get the new ASCII value.

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
