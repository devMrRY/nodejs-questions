# Node.js Interview Questions

## Table of Contents

* *[NodeJS APIs](nodejs-api.md)*
* *[NodeJS Coding Practice](nodejs-programming.md)*
* *[NodeJS Commands](nodejs-commands.md)*

<br/>

| Sl.No|  Questions       |
|------|------------------|
| 01. |[How V8 compiles the js code ?](#q-how-v8-compiles-the-js-code-)|
| 02. |[What are Hidden Classes in Nodejs ?](#q-what-are-hidden-classes-in-nodejs-)|
| 03. |[What is INLINE CACHE ?](#q-what-is-inline-cache-)|
| 04. |[What is FEEDBACK VECTOR ?](#q-what-is-feedback-vector-)|
| 05. |[Explain Garbage Collector ?](#q-explain-garbage-collector-)|
| 06. |[What are V8 templates ?](#q-what-are-v8-templates-)|
| 07. |[Explain Babel ?](#q-explain-babel-)|
| 08. |[What are React Refs ?](#q-what-are-react-refs-)|
| 09. |[Explain how browsers renders html ?](#q-explain-how-browser-renders-html-)|

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

## Q. ***What are V8 templates ?***

templates are the blueprint of js functions/objects in which you can wrap c++ functions/objects/structure and can change or call c++ things from js.

Functions templates: js functions with which one can associate c++ callbacks which gets invoked when js function gets called.

Object template: js objects with which two c++ callbacks accessor(gets invoked only on special property) and interceptor(gets invoked on each property of object) callbacks.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Explain Babel ?***

Babel is a transpiler which converts ES6 code into backward compatible ES5.

It is built on plugin system which converts it into AST (abstract syntax tree).

It's plugins provide him the instructions on how to convert the code.
as it's plugins are small so one plugin holds only one feature.

Presets are the default group of plugins used for transpilation.
 [@babel/preset-env, @babel/preset-flow, @babel/preset-react ( for React and it supports JSX Syntax ), @babel/preset-typescript]

**@babel/preset-env** :- it is responsible for knowing the babel to which level transformation is required.
    For example, you can create a .babelrc file in the root of your project. and add support for last 2 versions of the browser.
    This will ensure that when the browser is updated it will stop transpiling of the old browser version and will transpile for the new one.

**@babel/polyfill** : - it helps in providing the support of latest features of a language to work in browsers if browser provide no/less support for it.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What are React Refs ?***

SYNTAX => input ref="inputref" // referenced by this.refs.inputref

1. string refs are not composable (like if a library put a ref on passed children then you can't put your own ref) but callback refs are.
2. string refs gives references to outermost parent instead of acutal dom element or component
3. React have to keep track of current component which makes it little slow. also causes weird bugs when duplicated in bundle.js

**Cons of inline callback ref** :-
    it gets updated twice first render once with null and then with actual HTML element.

1. ref will get updated before componentDidMount and componentDidUpdate.
2. useRef will always return a same reference of object with { current: ''} property

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Explain how browsers renders html ?***

**DOM** is a high level (web api) provided by browser which reads a text file (with content type text/html and charset utf-8 ) Aka html/XML page.
DOM (forgiving nature) converts each html element (including comments etc) into it's corresponding node with the help of Node Class -> HTMLDivElement Class (constructor function).
eventually everything on webpage is just javascript (there's no such thing as html or css on browsers).

**CSSOM (CSS Object Model)** tree like str for our js nodes by specifying particular css selectors (means we are putting some properties like color, display etc in our js node's style attribute ).
every browser has it's own css file (user agent stylesheet) which will be inherited by each js node which provides default value to style attribute and based on some specificity rules those default
values can be overriden by external css comes from server.
CSSOM doesn't create node for non displaying js nodes like title, meta, head etc.
CSS is Cascading style sheet because each js node inherits default value from (user agent stylesheet) which is called cascading of styles.

Render Tree (DOM + CSSOM) it doesn't contain js nodes which doesn't hold any space inside pixel matrix (like elements with display: none property).

**Layout/reflow/browser** reflow process of calculating geometric information and position of each js node in pixels in viewport. This process also gets triggered on scroll/browser resizing etc.

Paint operation.

**Rasterization** process of painting each layer after dividing them into small sub layers and paints all of them seperately into software bitmaps and then uploads them to GPU textures.
each layer painting operation takes place into different threads to speedup the process.

**Composition** process of grouping all the layers created in previous step.

**Dom Parser** is responsible for parsing js nodes from html file. Dom parser has DomParser api which has parseFromString prototype method which is responsible from parsing html from 
string. 
Dom parser starts it's job asap it gets it's first few bytes from server.

all external files such as scripts, css, images downloads in seperate thread other than main thread in which html page is downloading.
script files download occurs in different thread but it pause the execution of main thread untill downloading for that scipt gets completed.
All .js files they are parser blocking

**Defer** :- downloads in parallel thread and execute at the end of html parsing.
**Async** :- downloads in parallel therad but halts parsing html after downloading script and starts executing script.

**Speculative parsing** :- like Promise.all(script1, script2)

css are not parser blocking but they are render blocking and sometimes script blocking.
css downloading is not incremental just to avoid FOUC (flash of unstyled content)

**Note :** It is not safe execute script file before css completely loads as if script runs prior to css loads completely it will execute with false values of style tag of a particular js node as it may be changed by later css.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
