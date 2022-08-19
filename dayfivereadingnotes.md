# Linked List

- Linked List
What is a Linked List
A Linked List is a sequence of Nodes that are connected/linked to each other. The most defining feature of a Linked List is that each Node references the next Node in the link.
There are two types of Linked List - Singly and Doubly. We will be implementing a Singly Linked List in this implementation.

- Traversal
When traversing a linked list, you are not able to use a foreach or for loop. We depend on the Next value in each node to guide us where the next reference is pointing. The Next property is exceptionally important because 
it will lead us where the next node is and allow us to extract the data appropriately.
The best way to approach a traversal is through the use of a while() loop. This allows us to continually check that the Next node in the list is not null. If we accidentally end up trying to traverse on a node that is null,
a NullReferenceException gets thrown and our program will crash/end.
When traversing through a linked list, the Current variable will tell us where exactly in the linked list we are and will allow us to move/traverse forward until we hit the end.
