# Node Depths

### Understanding the problem

Given a Binary Tree, I am asked to write a function that is going to find the depth of each node in the tree, sum up all of these depths and return the sum.

Suppose I am given the following Binary Tree:

```
tree =   1
      /     \
     2       3
   /   \   /   \
  4     5 6     7
```

The sum of all nodes' depths in the above Binary Tree is 10, because the depth of the node with value 1 is 0, the depth of the node with value 2 is 1, the depth of the node with value 3 is 1, the depth of the node with value 4 is 2...

#

### Recursive Approach 1

Create a variable that is going to keep track of the running sum of the nodes' depths. To find the depths of every node in the tree, I can traverse the tree, and at each node, compute its depth by adding 1 to the depth of its parent node, because the node are one level deeper than its parent node. After getting the depth of the node, add it to the running sum of depths.

I can traverse the tree either iteratively or recursively. I will traverse the Binary Tree recursively, because with the recursive approach I don't need to use extra data structure to keep track of the next node that needs to be visit and its depth, depending on whether I performs a iterative depth-first search or iterative breadth-first search on the tree.

I would define a new function that would be the actual recursive function. The recursive function is going to be called on each node starting from the root node. It would calculate the depth of each node and add the depth to the running sum of depths. Since the recursive function needs to update the running sum of depths, and the data type `Number` in JavaScript is not a reference type, meaning when I pass the running sum of depths to another function, it will create a copy of the variable und update that copy instead of the original one, I need to make the running sum of depths accessible to the recursive function without passing it around. So to do that, the recursive function will be defined within the main function.

The recursive function is going to receive two parameters: the first parameter is the node to be visited and the second parameter is its depth. Initially, the node is the root node and the depth is 0, because the distance between the root node and itself is 0. At each node, add the depth of the current node to the running sum of depths and check if the node has any children. If the node does have children, add 1 to the depth and that would be the depth of the children nodes, then call the recursive function passing in the child node and the depth of the child node. If the node to be visited is `null`, return; this is going to be the base case of the recursive function. When I get out of the recursive function, return the sum of depths in the main function.

### Time & Space Complexity

Average: O(n) time | O(h) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree.

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

#

### Recursive Approach 2

Instead of using a variable to keep track of the sum of depths, I can also use the call stack to keep track of it.

Given a Binary Tree, I can observe the sum of all nodes' depths in the Binary Tree is equal to the depth of the root node plus the sum of depths of all nodes in the remaining tree, that is the sum of depths of all nodes in the left subtree and the sum of depths of all nodes in the right subtree.

```
        1    --> depth of the root node  ---------------
     /     \                                             + => sum of all nodes' depths.
    2       3   ----                                     |
  /   \   /   \     |-> sum of the nodes' depths in the -
4      5 6     7  --    remaining tree

sum of depths = depth of root node + sum of depths of left subtree + sum of depths of right subtree
```

For each subtree, the sum of depths of all nodes in that subtree is going to be the depth of the root node of the subtree plus the sum of depths of all nodes in the remaining tree.

```
sum of depths = depth of root node
  + (depth of root node of left subtree + sum of the nodes' depths in the remaining tree)
  + (depth of root node of right subtree + sum of the nodes' depths in the remaining tree)
```

The equation can be expanded further, until the remaining tree contains zero node, because if a tree has zero node, the sum of nodes' depths is going to be 0. With that result, I can compute the sum of depths of the subtree whose root node is the parent node of the empty tree, the leaf node. I can then use the result to compute the sum of depths of the subtree whose root node is the parent node of the leaf node. Compute the sum further up, until the root node of the entire tree is reached.

### Time & Space Complexity

Average: O(n) time | O(h) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree.

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

#

### Iterative Approach

- Create a variable that is going to keep track of the running sum of the depths. Initially, set it to 0.
- Initialize an empty array that will be used as a stack to keep track of the next node needs to be visited and its depth. The node and its depths will be stored in an object, so the stack is going to be an array of objects.
- Create an object that contains the root node and its depth which is 0; append it to the stack.
- Loop until the stack is empty.
  - Pop a node off the stack.
  - Add its depth to the running sum of the depths.
  - If it has children nodes, for each node, store the child node and its depth, which is the depth of the parent node plus 1, to an object; add the object to the stack.
- return the sum of depths.

### Time & Space Complexity

Average: O(n) time | O(h) space, where n is the number of nodes in the Binary Tree and h is the height of the Binary Tree.

Worst(imbalanced Binary Tree): O(n) time | O(n) space, where n is the number of nodes in the Binary Tree.

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
