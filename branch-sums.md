### Understanding the problem

Given a Binary Tree, I am asked to write a function that computes all of the branch sums of the tree and returns them in an array ordered from leftmost branch sum to rightmost branch sum. In a tree, a branch is a path between the root node of the tree and any leaf node. A branch sum means the sum of all values in a branch.

Sample Input:

```
tree =   1
      /     \
     2       3
   /   \   /   \
  4     5 6     7
 / \
8   9
```

The output should be:

```
[15, 16, 8, 10, 11]
```

### Approach with iterative DFS - O(n) time | O(n) space, where n is the number of nodes in the Binary Tree.

Create an empty array to store the branch sums. Use Depth-First Search to traverse the binary tree. For each node, compute the sum of the values between the root node and the current node and store the result to the current node. If a node is a leaf node, compute the sum and add it to the resulting array.

DFS can be implemented using iterative approach or recursive approach. Here I will implement it iteratively. The iterative DFS use a stack to keep track of next nodes we need to explore. A stack is typically implemented with a dynamic array or a singly linked list. Since arrays in JavaScript are dynamic, that means ending or removing an element at the end of an array is amortized constant time, I will use an array as the stack.

- initialize an empty array to store the branch sums.
- initialize an empty array as a stack to keep track of the next nodes I need to visit. Add an array to the stack, where the first element is the root node, and the second element is current branch sum, which is the value of the root node.
- loop until the stack is empty

  - pop the element from the stack. Use array destructuring to get the current node and the current branch sum.
  - get the current node's child nodes. If there are child nodes, for each child node, compute its branch sum, which is the result of adding the current branch sum and the child node's value, store the child node and its branch sum in an array and add the array into the stack. Because of how stack works, if I add the left child node first to the stack, then at next iteration, I will first visit the right child node. Since the branch sums should be ordered from leftmost branch sum to rightmost branch sum, I need to add the right child node first.

  If the current doesn't have child nodes, that means a leaf node is reached, add the current branch sum to the resulting array.

- returns the resulting array.

### Iterative Solution

```js
// This is the class of the input root.
// Do not edit it.
class BinaryTree {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

function branchSums(root) {
  const sums = [];
  const stack = [];
  stack.push([root, root.value]);

  while (stack.length > 0) {
    const [node, branchSum] = stack.pop();
    const rightChild = node.right;
    const leftChild = node.left;

    if (!rightChild && !leftChild) {
      sums.push(branchSum);
      continue;
    }

    let currentBranchSum;
    if (rightChild) {
      currentBranchSum = branchSum + rightChild.value;
      stack.push([rightChild, currentBranchSum]);
    }
    if (leftChild) {
      currentBranchSum = branchSum + leftChild.value;
      stack.push([leftChild, currentBranchSum]);
    }
  }

  return sums;
}
```
