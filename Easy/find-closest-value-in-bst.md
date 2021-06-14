# Find Closest Value In BST

### Understanding the problem

Given a Binary Search Tree and a target integer value, I am asked to write a function that returns the closest value to that target value that's contained in the BST. There will be only one closest value.

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

The closest value to the target value in the BST is 13.

#

### Approach

Use a variable to keep track of the current node; initialize it to be the root node of the BST. I also need a variable to keep track of the closest value in the BST so far; initialize it to 0, and another variable to keep track of the smallest absolute difference I've currently seen, that is the absolute difference between the current closest value and the target value; initialize it to `Infinity`.

Compute the absolute difference between the current node's value and the target value, compare the result to the current smallest difference, if the result is smaller, set the node's value as the current closest value and update the current smallest absolute difference. Compare the target value to the current node's value, if the target value is less than the current node's value, explore the left subtree of the current node; if it is greater than the value, explore the right subtree; otherwise return the closest value, since the current node's value and the target value are equal to each other, which means there is no other value that can be closer to the target value than this value. Keep exploring the BST until the bottom of the tree is reached. Then return the current closest value.

### Time & Space Complexity

- Iterative:

  Average: O(log(n)) time | O(1) space, where n is the number of nodes in the Binary Search Tree.

  Worst: O(n) time | O(1) space, , where n is the number of nodes in the Binary Search Tree.

- Recursive:

  Average: O(log(n)) time | O(log(n)) space, where n is the number of nodes in the Binary Search Tree.

  Worst: O(n) time | O(n) space, where n is the number of nodes in the Binary Search Tree.

  The space complexity is on average O(log(n)) and the worst O(n), because each recursive call to `findClosestValueInBst` adds a new frame on the call stack, which means we are using extra memory. In other words, we'll be using O(h) memory, where `h` is the height of the tree.

### Iterative Solution

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

### Recursive Solution

```js
function findClosestValueInBst(tree, target) {
  return findClosestValueInBstImpl(tree, target, 0, Infinity);
}

function findClosestValueInBstImpl(
  tree,
  target,
  closestValue,
  smallestDifference
) {
  if (tree === null) return closestValue;

  const currentValue = tree.value;
  const currentDifference = Math.abs(currentValue - target);
  if (currentDifference < smallestDifference) {
    closestValue = currentValue;
    smallestDifference = currentDifference;
  }

  if (target === currentValue) {
    return closestValue;
  }

  const nextNode = target < currentValue ? tree.left : tree.right;
  return findClosestValueInBstImpl(
    nextNode,
    target,
    closestValue,
    smallestDifference
  );
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
