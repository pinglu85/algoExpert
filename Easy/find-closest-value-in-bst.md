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

We are going to use a variable to keep track of the current node and initialize it to the root node of the BST. In addition, we also need a variable to keep track of the closest value in the BST so far; initialize it to the value of the root node.

While current node is not `null`,

1. compute the absolute difference between the value of the current node and the target value, compare the result to the absolute difference between the closest value we've seen so far and the target. If we get to a smaller absolute difference, set the value of the current node as the current closest value.

2. Compare the target value to the current node's value:

   - if the target value is less than the current node's value, explore the left subtree of the current node;

   - if the target value is greater than the current node's value, explore the right subtree;

   - otherwise break out of the while loop, since the current node's value and the target value are equal to each other, which means there is no other value that can be closer to the target value than this value.

3. Return the closest value.

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
  let currNode = tree;
  let currClosestValue = tree.value;

  while (currNode !== null) {
    if (
      Math.abs(currNode.value - target) < Math.abs(currClosestValue - target)
    ) {
      currClosestValue = currNode.value;
    }

    if (target < currNode.value) {
      currNode = currNode.left;
    } else if (target > currNode.value) {
      currNode = currNode.right;
    } else {
      break;
    }
  }

  return currClosestValue;
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

### Iterative Solution in Go

```go
package main

type BST struct {
	Value int

	Left  *BST
	Right *BST
}

func (tree *BST) FindClosestValue(target int) int {
	currNode := tree
	closest := tree.Value

	for currNode != nil {
		if absDiff(currNode.Value, target) < absDiff(closest, target) {
			closest = currNode.Value
		}

		if target < currNode.Value {
			currNode = currNode.Left
		} else if target > currNode.Value {
			currNode = currNode.Right
		} else {
			break
		}
	}

	return closest
}

func absDiff(x, y int) int {
	if x > y {
		return x - y
	}

	return y - x
}
```

### Recursive Solution

```js
function findClosestValueInBst(tree, target) {
  return findClosestValueInBstImpl(tree, target, tree.value);
}

function findClosestValueInBstImpl(tree, target, closestValue) {
  if (Math.abs(tree.value - target) < Math.abs(closestValue - target)) {
    closestValue = tree.value;
  }

  if (target < tree.value && tree.left !== null) {
    return findClosestValueInBstImpl(tree.left, target, closestValue);
  }

  if (target > tree.value && tree.right !== null) {
    return findClosestValueInBstImpl(tree.right, target, closestValue);
  }

  return closestValue;
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

### Recursive Solution in Go

```go
package main

type BST struct {
	Value int

	Left  *BST
	Right *BST
}

func (tree *BST) FindClosestValue(target int) int {
	return findClosestValueImpl(tree, target, tree.Value)
}

func findClosestValueImpl(tree *BST, target, closest int) int {
	if absDiff(tree.Value, target) < absDiff(closest, target) {
		closest = tree.Value
	}

	if target < tree.Value && tree.Left != nil {
		return findClosestValueImpl(tree.Left, target, closest)
	}

	if target > tree.Value && tree.Right != nil {
		return findClosestValueImpl(tree.Right, target, closest)
	}

	return closest
}

func absDiff(x, y int) int {
	if x > y {
		return x - y
	}

	return y - x
}
```
