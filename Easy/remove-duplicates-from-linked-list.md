# Remove Duplicates From Linked List

### Understanding the problem

We are given the head of a singly linked list. The nodes of the linked list are going to be sorted in ascending order with respect to their values and the values are going to be integers. We are asked to write a function that is going to remove all the nodes with duplicate values and return the modified linked list. The linked list should be modified in place and the nodes should remain in their original order.

#

### Approach 1

Since the linked list is sorted, it means all of the duplicate values are grouped together. We traverse the linked list. At each step we compare current node's value to the value of the next node. If they are equal to each other, delete the next node: point current node's next pointer to the one after the next node. Otherwise we move on to the next node. Lastly, return the head of the modified linked list.

### Implementation

JavaScript:

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

Go:

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

### Complexity Analysis

- Time Complexity: O(N), where N is the number of nodes in the Linked List.

- Space Complexity: O(1).

#

### Approach 2

Instead of deleting the duplicate nodes one at a time, we can also remove all duplicate nodes in one go. When we get to a node whose next neighbor has the same value as the current node, we are going to keep looking at the nodes after that neighbor until we find a node that has a different value. Then we change the next pointer of the current node to point to the distinct node, which means we skip all of the duplicate nodes.

### Implementation

JavaScript:

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

Go:

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

### Complexity Analysis

- Time Complexity: O(N), where N is the number of nodes in the Linked List.

- Space Complexity: O(1).
