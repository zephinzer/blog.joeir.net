---
layout: post
title: "On Software Testing & Quality Assurance (Part II)"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

The importance of software testing is often overlooked in the development of software products. Why bother after all? After a feature is developed, if it is clicked and it works as expected, why should we bother investing time and effort to formally verify it works? Isn't the fact that it is working enough for us? Well it's not so simple.

To develop a simple app/software (which shall be used interchangeably hereafter) is trivial. However, problems come in when the software is more complex and requires multiple components with their own programmed logic to interact with other. As the saying goes, too many cooks spoil the soup.

So here comes testing. Testing ensures that each cook knows exactly what he/she is supposed to do and how it is supposed to be done, allowing many cooks to collectively create an even better soup.

This post is part *two* of a four-part series and is a recollection of knowledge I learnt during formal training in university coupled with my experience of contributing to open-source projects and development of [Mooziq](https://mooziq.sg).

This part concerns Test Strategies and Testing Methodologies, and provides an overview with some commentary on the different ways one can structure one's thoughts when coming up with a testing strategy for an application.

---

This post was originally published on my blog at https://blog.joeir.net/software-testing-pt-ii. Check it out for more articles like this!

[Part I - Test Scopes/Structures](https://blog.joeir.net/software-testing-pt-i)

[Part II - Test Strategies/Methodologies](https://blog.joeir.net/software-testing-pt-ii)

[Part III - Test Metrics](https://blog.joeir.net/software-testing-pt-iii)

Part IV - Test Implementation

---

### Test Strategies

#### Black Box Testing
Black box testing refers to testing without knowledge of a component's internal workings. Behaviour Driven Testing is an implementation of Black Box Testing.

#### White Box Testing
White Box Testing refers to testing with full knowledge of a components internal workings and its implementations include Static Code Analysis, Test Driven Development, State Driven Testing and Data Driven Testing.

#### Data Driven Testing
Data Driven Testing (DDT) utilises data as the medium of testing and can be applied for any system involving Create/Read/Update/Delete functionality.

The focus of *DDT* is to ensure the correctness of data manipulation. A set of data passed into a function should result in a predictable set of data returned from the function, and verification of this condition is enough to verify that the functionality is accurate and reliable.

*Data Driven Testing* is ideally implemented in Test Suites covering multiple components because that is where integrity of data has a higher risk of being corrupted. *DDT* is convincingly easy to implement since it consists of mindlessly creating mock data, throwing it at an oracle, and comparing the generated data with an expected set of data. However, the tester should be wary of corner cases that may occur in real-life data when using mock data. 



#### State Driven Testing
*State Driven Testing* (SDT) depends on the presence of internal flags that combinatorially indicate what state a certain component is in. 

For example in an implementation of an infinite list loader, one might use *SDT* to specify that after every call to a `get()`, the size of data within the infinite list should be equal to the existing number of items PLUS the number of items requested from the database. The flag in this example is the length of the data.

Another example is when UI elements are conditionally tied to controller variables. Clicking `<button id="a">` results in the display of a message which is controlled by variable `displayA`. Simulating a click on `<button id="a">` via browser hooks should result in the component state being changed such that `displayA` is evaluated as `true`.

*State Driven Testing* is relevant in 1) components with an easily exhaustible number of states, and 2) User Interface testing, and is better implemented in Test Cases where the state of a component can be asserted to be equal to an expected state.


#### Behaviour Driven Testing
This school of thought emphasises on the behaviour of a system and puts a lesser emphasis on lower level tests, favouring the correctness of higher level functionality. 

When time is of the essence, BDD helps by specifying the overall behaviour without going into the smaller details which can be implemented once the high level behaviour is proven to work as expected.

*Behaviour Driven Testing* is more relevant at the Systems Testing scope which describes higher level functionalities. The risk of using *BDT* lies in not knowing if individual functions within components are working as expected. At a higher level, the functionality can be verified to be working by comparing output, but this does not verify that the correct output was generated from a desirable state.


### Testing Processes
There exists a few school of thoughts when it comes to testing and there is no definite right/wrong answer regarding which method is right. Context matters heavily in deciding what testing strategy to depend on and testing strategies can and should be combined to form one's final testing strategy.

For example, if you had five components, three of which are purely composite of getters/setters, with the remaining two utilising data from the other three, it wouldn't make sense for you to blindly implement unit tests on all the getters/setters. It would make more sense to focus your testing efforts on the two data processing components.



#### Static Code Analysis
Static Code Analysis is the testing before the tests. Code analysis provides the developer with insights into the code that allows the developer to refactor code.

For example, Static Code Analysis can indicate unused variables and unreachable paths which allows the developer to check if the code logic is correct and if every line of code is reachable (otherwise what's the point of writing it?). 

For example, an assignment operation after a `return` statement is unreachable and Static Code Analysis will reflect it as an error. 

```javascript
var b = (function a() {
  var a = 1;
  return a;
  a = 2; // Unreachable line
})();
```

Another example can be seen in the below code snippet:

```javascript
var a = 1;
// codeblock:C
if(a === 1) {
  // codeblock:A
} else {
  // codeblock:B
}
```

It is obvious that `codeblock:B` will never be reached and a Static Code Analyzer can help to identify such logical flaws. The example provided is trivial, but consider a case where `codeblock:C` contains lines of code that do not affect the variable `a` and it becomes difficult for a human programmer to identify such an error.



#### Test Driven Development
By far the most preached about in engineering school, Test Driven Development (*TDD*) is often the *de facto* school of thought demonstrated to initialise disciples of the art into testing.

*TDD* constitutes writing test cases first according to a set of requirements _before_ writing even a single line of code and it is easy to prove that such a methodology results in code that is leaner in the sense that each function contains lesser lines of code and the total number of functions is among the minimal required for implementation of the software requirements.

Benefits of *TDD* comes in the form of self-documenting code. Client requirements are first translated into test cases, ensuring that all functionality is there before development begins. However, this same benefit is can also be its downfall. 

Consider that real-world software is not a static artifact. Client requirements can be ever-changing depending on business input and *TDD* results in lost time writing test cases representing requirements that may be changed on a whim. 

The focus on passing test cases also results in code that is hastily written and generally intolerant to errors not described in the test case. These errors become evident when requirements change and the domain of the function is expanded. Writing of test cases to reflect requirements often does not cover corner cases which might occur with real-life data.

However, *Test Driven Development* still finds relevance in smaller, one-off projects where requirements are set in stone from the start of the project and where maintainability is not of significant importance.
 



#### Boundary Testing
Boundary testing is the testing of boundary cases (ie corner cases) because that is where errors are likely to occur. For example, when testing a calendar date calculation functionality, dates 2 to 28 are generally safe because they exist in all months. 

Errors are typically more likely to happen on days 1, which may evaluate to 0 if the developer accidentally used a 0-based index, and at 31, which regardless of whether the calendar starts from 1 or 0, will result in at least 30 which will not be valid for February in all cases.



#### Random Testing
Random testing is the randomisation of input with an independent verifier to algorithmically calculate the expected output. Such testing has the benefit of being able to remove human bias when deciding on predefined values used in the test cases, which may be extremely useful in some situations, and redundant in others.

Consider a function involved in taking in a string of characters, storing it and displaying it on the user interface. With a human user tester, the chances of randomly coming up with a 50 letter word is low. Assuming a maximum character limit on each word to be 40, a human user tester without knowledge of the software's source code would never breech the limit.



#### Mutation Testing
*Mutation Testing* is the modification of the source code instead of the test values. By having an existing set of passing tests, changing the source code is expected to produce an error in correctness which the test cases should detect. 

If every mutation results in a failed test case, the test suite is considered all-perfect, otherwise, there are two possible explanations: 1) the test suite has not managed to consider all possible invalid inputs/outputs 2) there exists redundant code.

As the saying *Quis custodiet ipsos custodes?* goes, *Mutation Testing* has utility in testing the test cases for loopholes. It also has the added benefit of being able to help you refactor your code. However, implementation of *Mutation Testing* is tedious and a proper framework for most modern web technologies does not exist yet.


#### Process/Model Checking

Using *SDT* with knowledge of context of the processes affecting the states, we get our next type of testing methodology, *Process/Model Checking*, which is already found in hardware testing.

*Process/Model Checking* involves 1) an input of state variables, 2) processes that manipulate the state variables. Assertions are written by telling the checker to verify that a certain state can/cannot be reached (reachability test), that no two asynchronous process will be waiting on the same state change (deadlock analysis), that a process does not run indefinitely under any set of input states (divergence/termination condition analysis), and many other heuristics that are being researched upon currently.

Some examples of specialised software that allow for such testing are [Alloy](http://alloy.mit.edu/alloy/index.html) and [Process Analysis Toolkit](http://pat.comp.nus.edu.sg/) (PAT) which allow developers to model processes with states. Such methodologies are rarely used in software testing because of their complexity and difficulty in implementation, requiring the tester to model and even rewrite code to simulate functionality of the code in the software. 

### Conclusion
Test Strategies provide us with a mental framework that guide us on how we view the software: from a developer perspective (White Box) or from a user (Black Box)? - from a data point of view or from a behavioural point of view?

Test Processes provide us with tools that can be implemented to fulfil the test strategies. The list is non-exhaustive and so far includes only methodologies I have heard of and have used.

In the next part, we will move on to Test Metrics which covers analysing the quality and extent of testing.

Till the next post!

---

This post was originally published on my blog at https://blog.joeir.net/software-testing-pt-ii. Check it out for more articles like this!
