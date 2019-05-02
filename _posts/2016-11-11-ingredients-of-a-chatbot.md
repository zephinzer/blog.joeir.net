---
layout: post
title: "Ingredients of a Chat Bot"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

We started with grunts while hunting, perfecting it into speech and carvings on rocks; then we invented paper and pen, allowing us to depict with ever-increasing precision and potential publication, then came the typewriters, computers and finally the internet — websites, conveying human intention with a precision hitherto unheard of, spreading information as far as cables could be laid. Now, enter the chat-bot, the latest trend in the pursuit of form following function, that no one has really figured out how to use effectively (well, being honest here).

You’ve definitely seen the primordial kind of chat-bots and tried to break them. Oh those joyous moments when you enter a cheeky statement to find that you could break something, asserting your superiority as a human intelligence to be reckoned with. Well, those days are over. It’s the revenge of the machines. Artificial intelligences are quickly taking over industries, adequate computing capabilities have reached the everyday man such that real-time is a norm, and graphic displays have been perfected to the point it can display pixels and colours you can’t see. Take a tiny pinch of each field’s advancements, and be welcomed to the start of the age of chat-bots.

So what goes into a chat-bot that you can’t easily assert your superiority over? That’s what I’ll attempt to provide an overview of in this article after plunging myself into creating one for a recent hackathon. I will cover what makes a chat-bot from the outside, move on to the ontology of chat-bots, introduce tools to help you create one yourself, and finally some thoughts on how chat-bots can break through our deeply entrenched habits of scrolling and clicking.

- - -

1. User Interface and Experience
2. Behind-the-Scenes
3. Utterances, Intents, Entities, Flow, Dialogs, 4. Actions and Domains
5. Designing for Human Interaction
6. AI-as-a-Service (AIaaS)
7. Chat-Bot Platforms & Hosts
8. Potential Uses in Business

- - -

*This post was first published by me on the Government Digital Services (Singapore) blog at https://blog.gds-gov.tech/ingredients-of-a-chat-bot-cc846baeda9f*

- - -

### User Interface and Experience
A Chat UX is exactly what you’d expect. An agency represented as a chat between you and a bot that delivers what you want by transforming your expected clicks and scrolls into a personalised chat.

<img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*R_wvRVcAJhMCkI0DlMuD6Q.png" alt="UX Bear" />
<div style="text-align:center;">
<small>UX Bear
</small>
</div>

You’ve definitely seen a few websites and apps implementing this, but in case you haven’t, here’s the link to UX Bear shown above. Apps-wise, a local (Singapore) ride-sharing company, SWAT, which is still in beta testing has implemented it, as well as the now famous BusUncle bot on Facebook. Globally, there’s also the Quartz news app.

<img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*5rsKWjP2VDD6xzEKYpgUmQ.png" alt="SWAT — designed and made in Singapore, now aren’t you proud?" />
<div style="text-align:center;">
<small>SWAT — designed and made in Singapore, now aren’t you proud?
</small>
</div>

<img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*M8UP3LWzGmrdt_yHVbd6Hg.png" alt="The Quartz News App" />
<div style="text-align:center;">
<small>Quartz News App
</small>
</div>

<img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*NHRQLars-w0xmKO1I44f_g.png" alt="BusUncle" />
<div style="text-align:center;">
<small>
BusUncle - another local chatbot that tells you the timing to the next bus in terms of things you can do
</small>
</div>

As you might have noticed, it’s not exactly like chats where you have to type everything — that would waste too much time. By integrating elements of more traditional internet media such as pre-defined text, buttons and input support elements (see SWAT’s location retrieval input component above), the chat becomes much friendlier to humans. Compared to a website where one scrolls and clicks, it’s a brand new experience in information presentation and the first where technology actually feels integrated with human experience.

### Behind-The-Scenes
Get ready to be surprised. UX Bear (shown above) is based off a WordPress site. Don’t believe me? Append a /wp-adminto the URL and see where that lands up. The inner-workings of a chat UI can be any kind of data store, and much of the eventual experience lies in how the data is presented. Instead of having one wall of text appear in front of you, the chat UI throws information at you in bite-sized chunks. Information retrieval as far as I can observe with Chrome’s Network Tab Inspector remains the same, drawing a huge chunk from a database. The only difference lies in the logic required to display the retrieved data. In the case of UX Bear, the data is presented slowly at line breaks, creating an impression of someone talking to you.

Admittedly, none of the sites/apps presented have a fully functional Say-What-You-Want (SWYW) interface, and I couldn’t find any production-ready examples. So what would go on behind the scenes in an app of the future that implements a full SWYW logic? Why, artificial intelligence of course!

### Utterances, Intents, Entities, Flow, Dialogs, Actions and Domains
That’s a mouthful, and here’s where things get funky. Traditionally, information is stored and retrieved from a data store via Structured Query Language (SQL). This information has a schema which is known as a model. You might have a User model, a Profile model, a Products model and a Shopping Cart model. These models have fields which separate the data like columns in an Excel Spreadsheet, and they relate to each other via human-defined keys. The input is a query, and the output is structured data.

Enter Natural Language Processing (NLP), a sub-field of AI. Your input is no longer a structured query, but a natural human language, and your output is now a logical (in a human way) response to your input. This happens by evolving the way information is stored and retrieved, and involves an additional component which replaces the SQL. When your query arrives in a natural human language, the AI’s job is to process it into a response you as a developer can take action on.

Since human queries typically lack structure and may come in many forms, here we define the first of our heuristics, human speech, as Utterances. Belittling, I know.

Intents are statistically derived from a set of training data, in other words, your data store has to store many variations of what humans may say (Utterances, urgh), and map it to an Intent. For example, the Utterances: ‘i want coke’, ‘give me a coke’ and ‘coke please’ can all be mapped to an Intent we shall arbitrarily call UserWantsCoke. The process of doing this is termed Training and involves you, getting real humans to ask for coke, and then telling the AI that these are all the possible manifestations of a certain UserWantsCoke Intent.

Entities are broad terms for categories of things. For example, ‘coke’ is a ‘drink’, similar to ‘mango juice’ or ‘essence of pomegranate’. Entities are an addition to Intents and represent universal specifics (named stuff) that a user may say. How many ways are there of saying ‘Coconut Juice’? Putting Intents and Entities together gives you a flow of logic where the input is a variation of ‘I want Coke’, which involves an Entity called Drink.

So how about ‘I want roti prata’? You define a new Intent, UserWantsFood, with the Entity being Food. Extrapolating in a similar fashion, Intents and Entities complement each other and allow for each other to be better understood by the AI and allows for a more accurate retrieval of information for you. Stringing together Intents form a Flow which represents the sequence of intentions a user may have while using your chat-bot.

Finally, Domains can be seen as the context in which we are applying a Flow and represents the over-arching scenario which we are conversing with a user in. For example, small talk belongs to a ‘Small Talk’ Domain, ordering food belongs to an ‘Ordering’ Domain, which can share similar flows with an analogous scenario such as ordering food, asking about the weather belongs to a ‘Weather’ Domain *et cetera*.

### Designing for Human Interaction
If you haven’t figured it out, chat-bots are all about designing the perfect human-computer experience. It’s not about passing the Turing Test and being ‘a human’, it’s about creating a more natural flow that integrates itself with how humans behave. So how would you design one?
Let’s pretend we’re a chat-bot and put ourselves in the scenario (Domain!) of a person walking into a shop to order a drink. She says ‘I’m thirsty’. We map her Utterance to a UserIsThirsty Intent. Being a chat-bot, we need to reply, and this reply, is called an Action. Our Action, would be ‘What would you like to drink?’. This combination of Intent, Utterance and Action in a self-contained cycle is termed a Dialog — we receive an Utterance, map it to an Intent and take Action on it until we perceive a change in Intent.

So our customer ponders for awhile and then says ‘Give me a Coke’. At this point, the previous Dialog is over because the intention changes from UserIsThristy to UserWantsCoke, effectively starting on a fresh Dialog. So with this new Intent, we might have another Action to determine another Entity, Temperature, so we take Action and ask, ‘Would you like it iced, cold, room temperature, or boiled’? Entities can be objects, and they can also be named properties of an object. Our customer thinks it’s a hot day, so she says ‘Iced’. At this point, our chat-bot’s duty is over since we have completed a Flow in the Domain of ‘Ordering’ — we have deciphered what the user wants. What just happened?

Instead of presenting a page with 100 different drinks, food and desserts a user may choose from, we began our interaction by deciphering her Intent, which was to order a drink (since she was thirsty and assuming she is logical). Through Utterances (human input) and Actions (bot response) within a Dialog (a collection of human input and bot responses), we resolve an Intent and/or Entity, and move along our Flow through exit points in the Dialog which changes when the bot’s perceived Intent changes, and initiates a further Dialog until our final exit point where we have all the information to deliver what the user wants.

<img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*DHO_F4hhjsVsbYEGbB6bog.png" />


<img src="https://d262ilb51hltx0.cloudfront.net/max/1600/1*J9c79CLhSwxeluEGRrQ_gg.png" />

Designing a chat involves observing how humans behave when trying to express desire, and begins with creating flows of events that represent every possible human interaction in your intended domain of activity. For example, in a restaurant, there would be a flow for getting seated, another for ordering food and drinks, another for calling for assistance from waiters/waitresses, et cetera. From these Flows, define the Utterances, Intents and Entities and Actions possible and create Dialogs from them.

### AI-as-a-Service (AIaaS)
If all of that doesn’t sound complicated, well, try developing such a mechanism from scratch. People will pay for your product as people have for these. Welcome to AI-as-a-Service, which provides a back-end for the above described mechanism so that you don’t have to start from scratch.

During the hackathon I participated in, we worked with [API.ai](https://api.ai/) and [LUIS](https://www.luis.ai/) (still in beta). API.ai provides a true conversational back-end with batteries included. Batteries included means it allows for everything described above, from Domains, to Utterances, Intents and Actions, to Entity mappings, designed from your intended Flow. LUIS on the other hand, takes a more purist approach by only parsing Utterances, Intents and Entities and lets you handle the Actions and Dialogs using code. Working with both services involves AJAX calls, with API.ai returning a very complete schema indicating Intent, Entity and Actions, and LUIS returning a less complete schema including only Intent and Entity. Both can be used to define your own Actions, but API.ai allows you to pre-program your intended Actions via clicking instead of through code.

Some other such services I found via a Google search are [chatbots.io](https://developer.pandorabots.com/), [orbit.ai](http://orbit.ai), and [wit.ai](https://wit.ai).

### Chat-Bot Platforms & Hosts
So you’ve got out your flow, put some sugar, spice and everything nice into a quirky conversation personality for your chat-bot, what do you do with it? You make it public of course. Unless you want your own UX Bear, you’ll probably be using publicly available platforms that will provide you an existing chat UI. Here are some of them that allow for bot functionality.

<img src="https://cdn-images-1.medium.com/max/1000/1*2e2V5obZvnLpr5CKlyjA9A.png" />
<div style="text-align:center">
<small>
<a href="https://web.telegram.org/" target="_blank">Telegram</a> — the original chat-bot friendly platform. Talk to the @Botfather!
</small></div>

<img src="https://cdn-images-1.medium.com/max/800/1*zopcnngtwBgi3VBv-cqHdQ.png" />
<div style="text-align:center">
<small>Facebook Messenger, create your bot from the <a href="https://developers.facebook.com/" target="_blank">Developers Portal</a> and link it up with your back-end.
</small></div>

<img src="https://cdn-images-1.medium.com/max/1000/1*k-3olE5hMPB7vCZFNdesCw.png" />
<div style="text-align:center">
<small>Needs no introduction. (Or if you do, this is <a href="https://slack.com/" target="_blank">Slack</a>)
</small></div>

<img src="https://cdn-images-1.medium.com/max/800/1*O5iRzANOEVbCJr-9nAW_6g.png" />
<div style="text-align:center">
<small>Not a fan, but hey, if you have business needs, here’s <a href="https://www.skype.com/" target="_blank">Skype</a>.
</small></div>

Also, Microsoft has been working on a decent project involving chat-bots that allows you to write your bot’s underlying logic once and deploy it on all platforms at once. Check out the [Microsoft BotBuilder framework](https://dev.botframework.com), or their [GitHub repo](https://github.com/Microsoft/BotBuilder) if you prefer looking at source codes.

### Potential Uses in Business
Chat-bots are new. Really new. So new that probably no one knows how to use it except as a ‘It’s new and I want it’ product, and I’m willing to bet we’ll see the rise of countless chat-bots that lay waste to gigabytes of bandwidth. However, I’m also of the opinion we’ll eventually get this new mechanism of human-computer interaction sorted out and enter an age where bots can interact with us intuitively through chat as well as other sensors in the Internet-of-Things sector.

For now, what potential uses can there be?

1. Chat-bots definitely bring a new experience in user interaction and one of it’s main benefits is engaging the user at the point of desire. I don’t know about you but when I’m thirsty, I don’t conclude that I immediately want an iced green tea with less sugar. My instinct says ‘drink something’, and because my behaviour can be mapped to a machine-understandable intent, the machine is able to capture my intentions and desires from the very start. How it engages me then, will be the key to my giving of monies in exchange for my object of desire (a drink in this context).

2. A chat-bot’s charm lies in it’s ability to make a user feel like they’re conversing with someone. The first step to acceptance and trust of any technology is making the user feel comfortable, and feeling comfortable is the first step to being able to make a decision. Following this train of thought, agencies that rely on customer interaction offer a breeding ground for service ideas that offer real value.

3. Putting together the consequences of the first and second point, being able to engage at the point of desire, as well as providing a human-relatable feel, chat-bots allow their makers to express agency in a subtler, friendlier manner and has potential to grow in the ‘influencer’ economy. Advertisements and ‘sponsored posts’ are frankly a turn-off because we recognise their agencies in trying to make us part ways with our money, however, an adequately amicable chat-bot can break through those mental boundaries and get users to be more agreeable to products and services which we would otherwise immediately say no to.

3. Instead of being presented with complex screens detailing data you cannot easily make sense of and that turns you off, chat-bots (or rather, the chat user experience, but let’s discuss them as a whole) can introduce complex concepts by bringing users through a series of bite-sized, understandable concepts. Something like how live lectures bring more value in easing the learning curve than their accompanying notes, and education is a sector in which chat-bots can be applied with real value by filling gaps between face-time with educators.

5. Know of any more? Ping me for a chat, and let’s see if we can get something going(:

---

In conclusion, chat-bots are an emerging technology with lots of room for growth. Alone, they offer moderate potential by introducing a fresh mechanism for human-computer interactions, but looking forward and taking into account advancements in sensor technologies within the IoT sector, it won’t be long before chat-bots become the de facto interface for interacting with machines that understand us and our behaviour better than even ourselves. If you’re an engineer in the software industry, I urge you to try it out, you never know what uses you may find for it(; keep building, keep inventing!

P.S. Like this article? Give me some hearts! Till next time!

P.P.S. Shoutout to Nina, Kavan Koh, Lim Zui Young and Kia Hwee who were with me on this journey at BotFest and without whom some of these thoughts might not have been.

Edited by Nina Ee.

- - -
*This post was first published by me on the Government Digital Services (Singapore) blog at https://blog.gds-gov.tech/ingredients-of-a-chat-bot-cc846baeda9f*
- - - 
