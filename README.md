# Node.js Interview Questions

## Table of Contents

* *[NodeJS APIs](nodejs-api.md)*
* *[NodeJS Coding Practice](nodejs-programming.md)*
* *[NodeJS Commands](nodejs-commands.md)*

<br/>

| Sl.No|  Questions       |
|------|------------------|
| 01. |[How V8 compiles the js code ?](#q-how-v8-compiles-the-js-code)|
| 02. |[What are Hidden Classes in Nodejs ?](#q-what-are-hidden-classes-in-nodejs)|
| 03. |[What is INLINE CACHE ?](#q-what-is-inline-cache)|
| 04. |[What is FEEDBACK VECTOR ?](#q-what-is-feedback-vector)|
| 05. |[Explain Garbage Collector ?](#q-explain-garbage-collector)|

<br/>

## Q. ***How V8 compiles the js code ?***

Parsing the code
Compiling the code
Executing the code

1. Parsing splits all the code into small tokens and these token are then transfered to syntax parser which will create abstract syntax tree (AST).

2. V8 uses JIT compiler takes in which interpretetion and compiling goes hand in hand like
    first inerpretor checks for the patterns that are frequently used like variables, functions and compiles the code to optimize the process if type of variable or function arguments changes then 
    decompilation takes place the and control shifts to interpretetion again.

    V8 uses Ignition interpretor which takes AST as input and gives byte code as output while interpretion is taking place Turbofan compiler communicates with interpretor and converts byte
    code into machine code.

    Ignition interpretor interpretes the code using object structure where keys are the byte code and values are the functions (which are ordered in form of list) which handles the byte code.

3. In Execution phase: Memory heap and Call stack is used
    **Memory heap**:-  where all the variables and functions gets memory allocated
    **Call stack**:-  where all the functions calls get pushed and poped

        let Person = {name: "rahul"}
        Person.age = 20;

**Note**: In this above code for assigning new property age to Person object a linear search is required on list. to overcome that
      node.js uses Inline Cache and Hidden classes.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are Hidden Classes in Nodejs ?***

Hidden classes in nodejs => V8 creates a hidden class for every property addition in an object to fasten the process of retreival of property from an object as compared to non dynamic programming language.

if property addition order is same on two diff object with same keys then V8 make same hidden classes also use inline cache to retreive properties faster.
if order is diff then there will be different hidden class so inline cache can't be used so retreival of property will be slower.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is INLINE CACHE ?***

Inline Cache: Inline Cache is a data structure used to keep track of the addresses of the properties on objects, thereby reducing the lookup time. It tracks all the LOAD, STORE, and CALL events within a function, by maintaining a Feedback Vector.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is FEEDBACK VECTOR ?***

Feedback Vector is simply an array used to track all the Inline Caches of a particular function.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Explain Garbage Collector ?***

node.js uses 3 techniques for garbage collecting

**Parallel** : main js thread use help of few helper threads parallely to collect garbage in this way main thread stops for small time only.

**Incremental** : main js thread move back and forth from main execution and garbage collection untill all the garbage gets collected.

**Concurent** : In this main thread is not disturbed and all the garbage collection handled by helper threads.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>