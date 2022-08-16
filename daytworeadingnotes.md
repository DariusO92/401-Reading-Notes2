# Day Two Reading Notes

# Java imports

- link was not working kept getting a 403 error.



# Loops

-  Overview
   In this article, we'll look at a core aspect of the Java language – executing a statement or a group of statements repeatedly – using loops.

- Intro to Loops
  In programming languages, looping is a feature which facilitates the execution of a set of instructions until the controlling Boolean-expression evaluates to false.

Java provides different types of loops to fit any programming need. Each loop has its own purpose and a suitable use case to serve.

Here are the types of loops that we can find in Java:

Simple for loop
Enhanced for-each loop
While loop
Do-While loop

- For Loop
  A for loop is a control structure that allows us to repeat certain operations by incrementing and evaluating a loop counter.

- While Loop
  The while loop is Java's most fundamental loop statement. It repeats a statement or a block of statements while its controlling Boolean-expression is true.

- Do-While Loop
  The do-while loop works just like the while loop except for the fact that the first condition evaluation happens after the first iteration of the loop.

For a detailed example, have a look at the dedicated post: Java Do-While Loop.


# Arrays

- Declaring Array Variables
  To use an array in a program, you must declare a variable to reference the array, and you must specify the type of array the variable can reference. 
  Here is the syntax for declaring an array variable
- Syntax
  dataType[] arrayRefVar; // preferred way.
  or
  dataType arrayRefVar[]; // works but not preferred way.
- Creating Arrays
  You can create an array by using the new operator with the following syntax −

- Syntax
  arrayRefVar = new dataType[arraySize];
  The above statement does two things −

  It creates an array using new dataType[arraySize].

  It assigns the reference of the newly created array to the variable arrayRefVar.

  Declaring an array variable, creating an array, and assigning the reference of the array to the variable can be combined in one statement, as shown below −

  dataType[] arrayRefVar = new dataType[arraySize];
  
- Processing Arrays
  When processing array elements, we often use either for loop or foreach loop because all of the elements in an array are
  of the same type and the size of the array is known.


## Things I want to know more about

- Java imports the link would not open for me.