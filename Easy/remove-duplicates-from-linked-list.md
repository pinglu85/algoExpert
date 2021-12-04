# Remove Duplicates From Linked List

### Understanding the problem

I am given the head of a singly linked list. The nodes of the linked list are going to be sorted in ascending order with respect to their values and the values are going to be integers. I am asked to write a function that is going to remove all the nodes with duplicate values and return the modified linked list. The linked list should be modified in place and the nodes should remain in their original order.

#

### Approach 1

Since the linked list is sorted, it means all of the duplicate values are grouped together. So I would keep track of the node I am currently at, and compare its value to the value of the next node. If they are equal to each other, delete the next node; otherwise update the current node to the next node. Lastly, return the head of the modified linked list.

### Time & Space Complexity

O(n) time | O(1) space, where n is the number of nodes in the Linked List.

### Solution 1

```js
// This is an input class. Do not edit.
class LinkedList {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

function removeDuplicatesFromLinkedList(linkedList) {
  let currNode = linkedList;
  while (currNode !== null && currNode.next !== null) {
    const nextNode = currNode.next;
    if (nextNode.value == currNode.value) {
      currNode.next = nextNode.next;
    } else {
      currNode = nextNode;
    }
  }

  return linkedList;
}
```

### Solution 1 in Go

```go
package main

// This is an input struct. Do not edit.
type LinkedList struct {
	Value int
	Next  *LinkedList
}

func RemoveDuplicatesFromLinkedList(linkedList *LinkedList) *LinkedList {
	currNode := linkedList

	for currNode != nil && currNode.Next != nil {
		nextNode := currNode.Next
		if currNode.Value == nextNode.Value {
			currNode.Next = nextNode.Next
		} else {
			currNode = nextNode
		}
	}

	return linkedList
}
```

#

### Approach 2

Instead of deleting the duplicate nodes one at time, I can also remove all duplicate nodes in one go. I am going to loop over all of the nodes in the linked list. For each node I am going to check its value against its next neighbor's value, if they are the same, I am going to look at the next node of that neighbor and check if it has the same value as the current node; repeat the process until I find a node that has a different value than the current node; then I am going to change the next pointer of the current node to point to that node, which means all of the duplicate nodes are removed, and then I update the current node to that node.

### Time & Space Complexity

O(n) time | O(1) space, where n is the number of nodes in the Linked List.

### Solution 2

```js
// This is an input class. Do not edit.
class LinkedList {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

function removeDuplicatesFromLinkedList(linkedList) {
  let currNode = linkedList;
  while (currNode !== null) {
    let nextDistinctNode = currNode.next;
    while (
      nextDistinctNode !== null &&
      nextDistinctNode.value === currNode.value
    ) {
      nextDistinctNode = nextDistinctNode.next;
    }

    currNode.next = nextDistinctNode;
    currNode = nextDistinctNode;
  }

  return linkedList;
}
```

### Solution 2 in Go

```go
package main

// This is an input struct. Do not edit.
type LinkedList struct {
	Value int
	Next  *LinkedList
}

func RemoveDuplicatesFromLinkedList(linkedList *LinkedList) *LinkedList {
	currNode := linkedList

	for currNode != nil {
		nextDistinctNode := currNode.Next
		for nextDistinctNode != nil && currNode.Value == nextDistinctNode.Value {
			nextDistinctNode = nextDistinctNode.Next
		}

		currNode.Next = nextDistinctNode
		currNode = nextDistinctNode
	}

	return linkedList
}
```
