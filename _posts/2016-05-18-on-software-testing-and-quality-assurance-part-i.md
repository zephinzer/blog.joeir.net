---
layout: post
title: "On Software Testing & Quality Assurance (Part I)"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

The importance of software testing is often overlooked in the development of software products. Why bother after all? After a feature is developed, if it is clicked and it works as expected, why should we bother investing time and effort to formally verify it works? Isn't the fact that it is working enough for us? Well it's not so simple.

To develop a simple app/software (which shall be used interchangeably hereafter) is trivial. However, problems come in when the software is more complex and requires multiple components with their own programmed logic to interact with other. As the saying goes, too many cooks spoil the soup.

So here comes testing. Testing ensures that each cook knows exactly what he/she is supposed to do and how it is supposed to be done, allowing many cooks to collectively create an even better soup.

This post is part *one* of a four-part series and is a recollection of knowledge I learnt during formal training in university coupled with my experience of contributing to open-source projects and development of [Mooziq](https://mooziq.sg).

This part concerns the different levels of testing (Test Scopes) and the types of code structures (Test Code Structures) one can expect to see in code meant to test other code. 

---

This post was originally published on my blog at https://blog.joeir.net/software-testing-pt-i. Check it out for more articles like this!



[Part I - Test Scopes/Structures](https://blog.joeir.net/software-testing-pt-i)

[Part II - Test Strategies/Methodologies](https://blog.joeir.net/software-testing-pt-ii)

[Part III - Test Metrics](https://blog.joeir.net/software-testing-pt-iii)

Part IV - Test Implementation

---

## Test Scopes
There are generally four scopes of testing, Unit Testing, Integrated Testing, Systems Testing and User Acceptance Testing. 


### 1. Unit Testing
*Unit Testing* is the testing of individual components. In the context of human management, *Unit Testing* is the equivalent of an interview, ensuring that the individual candidate possesses the skills required to perform the job.

*Unit Testing* tests the internal functionality of each component and ensures the accuracy and reliability of the component. Much like how an interview for the position of an account might involve questioning on balance sheets and profit and loss statements. An example *Unit Test* case for a certain User component might be described as "*createUser() creates a user in the database and returns the ID of the newly inserted row.*" which can be verified by checking if a valid ID is returned.

Unit Tests can be automated with scripts where a pre-defined input is passed to a function whose output is then compared with a pre-defined output to check for correctness. As such, *Unit Tests* should be run on every code change to verify correctness of a newly developed function and to confirm that addition of the new function/removal of an old function does not affect the other functions of the component.

### 2. Integrated Testing
*Integrated Testing* tests the pairwise (and upwards) interaction between components, ensuring that data exchange and internal processing between two components results in no loss of data integrity.

*Integrated Tests* can be fully automated via test cases that are similar in implementation to *Unit Tests* but with inclusion of two or more components instead of one. 

An example *Integrated Test* between two components, User and Profile, can be described as "*createUser() automatically creates a new profile.*", with createUser() being called and then checking for existence of a Profile associated with the newly created User.

*Integrated Tests* can still be set to run on every code change but are more ideally left to run on every branch merge if following a proper Git Flow workflow where each component is developed on their own branch. When run on every code change, inclusion of *Integrated Tests* might incur some latency if test suites are sufficiently large, which is not ideal.

### 3. System Testing
*System Testing* tests the behaviour of the system disregarding the number of components the  functionality in question involves. For example, on pressing a button which triggers a cascade of code execution, *System Testing* verifies that the resulting outcome matches the intended outcome and can be said to more behavioural than scientific (*vis-a-vis* Unit/Integration Tests).

An example *System Test* on the functionality of creating a new User might be described as "*User registration results in the creation of a profile and an email notification to the user.*". The test case would verify that a new profile was created and that an email was sent out to the new user.

*System Tests* can also involve testing for efficiency with an example test case possibly being described as "*A search query with less than 20 search keywords should take less than 3 seconds*".

Most testing frameworks allow for timing of such functionality but it is no longer practical to run these tests on every code change because each test suite may take time to complete and verify. As such, *System Tests* should be run before initiating the building of resources or before pull requests.

*System Testing* is also commonly referred to as *End-to-End* testing.

### 4. User Acceptance Testing
*User Acceptance Testing* (UAT) tests how real humans interact with the system. Due to the fickle and subjective nature of humans, *UAT*s cannot be easily automated.

Tests at this level cannot be fully automated and instead requires event-triggered analytics which track users' actions on the user interface. An example test case might be described with "*User should be able to identify the menu button easily.*". How 'easy' is defined cannot be put into numbers because despite having analytics, if the user was not intending to look for the menu button, we as product creators can never know if the user found the menu button easily, although we can say for certain they did find it if the menu button was the first button to be clicked. 

I once read an article that claimed the best way of doing your *UAT* is by going to coffee places and treating people a cuppa in return for using your app. Ask them to perform certain actions and keep notes on how they interact with your interface. Personally with [Mooziq](https://mooziq.sg)'s UAT, I got friends to attempt completing a list of desirable actions (which should have been planned out in your Use Cases and Main Success Scenario [MSS] ) and noting how many taps/clicks they took to achieve the action, in other words, the question to be answered is: how quickly can they complete your *MSS*s?

*User Acceptance Tests* are typically performed at every major UI/UX change and are generally not required for back-end upgrades.

## Test Code Structures
At the implementation level, code structures meant to test a software can be generalized into three identifiable patterns: 1) Assertions, 2) Test Cases, 3) Test Suites, 4) Test Plan, with each pattern theoretically being able to accommodate an infinite number of the previous pattern as it's children.

### Assertions
At the heart of testing are assertions, which is the exact comparison of an expected result with the generated result. Assertions are atomic and represent the smallest test unit in a Test Plan. 

After comparison of expected and generated output, a mismatch causes an assertion error which likely indicates a test failure. On match, nothing happens and the test case proceeds towards the next assertion.


### Test Cases
Many assertions which verify the same function are collectively known as a Test Case. For example, if you were testing a function that validates an email address, there will be more than one criteria to pass a string off as an email address.

An email address consists of three sections: 1) username, 2) '@' symbol and 3) domain name. The username has a requirement of being alphanumeric, including periods (.) and underscores (_) but cannot start or end with a period or underscore. 

This can be tested by using assertions to verify that usernames such as _joe@joeir.net, .joe@joeir.net, joe._@joeir.net are invalid. These assertions are grouped into a single Test Case which could be labelled "*Username section of e-mail are alphanumeric and allow for periods and underscores, but cannot start with periods or underscores*".



### Test Suites
Multiple *Test Cases* form a *Test Suite* which is often tied to a component or a high level functionality. A *Test Suite* can contain children *Test Suites* to form a hierarchy of tests that covers a behavioural property of the system.

An example *Test Suite* might be described as *User Authentication Functionality*, with children *Test Suites* and *Test Cases* covering code that involves the high level functionality of User Authentication.


### Test Plan
The *Test Plan* contains one or more *Test Suites* together with documentation indicating Test Strategies and Methodologies used. 

*Test Plan* formulation, in contrast with *Test Suites*, depends heavily on context and different types of software, or even components, may require different Testing Strategies and Methodologies to verify their correctness.

An example *Test Plan* might be labelled with "*Data Integrity Testing*" which includes *Test Suites* and *Cases* that verify the integrity of the data in the context of individual components. Another *Test Plan* might be labelled with "*Behaviour Integrity Testing*", which verifies that high level functionality work as expected.

## Conclusion
Test Scopes are the levels of testing one can engage in. Unit Testing tests each unit within a component ie. the methods (functions) that make up the component. Integrated Testing tests pairwise interactions between components. System Testing tests high level functionality disregarding the components involved. User Acceptance Testing tests how humans interact with the system.

Test Code is composed of Assertions, which are lines within the code that assert a generated output value matches an expected output value. Test Cases are composed of multiple Assertions and represent a functionality test. Test Suites are composed of multiple Test Cases and represent a component. Test Suites are ways of organising Test Cases. Test Plans consist of multiple Test Suites differentiated by testing strategy and intended testing outcome. 

Till the next post!

---

This post was originally published on my blog at https://blog.joeir.net/software-testing-pt-i. Check it out for more articles like this!