### Understanding the problem

I am given an acyclic tree-like graph. Each node in this graph is going to be an instance of a `Node` class. Each node is going to have a name and optionally have some children nodes. I am asked to write a `depthFirstSearch` method on the `Node` class, that is going to performs a depth-first search on the graph. The method is going to accept an empty array as argument. As it traverses the graph, it is going to get the names of all nodes, put them in the input array and return that array. The nodes should be visited from left to right.

### Recursive Approach - O(v+e) time | O(v) space, where v is the number of nodes(vertices) in the input graph and e is the number of edges in the input graph.

To traverse the graph recursively, I would call a recursive function on each node starting from the calling node. The recursive function would append the node's name to the input array and iterate through its children; for each child node, the recursive function would be called again.

Since the `depthFirstSearch` is a method of the class `Nodes`, which means each `Node` instance has the method `depthFirstSearch` and the method can access the calling node's properties with `this`, I don't need to pass the node that is going to be visited to the function. In addition, because the graph is acyclic and is tree-like, meaning it is directed and all the nodes except the root node has only one parent, I don't need to use a data structure to keep track of the nodes that have been visited. So the `depthFirstSearch` method would be the actual recursive function. At each recursive call, I will pass down reference of the input array, and return it at the end.

**Note**

The time complexity of DFS is O(|V| + |E|) when the graph is represented as an adjacency list. The total time of DFS is number of subproblems \* time per subproblem, in other words, we can get the total time of DFS by summing up all of the subproblems' time. Since we visit each vertex _v_ once, the number of subproblems is equal to the number of vertices in the graph. Therefore we can write:

total time = ∑<sub>v ∈ V</sub>_(time for subproblem)_

At each vertex _v_, we mark _v_ as visited(in our problem, we append _v_'s name to an array), loop over its adjacent vertices and invoke the recursive function on each adjacent vertex. Let _m<sub>v</sub>_ denotes the number of edges coming out of the vertex _v_. The time for subproblem is 1 + _m<sub>v</sub>_. Therefore,

total time = ∑<sub>v ∈ V</sub>_(1 + m<sub>v</sub>)_ = |V| + ∑<sub>v ∈ V</sub>_m<sub>v</sub>_

In a directed graph, ∑<sub>v ∈ V</sub>_m<sub>v</sub>_ is equal to |E|, whereas in an undirected graph, we have ∑<sub>v ∈ V</sub>_m<sub>v</sub>_ = 2|E|. Thus the time complexity of DFS is O(|V| + |E|).

_Reference_: [Verifying DFS complexity for directed and un-directed graph](https://stackoverflow.com/questions/24024331/verifying-dfs-complexity-for-directed-and-un-directed-graph)

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
