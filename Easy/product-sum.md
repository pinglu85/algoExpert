# Product Sum

### Understanding the problem

I am given a "special" array, which is a non-empty array that contains integers and optionally other "special" arrays. I am asked to write a function that is going to return the sum of the elements in the "special" array. If I encounter a "special" array inside a "special" array, I need to sum the elements in the inner array up and then multiply the result by the depth of the inner array. For instance, if the input array is `[3, [4, 5]]`, then the result should be `3 + 2 * (4 + 5) = 21`; if the input array is `[6, [4, [5]]]`, then the result should be `6 + 2 * (4 + 3 * 5) = 44`.

### Approach - O(n) time | O(d) space - where n is the total number of elements in the array, including the sub-elements, and d is the greatest depth of "special" arrays in the array.

I am going to solve this problem recursively, and the function `productSum` is going to be my recursive function. First, I would initialize a variable that is going to store the sum of the current input array; initially, it is equal to 0. I will then loop through all of the elements in the input array. For each element, I am going to check if it is an array; if it is not, I am going to add the value to the sum; otherwise I am going to call the recursive function passing in the array and the depth of the array, which is the depth of the outer array, the input array, plus one; initially, the depth is going to be 1 and it will be passed to the function `productSum`; then I will add the returned value of the recursive function to the sum. After I get through all of the elements inside the array, I am going to multiply the sum by the depth of the input array and return the result.

### Solution

```js
function productSum(array, depth = 1) {
  let sum = 0;

  for (const element of array) {
    if (Array.isArray(element)) {
      sum += productSum(element, depth + 1);
    } else {
      sum += element;
    }
  }

  return sum * depth;
}
```
