---
layout: post
title:  "That CI/CD Thingy: Principles, Implementation & Tools"
published: true
---

> NOTE: This is a post migrated from my Medium blog at [https://medium.com/@joeir](https://medium.com/@joeir).

# That CI/CD Thingy: Principles, Implementation & Tools

![cover image](https://cdn-images-1.medium.com/max/800/1*TNJ7Rpr5G1OJHtKH-IBEFw.png)

We're big on testing, and the concept of Continuous Integration/Delivery (CI/CD) is pretty much core to our processes. For the uninitiated, CI/CD is a concept which is implemented as a pipeline that facilitates the merging of freshly committed code into the main code base and allows different types of tests to be run at each stage - fulfilling the **integration** aspect - and ending it's run with a deployment of committed code into the actual product that end-users see - fulfilling the **delivery** aspect.

CI/CD is essential to software development using Agile methodologies which recommends the use of automated testing to get working software into the hands of real users as quickly as possible. This allows stakeholders and users to access newly created features and provide feedback as soon as possible, so features can be iteratively improved upon.


---

I've recently had the pleasure of setting up a CI/CD pipeline for an as-of-yet-under-wraps project that targets local PMETs (Professionals, Managers, Executives and Technicians) who are having difficulty in finding employment, and documented here is what I've done and some personal thoughts on the philosophy of CI/CD.

P.S. If you're interested in keeping up with our development efforts, subscribe (follow) to our humble blog to get updates on our journey in disrupting Singapore's citizen-facing government technology [read about our other awesome product targeted at SME level businesses here](https://blog.gds-gov.tech/how-we-made-applying-for-grants-easier-e24d8182a8f5).)

## TL;DR
In summary, we will be covering:
First Principle of CI/CD: Segregation for Stakeholder Responsibility
Second Principle of CI/CD: Risk Reduction
Third Principle of CI/CD: Short Feedback Loop
Environment Implementations in CI/CD
Tools for CI/CD

Let's begin.


---

# First Principle of CI/CD: Segregation for Stakeholder Responsibility

One fundamental benefit of CI/CD is allowing just-in-time involvement from the various stakeholders in any given project.

🏃 **Developers and Designers (Devs)** create the experience and logic of the product given user stories, and it is their responsibility to create features that work, proving it works through unit tests.

👮 **Quality Engineers (QEs)** are responsible for maintaining product quality, and review features as they are Done (see the Definition of Done) and write end-to-end (E2E) feature/acceptance tests to establish that a user journey is satisfactory.

💼 **Business Analysts (BAs) and Product Owners (POs)** are responsible for human relevance (aka business value), and this is confirmed through interaction with actual users and coming up with user stories that make sense to them. They coordinate and review results from user acceptance tests to validate past user stories and possibly create new ones based on feedback.

🎩 **Operations (Ops)/DevOps Engineers** are responsible for the availability of the product to users and their work revolves around the CI/CD, scaling it as necessary and designing the code logistics so that code from developers can move to a production environment that end-users can access.

👑 **Users** are responsible for well, using the product, which implies that the product should be desirable and usable for them, justifying the product team's construction efforts.

Each stage in the CI/CD pipeline creates an environment customised for the use case of each of these groups of stakeholders in a product team to ***allow different roles to take ownership of their respective stage of testing***, ensuring the integrity of features that reach the end-user-facing product.


---

# Second Principle of CI/CD: Risk Reduction

Each stage in the CI/CD pipeline is created to reduce risk in a certain aspect which can be associated with the responsibility of a stakeholder group . Developers are responsible for logic and write tests to reduce the risk of broken logic, QEs are responsible for user flow integrity and write tests to reduce the risk of broken flows/user stories, BAs and POs are responsible for usability and engage in user acceptance testing to reduce the risk of creating unusable/undesirable features, Ops/DevOps are responsible for availability and engage in CI/CD maintenance, deployment related affairs (think database schema/data migrations) and scaling to reduce the risk of product unavailability.

As each stage is designed to reduce a specific risk, it is important to note that when setting up a pipeline, each stage should also serve as a gateway for quality control that prevents broken/undesired features from getting past.


---

# Third Principle of CI/CD: Short Feedback Loop

The reason we implement a CI/CD pipeline is to use machines to do things which humans otherwise would have to spend a great amount of time on, so that we can reduce the time taken for feedback on developed features. We strive to get the code to the user as soon as possible.

But why use machines? ***Because humans don't scale, machines do***. With scaling, we cut the time taken to test our software and it allows us to automate the deployment process, all of which will take a much longer time if delegated to a human. Automation is king in CI/CD.

However, in more error-prone situations that require human input, automation might not be desirable/not be possible. For example, we can never automate a human tester when it comes to usability. Another more relevant example is when production data migrations are required. Automate your code logistics (how code is packaged/moves between stages) as much as possible, but keep in mind that automation is ultimately meant to ***reduce time taken for getting committed code into the product***. If errors that potentially demand more time to fix than to prevent are present, avoid automation and keep to the objective of achieving a short feedback loop.


---

# Environment Implementations in CI/CD

Here we cover the different environments in a CI/CD pipeline implementation and what happens at each stage, taking reference from how we are doing things for the projects I am on. Each environment is represented as a branch within our source control code repository.

## 🚧 Development
Our development team is small and we are utilising [trunk based development](https://trunkbaseddevelopment.com/) where all commits get pushed to a single `development` branch. This branch serves as our development environment and runs unit/systems tests on all committed code. Once all tests have passed, the current version of the `development` branch is promoted to Quality Assurance (the `qa` branch), and deployed to a Development version of our application so that all developers can view, test and checkout code whether it is working or not, allowing for more collaborative development.

## 🔦 Quality Assurance
Our product goes through the `production` build process here, and unit/systems tests are run again in an environment with application level production configurations. On success, the code is deployed to a QA version of our platform and feature tests are run twice a day on this version of the platform. On passing of the regression tests, code in the `qa` branch is promoted to the `uat` branch.

## 🚦 User Acceptance Testing
We allow access to POs to assess if features have been created as they had been envisioned and communicated via user stories. On acceptance of stories through testing with a selected group of users, the stories are marked as being accepted and will be released to production at the next deployment.

## 🍀 Systems Integration Testing & Staging
At this stage, we deploy our application to an environment which exactly mimics the production environment and run tests to confirm that our software will work as expected in production when it is deployed. At this stage, we also run non-functional tests such as load-testing and security testing to confirm that our application running in production will be secure. When all tests have passed, we finally reach…

## 🚀 Production
This is where end-users get to assess released features. Nuff' said.


---

# Tools for CI/CD

📍 On-Premise
GitLab CI, TeamCity, Bamboo, GoCD Jenkins, Circle CI
🌤 Cloud-Based
BitBucket Pipelines, Heroku CI, Travis, Codeship, Buddy CI, AWS CodeBuild
🇸🇬 Government Specific
hats, Nectar

Our team is using [GitLab](https://about.gitlab.com/) as our code repository and CI runner due to on-premise hosting requirements. Some alternatives that offer on-premise options which we are using for other projects are [Bamboo](https://www.atlassian.com/software/bamboo) by Atlassian, [TeamCity](https://www.jetbrains.com/teamcity/) by JetBrains and [GoCD](https://www.gocd.org/) by ThoughtWorks. There is also [Jenkins](https://jenkins.io/) and [CircleCI](https://circleci.com/) which we have never used in a project (leave us a ping/comment if you have and found it awesome!)

Should you not have the requirement of on-premise hosting, there exist many other cloud-based CI tools such as [Codeship](https://codeship.com/), [Buddy CI](https://buddy.works/) and [Travis](https://travis-ci.org/). For personal projects, an increasing number of code repositories are also providing all-in-one solutions which give you access to their own CI tools. BitBucket recently launched its own CI called [Pipelines](https://bitbucket.org/product/features/pipelines) for free - limited by computing time, and so did [Heroku](https://devcenter.heroku.com/articles/heroku-ci). GitHub also has free integrations to [Travis](https://travis-ci.org/) if your project is open-source. If you're on Amazon Web Services and have got a little budget, Amazon has their own CI, [CodeBuild](https://aws.amazon.com/en/codebuild/), as well.

If you're in the government and looking to improve the quality of your applications that cannot be hosted in the public cloud, we understand your difficulties in adopting publicly available tools, and we have built some cool government-ready versions for you to work with: like our recently launched [Hive Agile Testing Solution](https://medium.com/r/?url=https%3A%2F%2Fblog.gds-gov.tech%2Four-first-agile-quality-engineering-product-2f15a883926) which allows you to run regression tests on your application to ensure that features don't break when you update your platform. Creating a modern web application, need tests to ensure compliance with government policies and a deployment environment as well? Check out [Nectar](https://medium.com/r/?url=https%3A%2F%2Fblog.gds-gov.tech%2Fnectar-10e0eb1581cf), the Platform-as-a-Service for government applications which provides compliance testing and also deployment to the government cloud for applications created using more recent technologies and methodologies.


---

Have fun and never stop creating with quality(:

---

# 🍻 Thanks for Reading
If you've benefitted/enjoyed this article, consider clicking on the 💚 (⬇️ for mobile, ⬅️ for desktops) to improve the visibility of this post so that Medium will share this with more people like yourself and they can benefit as well(: you can follow me too to receive updates when I make posts like this.

🔔 **P.S.A.**: Are you in Singapore and want to make a difference to citizen-facing government technologies? We're currently looking for rockstar engineers with superpowers in RoR, Node.js and React.js - and yes, your dear gahmen is moving away from 90s technologies 😱- so ping me at [joseph_goh@tech.gov.sg](mailto:joseph_goh@tech.gov.sg) if you're liking what you're getting from us~ cheers!