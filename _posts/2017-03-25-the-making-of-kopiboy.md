---
layout: post
title: "The Making of KopiBoy"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

For my latest pet project together with my significant other, I made a chat bot named KopiBoy that helps to magically come up with a nearby café in Singapore based on your location, and we're ready to release Version One to the public. You can <a href="https://m.me/kopiboy.bot" target="_blank">find it here</a>, and here's how it was done.

# Objectives
We decided against this project as a business endeavour, instead focusing on properly going through the relevant technologies and design philosophies. With that, we had the objectives of:

- Understanding the various technologies behind a chat bot
- Discovering how to structure a chat bot application code-wise
- Finding cool cafés

# Perceived Use Cases
- As a Couple, we would like to find a nearby café to hang out at
- As a Café Hopper, I would like to discover new 'hipster' cafés to chill out at and put on my café-list
- As a group of friends, we would like to find cool nearby cafés to have coffee as we chat.

# The Journey
## Mode of Delivery
Having the objective of going through chat bot related technologies, our Mode of Delivery was somewhat set in stone - a chat bot. Sure, we could've made a website, but why not try a new technology and way of delivering information to people?

## Platform Decision
Seeing that our use-cases were targeted at the everyday user, we chose the Facebook Messenger platform based on our experience and impression of what platform most users would be on. We considered alternatives like Telegram and Slack but Telegram did not have as huge a following and Slack was meant for professional teams/groups with agencies and did not fulfil the demographic of recreational-activity-seeking users.

## Data Source
Initially, we went along with a manually curated list of cafés, which proved to be tedious, and Version One (the one you can see) uses Google Maps to search for cafés.

We saw the obvious benefit of having a curated list of cafés, but after some [dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food), we realised that there would be places without curated cafés even within a 2km boundary, and we also realised that an exponential amount of work would have to go into inserting the information, and we eventually decided against using a curated list since Version One would just be a prototype to test demand.

## User Experience Design
Consumer facing chat bots as a developer commodity are really new and we guessed that many people would not be accustomed to actually talking to a chat bot in a conversational manner. So for Version One, we decided against a conversational interface, choosing to use more traditional elements like Lists, Cards and Buttons to move users through a conversation. Our intended user journey was:

```bash
User: Hello!
Bot: <WelcomeMessage>
User: *Selects Nearby/Anywhere option*
Bot: <DistanceSelectionMessage>
User: *Selects distance (if Nearby)*
Bot: <LocationRequestMessage>
User: *Sends location*
Bot: <CafeCard>
```

## Analytics
As with any product, we take into account certain metrics to see which aspects of the product perform the best, should be retained/should be taken out. We chose to implement Google Analytics server-side to understand how users tended to interact with our bot, with analytics event being sent for:

1. 'Anywhere' option selection
- 'Nearby' option selection
- Distance selection
- 'Show Me Another' option selection

The 'Anywhere' and 'Nearby' option statistics would allow us to understand whether Users preferred a random café and were willing to travel, or if they were searching for a nearby café, which would influence which option makes it to Version Two.

The Distance selection metric would allow us to see how far people would generally be willing to travel for a café and would allow us to optimise our list of cafés for discovery within a certain radius.

The 'Show Me Another' option would hopefully give us insights into how good our café deciding mechanism is.

# Technologies & Geeky Stuff

## Application Architecture
We used Node 6 for the runtime and paired it together with an open-source wrapper for the Facebook Messenger API. 

We also used a MySQL instance to store our curated list of cafés and we used Redis to cache information that needs to be persisted across multiple Actions, which also allows us to provide the convenience of Users not having to send their location twice.

Management of data was done using an open-source tool called Sequelize which allowed us to create migrations if we required new tables, and allowed us to create seed files which we could use to create a set of pre-defined data (cafés) on application deployment.

## Information Flow
Upon reception of a user interaction from Facebook, we sort the interaction into Postback/Message. Postbacks are pre-defined actions which we tag to Buttons when sending responses to users. Messages in this context refers to any interaction which is not a Postback.

Messages that are text are automatically routed to an Action that triggers a welcome message. Messages that are locations trigger a cache in our Redis. Depending on the Postback, we cache it in Redis too and depend on the previously triggered Postback to decide on the eventual Action on reception of a location message.

When we have a distance and a location, we trigger the search for a café and then return the café to the user.

## Code Structure
The Facebook Messenger wrapper we used implements a Messages handler and a Postback handler. We use these to categorise the type of user interaction. We map each Postback to an Action by a 'foreign-key'ish ID which was an intentional tight coupling (benefits explained in Testing).

Actions were implemented as micro-components which allows us to trigger Action redirects depending on context. One use case of this was when we implemented our 'Show Me Another' button that required us to re-route from that Action to the most recently called Action.

## Data Management
In our first iteration, we used Google Sheets to manage a list of curated cafés which we imported into our application during deployments.

Google Sheets had an 'Export as CSV' function which we used to save a CSV file locally, converting it into a JSON file thereafter, which was used as a seed file and refreshed the production data with every deploy. This allowed us to input information in a human-friendly way that let us enter data simultaneously (as opposed to a Git-tracked file which would've been inviting for merge conflicts)

## Testing
Having tightly coupled Postbacks and Actions and having Actions implemented as their own micro-components allowed us to test the Actions without going through the whole flow of the code. If the Actions ran, we can be sure that it came from the assigned Postback.

We used the Mocha framework together with Chai and Sinon to test all Actions. Sinon was used to stub utility functions to make sure the links between components were maintained as we developed new features.

We did not implement feature/acceptance tests as there was no easy way of doing so for chat bots as of time of publication.

## Deployment
The Facebook Messenger platform communicates with our server application via SSL, and the fastest way of setting up such a server was to use a service that provided SSL certificates out of the box. Additionally, the website URL was insignificant since Users would not be accessing a website in the case of chat bots.

Given such requirements, we chose Heroku to host our bot during our testing phase. Heroku provided a free `xxx.herokuapp.com` sub-domain with SSL included. Heroku also allowed for 18 hours of operations followed by 6 hours of mandatory downtime which we were okay with. During this phase, we used a NewRelic add-on to keep our application alive so that when our friends used it, the reply would come quicker.

When we were ready to release it to the public a few days ago, we upgraded our Heroku plan to a basic paid plan which allows our bot to run 24/7.

## User Interface
Chat bots have the advantage of having readily available user interfaces in the form of messaging platforms so not much work was done here. We utilised the standard Facebook Messaging API together with the accompanying Messenger elements.


# Quirks & Difficulties

## Data Creation & Import
As afore-mentioned, we used Google Sheets to facilitate data entry. One quirk we found was in the CSV export functionality of Google Sheets which occasionally created additional columns and crashed our application when attempting to convert blank columns into fields.

## Session Management
Late in the development stage when we were considering how to remember information a User has already sent us, we faced the difficulty of keeping track of the User's location seeing that we have a 'Show Me Another' button which we would have to use similar parameters to conduct another search.

We had to implement this section using a Redis data store which worked pretty well. 

## Information Presentation
The Facebook Messenger elements were restrictive. For example, swipable cards could not contain more than 80 characters in the title or subtitle, making it difficult for us to display more information - which could've been a good thing because it made us re-think how we could condense the information to only the most relevant stuff.

However, we also encountered awkward white space when we paired two cards in a carousel element where one card had an image and the next, which was intended to be textually informative), only had text. The character count limit created wasted screen real estate and we still think it's an eye-sore.

# Moving Forward
We're looking forward to watch how people use our bot (if at all), but it has been a really cool learning experience trying to build our own bot. Some technical questions we will be exploring in future are:

1. Creation of a staging/production environment to test the bot before it gets deployed
2. E2E testing of a chatbot application given no internet access
3. Migration to a conversational interface from a button flow
4. Merging of our curated list with Google results (possibly by always recommending from the curated list, but using Google on failure to find a curated café)

- - -

I hope this post was useful to those exploring chat bot technologies and/or those interested to know what goes on behind the scenes in chat bots.

Never stop making!
