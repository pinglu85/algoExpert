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

### Recursive Approach 1 - Average: O(n) time | O(h) space, Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

Create a variable that is going to keep track of the sum of the nodes' depths. Traverse the tree. At each node, calculate its depth, and add it to the current sum of depths. I can traverse the tree either iteratively or recursively. I will traverse the Binary Tree recursively, because with the recursive approach I don't need to use extra data structure to keep track of the next node that I need to visit and its depth, depending on whether I performs a iterative depth-first search or iterative breadth-first search on the tree.

To traverse the tree recursively, I would define a new function that would be the actual recursive function. The recursive function is going to be called on each node starting from the root node. It would calculate the depth for each node and add the depth to the sum of depths. Since the recursive function needs to update the sum of depths, and the data type Number in JavaScript is not a reference type, meaning when I pass the sum of depths to another function, it will create a copy of the variable und update that copy instead of the original one, I need to make the sum of depths accessible to the recursive function without passing it around. So to do that, the recursive function will be defined within the main function. The recursive function is going to receive two parameters; the first parameter is the node to be visited and the second parameter is the depth. Initially, the node is the root node and the depth is 0. At each node, it would add the depth to the sum of depths and check if the node has any children. If the node does have children, increase the depth by 1, then call the recursive function passing in the child node and the increased depth. If the node to be visited is `null`, return; this is going to be the base case of the recursive function. When I get out of the recursive function, return the sum of depths in the main function.

### Recursive Solution 1

```js
function nodeDepths(root) {
  let sumOfDepths = 0;

  const calculateSumOfDepths = (node, depth) => {
    if (node === null) return;

    sumOfDepths += depth;
    calculateSumOfDepths(node.left, depth + 1);
    calculateSumOfDepths(node.right, depth + 1);
  };
  calculateSumOfDepths(root, 0);

  return sumOfDepths;
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

### Recursive Approach 2 - Average: O(n) time | O(h) space, Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

Instead of using a variable to keep track of the sum of depths that I have this far, I can also use the call stack to keep track of it.

When traversing a Binary Tree starting from the root node, I could say the depths' sum of all nodes in the Binary Tree is equal to the depth of the root node plus the sum of all nodes' depth in the subtree.

```
         1    --> depth of the root node  ---------------
      /     \                                             + => sum of all nodes' depths.
     2       3   ----                                     |
   /   \   /   \     |-> sum of the nodes' depths in the -
  4     5 6     7  --    remaining tree
```

If the Binary Tree has zero node or has only one node, then the depths' sum is zero. Suppose the Binary Tree has three nodes, the root node and its two child nodes, then I can get the depths' sum
by adding the depth of the root node to the sum of all nodes' depths that are below the root node, that is the sum of nodes' depths in the left subtree of the root node plus the sum of nodes' depths in the right subtree of the root node:

```
sum of depths = depth of root node + sum of depths of left subtree + sum of depths of right subtree
```

Since the child nodes of the root node have zero children, the sum of nodes' depths in the left subtree is the left node's depth plus 0, and the sum of nodes' depths in the right subtree is the right node's depth plus 0.

The equation above is the recursive case of the recursive function, and the base case is a Binary Tree with zero node. Every time I call the recursive function, I will pass the node and its depth.

### Recursive Solution 2

```js
function nodeDepths(root) {
  return calculateNodeDepths(root, 0);
}

function calculateNodeDepths(node, depth) {
  if (node === null) return 0;
  return (
    depth +
    calculateNodeDepths(node.left, depth + 1) +
    calculateNodeDepths(node.right, depth + 1)
  );
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

### Iterative Approach - Average: O(n) time | O(h) space, Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

- Create a variable that is going to keep track of the sum of the depths. Initially, set it to 0.
- Initialize an empty array that will be used as stack to keep track of the next node needs to be visited and its depth. Each node that needs to be visited and its depth is going to be stored in an object. So the stack is going to be an array of objects.
- Create an object that contains the root node and its depth which is 0; append it to the stack.
- Loop until the stack is empty.
  - Pop a node from the stack.
  - Add its depth to the sum of the depths.
  - If it has child nodes, for each node, store the child node and its depth, which is the depth of the parent node plus 1, to an object; add the object to the stack.
- return the sum of depths.

### Iterative Solution

```js
function nodeDepths(root) {
  let sumOfDepths = 0;
  const stack = [{ node: root, depth: 0 }];

  while (stack.length > 0) {
    const { node, depth } = stack.pop();

    sumOfDepths += depth;

    if (node.left !== null) {
      stack.push({ node: node.left, depth: depth + 1 });
    }

    if (node.right !== null) {
      stack.push({ node: node.right, depth: depth + 1 });
    }
  }

  return sumOfDepths;
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
