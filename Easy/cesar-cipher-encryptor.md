# Cesar Cipher Encryptor

### Understanding the problem

Given a non-empty string that consists of lowercase English letters and a non-negative integer `key`, I am asked to write a function that is going to shift each letter in the given string `key` positions in the alphabet and return the new string.

#

### Approach 1

To shift a English letter `k` places down in the alphabet, we can convert it to ASCII value, add `k` to it,then convert the new ASCII value back to the corresponding letter. We can use the formula `newPos = (oldPos + k) % 26` to handle the circular shifting. The modulo will give us a value between `0 - 25`. For instance, `(25 + 1) % 26 = 0`, `(25 + 26) % 26 = 25`, `(25 + 27) % 26 = 0`. However, the result of `122 % 26` is `18`, where `122` is the ASCII value of letter `z`, and `97 % 26 = 19`, where `97` is the ASCII value of letter `a`. If we do `(97 - 19) % 26`, then we get `0`, and `(122 - 19) % 26 = 25`, `(122 + 1 - 19) % 26 = 0`. So to get the position of the new letter in the alphabet, we can use the formula `newPos = (oldAsciiValue + k - 19) % 26`. Then we can add `97` to the position to get the ASCII value.

- Initialize an empty array `newChars` to store the new characters.

- Iterate through the input string.

  - For each character in the string, get its ASCII value. Then use the formula `(ASCII code + key - 19) % 26 + 97` to get the new ASCII value.

  - Convert the new ASCII value back to character and push it into the `newChars` array.

- Join all the characters in the `newChars` array into a string and return the string.

### Implementation

```js
const LOWER_A_CODE = 97;
const ALPHABET_OFFSET = 19;

function caesarCipherEncryptor(string, key) {
  const newLetters = [];

  for (const letter of string) {
    newLetters.push(getNewLetter(letter, key));
  }

  return newLetters.join('');
}

function getNewLetter(letter, key) {
  const newLetterCode =
    ((letter.charCodeAt(0) + key - ALPHABET_OFFSET) % 26) + LOWER_A_CODE;
  return String.fromCharCode(newLetterCode);
}
```

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input string.

#

### Approach 2

To deal with circular shifting we can also use the formula: `new ASCII value = (ASCII value - 97 + key) % 26 + 97`, where `97` is the ASCII value of `a`. `ASCII value - 97` gives us the position of the letter in the alphabet (0-indexed) and `% 26` handles the overflow.

Credit: https://stackoverflow.com/a/57730559/10183557

### Implementation

```js
const LOWER_A_CODE = 97;

function caesarCipherEncryptor(string, key) {
  const newLetters = [];

  for (const letter of string) {
    newLetters.push(getNewLetter(letter, key));
  }

  return newLetters.join('');
}

function getNewLetter(letter, key) {
  const newLetterCode =
    ((letter.charCodeAt(0) - LOWER_A_CODE + key) % 26) + LOWER_A_CODE;
  return String.fromCharCode(newLetterCode);
}
```

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input string.

#

### Approach 3

We can also handle the circular shifting with `(ASCII value + key) % 122`. However, it won't handle the case where `key` is greater than `26`, i.e. `(122 + 27) % 122 = 27`, so we need to do `key % 26` first. The modulo operation will give us the correct ASCII value when `ASCII value + key < 122`, but returns `0` if `ASCII value + key == 122`, and gives us a value in the range of `1 - 26`, if `ASCII value + key > 122`. Thus we will apply this formula only when the new ASCII value is greater than `122`, and add `96` to the result of the modulo operation to get the final ASCII value.

### Implementation

```js
const LOWER_Z_CODE = 122;

function caesarCipherEncryptor(string, key) {
  const newLetters = [];
  key %= 26;

  for (const letter of string) {
    newLetters.push(getNewLetter(letter, key));
  }

  return newLetters.join('');
}

function getNewLetter(letter, key) {
  let newLetterCode = letter.charCodeAt(0) + key;

  if (newLetterCode > LOWER_Z_CODE) {
    newLetterCode = 96 + (newLetterCode % LOWER_Z_CODE);
  }

  return String.fromCharCode(newLetterCode);
}
```

### Time & Space Complexity

O(n) time | O(n) space, where n is the length of the input string.
