---
layout: post
title: "On Software Testing & Quality Assurance (Part III)"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

The importance of software testing is often overlooked in the development of software products. Why bother after all? After a feature is developed, if it is clicked and it works as expected, why should we bother investing time and effort to formally verify it works? Isn't the fact that it is working enough for us? Well it's not so simple.

To develop a simple app/software (which shall be used interchangeably hereafter) is trivial. However, problems come in when the software is more complex and requires multiple components with their own programmed logic to interact with other. As the saying goes, too many cooks spoil the soup.

So here comes testing. Testing ensures that each cook knows exactly what he/she is supposed to do and how it is supposed to be done, allowing many cooks to collectively create an even better soup.

This post is part *three* of a four-part series and is a recollection of knowledge I learnt during formal training in university coupled with my experience of contributing to open-source projects and development of [Mooziq](https://mooziq.sg).

This post concerns Testing Metrics which helps you determine how well your test strategy was designed and how covered is your application. Think insurance coverage review.

---

This post was originally published on my blog at https://blog.joeir.net/software-testing-pt-iii. Check it out for more articles like this! Other parts:



[Part I - Test Scopes/Structures](https://blog.joeir.net/software-testing-pt-i)

[Part II - Test Strategies/Methodologies](https://blog.joeir.net/software-testing-pt-ii)

[Part III - Test Metrics](https://blog.joeir.net/software-testing-pt-iii)

Part IV - Test Implementation

---

## Testing Metrics
No post on a topic claiming to be of significant importance in product development is complete without quantifiable metrics that serve to measure how effective its implementation was. 

While these metrics provide quantitative data about the extent of tests done on code, they are not indicative of the quality or logic of the tests and as such should always be used in conjunction with code reviews to ensure that tests are both valid and accurate.

Examples provided will be in JavaScript since that's what I've been using for the better part of two years.

### Function Coverage
Function coverage tests for how many of the functions in a component/system are tested.

Consider a class with three methods as follows.
```javascript
var randomClass = function() { return this; }
randomClass.prototype.methodA = function() { return 0; }
randomClass.prototype.methodB = function() { return 1; }
randomClass.prototype.methodC = function() { return 2; }
```
100% function coverage for the Object, LeWildObject, would involve at least one test each for `methodA()`, `methodB()` and `methodC()`. 100% function coverage is usually not necessary since each component will typically have getters and setters that just retrieve and set information, and those functions need no testing.

### Statement Coverage
Statement coverage is the number of statements involved in the execution of the tests divided by the number of statements in the entity being tested.

Consider a function as follows with line numbers on the left.
```javascript
1| function Foo() {
2|  var a = 1;
3|  var b = 2;
4|  if(a == 1) {
5|    return 3;
6|  } else if(b == 4) {
7|    return 5;
8|  }
9| }
```
In the function above, there are 6 executable statements (exclude lines of code with just brackets). For this example, because `a` is always 1 when it reaches line 4, statements on lines 6 and 7 will never be executed - or - only 4 out of the 6 statements are executed. Hence, statement coverage can only be at 66% at best.

### Branch Coverage
Most popular programming languages can be structured visually as a Control Flow Graph. A control flow graph is a node-vertex graph with each node representing a line of code and each vertex connecting lines of code which are executed consecutively. Taking the following code as an example: 
```javascript
1|  function Bar() {
2|    var a = 16;
3|    var b = 1;
4|    for(var c = 0; c < a; ++c) {
5|      b *= b;
6|    }
7|    if(a == 1) {
8|      if(c > 3) {
9|        b = 1;
10|     } else {
11|       b = 2;
12|     }
13|     return 3;
14|   } else if(b == 4) {
15|     return 5;
16|   }
17| }
```
We can construct a control flow graph as follows annotated with a section (A/B/C) on the right:
```
             2           A
             |           .
             3           .
             |           .
         /-- 4 <-\       B
         |   |   |       .
         |   5 --/       .
         |               .
         \-> 7           C
            / \          .
           8   14        .
          / \   | \      .
         9   11 E  15    .
         |    |     |    .
         E    E     E    .
```
Section A is trivial. When it reaches Section B, the code can either go to statement 5 or 7. In Section C, the code can go to either 8 or 14 from 7. From 8, the code can go to 9 or 11, and from 14, the code can either end (E) or go to 15. This is known as code branching and *Branch coverage* is the number of branches executed in the test over the total number of branches. In Section A there are no branches, in Section B, there is one branch, and in Section C there are 2^2 (=4) different possible branch patterns { 7->{8,14}, 8->{9,11}, 14->{E,15} }. 

If a test goes from 2->3->4->5->4->7->8->9->E, it has only covered 2 branches out of the possible 5, and branch coverage is 40%.

### Parameter Value Coverage
Parameter Value Coverage (PVC) is the number of tested parameter values over the total number of tested values. Coverage quantisation from *PVC* can only be done when the domain of a function is specified in the code since it will not make sense to test every possible integer. Alternatively, values can be categorised via boundary value analysis and the categories of values can be treated as the domain of a function.

For example consider a calendar function which takes in a month's 0-based index and returns the month's name based on an incremental loop. To exhaustively test it, we will require 12 test cases, one for each month. By applying boundary value analysis, we test only 0 and 11 which should correspond to January and December respectively. Because we know that the function is based on a loop, if 0 and 11 are correct, we can be sure that values from 1 to 10 inclusive are correct. 

Applying the example to *PVC*, our parameter values can be categorised as start and middle, with start being 0 and middle being numbers from 1 to 11.


### State Coverage
State Coverage assumes that a component can be represented as a [finite state machine](https://en.wikipedia.org/wiki/Finite-state_machine). Consider the following class:
```javascript
var Room = function() { 
  this.wallLight = true;
  this.aircon = true;
  this.fan = true;
  return this; 
}
Room.prototype.turnOff = function(what) {
  this[what] = false;
}
Room.prototype.turnOn = function(what) {
  this[what] = true;
}
```
The total number of states is 2 x 2 x 2 because there are 3 objects in the room which can be turned on or off. Hence, we would have 8 states to consider in our testing. We can reduce the number of states we should test by eliminating the states which will not be reached. For example, assuming the aircon and fan would never be turned on at the same time. We term this an unreachable state and hence can be cut off from the number of tests.

## Conclusion
While testing is important, testing metrics are equally important to understand how well your test code is covering your production code. Test metrics can help you as a product manager/developer to be able to manage your risk by allowing you to see which parts of code are not tested, and whether they need to be tested for higher level functionality to be considered working.

In the next post, we will cover the implementation of tests for a web application based on Angular and Node.js. 

Till then!


---

This post was originally published on my blog at https://blog.joeir.net/software-testing-pt-iii. Check it out for more articles like this!
