# Node Depths

### Understanding the problem

Given a Binary Tree, we are asked to find the depth of each node in the tree and return the sum of all nodes' depths. The depth of the root node is `0`.

Example:

```
Binary tree:
             1
           /   \
          2     3
        /   \
       4     5

Output: 6
Explanation:
                    1 --- depth: 0
                  /   \
    depth: 1 --- 2     3 --- depth: 1
               /   \
 depth: 2 --- 4     5 --- depth: 2

sum = 0 + 1 + 1 + 2 + 2 = 6
```

#

### Approach 1: Recursive

Suppose the binary tree is like so:

```
          1
        /    \
      2       3
    /   \    /
   4     5  6
```

If we had the answer to the left and right subtrees of the root node `1`, then we could obtain the sum of all nodes's depths by `depth of root node + sum of depths of left subtree + sum of depths of right subtree`. To get the sum of nodes' depths of a subtree, we can apply the same logic. If a tree contains zero node, we can just return `0`, because if a tree has zero node, the sum of nodes' depths is going to be `0`. This leads to a natural recursive algorithm.

### Implementation

JavaScript:

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

Go:

```go
package main

type BinaryTree struct {
	Value       int
	Left, Right *BinaryTree
}

func NodeDepths(root *BinaryTree) int {
	return calculateNodeDepths(root, 0)
}

func calculateNodeDepths(node *BinaryTree, depth int) int {
	if node == nil {
		return 0
	}

	return depth + calculateNodeDepths(node.Left, depth+1) + calculateNodeDepths(node.Right, depth+1)
}
```

### Complexity Analysis

- Time Complexity: O(N), where N is the number of nodes in the Binary Tree.

- Space Complexity: O(H) on average, where H is the height of the Binary Tree. In the worst case, when we are dealing with a Binary Tree that is very skewed, the algorithm takes O(N) space, where N is the number of nodes in the Binary Tree.

#

### Approach 2: Iterative

Alternatively, we could solve the problem iteratively. We traverse the binary tree using iterative depth-first search and keep track of the running sum of the nodes' depths. At each step, we add the depth of current node to the running sum. Therefore, we also need to store the depth of each node. We could either store the depth of a node alongside with the node in the stack or use another stack to store the depths.

### Implementation

JavaScript:

```js
function nodeDepths(root) {
  let sumOfDepths = 0;
  const stack = [root];
  const depths = [0];

  while (stack.length > 0) {
    const node = stack.pop();
    const depth = depths.pop();

    sumOfDepths += depth;

    if (node.left) {
      stack.push(node.left);
      depths.push(depth + 1);
    }

    if (node.right) {
      stack.push(node.right);
      depths.push(depth + 1);
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

Go:

```go
package main

type BinaryTree struct {
	Value       int
	Left, Right *BinaryTree
}

type level struct {
	node  *BinaryTree
	depth int
}

func NodeDepths(root *BinaryTree) int {
	sumOfDepths := 0
	stack := []level{level{root, 0}}

	for len(stack) > 0 {
		node, depth := stack[len(stack)-1].node, stack[len(stack)-1].depth
		stack = stack[:len(stack)-1]

		sumOfDepths += depth

		if node.Left != nil {
			stack = append(stack, level{node.Left, depth + 1})
		}

		if node.Right != nil {
			stack = append(stack, level{node.Right, depth + 1})
		}
	}

	return sumOfDepths
}
```

### Complexity Analysis

- Time Complexity: O(N), where N is the number of nodes in the Binary Tree.

- Space Complexity: O(H) on average, where H is the height of the Binary Tree. In the worst case, when we are dealing with a Binary Tree that is very skewed, the algorithm takes O(N) space, where N is the number of nodes in the Binary Tree.
