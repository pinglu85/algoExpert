### Understanding the problem

Given a Binary Tree, I am asked to write a function that returns the sum of all nodes' depths in the Binary Tree.

Suppose I am given the following Binary Tree:

```
tree =   1
      /     \
     2       3
   /   \   /   \
  4     5 6     7
```

The sum of all nodes' depths is 10, because the depth of the node with value 2 is 1, the depth of the node with value 3 is 1, the depth of the node with value 4 is 2...

### Recursive Approach - Average: O(n) time | O(h) space, Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

Create a variable that is going to keep track of the sum of the nodes' depth. Traverse the tree. At each node, calculate its depth, and add it to the current sum. I can traverse the tree either iteratively or recursively. I will go with the recursive approach, so that I don't need to use extra data structure to keep track of the next node that I need to visit and the current depth depending on whether I performs a iterative depth-first search or iterative breadth-first search on the tree.

To traverse the tree recursively, I would define a new function that would be the actual recursive function. The recursive function is going to be called on each node starting from the root node. It would calculate the depth for each node and add the depth to the depths' sum. Since the recursive function needs to update the depths' sum, and the data type number in JavaScript is not a reference type, meaning when I pass the variable depths' sum to another function, it will create a copy of the variable depths' sum und update that copy instead of the original one, I need to make the variable accessible to the recursive function without passing it around. So to do that, the recursive function will be defined within the main function. The recursive function is going to receive two parameters; the first parameter is the node to be visited and the second parameter is the depth. Initially, the node is the root node and the depth is 0. At each node, it would add the depth to the depths' sum and check if the node has any children. If the node does have children, increase the depth by 1, then call the recursive function passing in the child node and the increased depth. If the node to be visited is `null`, return; this is going to be the base case of the recursive function. When I get out of the recursive function, return the depths' sum in the main function.

### Recursive Solution

```js
function nodeDepths(root) {
  let depthsSum = 0;
  const calculateDepthsSum = (node, depth) => {
    if (!node) return;

    depthsSum += depth;
    calculateDepthsSum(node.left, depth + 1);
    calculateDepthsSum(node.right, depth + 1);
  };
  calculateDepthsSum(root, 0);

  return depthsSum;
}

// This is the class of the input binary tree.
class BinaryTree {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
```
