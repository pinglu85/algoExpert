# Remove Duplicates From Linked List

### Understanding the problem

I am given the head of a singly linked list. The nodes of the linked list are sorted by their values, which are integers. I am asked to write a function that is going to remove all the nodes with duplicate values and return the modified linked list. The linked list should be modified in place and the nodes should remain in their original order.

### Approach - O(n) time | O(1) space, where n is the number of nodes in the Linked List

Since the linked list is sorted, I would keep track of the node I am currently at, and compare its value to the value of the next node, if they are equal to each other, delete the next node; otherwise update the node I am currently at to the next node. Lastly, return the head of the modified linked list.

### Solution

```js
// This is an input class. Do not edit.
class LinkedList {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

function removeDuplicatesFromLinkedList(linkedList) {
  let currentNode = linkedList;
  while (currentNode !== null) {
    let nextNode = currentNode.next;
    if (nextNode?.value === currentNode.value) {
      currentNode.next = nextNode.next;
    } else {
      currentNode = nextNode;
    }
  }

  return linkedList;
}
```