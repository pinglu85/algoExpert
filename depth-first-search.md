### Understanding the problem

I am given a `Node` class which has a `name` and an array of `children` nodes, which might be empty. Nodes are going to form an acyclic tree-like graph. I am asked to write a `depthFirstSearch` method on the `Node` class. The method accepts an empty array as argument, and it is going to traverse the graph with depth-first search, store all the names of the nodes in the input array in DFS traversed order and return the input array. The nodes should be visited from left to right.

### Recursive Approach - O(v+e) time | O(v) space, where v is the number of nodes(vertices) in the input graph and e is the number of edges in the input graph.

To traverse the graph recursively, I would call a recursive function on each node starting from the calling node. The recursive function would append the node's name to the input array and iterate through its children; for each child node, the recursive function would be called again.

Since the `depthFirstSearch` is a method of the class `Nodes`, which means each `Node` instance has the method `depthFirstSearch` and the method can access the calling node's properties with `this`, I don't need to pass the node that is going to be visited to the function. In addition, because the graph is acyclic and is tree-like, meaning it is directed and all the nodes except the root node has only one parent, I don't need to use a data structure to keep track of the nodes that have been visited. So the `depthFirstSearch` method would be the actual recursive function. At each recursive call, I will pass down reference of the input array, and return it at the end.

### Recursive Solution

```js
// Do not edit the class below except
// for the depthFirstSearch method.
// Feel free to add new properties
// and methods to the class.
class Node {
  constructor(name) {
    this.name = name;
    this.children = [];
  }

  addChild(name) {
    this.children.push(new Node(name));
    return this;
  }

  depthFirstSearch(array) {
    array.push(this.name);
    for (const child of this.children) {
      child.depthFirstSearch(array);
    }

    return array;
  }
}
```
