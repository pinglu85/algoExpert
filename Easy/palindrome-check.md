# Palindrome Check

### Understanding the problem

We are asked to write a function that is going to determine whether or not a given string is a palindrome, in other words, whether the given string reads the same backward as forward.

#

### Approach 1: Two Pointers (Iterative)

Suppose we have the following string, which is a palindrome:

```
racecar
```

We can observe that the first character of the string is equal to the last character, the second character is equal to the second to last character, ..., except the middle character. So to determine whether a string is a palindrome we can walk from both the start and the end of the string to the middle and comparing every character against the other. If we reach the middle character without any mismatches, then the string is a palindrome.

**Algorithm**

- Initialize two pointers `left` and `right` to the first index of the string and the last index of the string respectively.

- While the left pointer comes before the right pointer,

  - Compare the characters that the two pointers are pointing at. If they are not the same, return `false`.
    Otherwise, move the left pointer to right and the right pointer to left.

- If the loop ends without returning `false`, it means there are no mismatches, so the string is a palindrome, we return `true`.

### Implementation

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

### Complexity Analysis

- Time Complexity: O(N), where N is the length of the string.

- Space Complexity: O(1).

#

### Approach 2: Two Pointers (Recursion)

We can rewrite the above iterative solution recursively.

### Implementation

```js
function isPalindrome(string, leftIdx = 0) {
  const rightIdx = string.length - 1 - leftIdx;

  if (leftIdx >= rightIdx) return true;

  return (
    string[leftIdx] === string[rightIdx] && isPalindrome(string, leftIdx + 1)
  );
}
```

### Complexity Analysis

Given N as the length of the string.

- Time Complexity: O(N).

- Space Complexity: O(N).

#

### Approach 3: Reverse the input string (Store the reversed version as a string)

If a string is a palindrome, the reversed version of the string is the same as the original string.

```
"racecar"
reversed: "racecar"

"hello"
reversed: "olleh"
```

So to determine whether a string is a palindrome, we can just reverse the input string and compare the reversed string to the original one.

### Implementation

```js
function isPalindrome(string) {
  let reversedString = '';

  for (let i = string.length - 1; i >= 0; i--) {
    reversedString += string[i];
  }

  return reversedString === string;
}
```

### Complexity Analysis

Given N as the length of the string.

- Time Complexity: O(N^2).

  In most programming languages strings are immutable. When appending a character to a string, a new string must be created, which is an O(N) operation. To create a reversed version of a string of length N, we will append N characters to the reversed string. Thus the overall time complexity is O(N^2).

- Space Complexity: O(N).

#

### Approach 4: Reverse the input string (Store the reversed version as an array of characters)

We can reduce the reverse from O(N^2) to O(N) by using an array to store the reversed version of the input string, since inserting an element at the end of an array is an amortized O(1) operation as long as the array is implemented as a dynamic array.

### Implementation

```js
function isPalindrome(string) {
  const reversedChars = [];

  for (let i = string.length - 1; i >= 0; i--) {
    reversedChars.push(string[i]);
  }

  return reversedChars.join('') === string;
}
```

### Complexity Analysis

Given N as the length of the string.

- Time Complexity: O(N).

- Space Complexity: O(N).
