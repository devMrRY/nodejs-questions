# Node.js Interview Questions

<br/>

| Sl.No|  Questions       |
|------|------------------|
| 01. |[How V8 compiles the js code ?](#q-how-v8-compiles-the-js-code-)|
| 02. |[What are Hidden Classes in Nodejs ?](#q-what-are-hidden-classes-in-nodejs-)|
| 03. |[What is Inline Cache ?](#q-what-is-inline-cache-)|
| 04. |[What is Feedback Vector ?](#q-what-is-feedback-vector-)|
| 05. |[Explain Garbage Collector ?](#q-explain-garbage-collector-)|
| 06. |[What are V8 templates ?](#q-what-are-v8-templates-)|
| 07. |[Explain Babel ?](#q-explain-babel-)|
| 08. |[What are React Refs ?](#q-what-are-react-refs-)|
| 09. |[What is Sharding and what are the pros and cons ?](#q-what-is-sharding-and-what-are-the-pros-and-cons-)|
| 10. |[What is DB Clustering ?](#q-what-is-db-clustering-)|
| 11. |[What is Shadowing ?](#q-what-is-shadowing-)|
| 12. |[Explain how browsers renders html ?](#q-explain-how-browsers-renders-html-)|
| 13. |[Difference between rem and em ?](#q-difference-between-rem-and-em-)|
| 14. |[What is Image map ?](#q-what-is-image-map-)|
| 15. |[Explain inline Boxes ?](#q-explain-inline-boxes-)|
| 16. |[Explain Clustering in nodejs ?](#q-explain-clustering-in-nodejs-)|

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

## Q. ***What is Inline Cache ?***

Inline Cache: Inline Cache is a data structure used to keep track of the addresses of the properties on objects, thereby reducing the lookup time. It tracks all the LOAD, STORE, and CALL events within a function, by maintaining a Feedback Vector.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is Feedback Vector ?***

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



## Q. ***What is Sharding and what are the pros and cons ?***

It is a technique of horizontal scaling to reduce the load on main database.
It constitues 3 components:-

1. Config Server (one)
2. Query Router (many)
3. Shards (many)

***Config Server*** :- listens request and had metadata for query as well.

***Query Router*** :- responsible for making query to shards gets data from shards and pass it to Config Server.

***Shards*** :- Stores actual data based on shard key.

**Note** :- Sharding at collection level needs sharding enabled at db level as well

2 types of Sharding techniques :-

a. ***Range Based Sharding*** :- In this data is divided in ranges.<br/>

eg: user starts with name 
    (a-f) shard 1
    (g-m) shard 2
    (n-z) shard 3

b. ***Hash Based Sharding*** :- it used shard key (it should be static). and shard key can be  combination of multiple columns as well. Uniform distribution of data as our hashed algorithm decides data should go in which shard
    
increasing and decreasing no. of shards is difficult as our hashing algorithm needs to be changed
and data migration needs to be done.
solution of this drawback is consistent hashing in which along with data we hash server also and
make copy of these servers so when any server got replaced/added then less no. of data needs to 
be migrated.

**Pros of Sharding**
1. reduces the load from main database.
2. if sharded properly can speed up the db query process.
3. can be good solution for db failOver (on downtime switch to secondary db).
4. maintains load balancing at db level

**Cons of Sharding**
1. no uniform distribution strategy of data.
2. can increase the complexity for data querying if data needed by query is present in diff shards.
3. lots of chunk data
4. some shards may act as load enhancer which may eventually leads to server failure.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is DB Clustering ?***

clusters are of 2 types

* Replica sets
* Sharded clusters

|                Replica sets                 |             Sharded clusters             |
|---------------------------------------------|------------------------------------------|
| Maintain same data set in db instances      | Stores diff data in diff db instances    |
| provides redundancy and high availability   | used when have huge data set             |
| Load Balancer                               |                                          |
| FailOver                                    |                                          |

**Note** clustering constitutes 1 primary node and multiple secondary nodes. If the primary node goes down then one of the secondary node will act as primary node and once the previous primary node works again it will join the secondary instances.

rc.status();

rc.initiate();  // initiates replica set on primary set

rc.add("localhost:1234");

mongodb://localhost:1234,localhost:4567,localhost:8901/?replicaSet=myReplicaSet

In **Load Balancing** also we have 1 primary node and multiple secondary nodes.
write privelage is available to primary node and on of the below read preferences have to be set.

1. Nearest
2. primary
3. primaryPreferred
4. secondary
5. secondaryPreferred

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is Shadowing ?***
Shadowing is a technique in js by which you can shadow the a variable in current lexical scope.

    eg: var a='rahul';
        {
            let a='test';  // shadowing var a with let a
            console.log(a);
        }
        console.log(a);

***Illegal shadowing*** :- when try to shadow let, const variables by var variable having same name

    eg: let a='rahul';
        {
            var a='test';   // illegal shadowing
            console.log(a);
        }
        it will throw syntax error that a is already defined.

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

## Q. ***Difference between rem and em ?***

**em** :- relative to parent element

* The font-size of the .child element will be 40px (2*20px).
* The margin of .child will be 60px. That’s 1.5 times the font-size of our element (1.5*40px).

        <style>
            .parent {
                font-size: 20px;
            }
        
            .child-em {
                font-size: 2em;
                margin: 1.5em;
            }
        </style>
**rem** :- relative to root font-size if root not mentioned then browsers default which is 16px

* The font-size of the .child element will be 60px (2*30px).
* The margin of .child will be 45px. That’s 1.5 times the font-size of the html element (1.5*30px).

        .html {
            font-size: 30px;
        }
    
        .parent {
            font-size: 20px;
        }
    
        .child-rem {
            font-size: 2rem;
            margin: 1.5rem;
        }
          


<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***What is Image map ?***

Map is tag in HTML5 with which one can describe a sub sections of same image perform different events.

        <img src="workplace.jpg" alt="Workplace" usemap="#workmap">

        <map name="workmap">
            <area shape="rect" coords="34,44,270,350" alt="Computer" href="computer.htm">
            <area shape="rect" coords="290,172,333,250" alt="Phone" href="phone.htm">
            <area shape="circle" coords="337,300,44" alt="Coffee" href="coffee.htm">
        </map>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Explain inline Boxes ?***
Inline Boxes are of two types:

* Replaced Boxes (img, input, video, iframe etc) :- whose width and height are out of the scope of css and depends on the link provided in them.
* Irreplaced Boxes :- (span, q, textarea, strong, a etc)

Inline Irreplaced Boxes have only left, right margins
no effect of top, bottom padding unless some background is given to the element.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Explain Clustering in nodejs ?***

Clustering in node.js is the ability of nodejs to run multiple nodejs instances on seperate threads that will listen to the same port
using the cluster module.

**Note** when isolation is not required then use worker_thread module which will allow running multiple application thread in single
instance.

        import cluster from 'node:cluster';
        import express from 'express';
        import { cpus } from 'node:os';
        import process from 'node:process';

        const numCPUs = cpus().length;
        const app = express();
        

        if (cluster.isPrimary) {
            console.log(`Primary ${process.pid} is running`);

            // Fork workers.
            for (let i = 0; i < numCPUs; i++) {
                cluster.fork();
            }

            cluster.on('exit', (worker, code, signal) => {
                console.log(`worker ${worker.process.pid} died`);
            });
        } else {
            app.listen(process.env.PORT, () => {
                console.log(`server is running on port ${process.env.PORT}`)
            });

            console.log(`Worker ${process.pid} started`);
        }

cluster.fork() method uses the child_process.fork() method.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
