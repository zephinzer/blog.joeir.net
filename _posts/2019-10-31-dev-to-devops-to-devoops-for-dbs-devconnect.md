---
layout: post
title: "Dev > DevOps > DevOops! (Transcript for DBS DevConnect 2019)"
published: true
---



# Foreword

This is a planned transcript for a talk given at DBS DevConnect 2019 - an internal event for DBS Singapore.

# Introduction

Hello everyone, I'm Joseph and I'm a Site Reliability Engineer from the Agile Consulting & Engineering tribe of GDS Singapore. For those who don't know us, we're a department within GovTech that is focsued on digital transformation at the ground-level within GovTech.

Today, two of us, Barry and myself, have been invited to talk about some products we've been working on over the past few years and how trying to adopt a culture of DevOps has played out so far, and Barry's graciously allowed me to go first for my project, MyCareersFuture, which he is also on, on a personal story of how I went from Dev to DevOps, to DevOops. You'll see why later.

# The Point Of It All

So someone once told me to begin with why. So here's the key takeaway I'd like everyone to have before I drone on and people begin falling asleep or going for lunch, and that is, DevOps is a culture, not a role. Read that again, DevOps is a culture, not a role.

# Then Me

Now that we're done with the why, here's some things more about why I think I might have something to offer you. I'm at heart a software engineer whether by day or by night, and I've been dabbling in operations for the past two years or so since we've gone live. Also, if you're here because you wanted to see MyCareersFuture fail, you can buy me ice-cream later and ask nicely.

# MyCareersFuture

Alright, so let's get started with MyCareersFuture - why does it even exist?

Well, the primary objective from which everything else stems from on why we exist is to: reduce unemployment in mature PMETs.

# Business Objectives

To that extent, the core objectives that our business elements have identified is:

1. To nudge jobseekers to upskill. Through our design research phase, one of the primary reasons we identified on why mature PMETs may fail to find a jobs is that they don't know about skills they may be missing out on, which leads us to the second point.
2. To raise awareness of in-demand skills. If you use our website, you'll see that we focus heavily on skills so that jobseekers may be able to see what skills are being asked for in the current economy.
3. To facilitate usage of government grants. How many of you here know that the Singapore government has schemes that reward both employer and prospective employees for hiring local? (respond accordingly)

# With Some Numbers

To give you an idea of the scale that we are operating at - which probably isn't very big - we have roughly 73k job views a day resulting from 35k job searches. Every day we also get roughly 17k sessions and roughly 10k job applications.

# Supported By

On the development side of things, our team is roughly 20 people split into 2 squads, one for the jobseekers' portal, and another for the employer's portal. About a total of 22 commits happen a day with about 4 deployments to staging every day.

Our system is comprised of roughly 8 services with 3 of them having an associated database, all inside 2 kubernetes clusters with a pod count of roughly 280.

# Delivered Via

For the development workflow, we follow an iterative development methodology with 2 week sprints and production deployment every 2 weeks.

So yeah, basically I was trying to avoid having to say that word that's been abused so badly it's not even funny anymore - Agile.

# Agile

So why Agile has come up is because I personally think it's meaningless to talk about DevOps without truly understanding Agile, because that's how DevOps as a concept even became useful.

Agile for the development team in terms of the product implementation, can essentially be summed up as "continuous delivery of value through working software". This is important.

## Continuous Delivery

So let's break my assertion down to smaller parts. "Continuous delivery of value", this means that the software at all times should be one, deployable - if it's not, it could never provide value, two, maintainable - if it's not, we couldn't be continuous abou things, and lastly, if it wasn't obvious enough, valuable - this means that what we're deploying brings value to the end-user.

## Working Software

And the next part which is pretty short, but equallity important is "working software". I don't know about you, but "working software" to me isn't just stuff that satisifes the happy path. "Working software" should imply that the code has been one, tested - this is subjective because testing is something we can never have enough of, two, available - which means that the service should be capable of interacting with the user, and lastly valuable - basically business value too.

# Service Oriented Architecture

Early on in the project, we decided that since this would be a citizen facing product, we should be optimising for user experience. And this resulted in a decision to adopt a service-oriented architecture.

## Quicker Delivery of Value

Firstly, this enables us to deliver value quicker. As compared to having a monolith with a pipeline that took an hour to pass, we could push code to a smaller service which has a shorter continuous integration pipeline which would result in a new feature being delivered more quickly.

## Higher Resiliency and Availability

Another benefit we saw of SoAs was that it would allow us higher service resiliency. If one service goes down because of a critical error, this wouldn't affect the rest of the system. This may be clearer in the next slide on

## Graceful Degradation of User Experience

Graceful degradation of the user experience. Putting the services into context, we could have a notifications service, a jobs service, and a profiles service. Assuming the notifications service went down because of a runtime exception, users would simply miss out on notifications instead of missing out on the entire service

## Facilitate re-use of services

At GDS, we are just one of many teams that's pushing the boundaries of how we're creating software. SoA also allowed us to create services which may be able to be used by other teams with similar needs. For example, every application would require a notifications service. Going from how we did things, assuming we had already written a robust notifications service, this same service could be easily used in other products that also required a notifications service.

## Lower cost through targeted scaling

Lastly, SoA offers us the ability to scale services as demanded by business needs. For example if notifications was heavily used, we could scale it independently of the other services. Alternatively, if users really liked updating their skills profile, we could simply scale our profile services instead of scaling everything else. It's taxpayers money after all and we think we owe it to you all.

## Summary

So in summary, we hoped that adopting a SoA would enable us to have lower risk deployments, quicker deployments, be easier to maintain, and offer a better user experience on the overall through graceful degradation of services.

# Continuous Integration/Delivery

In order to support the velocity at which we would be releasing updates, we also decided on adopting continuous integration & delivery, which you probably have already heard of, but if you haven't, it's basically lots of automated tests ending in a deployment and it automates most of the boring things that us humans are generally pretty bad at.

Here's a list of them which are available in our systems but the details don't really matter and I hope it's not intimidating because we are a 2 years old product and the more mature a product is, the more tests we need to ensure we don't break anything while we add features. Basically, just automate all of your tests, because this results in:

1. A quicker feedback cycle - which developers usually love
2. Verification that it works as expected - low inertia to run tests means tests will be run more often
3. Verification that it works as required - it gets to the user quicker
4. Can be deployed - if it passes the pipeline and gets deployed to staging, it's good to go

# Summary of Overview

So in summary of everything said so far, our golden objective is to reduce unemployment in local PMETs and we were commissioned to implement a product that would help with this. We decided on doing a web service using firstly, an iterative development methodology, Agile, which basically means to continuously deliver value through working software, which means it should be deployable, maintainable, valuable, tested, and available. Secondly, a service oriented architecture which allowed for more resilient, more easily maintained, and easier to deploy code. And lastly, continuous integration and delivery, which enabled shorter feedback cycles, and allowed us to sleep at night knowing our code was well-tested and consistently deployable.

# So

So now that I've given you an overview of the business need and current scale of our product, here comes the fun part.

This is the first time I'm giving a presentation in the form of a story and as an engineer that's usually at a corner writing code, I promise you this is scary. But here goes. So remember this picture?
































