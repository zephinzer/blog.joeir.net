---
layout: post
title: Conducting the Technical Interview
published: true
---

I've been conducting interviews for software engineers together with a couple of like-minded colleagues for some time now; and I thought it'd be nice to share my learnings and experiences from this journey of trying to be both a nice human, as well as a reliable gatekeeper for the team.

This piece that follows will try to provide some insights to an ever-improving attempt to answer the question ***"what does it take to conduct an awesome technical interview?"*** I'll be covering how the interview process for software engineers at my tribe and product team have evolved over the years, followed by some principles and methodologies which I've found to be constructive when on the interviewing panel.

The context of what follows will be within my experience interviewing software engineers at the Agile Consulting & Engineering (ACE) tribe, and eventually the [MyCareersFuture (MCF)](https://www.mycareersfuture.sg/) product team. We're all under [Government Digital Services (GDS) aka Hive](https://www.hive.gov.sg/), the "digital transformation" department of GovTech, which is where you should be applying to should you want in (my direct email also follows after the article 😊)

> This post is also published [on Medium at this URL](https://medium.com/@joeir/4a3697fe97b2?source=friends_link&sk=916c97f9cd30b02efcf80f2cf0ef6cfa)

And since I'm *kinda-sorta-ish* a fan of Simon Sinek, let's [start with why](https://en.m.wikipedia.org/wiki/Start_With_Why) I became interested in improving the interview process for engineers-



# 🤔 Starting with Why



> "In organisations, people are our greatest assets."

Wrong.

**People are not assets.** People are literally the organisation. And how do people get into an organisation? The interview, of course!

The interview process when done with intent presents opportunities for both the applicant as well as the organisation:

1. The interview is an applicant's only reliable signal as to what they're really signing up for beyond the foosball tables and free-flow bars
2. The interview is the organisation's best-attempt at making an informed decision

With recent shifts in perception of technical expertise from being a cost-centre to being an enabler, the interview is as important as the fancy perks organisations seem to offer these days to attract the best-of-the-best.

First impressions go both ways, and it'd be a pity if we had a unicorn sitting in front of us, and they were the ones saying no thanks. It'd also be a greater pity if we had that unicorn sitting in front of us, and we couldn't recognise them.

To me, it was the initial culture at GDS which enabled the larger organisation to be able to reach the level of capability and community standing we are at today (just ask any software guy from the 80s what they thought about IDA); and sustaining culture requires dedicated intent: intent to hire for a team that inspires and challenges each other, intent to hire not just to fill a headcount or seem full-strength on paper.

I want to work with awesome people, and being part of the interviews and giving it my best-shot was one way I believed would help to sustain the culture I was enamoured with.

TODO: image

And we begin with how it all started…



# 🐒 Evolution of The Technical Interview



## Episode I: The Algorithm Menace (circa 2016)


When I first came onboard, our interview process looked like something out of a 2010s tech-giant in Singapore. There was the pre-invitation dance with the hiring manager where I was like *"government \*cannot\* be cool,"* and he was like *"oh come see it for yourself!"*; so I did one fine Tuesday at 9:30AM and I was like *"oh indeed it is cool these days,"* and the rest is history.

*"Please set aside three hours for this assignment,"* came the e-mail which contained a link to HackerEarth and one of those algorithmic technical assessments.

This was followed by a face-to-face chat where I met more senior members from the team that I'd be working with, a live-programming session to make sure I could actually code, a team-lunch for assessing culture-fit, and finally the boss-stage interview with the hiring manager and the HR team.
Fortunately I was deemed worthy and joined in September of 2016.

This interview format worked back then when we were at about 30 people and not in any rush to scale up.

Problems were soon exposed when the need to scale came upon us.


## Episode II: Attack of the Applicants (Scaling up – circa '17-'18)


The GDS department started gaining exponential traction within the government with project requests coming in quicker than we could handle. We had to scale; and our hiring manager knew it. This resulted in many candidates being dropped into our Trello board "*for (y)our consideration*".

As we continued with the interviews as we'd always have, we observed three things that influenced us to want to make changes to the process:

1. **Being proficient at algorithms wasn't indicative of overall engineering talent.** While the algorithmic technical assessment was a good filter for technical and conceptual depth, it didn't assess for the kind of skills we needed on a daily basis which was more engineer-*ish*/systems-level type of work. We needed a more relevant way to assess how someone would fare doing what we do on a daily basis.
2. **More experienced candidates tend to be filtered out by algorithmic technical challenges.** As the team grew with the existing interview format, we noticed more junior-level developers and few seniors. This was perceived to be a problem because most brilliant and self-aware juniors seek growth - and there were few who had 'been there done that' to provide that guidance in our ranks. Also, who were we kidding if we were to claim that we needed these experienced people to know how to detect and reverse palindromes under data type constraints? We wanted to unlock capabilities that an experienced software engineer would expose the team to.
3. **The answer for the problem was already online and one candidate had copied most of it.** This became a problem because the candidates would stumble at the live-programming session after we had already invested a couple or more hours in them. And time wasn't on our side. We needed a methodology that would allow us to invest less time while providing a worthy challenge; and every algorithmic question that could possibly be discussed on coding sites was already out there.

One assessment I proposed and implemented (and which a few poor souls were subject to) was done in an attempt to change the strategy from testing to assessing. This came about in the form of a multi-page timed questionnaire that had questions across engineering fields that were applicable to us - stuff like how HTTP works, how backends/frontends interact, some trivia questions on JavaScript and Ruby (our only two languages back then) - with the objective of seeing which fields a candidate scored better in and assessing whether they would ***fit our team*** *vis-a-vis* ***be good enough***.

It was a disaster.

People read things at different speeds; people felt a need to score 100% in all fields and felt stressed when they couldn't. Candidates on the overall gave us feedback that it was anxiety-inducing. Not the kind of assessment that would prove effective in screening given that we weren't allowing people to demonstrate their best efforts under reasonable stress.


## Episode III: A Solution Awakens (circa '19)


After multiple other experiments like a room-booking system and a URL shortener (*spoiler: both didn't last very long*), we arrived at a palatable technical assessment that wasn't stressful for candidates, wasn't time consuming for us, involved more concepts we use on a daily basis, and which still retained the crux of an important aspect of algorithmic thinking.

Presenting, [our TODO-finder](https://gitlab.com/mycf.sg/challenges/dev-checkin):

What I personally like about this technical challenge is that it solves most - if not all - of the problems that surfaced as we scaled:

1. The challenge is more similar to work we do on an everyday basis and selects for expertise in areas that's relevant to us. It's more like a *fizz-buzz* for developers on a product team.
2. More experienced candidates who've forgotten all their CS algorithms would still be able to complete it since it relies on creative expression as much as technical expertise.
3. Assessment lies in the how and why, not in the what. This enables a spectrum of correctness over a graded one-dimensional score, removing the pressure to "get it right" and consequently the need to copy a "right" answer.

For me, point (3) was especially important because as with most problems we need to solve, there is no "right" answer. It's always about trade-offs and this allows for nice things like discussions about base assumptions made, system design based on past experiences, how/why code was structured in a certain way, why tests were designed this way *et cetera*  -  all of which are more useful to know than the thought process behind an algorithm that's probably been covered multiple times on "land that developer job"-type sites.



# 🚦 The Current Interview Process at MCF



Throughout the process of revising the technical assessment, we'd been more-or-less consistent with the end-to-end interview process, and here are the 4 stages of our interview process you'd go through if you're applying to MCF (*hint hint*):


## #1: Courtship


Candidates typically come in through our tribe lead, Steven Koh (he has [written a post on his processes](https://blog.gds-gov.tech/hire-the-best-tech-talent-in-singapore-part-1-cc37d1dac93d?gi=313c67b872bd)), recommendations from/connections with our colleagues, and sometimes via the HR department.

Interesting resumes are floated to a Trello board where hiring representatives from the various product teams typically filter based on their current needs. After an applicant is selected for, contact is made by any member of the team who's interested.


## #2: Technical Chops


For MCF, we use the afore-mentioned technical assessment. The team member initiating contact with the applicant confirms their desire to apply to us, and following which sends the link to them. Upon receiving the response, he/she will float it internally to the team to assess the quality of the assignment. Should it be of sufficient quality, the applicant gets invited to an onsite interview (virtual these days, but you get the idea).

> Note: The technical assessment segment differs from team to team depending on contextual and perceived needs so don't be surprised if you're applying to a different product team and the process doesn't seem familiar!


## #3: The Face-to-Face


Ah, the scary one with people.

Should an applicant reach this stage, we will arrange a 2–3 hour face-to-face interview with 3–4 working-level members of the team that ideally spans from junior to senior. This stage allows us to assess and be assessed by a candidate for culture-fit and primarily serves to introduce the team and its dynamics to them.

This is followed by a live-programming section where we work with applicants on their solution to the technical assessment. It's pretty much a creative process that involves exploration of the code architecture and design, and working with the candidate to change features/add new ones.


## #4: The Final Countdown


Should a candidate be passed by the working-level members of the team, a final round is arranged with the hiring manager/HR/product team lead together with 1–2 other working-level members who were not in the initial face-to-face interview. This stage further assesses the candidate's culture fit and tends to introduce the larger organisation, GovTech, to them.
Once a candidate is through, we wait for them to join us about a month later-


- - -


And that concludes the hard process. The subsequent sections focus on the intricacies of the interview and what I've personally experienced and learnt from being part of the interviewers' panel.



# ⚓️ Interview Objectives
As with any interview, the objective of our interview process is to enable both sides of the table to make an informed decision. The optimisations we can undertake then follows as a (non-)trivial constraint problem: **How can we spend as little time as possible to gain/provide as much data as possible so that we can make an informed decision?**



# 🔦 Interview Heuristics
When it comes to the culture-fit chat with candidates, the mental model I've found most constructive considers three aspects of a candidate:

1. Culture
2. Capability
3. Cause

While definitely not a strict three-bucket rule, being able to broadly categorise conversations into these aspects has helped me with identifying areas that have/have not been covered so I could influence the conversation to expose data.

Keeping track of data exposed by either applicant or interviewers also provides me with a way to reason about the *good vibes* (or otherwise) that I'm getting about the person in front of me.


## Culture


Culture can be said to be a collective and unspoken *modus operandi* in any organisation of humans. In the context of an interview, assessing for culture is similar to assessing a person's personality with the objective of making a best-guess effort at how someone would interact and grow with the existing team and how the team dynamics might change with his/her presence.

Some examples of questions/statements that expose data related to culture:

1. Tell us something you're passionate about.
2. What do you do during your free-time?
3. How do you think your ex-colleagues/project-mates would describe you?
4. A superior comes to you at 6pm telling you she needs something done by that day. How would you decide on what to do? (*Editor note: this doesn't happen at MCF, but it wouldn't hurt to know!*)
5. Assuming someone on the team tells you your recent code contributions suck. How would you address the situation? (*Editor note: again, just so you know, this doesn't happen at MCF*)


## Capability


I hear you say, "*but we already have a technical assessment?*"  Capability in the context of the non-technical segment of the interview refers to non-technical "*special powers*" a candidate might have that cannot be easily expressed on a resume.

These could be things such as level of self-awareness, clarity of communication, breadth of knowledge, cross-domain areas of knowledge, experience handling difficult situations/people *et cetera*.

Some examples of questions/statements that expose data related to capability:

1. How would you handle (*conjure a socially difficult situation up, like...*) two colleagues who couldn't agree on spaces VS tabs?
2. Could you explain the concept of (*for example only...*) "Public Key Infrastructure" like we were 5?
3. (*Compare two things, like...*) JavaScript VS Python, go.
4. Assuming you were a (*some role, like...*) solution architect, how would you begin designing an e-commerce system that (*introduce quirky business contraint, like...*) prides itself on having the "quickest delivery times"?


## Cause


Cause in a nutshell is best expressed as a question, "what keeps you awake at 3am?". Beyond doing a good job and getting paid well, I think it's important to know why an applicant is doing what they do.

Knowing why an applicant is in the game enables a better decision to be made by allowing the organisation to get a sensing of the duration a person will stick around in a particular role, what career needs they might have, what challenges we can provide them, and assess how that all ties in with our overall capability needs at that point in time.

Some examples of questions/statements that expose data related to cause:

1. What problems do you want to solve?
2. Why do you specifically want to join (*in our context...*) GovTech instead of other tech companies?
3. You're applying to be a (*the position, for example...*) DevOps engineer, how do you see yourself growing in this role?
4. Imagine it's 2050, what would a typical day in your life be like?



# 💡 Interview Principles



## Fear begets Obscurity

Interviews are anxiety-inducing by their inherent nature. There's a human in front of you who doesn't know anything about you at that point in time. Yet after an hour or two, they will need to decide whether you get to join the party. How obnoxious of them to think they know you!

Despite the injustice of it all, interviews are a necessary evil and probably the only way of attempting an informed decision on whether or not to bring a person on board. As interviewers, it is up to us to make the best of it with the objective of spending as little time as possible to gain as much data as possible, to make an informed decision.

Helping a candidate to feel at ease is one way to do so, and the trick lies in getting them to feel safe enough to be themselves.

Try to recall the last time you felt scared. Even if you're not the type to lie, it's likely that the desire to stay out of trouble would have caused the thought to cross your mind. Fear makes it conducive for people to lie/hide, so don't give the human in front of you a reason to fear.

In safety, we express more. In safety, we learn more about each other.

- - -

On implementing this:

1. **Shake the interview process up.** Anticipation and hope tends to lead to interviewers being imagined as firing squads. When an interview is "like an interview", this affirms the fear. Shake things up by introducing elements to the interview that applicants wouldn't expect and which would take their minds off the session being "an interview". One example I'll always remember is how Steven Koh engaged me in conversation about achieving parallel read-write accesses to relational databases over ping-pong (whut? ikr).
2. **Show vulnerability.** It's normal (even admirable) to want to show your best side to the interviewee. However, I've found that injecting conversations of how the team can fail and has failed (*c'mon, no team is perfect*), empowers the applicant and triggers off most engineers' problem-solving nature which take their minds off "the interview".
3. **Shift the conversation from answers to opinions.** I've found that phrasing questions in a way that requests for opinions over facts opens people up. Anyway, the point of the interview is to assess the candidate, not their knowledge. There's Google for cold hard knowledge.


## Culture > Capability || Cause


I quote a recent message my colleague sent to me while discussing soft skills in engineering which I felt was expressed in a pretty eloquent manner.

Describing one of his previous professional stints (might sound a little like bragging but I pinky-promise it's not, and that this is what was said),

> "MCF seems to be interested in getting the job done, whether the job is actual delivery feature or sorting out internal processes. XYZ however, feels like people are just trying to please whoever has authority. we have delivery managers accepting any deadline thrown at them without discussing with the team, we have tech leads choosing tech stack because the product owner wanted it. having bad culture doesnt [sic] just make it depressing to work but it actually makes work less effective"

What makes culture so important to get right is that it's based on the collective, perceived and unspoken *modus operandi* of the individuals in any group of humans.

Maintaining a constructive culture is a tough balancing act, with one unsavoury apple potentially spoiling the lot (whatever 'spoil' means in your context) either through direct or indirect interventions.

As an interviewer, I've found it's also important to note that what works for one team may not work for another and it's crucial that interviewers assess this before the interview based on their own context. The afore-mentioned description of the other team didn't work for my colleague, but it might have worked for others who enjoy the pressure/prefer to leave the decision of technologies to others.

There is no right or wrong in culture, only constructive to the objective, or otherwise.

- - -

On implementing this:

1. **Start conversation topics with culture-category questions.** Capabilities and cause can be exposed through continuing the conversation on how they achieved it and why they decided to pursue it.
2. **Get them to talk about themselves.** Assessing a person's personality is only possible when they feel safe to be who they truly are over who they are in interviews, and what better way to do that then getting candidates to talk about what they know best - themselves?
3. **Respond with kindness and acceptance.** A continuation from the previous point: our response as interviewers will decide whether or not a candidate feels comfortable with us. In anyways, we shouldn't be judging people without living their lives.


## Assess not Test


We begin with the distinction: the output of a test is a measurement; the output of an assessment is data.

Testing by its very nature of desiring a result/measurement, assumes that there exists a "best" (or "only") way resulting in applicants being graded on a single scale that "is ideal". But the truth is in a constantly evolving environment such as the one we are in today, no one truly knows a "best" or "only" or "ideal" way. Not even leaders of organisations.

Assessing on the other hand comes with the premise that the interviewer acknowledges that there exists variations and different dimensions that require interpretation within a larger context and objective. This leads to a process that exposes and collects information without interpretation which I argue is more constructive than processes derived with *testing* in mind.

While it's always easier to test rather than assess, it's much more constructive in interviews to assess; and especially so in our context of hiring for a product team that in my opinion requires a skilfully crafted level of diversity in terms of opinions and technical expertise in order to excel.

- - -

On implementing this:

1. **Be child-like curious.** Changing our mindset to one that's curious opens up the possibility for collaborative exploration which exposes data that will remain hidden if we choose to affirm instead (for example by asking something, and then going *"right?"*)
2. **Know your team.** In order to be able to interpret data collected, we need to know what we're looking for. What aspects of culture/capability/cause seems to be lacking in the team? This hints at the types of questions we should be asking to gain more data in specific aspects and this also means that especially so in teams whose value lies in their collective technical expertise, the HR department should try to avoid being the gatekeeper as far as company policy allows for it.



# 🧸 Improving Skills Related to Interviewing



So you're an engineer reading this and your forward-thinking HR department has tasked you to interview an applicant. Here are some ways that I found I could improve on myself during interviews:

1. **Ask better questions.** An answer cannot exist without a question, so it follows that the quality of your questions will impact the quality of your answers. Here's a nice piece on asking questions that I came across recently  - [ 7 ways leaders can ask better questions](https://marker.medium.com/7-ways-leaders-can-ask-better-questions-e26d3b2c0b73). Some easy rules that I've found to work in most contexts would be to choose to ask "how" and "why" over "what"/"who"/"when".
2. **Improving somatic expression.** Communication is only 7% verbal and 93% non-verbal. How safe someone feels to express themselves has much less to do with what was said, and more to do with how it was said. How do people perceive your accent? What does your body language say about you? Are your body language, emotions, and use of language congruent? In the area of somatics, there's no one dimensional scale that enables us to "become better". Becoming "better" at our somatic expression is **a matter of building range**. *Can I appear confident if it's constructive to be? Can I appear loving if it's constructive to be?*
3. **Letting go.** It's normal to want to feel in control of your environment. In interviews however, I've found that trying to maintain my control over my environment, resulted in me depriving control from the candidate. This often leads to a conversation that reaffirms my beliefs rather than enabling the candidate to express theirs. This tended to deprive me of data that would've been pivotal. Similar to the previous point on somatics, letting go is about knowing yourself. What emotions are in play – for you – when you desire to be in control of the environment. One excellent book I've read on this is [Emotional Agility by Susan David](https://www.goodreads.com/book/show/27209485-emotional-agility).



# 🌈 A Conclusion Because I Heard People Like Structure



Conducting an awesome technical interview involves much more than just 'having a chat with' or 'testing' an applicant. When done well, the technical interview should expose enough data for both the applicant as well as the organisation to know their answer.

Having a clear definite "yes" or "no" should ultimately be the objective, and an interview that enables either party to be completely sure of the other, is really what defines a good interview.



# 🔌 THAT Shameless Plug



## Interested to do #techforpublicgood?


We've been expanding as an organisation and there never seems to be enough people.

If you're gifted/talented/passionate about software development and wish to apply yourself to change how technology is being delivered in the public sector for public good, feel free to hit me up at [joseph_goh@tech.gov.sg](mailto:joseph_goh@tech.gov.sg).


## Acknowledgements


Shoutout to the partner-in-crime, Ryan Goh, and (more recently) Sze Ying,  Dickson Tan, and Wynn, for walking this journey with me and driving me to improve how things are with our technical assessments.

Also, thanks to Steven Koh who's provided sagely guidance over the years to the ACE Hiring (not firing) Squad.

> P.S. Thank you to Sze Ying, Dickson, and Wynn for being unpaid editors
