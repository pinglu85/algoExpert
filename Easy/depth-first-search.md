# Depth-first Search

### Understanding the problem

We are given an acyclic tree-like graph. Each node in this graph is going to be an instance of a `Node` class, and is going to have a name and optionally some children nodes. We are asked to write a `depthFirstSearch` method on the `Node` class, which is going to accept an empty array as argument and perform a depth-first search on the graph. As we traverse the graph, we need to store the names of all the nodes in the input array and return that array. The nodes should be visited from left to right.

#

### Approach

Since the `depthFirstSearch()` is a method of the `Node` class, each `Node` instance is going to have the method and the method can access the calling node's name and its children nodes with `this`. In addition, since the graph is acyclic and tree-like, it means the graph is directed and all the nodes except the root node has only one parent, we don't need to use a data structure to keep track of the nodes that have been visited. The `depthFirstSearch()` method is going to grab the calling node's name and append it to the input array, then it would loop through the calling node's child nodes. At each child node, it is going to call the child node's `depthFirstSearch()` method passing in the input array. Lastly, it would return the input array.

### Implementation

JavaScript:

```js
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

Go:

```go
package main

// Do not edit the class below except
// for the depthFirstSearch method.
// Feel free to add new properties
// and methods to the class.
type Node struct {
	Name     string
	Children []*Node
}

func (n *Node) DepthFirstSearch(array []string) []string {
	array = append(array, n.Name)

	for _, child := range n.Children {
		array = child.DepthFirstSearch(array)
	}

	return array
}
```

### Complexity Analysis

Let |V| be the number of nodes(vertices) in the input graph and |E| the number of edges.

- Time Complexity: O(|V| + |E|) when the graph is represented as an adjacency list. The total time of DFS is number of subproblems \* time per subproblem, in other words, we can get the total time of DFS by summing up all of the subproblems' time. Since we visit each vertex _v_ once, the number of subproblems is equal to the number of vertices in the graph. Therefore we can write:

total time = ∑<sub>v ∈ V</sub>_(time for subproblem)_

At each vertex _v_, we append _v_'s name to an array, loop over its adjacent vertices and invoke the recursive function on each adjacent vertex. Let _m<sub>v</sub>_ denotes the number of edges coming out of the vertex _v_. The time for subproblem is 1 + _m<sub>v</sub>_. Therefore,

total time = ∑<sub>v ∈ V</sub>_(1 + m<sub>v</sub>)_ = |V| + ∑<sub>v ∈ V</sub>_m<sub>v</sub>_

In a directed graph, ∑<sub>v ∈ V</sub>_m<sub>v</sub>_ is equal to |E|, whereas in an undirected graph, we have ∑<sub>v ∈ V</sub>_m<sub>v</sub>_ = 2|E|. Thus the time complexity of DFS is O(|V| + |E|).

**Reference**: [Verifying DFS complexity for directed and un-directed graph](https://stackoverflow.com/questions/24024331/verifying-dfs-complexity-for-directed-and-un-directed-graph)

- Space Complexity: O(|V|).
