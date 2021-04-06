### Understanding the problem

Given a Binary Tree, I am asked to write a function that computes all of the branch sums of the tree and returns them in an array ordered from leftmost branch sum to rightmost branch sum. In a tree, a branch is a path that starts at the root node and ends at one of the leaf nodes. A branch sum means the sum of all values in a branch.

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

### Approach with recursive DFS - O(n) time | O(n) space, where n is the number of nodes in the Binary Tree.

Instead of using a stack to keep track of the next node that need to be visited and the current branch sum, use the call stack to track these info.

- initialize an empty array to store the branch sums.
- write a helper function that will be recursively invoked. The function takes in three arguments. The first argument is the node needs to be visited; the second argument is the current branch sum; and the last argument is the array that stores the result. When it gets called for the first time, the node to be visited is the root node of the tree and the current branch sum is 0. In the helper function:
  - check if the node to be visited is null. If it is, return.
  - calculate the new current branch sum by adding the value of the node to the current branch sum.
  - if the node doesn't have any children, push the sum into the array that stores the result.
  - recursively call the helper function passing in the left child of the node, updated current branch sum and the array that stores the result.
  - recursively call the helper function passing in the right child of the node, updated current branch sum and the array that stores the result.
- when I get out of the helper function, return the resulting array.

**Note regarding the space complexity**: Each recursive call to the helper function adds a new frame on the call stack. On average we will never have more than log(n) recursive calls on the call stack, since we eliminate half the nodes in the remaining tree at each recursive call. In the worst case, when we are dealing with a very imbalanced binary tree, we would have O(n) space from the recursive calls, since we have n recursive calls on the call stack at once. Besides the space from the recursive calls, we also return an array of branch sums. The size of the array is same as the number of branches in the Binary Tree, which is the number of leaf nodes in the Binary Tree. There are roughly half of n leaf nodes in the Binary Tree and half of n is equal to O(n) in the space time complexity analysis.

### Recursive Solution

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
  calculateBranchSums(root, 0, sums);
  return sums;
}

function calculateBranchSums(node, branchSum, sums) {
  if (!node) return;

  const newBranchSum = branchSum + node.value;
  if (!node.left && !node.right) {
    sums.push(newBranchSum);
  }

  calculateBranchSums(node.left, newBranchSum, sums);
  calculateBranchSums(node.right, newBranchSum, sums);
}
```
