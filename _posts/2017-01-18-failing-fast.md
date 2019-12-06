---
layout: post
title: "Failing Fast through Code: How We Do MVPs"
published: true
---

> NOTE: This is a post migrated from my Medium blog at [https://medium.com/@joeir](https://medium.com/@joeir).

![cover image](assets/images/2017-01-18-failing-fast/0_Mz5SJLUPRB7sXQUJ.jpg)

# Failing Fast through Code: How We Do MVPs

At the Agile Consulting & Engineering (ACE) team of Government Digital Services (GDS), GovTech, we focus on driving change in software development methodologies within the government from Waterfall to Agile. For the uninitiated, the goal of the Agile philosophy can be said to be ‘reducing the cost of change’. Being Agile, we acknowledge that change is the only constant, and we work around that constant, structuring our development efforts such that changes in the business climate can be effected as quickly as possible in our software. One way that we achieve that is by quickly validating and quickly understanding whether a hypothesis is true or false, or in other words: failing fast.

To fail, we must first have tried, and in alignment with our focus on being Agile, we aim to make our attempts as lean as possible to reduce the cost of change. Instead of creating a monolith involving significant time and effort, we create Minimum Viable Products (MVPs) with only the most pressing requirements, reducing the cost of change had we built something more substantial.

- - -

The following is a shallow technical article that provides an overview of the tools and processes we use at ACE to quickly prototype and test out ideas. As the Agile Manifesto goes: individuals and interactions over processes and tools, so rather than taking what we’ve highlighted as an ideal, consider your team, how they function, and see if our processes fit your working style.

# Our Stack In A Nutshell

Quickfire development can only happen when all team members are on the same page with regard to tools, languages, runtimes and frameworks. At ACE, we work primarily with Ruby on Rails, React.js and good ol’ HTML/CSS/JavaScript. Our MVP stack was designed to leverage our existing development capabilities, which we use daily for our larger, production-ready systems.

TL;DR, we use the following software to create our MVPs:
- Pivotal Tracker (project management)
- Ruby on Rails/Materialize (runtime & framework)
- BitBucket with Git (version control)
- CodeShip/Buddy (continuous integration/delivery)
- Heroku (deployment)

- - -

# Project Management

We use Pivotal Tracker following a ScrumBan way of managing user stories and tasks. User stories are always written as features from a user’s point of view. An example:

> As a User, I can log in with an e-mail and password so that I can secure private information specific to my account.

We also adapt the Scrum ceremonies of:
- **Stand-Up**: where we update each other on the progress of user stories we are working on. Unlike our main projects where we practice a daily Stand-Up as detailed by Scrum, we might not do this every day for creation of MVPs, depending on project significance and timeline.
- **Sprint Planning**: where one of us who has been assigned the role of Product Owner prioritises user stories in our backlog and the others decide how we are going to carry out the development work for the upcoming sprint.
- **Product Backlog Refinement**: where we do a cursory discussion of the technical tasks related to specific user stories, thereafter assigning them with story points depending on complexity so that we can understand our development velocity.
- **Sprint Review**: where the development team showcases features to the Product Owner(s) and gather feedback on the development work done during the preceding sprint. User stories are accepted throughout the week as and when the Product Owner can be notified of our progress.
- **Retrospective**: where we discuss processes in the last sprint, come up with new processes to try and stop other older ones that the team does not feel brings value.

It is then left to us to work on the stories assigned to the current sprint. Our sprints are conducted in 2 week time frames.


> Read more: [Agile Manifesto](http://agilemanifesto.org/), [Scrum Methodology](http://scrummethodology.com/)
> 
> Tools: [Pivotal Tracker](https://www.pivotaltracker.com/), [Trello](https://trello.com/)

- - -

# Runtime & Framework

Ruby on Rails is our choice for building MVPs because we use it in our everyday development work and all our developers are familiar with it, making it easier to get more developers onboard as and when needed. Rails comes with out-of-the-box support for open-source relational databases, such as PostgreSQL, so we use these instead of an enterprise database.

Rails also allows for database migrations which is helpful when doing development work based on user stories which may cut across different parts of the technology stack. For example, a user story regarding logging in may require a single developer to make changes to the database model, the back-end controller, and also the front-end view. If you have three such stories being developed on by three different developers, schema conflicts might happen if someone else’s code runs on your local machine expecting a certain table to exist. Migrations in Rails prevents this from happening by refusing to run if migrations are not up to date.

The UI framework we use during the MVP phase is Materialize, which is similar to Bootstrap that we are using for our main projects.

For dependency management, we use Yarn on top of Node Package Manager (NPM) to manage our JavaScript dependencies if any. Yarn allows for specification of exact module versions which reduces risk of errors due to minor version changes which NPM alone might not catch.

> Tools: [Ruby on Rails](http://rubyonrails.org/), [Materialize](http://materializecss.com/), [Yarn](https://yarnpkg.com/), [NPM](https://www.npmjs.com/)

- - -

# Version Control

We use Git together with Trunk Based Development to hasten the development workflow given that we work on MVPs in smaller teams. Git allows for easy deployment to our Continuous Integration/Delivery pipelines when used in conjunction with a cloud repository service.

To have a single source of truth for our code, we use BitBucket which offers hosting of private repositories. BitBucket also has included functionality to link repositories via webhooks and integrations to 3rd party platforms such as our Continuous Integration/Delivery pipeline covered below.

> Read more: [From Git Flow to Trunk Based Development](http://team-coder.com/from-git-flow-to-trunk-based-development/)
> 
> Tools: [Git](https://git-scm.com/), [BitBucket](https://bitbucket.org/)

- - -

# Automated Testing

Being Agile means we **iterate fast while keeping software functional**, and we can only do that if our testing processes are automated. Since we use Ruby on Rails, we use the *bundled* (hah!) RSpec to validate our back-end logic. Should React.js be used, we might include Karma, Mocha, Chai and Enzyme.

In contrast with our production-ready systems, we avoid writing front-end tests for our MVPs simply because testing does not provide any value to our users and we are not even sure if our perceived requirements actually fit the needs of our users yet.

Also for MVPs, our unit, system and regression tests are configured to run together upon pushing of any code, allowing us to immediately know if a code contribution wasn’t playing nice with existing code, and blocking error causing code from getting any closer to the production environment.

> Tools: [RSpec](http://rspec.info/), [Karma](https://karma-runner.github.io/), [Mocha](https://mochajs.org/), [Chai](http://chaijs.com/), [Enzyme](https://github.com/airbnb/enzyme)

- - -

# Continuous Integration/Delivery

In alignment with the Agile philosophy, our aim is to ship as soon as possible so that we can allow our intended users to use and give feedback on features we have recently created, allowing us to not only make changes quickly but also to validate our perceived use cases.

We implement this through a Continuous Integration/Delivery (CI/D) pipeline that is configured to send code from the `master` branch of our code in BitBucket, to CodeShip/Buddy, and if all tests pass for the given commit in `master`, the code is automatically forwarded to our `staging` environment in our deployment platform.

> Read more: [Setting up Bitbucket projects on Codeship](https://blog.codeship.com/codeship-bitbucket-tutorial/), [Deploy to Heroku](https://documentation.codeship.com/basic/continuous-deployment/deployment-to-heroku/)
> 
> Tools: [CodeShip](https://codeship.com/), [Buddy CI](https://buddy.works/)

- - -

# Deployment

We use Heroku to deploy our code. Build support for Rails in Heroku comes out-of-the-box: we simply add the Ruby-on-Rails buildpack, the database add-on for Postgres, and we’re good to go.

We define two applications in our Heroku project and create a pipeline consisting of `staging` and `production`, where `staging` is the hosted application which we use to do user validation and accept/decline user stories, and `production` is the final working application.

On passing tests in our CI/D environment, code gets pushed to `staging` where we confirm that our configurations work on Heroku's servers. Once we verify that it works, we manually promote `staging` to `production` and associated features are considered **shipped**.

> Tools: [Heroku](https://heroku.com/)

- - -

# In Summary

> Success is not a destination, but an iterative process.

We can’t have quality without quantity, and while all of us wish we could come up with excellent ideas that our intended users love from the start, more often than not it doesn’t happen. Success is not a destination, but an iterative process. Try fast, fail fast, and succeed sooner.

I hope this post was useful to you (hit the hollow green heart if it was!). If you’re a web developer, interested in what we do and wish to use your development skills to create impactful citizen-centric applications, well, you’re in luck, we’re expanding and we’re hiring to fill developer positions. Hit me up at [joseph_goh@tech.gov.sg](mailto:joseph_goh@tech.gov.sg) and let’s work something out 😀

- - -

Shoutout to [Kah Kong Poh](https://medium.com/u/a78e151fbe1e) for technical advice on Scrum and [Nina Ee](https://medium.com/u/83cd31a78a4e) for making sentences more awesome.
