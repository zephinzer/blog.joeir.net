---
layout: post
title: "Makings Of A Chatbot"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

In reference to an earlier conceptual post titled Ingredients of a Chat Bot found [here](https://blog.joeir.net/ingredients-of-a-chat-bot) (or on [Medium](https://blog.gds-gov.tech/ingredients-of-a-chat-bot-cc846baeda9f#.fg5sd9dhd)), this post is a follow-up post due to requests I've since received on how to actually make a walkin' talkin' chat bot. This post will guide you through making your very own fully conversational chat bot on Messenger using a basic technology stack and is a write-up for my Brown Bag presentation on 24th February 2017 at Hive, Government Digital Services (the session is open only to public officers but hit me up if you'd like me to give the same presentation elsewhere!).

In summary, the following will be covered:

- - -
1. <a href="#chat-bot-stack">Our Chat Bot Stack</a>
1. <a href="#pre-setup">Pre-Setup</a>
1. <a href="#facebook-app-setup-part-i">Facebook App Setup (Part I)</a>
  1. <a href="#facebook-app-setup-i-create-app">Create a Page
  1. <a href="#facebook-app-setup-i-messenger-cap">Add Messenger Capabilities</a>
     1. <a href="#facebook-app-setup-i-messenger-cap-page">Create a Page</a>
     1. <a href="#facebook-app-setup-i-messenger-cap-select">Select A Page For Your Bot</a>
  1. <a href="#facebook-app-setup-i-tokens">Note Required Tokens</a>
  1. <a href="#facebook-app-setup-i-webhook">Initialise Webhook Configuration</a>
1. <a href="#api-ai-setup">API.AI Setup</a>
  1. <a href="#api-ai-setup-agent">Create an Agent</a>
  1. <a href="#api-ai-setup-domains">Setup Small Talk Domain</a>
  1. <a href="#api-ai-setup-required-tokens">Note Required Tokens</a>
1. <a href="#heroku-setup">Heroku Setup</a>
  1. <a href="#heroku-setup-create">Create A New App</a>
1. <a href="#code-setup">Code Setup</a>
  1. <a href="#code-setup-repository">Setting up our Repository</a>
     1. <a href="#code-setup-repository-clone-seeder">Clone our Seed Project</a>
     1. <a href="#code-setup-repository-reinit-seed">Re-initialize Seed Project</a>
     1. <a href="#code-setup-repository-integrate-heroku">Integrate Heroku</a>
  1. <a id="code-setup-included-files">Included Files</a>
     1. <a href="#code-setup-included-files-config">config.json</a>
     1. <a href="#code-setup-included-files-index">index.js</a>
         1. <a href="#code-setup-included-files-index-error-handling">Error Handling Code</a>
         1. <a href="#code-setup-included-files-index-message-handling">Message Handling Code</a>
  1. <a href="#code-setup-configuration">Configuration</a>
  1. <a href="#code-setup-deploy">Deploy</a>
1. <a href="#facebook-app-setup-part-ii">Facebook App Setup (Part II)</a>
  1. <a href="#facebook-app-setup-ii-callback-permissions">Callback URL & Permissions</a>
  1. <a href="#facebook-app-setup-ii-subscribe-page">Subscribe To Page</a>
1. <a href="#checking-it-works">Checking it Works</a>
1. <a href="#next-steps">Next Steps</a>
  1. <a href="#next-steps-api-ai-customisation">API.AI Customisation</a>
  1. <a href="#next-steps-code-customisation">Code Customisation</a>
  1. <a href="#next-steps-fbmessenger-chat-ui">Facebook Messenger Chat UI</a>
  1. <a href="#next-steps-bot-public">Making your bot Public</a>
- - -

Without further ado, I present...

<a id="chat-bot-stack"></a>
## Our Chat Bot Stack

The technology stack we will be using consists of:

1. **Facebook Messenger**
  - The user interface for our chat bot application.
2. **API.AI**
  - Used for processing of natural language from user input.
3. **Heroku**
  - Used for hosting our chatbot server online.
4. **Node & Node Package Manager (NPM)**
  - Used for our server and dependency management respectively.
5. **Git**
  - Used for revision control in order to send to Heroku.

<a id="pre-setup"></a>
## Pre-Setup

Before we begin, there are some software we need to make available on our local machine. Carry out the following 6 steps before continuing.

1. Download and install the **Heroku Command Line Interface (CLI)**.
  - [Download](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)
2. Download and install **Git**.
  - [Download](https://git-scm.com/downloads)
3. Download and install **Node (with NPM bundled)**.
  - [Download](https://nodejs.org/en/download/)
4. [*OPTIONAL BUT RECOMMENDED*] Download and install **a code editor**: my favourite is Visual Studio Code but you'll do fine with Notepad++ too.
  - [Download Visual Studio Code](https://code.visualstudio.com/Download)
  - [Download Notepad++](https://notepad-plus-plus.org/download/v7.3.html) (Windows only)
5. Sign up for a **Heroku account**.
  - [Sign Up](https://heroku.com)
6. Sign up for an **API.AI account**.
  - [Sign Up](https://api.ai/)

<a id="facebook-app-setup-part-i"></a>
## Facebook App Setup (Part I)

We proceed with setting up of our Facebook App which will be necessary for the display of our chat bot. Go to the [Facebook Developer Portal](https://developers.facebook.com/) and login with your Facebook account. 


<a id="facebook-app-setup-i-create-app"></a>
### Create a Facebook App


Click on '**My Apps**' on the top right and click the green button '**+ Add a New App**'.

![](/content/images/2017/01/fb-01.jpg)

Fill up the **Display Name** field with a name of your choice and the **Contact Email** field with your e-mail. 

Once you're in, you'll be seeing the dashboard for your Facebook App.


<a id="facebook-app-setup-i-messenger-cap"></a>
### Add Messenger Capabilities


Scroll down to Messenger and click on **Get Started**.

![](/content/images/2017/01/fb-02.jpg)


<a id="facebook-app-setup-i-messenger-cap-page"></a>
#### Create a Page


You may skip this step if you already own a page you wish to attach your bot to. Scroll down to **Token Generation** and click on the **Create a new Page** link.

![](/content/images/2017/01/fb-03.jpg)

Set up your new page with settings you'd like. You could make it into your personal page or a fictional page for the sake of a chat bot.

![](/content/images/2017/01/fb-04.jpg)

![](/content/images/2017/01/fb-05.jpg)

Set up your Page with settings you'd like (doesn't matter for creation of a chat bot. 

Once you're done, refresh the page with the **Token Generation** section and click on **Select a Page**.


<a id="facebook-app-setup-i-messenger-cap-select"></a>
#### Select A Page For Your Bot


Select the page you just created/have assigned for your bot and login to your own application.

![](/content/images/2017/01/fb-06.jpg)

![](/content/images/2017/01/fb-07.jpg)

Select OK and a new **Page Token** should have been generated for you.


<a id="facebook-app-setup-i-tokens"></a>
### Note Required Tokens


Copy the **Page Token** and paste this somewhere on your computer. This is your **Facebook Page Token** which will be used to link your chat bot to your Facebook Page.

Click on **Dashboard** inside the left side menu bar and click on **Show** beside the **App Secret** field. Copy this value and paste it somewhere safe. This is your **Facebook App Secret Token** and is used to authenticate your bot with Facebook's servers.

![](/content/images/2017/01/fb-09.jpg)


<a id="facebook-app-setup-i-webhook"></a>
### Initialise Webhook Configuration


Click on **Messenger** under the **Products** heading in the side menu on the left and scroll further below to the **Webhooks** section and click on **Setup Webhook**.

![](/content/images/2017/01/fb-08.jpg)

A dialog box titled **New Page Subscription** should have appeared. This is where we create a new tab to set up our other infrastructure that will provide us with the required **Callback URL** and **Verify token**.

The webhook is for letting Facebook know where your server is in order to send messages that users enter into Messenger, to your chat bot application for processing. So hold this page, create a new tab to proceed, and we'll continue on this section later in Part II.


<a id="api-ai-setup"></a>
## API.AI Setup


Go to [API.AI](https://api.ai), register an account by **Signing in with Google**, or **Login** and access your client dashboard.


<a id="api-ai-setup-agent"></a>
### Create an Agent


Get started by clicking on **CREATE AGENT**. Think of an Agent like a human with an agency - each chat bot has a purpose and is represented by an Agent.

![](/content/images/2017/01/apiai-01.jpg)

Name your agent, select if you'd like your agent to be *Public* or *Private*, write a *DESCRIPTION*, select the *DEFAULT TIME ZONE* and click on **SAVE** to create your agent. 

![](/content/images/2017/01/apiai-02.jpg)


<a id="api-ai-setup-domains"></a>
### Setup Small Talk Domain


After your Agent has been created, checkout the side-menu on the left and click on **Domains**. Boxes should appear in the main navigation panel with categories such as *Small Talk*, *Booking*, *Weather* *et cetera*. These are different general areas which API.AI has pre-programmed intelligence to generate responses to. For the purpose of our bot which should be able to hold a basic conversation, we select **Small Talk** by clicking on the toggle. Click on the **Small Talk** box now and enable **Fulfillment**. Notice the *0% Customized* at the top of the dialog box. You can click this to customise API.AI to send responses in your language, in the interest of brevity, we will skip this for now (prepare about an hour to do this if you wish to fully customise your bot another time)

![](/content/images/2017/01/apiai-03.jpg)


<a id="api-ai-setup-required-tokens"></a>
### Note Required Tokens


Close the dialog and click on the gear icon beside your Agent's name in the left side-menu. You should reach the settings page with a heading *API Keys*. Copy the **Client access token** and paste it somewhere safe. This is the **API.AI Client Token** that identifies your API.AI agent and will be used later in the code.


<a id="heroku-setup"></a>
## Heroku Setup


If you haven't, get to the [Heroku Website](https://www.heroku.com/) and click on the purple **Sign Up** button. Complete the registration process and you should be brought to the Dashboard page displaying your *Personal Apps*.


<a id="heroku-setup-create"></a>
### Create A New App


Click on the **New** button on the top right of the page and select **Create new app**. 

![](/content/images/2017/01/heroku-01.jpg)

Fill up your **App Name** and click on the big purple button **Create App**. Your App Dashboard should load. Notice the *Deployment* section and the default selected **Heroku Git**. 

![](/content/images/2017/01/heroku-02.jpg)

Time to get our hands dirty!


<a id="code-setup"></a>
## Code Setup


Let's get started with our favourite activity. 

We will be deploying our application via the **Heroku Command-Line-Interface** which you should have downloaded (but [if you haven't, click here to do so](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)).

Fire up your console (for Mac, go to Spotlight/Dashboard and search for Terminal, for Windows, get your Command Prompt up and running). The black screen with mono-spaced words shall be known as Terminal from now on regardless of platform.

<a id="code-setup-repository"></a>
### Setting up our Repository

We proceed by setting up our repository. Go to a location of your choice and make a new folder to store our chatbot.


<a id="code-setup-repository-clone-seeder"></a>
#### Clone our Seed Project


Navigate in the Terminal to your created folder and clone the `botseeder` repository from [this GitHub repository](https://github.com/zephinzer/botseeder) (disclaimer: this is one of my seeder projects). Note the trailing period at the end of the second command

```bash
$ # for OS X/Linux-based systems
$ cd /path/to/project/folder
$
$ # for Windows
$ cd "C:\<PATH TO PROJECT FOLDER>"
$
$ git clone https://github.com/zephinzer/botseeder.git .
```


<a id="code-setup-repository-reinit-seed"></a>
#### Re-initialize Seed Project


Re-initialize your git repo by deleting the `/.git` folder and setting it up again.

```bash
$ # for OS X/Linux-based systems
$ rm -rf .git
$
$ # for Windows
$ rmdir /s .git
$
$ git init
```


<a id="code-setup-repository-integrate-heroku"></a>
#### Integrate Heroku


Login with Heroku and configure the Git repository with Heroku's settings.

```bash
$ # you will be asked to authenticate with your email and password when logging in
$ heroku login
$ # tell Heroku to insert it's own remote address
$ heroku git:remote -a <heroku-app-name>
```

With some code ready for us to use, let's analyse what the code does. There are two files of importance in the repository right now, `config.json`, which contains the configuration details for your chat bot, and `index.js`, which contains the code required to run the chat bot server.


<a id="code-setup-included-files"></a>
### Included Files


<a id="code-setup-included-files-config"></a>
#### config.json


```json
{
	"APIAI_API_URL": "https://api.api.ai/v1",
	"APIAI_CLIENT_TOKEN": "",
	"FACEBOOK_APP_SECRET": "",
	"FACEBOOK_PAGE_TOKEN": "",
	"FACEBOOK_VERIFY_TOKEN": ""
}
```


<a id="code-setup-included-files-index"></a>
#### index.js


We start off with some dependencies followed by initialising of the bot by creating an instance of the MessengerBot object using values from `config.json`.

```javascript
const bot = new Bot({
	token: CONFIG.FACEBOOK_PAGE_TOKEN,
	verify: CONFIG.FACEBOOK_VERIFY_TOKEN,
	secret: CONFIG.FACEBOOK_APP_SECRET
});
```

Now that we have our `bot` instance, we define some utility functions, `displayError()` and `displayMessage()` for use in future code.


<a id="code-setup-included-files-index-error-handling"></a>
##### Error Handling Code


We define the error handling code of the bot which tells the bot application to print to the Terminal in the event of an error.

```javascript
bot.on('error', function(err) {
	displayError(err);
});
```


<a id="code-setup-included-files-index-message-handling"></a>
##### Message Handling Code


Next we define the successful use case code for our bot which retrieves the sender's Facebook Messenger ID and message, sends the message to API.AI, receives and parses the response from API.AI and sends the message back to the sender via their Facebook Messenger ID.

```javascript
bot.on('message', function(payload, reply) {
	/// fastidiously load required variables
	const query = payload.message.text;
	const senderId = payload.sender.id;

	bot.getProfile(senderId, function(error, profile) {
		if(error) {
			/// show the error and exit from this function
			return displayError(error);
		} else {
			/// show the user's profile on our console
			displayMessage(profile);
		}

		/// prepare variables for call to API.AI
		const lang = 'en';
		const sessionId = senderId;
		const postAddress = `${CONFIG.APIAI_API_URL}/query`;
		const authorization = `Bearer ${CONFIG.APIAI_CLIENT_TOKEN}`;
		const contentType = 'application/json; charset=utf-8';
		const version = { v: '20150910' };
		const postData = { query, lang, sessionId };

		/// actually call API.AI
		superagent.post(postAddress)
			.set('Authorization', authorization)
			.set('Content-Type', contentType)
			.query(version)
			.send(postData)
			.end((err, apiAiResponse) => {
				/// parse response from API.AI
				const result = apiAiResponse.body.result;
				const ourResponse = {
					text: result.fulfillment.speech
				};

				/// send human response to user
				reply(ourResponse, (err) => {
					return displayError(err);
				});
			});
	});
});
```

The last bit of code is where we create the server to begin receiving calls from Facebook.

```javascript
http.createServer(bot.middleware()).listen(process.env.PORT || 11807);
```


<a id="code-setup-configuration"></a>
### Configuration


Recall that we previously saved three keys:

- Facebook Page Token
- Facebook App Secret Token
- API.AI Client Token

Inside `config.json`, set `FACEBOOK_PAGE_TOKEN` to the **Facebook Page Token**, `FACEBOOK_APP_SECRET` to the **Facebook App Secret Token**, and `APIAI_CLIENT_TOKEN` to the **API.AI Client Token**. Set `FACEBOOK_VERIFY_TOKEN` to something you'd like to use to authenticate with Facebook - it can be of any length and any content, just make sure you remember it!


<a id="code-setup-deploy"></a>
### Deploy


We're done with setting up our code so let's deploy it to see if it works.

From the Terminal, run the following commands which adds your files to the Git repository and then pushes it up to Heroku.

```bash
$ git add .
$ git commit -am "Initial commit"
$ git push heroku master
```

A whole block of text should appear showing what is going on in Heroku and it should end off with the following message:

```
remote: -----> Launching...
remote:        Released v3
remote:        https://<HEROKU_APP_NAME>.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/<HEROKU_APP_NAME>.git
 * [new branch]      master -> master
```

Note the line with <span style="font-family:monospace">**&lt;HEROKU_APP_NAME&gt;.herokuapp.com**</span>. This is where your bot is now hosted. Let's continue by completing the Facebook App setup.


<a id="facebook-app-setup-part-ii"></a>
## Facebook App Setup (Part II)


We pick up where we left off at the screen where we were supposed to setup the Webhook.


<a id="facebook-app-setup-ii-callback-permissions"></a>
### Callback URL & Permissions

In the **Callback URL** section, set it to our Heroku app's URL with a `/webhook` prefix: <span style="font-family: monospace;">**&lt;HEROKU_APP_NAME&gt;.herokuapp.com/webhook**</span> (note the additional `/webhook` path!) followed the content of your `FACEBOOK_VERIFY_TOKEN` configuration, and check the ***messages***, ***messaging_postbacks*** and ***messaging_optins*** checkboxes.

![](/content/images/2017/01/fb-10-2.jpg)

Click on **Verify and Save**. You should experience a page refresh. Scroll down to the **Webhooks** section again and note the green *Complete* indicator.


<a id="facebook-app-setup-ii-subscribe-page"></a>
### Subscribe To Page


Click on **Select a Page** and select the page you created in Facebook Setup Part I and click **Subscribe**. This hooks the page up with your bot.

![](/content/images/2017/01/fb-12.jpg)

<a id="checking-it-works"></a>
## Checking it Works

Let's go talk to our chatbot! Click on [this link](https://www.facebook.com/bookmarks/pages) to access your pages and select the page you created for your bot. Scroll down to the **About** section (it's on the right side of the page) and click on the **Message Now** button.

![](/content/images/2017/01/fb-11.jpg)

Enter in a simple *hi* and congratulate yourself on successfully creating your first bot.

![](/content/images/2017/01/fb-13.jpg)

<a id="next-steps"></a>
## Next Steps

<a id="next-steps-api-ai-customisation"></a>
### API.AI Customisation

Now that you've got your bot up and online, it's time to play with **Intents** and **Entities** inside API.AI to setup specific conversation flows. 

<a id="next-steps-code-customisation"></a>
### Code Customisation

Along with customising your Intents and Entities in API.AI, you'll need to add in code to decipher them and respond accordingly.

For this tutorial, we have used the NPM package `messenger-bot` to interface with the Facebook Messenger API, and `superagent` to send AJAX calls to API.AI endpoints. You can find more documentation at [`messenger-bot`'s NPM page](https://www.npmjs.com/package/messenger-bot) and [`superagent`'s homepage](https://visionmedia.github.io/superagent/).

<a id="next-steps-fbmessenger-chat-ui"></a>
### Facebook Messenger Chat UI

Facebook Messenger has one of the best Chat UIs out there among others. I've personally tried Telegram, Slack and Skype and only Facebook Messenger has the additional UI features that make chat bots useful. A quick example is the ability to request for location and have your user send their location to you from Messenger.

More documentation can be found at the [Messenger Docs Portal](https://developers.facebook.com/docs/messenger-platform).

<a id="next-steps-bot-public"></a>
### Making your bot Public

For now your bot is available only to yourself but you can make it public so everyone can talk to it. You'll need to submit a Terms of Service document and a Privacy Policy document to the Facebook App via the Facebook Developer Portal and submit a request to Facebook to make it public.

- - -

I hope this hands-on tutorial has benefitted you and if it has, give me some hearts on Medium or share this article with your peers who might be interested.

Till next time, continue making stuff! Cheers-