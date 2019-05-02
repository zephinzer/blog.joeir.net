---
layout: post
title: "Joe's Project Setup SOP"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

This post is an opinionated documentation of my standard operating procedure (SOP) for embarking on a new project involving tech.

To put things in context, the SOP that follows has been used by me and proven good for creation of web-based products & services using the [Agile](http://agilemanifesto.org/) methodology. I have no stakes in (though I wish I did) the recommended third party platforms, and my focus is on *free*, or at minimum, *free for small teams* (1-5 people) software that works out-of-the-box. Note that if you're following it, you may not need every step depending on the scale of your project - think before you implement!

The steps herein will get you set up to a point where:

1. Multiple channels exist for various levels of team communication,
2. Code and data can move from development to deployment safely/securely & customers can access your site,
3. Search engines can pick up and link your sites together, and
4. You'll be all set to begin mass marketing efforts. 

Moving forward however, is your action to take. Here goes.

# 1. Mother Infrastructure

## 1.1 Central Source of Truth
Create a general [Gmail](https://gmail.com) account for use with all things related to project of interest.

# 2. Technical Infrastructure

## 2.1 Domain Name
Purchase domain name from [NameCheap](https://namecheap.com) (check for promo coupons [HERE](https://www.namecheap.com/promos/coupons.aspx) before registration).

## 2.2 Custom Domain Email
Set up email infrastructure via [Zoho Mail](https://zoho.com/mail) (create a referral from another account so we have 10 users instead of 5). Create at least one account with an address *webmaster@yourdomain.com*. Ping me for a referral.

## 2.3 Technical Communications
Set up communications infrastructure: [MailGun](https://www.mailgun.com/) if platform requires mail sending functionality, [Twilio](https://www.twilio.com/) if platform requires text message sending functionality.

## 2.4 A Home for your Code
### 2.4.1 BitBucket
[BitBucket](https://bitbucket.org) offers free private repos and comes with 5 free users which should be enough for project initilization. Comes with a free CI runner too.

### 2.4.2 GitHub
[GitHub](https://github.com/) offers free repos for open sourced projects. Otherwise, private repos start at $9/user which is still decent if you've got funding. Does not come with a free CI.

### 2.4.3 GitLab
Not for the faint hearted to set up, attempt only with a strong technical team. Set up a t2.medium instance on AWS or a 4GB droplet on DigitalOcean and install GitLab. Costs roughly $120 a year but you get unlimited repos, teams and a fully functional CI runner engine.

## 2.5 Continuous Integration
Set up continuous integration/delivery tools on [CodeShip](https://codeship.com/). Do a second commit and push to confirm build success.

## 2.6 Continous Delivery
Set up an account in [Heroku](https://heroku.com), link DNS records with Heroku and create both a Staging and Production version. Configure CodeShip to deploy to Heroku on build & test success. Do a third commit and push to confirm deployment success. Promote Staging to Production to confirm first release success.

## 2.7 Secure The Goods
If your application needs SSL support, set up new Droplet on [DigitalOcean](https://www.digitalocean.com/) and set up a CentOS server. Create a clone of Staging/Production by pulling from Heroku.

Set up an account at [StartSSL](https://www.startssl.com/), create a free certificate for your domain and link it up with Digital Ocean (or you could pay $22/month to Heroku to let them do it for you).


## 2.8 Quick & Dirty Database
Set up a [Firebase](https://firebase.google.com/) account for a quick & dirty back-end but consider switching to a relational database eventually.

## 2.9 Analytics
Set up a [Google Analytics](https://www.google.com/analytics/) property/account.

## 2.10 Facebook App
Create a [Facebook App](https://developers.facebook.com/) for your service. Facebook logins are one of the easiest to implement. Plus point, you get to collect emails.

# 3. Management Architecture

## 3.1. Document Repository
Set up [Google Drive](https://www.google.com/drive/) for document sharing.

## 3.2 Project Management
Set up project management tools if working in a group: [Trello](https://trello.com/) for small projects and/or kan-ban based models, [Pivotal Tracker](https://www.pivotaltracker.com/) for larger projects and/or scrum based models. Paid alternatives: [Basecamp](https://basecamp.com/). Search for the API key and pass it to Technical Architecture.

## 3.3 Internal Communications
Set up communication channels: [Slack](https://slack.com/) or [HipChat](https://www.hipchat.com/) or [Telegram](https://telegram.org/) or plain ol' email depending on size of team/scope of project.

## 3.4 Legal Affairs
For potential startups, do up founder contracts using tools like [LawCanvas](https://lawcanvas.com/) and [Dragon Law](https://dragonlaw.io/). Hint: Find your ways.

# 4. Go-To-Market Architecture
## 4.1. Mode of Payments
### 4.1.1 PayPal
Set up a [PayPal](https://www.paypal.com/sg/webapps/mpp/merchant) account to receive small monies, and...
### 4.1.2 Stripe
A [Stripe](https://stripe.com/) account for larger amounts. 

## 4.2 Marketing & Publicity
Set up the following platforms as needed depending on the type of content served:

### 4.2.1 Facebook
Almost always a must-have. Targets the general population. [Sign up](https://facebook.com)

### 4.2.2 Instagram
If you're targeting the younger population. [Sign up](https://instagram.com)

### 4.2.3 Medium
For demonstration of thought leadership. [Sign up](https://medium.com)

### 4.2.4 YouTube
For easy video-hosting. Use it to host unlisted videos for investors/beta-testers/in-app videos. [Sign up](https://www.youtube.com/)

### 4.2.5 Pinterest
For E-Commerce/Lifestyle projects. [Sign up](https://www.pinterest.com/).

### 4.2.6 Tumblr
For casual blogging purposes without the hassle of other platforms. [Sign up](https://tumblr.com)

### 4.2.7 SoundCloud
If you've got audio content. [Sign up](https://soundcloud.com).

### 4.2.8 Twitter
Getting irrelevant but useful for bite-sized information if that's what you need. Useful for American focus. [Sign up](https://twitter.com).

## 4.3 Advertising Capabilities
Set up advertising profiles on Facebook & Instagram (set it up once via Facebook).

## 4.4 Search Engine Optimisation
Set up a [Google Web Masters](https://www.google.com/webmasters) account and submit the Sitemap URL.

## 4.5 Investor Sourcing
For startup types: get company profile up on [Angel Co](https://angel.co/), [Tech in Asia](https://www.techinasia.com/) and [LinkedIn](https://www.linkedin.com/).

- - - 

Go forth and fail fast(;

**CHANGELOG**

21 May 2017 - reformatted
