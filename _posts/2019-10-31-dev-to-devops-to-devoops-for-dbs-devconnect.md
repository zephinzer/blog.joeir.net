---
layout: post
title: "Dev > DevOps > DevOops! (Transcript for DBS DevConnect 2019)"
published: true
---



# Foreword

This is a planned transcript for a talk given at DBS DevConnect 2019 - an internal event for DBS Singapore.

Location: 

# Introduction

Hello everyone, I'm Joseph and I'm a Site Reliability Engineer from the Agile Consulting & Engineering tribe of GDS Singapore. For those who don't know us, we're a department within GovTech that is focused on digital transformation at the ground-level within GovTech.

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

# Continuous Integration/Delivery (CI/CD)

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

That's the entire MyCareersFuture team in all it's glory at the start of the project.

# And...

And here's the story of my life.

So like how all good stories begin, we begin with once upon a time, there was a software developer. No prizes for guessing who that is.

At that time, he had already dabbled in most aspects of software development, and he wanted to know more. To be specific, what was production-ready software and how could he get there?

He had recently learnt about continuous integration and delivery and while he didn't have a chance to work on it in the previous product he was on, he often wondered what it took to create such a pipeline. A pipeline that sent code into the 'clouds'? What sorcery is that?

About a year after he had joined GovTech, a new product finally emerged. Again, no prizes for guessing which product it was. So he got to work on pipelines, learning everything there was to learn about enabling a product to be delivered quickly yet safely.

Until, the team noticed! And it was at this point in time where our dear developer, ahem, yours truly, was fascinated with operations and oiling the wheels for developers to send code up into the clouds. So he helped to deploy code to production, automating whatever he could and helping whoever he could along the way to get their code into production.

And this continued. Until.

The team became reliant on him to do anything remotely operations related. At this point in time, we had grown to a decent size and our initial application had been split into about 3 services, A.K.A., when problems start to surface.

So rely on him they did. And here is the first mistake I'd like to admit to.

# Having a dedicated "DevOps" engineer

As you might be able to tell from what happened, an unsaid "handover" of duties surfaced. Namely between that of "developer" and "operations". While this was fine at the beginning when we haven't scaled, things started to get messy once it hit even a few services. For example, dependencies began to become mismatched, different repositories began to have different code styles.

But more importantly, this also resulted in a loss of ownership over what delivering working software meant. Developers stopped caring whether code they wrote was actually deployable. If it worked on their machines, it was expected to work in production.

And my assertion is that this - I.E., having a DevOps Engineer role - is the start of a slippery slope towards what my team and I now call DevOops.

# So we continue

So meanwhile, more team members began joining the team. Yay, more hands means faster delivery right? Yes, well, except we focused too heavily on the delivery instead of continuously questioning if what we were doing was right. This resulted in the some processes remaining the way they always were.

In our situation, this happened to be testing. So at the start, we had a set of end-to-end tests which ran, and when it passed, the code gets deployed. When the second service came, we did these end-to-end tests on both services. Then three. Then four.

This resulted in all-or-nothing deployments.

Which really defeats the point of having a Service-Oriented Architecture doesn't it?

So mistake number 2.

# Distributing services but not the processes

Because of the pressure to deliver coupled with the unsaid "handover" which we adhered to because we had a "DevOps Engineer", we failed to align the software architecture with the processes we were adopting. In this case, while the behaviour of our code was separate and decoupled, our testing was binding these systems together so that they could no longer be independently deployed.

We tried to move away from a monolithic architecture, but we ended up making it even more complex. We called our state then, a distributed monolith. Because y'know, distributed sounds sexy and we like to see our mistakes as successes.

And the main takeaway I would like you to remember from this is, failing to align your processes with your architecture can result in the negation of the benefits of your chosen architecture. And having a proper culture of DevOps would have mitigated this from happening because everyone would have been keeping deployability in mind.

# And we continue

Because of our then-massive deployments involving multiple services, production deployment became a weekly affair. We even had developers carefully watching out for the pipeline and letting the DevOps team know whenever a build coincidentally turned all-green.

And then it happened. One day, the system went down.

And then came the email that really changed things. "In light of recent events, we shall defer deployments to when we have the least active users. From Google Analytics, this seems to be around 6-7pm, when they're having dinner". Yeah, but what about our dinner? But we had to comply.

And here I'd like to stop time and highlight the third mistake we made.

# Making deployments eventful

And this comes in the form of production deployments being eventful. First was the pipeline, which to be fair was self-inflicted, and second was orders from above, because y'know we're the government and one mistake usually means an extra clause in an already huge book of rules.

So what's wrong with deployments being eventful is that it further deepens the already deep hole we dug for ourselves when deployments became a weekly affair.

First, having after-work deployments meant that not everyone would be able to stay back. I'm sure some of you are parents and you'll probably agree, in theory at least, if I say your children's welfare is more important to you than any work you could do for a company.

We're humans, face it. All of us want to give our best, and coercing people, if that's the right term, is not enabling us to give our best during our working hours - because where's my dinner right? 

So deployments should be boring. They should be everyday. So that we can empower the development team and acknowledge our team members as humans beyond just resources.

Also, if a deployment needs to be "prepared" for, that's a strong indicator that a "handover" is present.

Wanna know a trick to test your own teams? Let's deploy on Friday at 5pm. How does that sound? Notice whether you feel fear or indifference. If it's fear, ask yourself why and I think it'll lead you to the improvements you need to make to your development lifecycle.

# And we continue again

So we reluctantly continued with after-work deployments. At the start, developers were still fairly enthusiastic - because it's eventful - and some stayed back to help out with the deployments.

And then time went by. Till there were only five of us after a negotiated lunchtime deployment. Basically that's all the developers who were there at the start of the project. And that's a sneak peek of Barry who's coming up later.

And more time went by, until.

This was a fairly recent message by one of our product owners. Can you spot the danger signs? I'll give you five seconds.

...

Ok your time is up. Check out this line. "Devs are not required for this deployment". Oh, so now it's official and the team is being told how technical work should be conducted by the business elements.

And yet, it happened again. For the second time, our site became unusable with a persistent error.

And the reason for these errors was that in the name of optimisation, the DevOps team had created read replicas of our database, with only one writable instance. However, developers did not know this and was sending write operations to the read-only connection.

Since no developers were around, and it was officially stated that no developers were required, we were in some pretty deep shit.

# Losing touch with the codebase

Which brings me to what I would like to highlight as mistake number 4. Losing touch with the codebase.

Never, ever lose touch with the codebase.

If you can't code and are deploying things, there is no culture of DevOps.

If you don't know what you're deploying, you're ops.

And finally, if you can't fix what you're deploying, then who are the people who can and why aren't they there? We're supposed to be delivering working software.

And yeah, public service announcement, if you haven't got where this point was going. Putting developers and operators together is not DevOps. That's just a cross-functional team, but it does not create a culture of DevOps that's needed to continuously deliver working software.

# So it should have ended here

So it should have ended here, but something happened pretty recently that I thought would make a good number 5. Because 4 points don't sit well with many people for some reason. Right? Haha

So a month ago, a merge request came into the code base. It looked innocent in all it's newborn code splendor. A nice simple title, approvals by team members and even a polite little blue Review Me tag.

What happened inside this merge request was however potentially disastrous to a future deployment.

So we had an existing GraphQL interface to this service, and this merge request added a new RESTful interface. This is usually already a no-go architecture-wise. But what made it dangerous was that developers did not know about the API Gateway we had deployed in front of the service in question. Having a RESTful endpoint would have resulted in no requests being routed to it, and lots of complaints from angry citizens.

The right thing to have done would have been to branch off from the service into a new service. For context, the original service was handling requests related to users' profiles, but the RESTful endpoint was for retrieving job applications. So we asked the developers why didn't they put it in a new service since the business domain was different.

The answer was that creating a new service seemed too troublesome and the product owners wanted it last week.

And this brings me to the final mistake I think we made on our journey towards "DevOps" whatever it meant.

# Not making it easy to do the right thing

In a nutshell, we can have all the ideals we want, communicate them well, and have everyone agree. But when it comes down to delivery pressure from product owners, all that matters is how easy it is to do the right thing.

Everyone wants to feel, erm, do the right thing. But how easy is it for them to do so? Education, opportunity, and safety are key to this. And in retrospect, the founding technical members of the team, which includes myself, did not communicate the ideals of a Service-Oriented-Architecture well enough. Insufficient education.

Having a dedicated DevOps team also resulted in a lack of opportunities for developers to have a go at deployments and operations. Coupled with a lack of safety, this influenced the team such that developers no longer knew how their code would perform in production. Why lack of safety? With every botch during deployments, we had more rules coming our way. Would you feel safe?

So to end this point, if developers on the team don't know or care what happens to their code after they hit `git push`, it's really just dev. Not DevOps.

# TL;DR

And that's the end of my sharing. 

Incase you're wondering, no, our project is not on fire. These were things that we began to recognise was threatening the team's culture and we have started to shift things around as new members continue onboarding. This was a picture at our most recent deployment and we've begun to engage developers once again in our deployments.

We've still a long way to, I don't want to print a pretty picture, because mistakes are how we improve and I hope my sharing today was useful to you for identifying potential early warning signs of what can happen to a team that was focused on implementing DevOps.

So the real TL;DR, incase you've been sleeping or have already got your lunch.

One. Avoid having a "DevOps Engineer" role. This to me is the most important point and the one point that would probably have enabled us to avoid most of the sticky situations we found ourselves in.

Two. Always remember to align your processes with your architecture. Just because something used to work doesn't mean it'll always work. Be careful of scaling.

Three. Make deployments boring and everyday. Because y'know, people have lives outside work and we want to enable people to give their best while they're at work.

Four. Never lose touch with the codebase. Otherwise you're ops, and those of you who've done ops would agree that doing work which keeps the lights on is a surefire way to drive your most talented engineers away.

Five. Make it easy to do the right thing. People want to do the right thing, let them.

Oh, and did I mention rule number one? Avoid having a "DevOps Engineer" role. And with that,

# Curtain Call

I am Joseph Matthias Goh, you can stalk me using my handle, at-z-e-p-h-i-n-z-e-r on GitHub or GitLab, and I'm a... DevOps Engineer at GovTech Singapore.

Questions, fan-mail, future engagements or ice cream sponsoring can find me at joseph_goh@tech.gov.sg, and I will now hand over the time to Barry to talk about a product that came up AFTER MyCareersFuture, had less resources, and a much shorter timeline than we had.