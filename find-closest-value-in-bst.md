### Understanding the problem

I will be given a Binary Search Tree and a target integer value. I am asked to write a function that returns the closest value to that target value in the BST. There will be only one closest value.

For example,

```
bst =   10
      /     \
     5      15
   /   \   /   \
  2     5 13   22
 /          \
1           14
target = 12
```

The closest value to the target value in the BST is 13. That means, we return the first value we found that is the closest value to the target value.

### Approach - O(log(n)) time | O(1) space

Use a variable to keep track of the current node; initialize it to be the root of the BST. I also need a variable to keep track of which node's value is the closest value so far; initialize it to 0, and another variable to keep track of the smallest difference I've currently seen, that is the absolute difference of the current closest value and the target value; initialize it to `Infinity`.

Calculate the absolute difference of the current node's value and the target value, compare the result to the current smallest difference, if we get to a smaller difference, set the node's value as the current closest value. Compare the target value to the current node's value, if the target value is less than the current node's value, move on to the left child; if it is greater than the value, move on to the right child; otherwise return the closest value, since the current node's value and the target value are equal to each other, which means we cannot find any value that is closer to the target value than this value. Keep updating the current node until the bottom of the tree is reached. Then return the current closest value.

### Solution

```js
function findClosestValueInBst(tree, target) {
  let currentClosestValue = 0;
  let currentSmallestDifference = Infinity;
  let currentNode = tree;

  while (currentNode !== null) {
    const currentValue = currentNode.value;
    const currentDifference = Math.abs(currentValue - target);
    if (currentDifference < currentSmallestDifference) {
      currentSmallestDifference = currentDifference;
      currentClosestValue = currentValue;
    }

    if (target < currentValue) {
      currentNode = currentNode.left;
    } else if (target > currentValue) {
      currentNode = currentNode.right;
    } else {
      break;
    }
  }

  return currentClosestValue;
}

// This is the class of the input tree. Do not edit.
class BST {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
```
