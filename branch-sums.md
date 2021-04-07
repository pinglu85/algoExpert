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

Create an empty array to store the branch sums. Use Depth-First Search to traverse the binary tree. For each node, add its value to the sum of all nodes above it, store the result and the node in a data structure, so that I can keep track of the running sum, which means when I visit a new node, I can retrieve the sum of all nodes above the new node. If a node is a leaf node, append the updated running sum to the branch sums array.

DFS can be implemented using iterative approach or recursive approach. Here I will implement it iteratively. The iterative DFS use a stack to keep track of next nodes need to be visited. A stack is typically implemented with a dynamic array or a singly linked list. Since arrays in JavaScript are dynamic, that means ending or removing an element at the end of an array is amortized constant time, I will use an array as the stack.

- initialize an empty array to store the branch sums.
- initialize an empty array as a stack to keep track of the next nodes I need to visit and the running sum till that node. The stack is going to be an array of objects; each object stores the node to be visited and the running sum. Initially, the stack has only one object, which contains the root node and the running sum till the root node that is the value of the root node.
- loop until the stack is empty

  - pop an element from the stack. Get the node and the running sum till that node.
  - If the node has zero child nodes, append the running sum to the branch sums array and continue the loop.
  - Otherwise, for each child node, calculate the the new running sum by adding the value at that child node to the running sum, store the child node and the new running sum in an object and append the object to the stack. Because of how stack works, if I first append the left child node to the stack, then at next iteration, I will visit the right child node first. Since the branch sums should be ordered from leftmost branch sum to rightmost branch sum, I need to append the right child node first.

- returns the branch sums array.

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
  const stack = [{ node: root, runningSum: root.value }];

  while (stack.length > 0) {
    const { node, runningSum } = stack.pop();

    if (!node.left && !node.right) {
      sums.push(runningSum);
      continue;
    }

    let newRunningSum;
    if (node.right) {
      newRunningSum = runningSum + node.right.value;
      stack.push({ node: node.right, runningSum: newRunningSum });
    }
    if (node.left) {
      newRunningSum = runningSum + node.left.value;
      stack.push({ node: node.left, runningSum: newRunningSum });
    }
  }

  return sums;
}
```

### Approach with recursive DFS - O(n) time | O(n) space, where n is the number of nodes in the Binary Tree.

Instead of using a stack to keep track of the next node that needs to be visited and the running sum, use the call stack to track these info.

- initialize an empty array to store the branch sums.
- write a helper function that will be recursively invoked. The function takes in three parameters. The first parameter is the node needs to be visited; the second parameter is the running sum; and the last parameter is the branch sums array. Call the helper function in the main function passing in the root node of the tree as the node to be visited, 0 as the running sums and the empty branch sums array. In the helper function:
  - check if the node to be visited is null. If it is, return.
  - calculate the new running sum by adding the value of the node to the running sum.
  - if the node doesn't have any children, append the new running sum to the branch sums array.
  - recursively call the helper function passing in the left child of the node, the new running sum and the branch sums array.
  - recursively call the helper function passing in the right child of the node, the new running sum and the branch sums array.
- when I get out of the helper function, return the branch sums array as part of the main function.

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

function calculateBranchSums(node, runningSum, sums) {
  if (!node) return;

  const newRunningSum = runningSum + node.value;
  if (!node.left && !node.right) {
    sums.push(newRunningSum);
  }

  calculateBranchSums(node.left, newRunningSum, sums);
  calculateBranchSums(node.right, newRunningSum, sums);
}
```
