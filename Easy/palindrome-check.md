# Palindrome Check

### Understanding the problem

I am asked to write a function that is going to determine whether or not a given string is a palindrome, in other words, whether a given string reads the same backward or forward.

#

### Approach 1: Two Pointers

Suppose we have the following string, which is a palindrome:

```
racecar
```

We can observe that the first character in the string is the same as the last character in the string, the second character is the same as the second last character, ..., until we reach the middle character. So we can
check whether or not a string is a palindrome by walking from both the start and the end of the string to the middle and comparing every character against the other. If we reach the middle character without any mismatches, then the string is a palindrome.

- Initialize two pointers, one pointing to the start index of the string and the other one to the final index of the string.

- While pointer 1 is smaller than pointer 2,

  - Compare the characters that the two pointers are pointing at. If they are not the same, return `false`

- When the loop ends without returning `false`, it means there are no mismatches, so the string is a palindrome, we return `true`.

### Time & Space Complexity

- Iterative: O(n) time | O(1) space, where n is the length of the string.

- Recursive: O(n) time | O(n) space, where n is the length of the string.

### Iterative Solution

```js
function isPalindrome(string) {
  let leftIdx = 0;
  let rightIdx = string.length - 1;

  while (leftIdx < rightIdx) {
    if (string[leftIdx] !== string[rightIdx]) return false;

    leftIdx++;
    rightIdx--;
  }

  return true;
}
```

### Recursive Solution

```js
function isPalindrome(string, leftIdx = 0) {
  const rightIdx = string.length - 1 - leftIdx;

  if (leftIdx >= rightIdx) return true;

  return (
    string[leftIdx] === string[rightIdx] && isPalindrome(string, leftIdx + 1)
  );
}
```

#

### Approach 2: Brute Force

If a string is a palindrome, the reversed version of the string is the same as the original string.

```
"racecar"
reversed: "racecar"

"hello"
reversed: "olleh"
```

So to determine whether a string is a palindrome, we can just reverse the input string and compare it to the original one.

### Time & Space Complexity

- Storing the reversed version of the input string as a string: O(n^2) time | O(n) space, where n is the length of the string. The reason that it takes O(n^2) time is because in most programming languages strings
  are immutable. When appending a character to a string, a new string must be created, which is an O(n) operation. To create a reversed version of a string of length n, we will append n characters to the reversed string. Thus the overall time complexity is O(n^2).

- Using an array to store the reversed string:
  O(n) time | O(n) space, where n is the length of the string.

### Solution with string

```js
function isPalindrome(string) {
  let reversedString = '';

  for (let i = string.length - 1; i >= 0; i--) {
    reversedString += string[i];
  }

  return reversedString === string;
}
```

### Solution with array

```js
function isPalindrome(string) {
  const reversedChars = [];

  for (let i = string.length - 1; i >= 0; i--) {
    reversedChars.push(string[i]);
  }

  return reversedChars.join('') === string;
}
```
