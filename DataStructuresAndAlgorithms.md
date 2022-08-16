 # Data Structures and Algorithms
 
 ## Reading Notes

- Data Structures are a specialized means of organizing and storing data in computers in such a way that we can perform 
  operations on the stored data more efficiently. Data structures have a wide and diverse scope of usage across the fields 
  of Computer Science and Software Engineering.

- Arrays An array is a structure of fixed-size, which can hold items of the same data type. It can be an array of integers,
  an array of floating-point numbers, an array of strings or even an array of arrays (such as 2-dimensional arrays). Arrays
  are indexed, meaning that random access is possible. 
- Array operations
  Traverse: Go through the elements and print them.
  Search: Search for an element in the array. You can search the element by its value or its index
  Update: Update the value of an existing element at a given index
  Inserting elements to an array and deleting elements from an array cannot be done straight away as arrays are fixed in size. 
  If you want to insert an element to an array, first you will have to create a new array with increased size (current size + 1), 
  copy the existing elements and add the new element. The same goes for the deletion with a new array of reduced size.

- Linked Lists
  A linked list is a sequential structure that consists of a sequence of items in linear order which are linked to each other. 
  Hence, you have to access data sequentially and random access is not possible. Linked lists provide a simple and flexible 
  representation of dynamic sets.
  Elements in a linked list are known as nodes.
  Each node contains a key and a pointer to its successor node, known as next.
  The attribute named head points to the first element of the linked list.
  The last element of the linked list is known as the tail.
  Linked list operations
  Search: Find the first element with the key k in the given linked list by a simple linear search and returns a pointer to this element
  Insert: Insert a key to the linked list. An insertion can be done in 3 different ways; insert at the beginning of the list, 
  insert at the end of the list and insert in the middle of the list.
  Delete: Removes an element x from a given linked list. You cannot delete a node by a single step. A deletion can be done 
  in 3 different ways; delete from the beginning of the list, delete from the end of the list and delete from the middle of the list.

  - Stacks
    A stack is a LIFO (Last In First Out — the element placed at last can be accessed at first) structure which can be commonly 
    found in many programming languages. This structure is named as “stack” because it resembles a real-world stack — a stack of plates.
    Stack operations
    Given below are the 2 basic operations that can be performed on a stack. Please refer to Figure 3 to get a better understanding of the stack operations.
    Push: Insert an element on to the top of the stack.
    Pop: Delete the topmost element and return it.


  ## Discussion Questions
 - What is 1 of the more important things you should consider when deciding which data structure is best suited to solve a particular problem?
 - How can we ensure that we’ll avoid an infinite recursive call stack? Make a base case.