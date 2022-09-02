---
layout: post
title: Things to consider when setting up an on-call programme
date: 2022-09-02
---

An on-call programme is the team and process you set up for responding and resolving incidents.  
For instance your website goes down and stop customers from purchasing merchandise then what?

There's no magic formula and how you go about it is contextual.  
It'll depend on your product, company size, culture.

In this post, I've listed what's important to consider when thinking about on-call, it'll help you and future-me when I have to think about this again.


## Understand the impact of incidents

First, you need to understand the impact of incidents on your revenue and reputation.  
You want to know the impact on customers, your business and translate this in dollars or whatever your currency is.

Not all incidents are the same. I recommend listing the main scenarios and their impact like this:

| # | Scenario      | Impact | Estimated Impact ($)
| ----------- | ----------- | ----------- | ----------- |
| 1 | Website mymarket.co is fully down | Customer can't buy merchandise  | 10,000$ / hour
| |    | Customers end up phoning customer service   | 1,000 / hour
| 2 |  Newsletter sign up page down  | Customers can't sign up to newsletters   | 100$ / hour

It's not always easy to estimate the impact in $, for instance how do you translate reputation impact.  
But you should have a rough idea or look at collecting data to better understand this.

## Decide on Service Level Agreements (SLAs)

SLAs or Response Time define how quickly the on-caller responds to incidents when they're paged.

If your product is for businesses with the vast majority of your users working 9-5 weekdays, you might tolerate weekends and overnight downtime so your outside of hours SLA might be "respond by next working day".  
But if you're a market place and your site is down, you probably want a tight SLA like "respond within 10 minutes".

I recommend adding your minimum SLAs to the table you built above:


| # | Scenario      | Impact | Estimated Impact ($) | Minimum SLAs
| ----------- | ----------- | ----------- | ----------- | ----------- |
| 1 | Website mymarket.co is fully down | Customer can't buy merchandise  | 10,000$ / hour | Within 10 mins 24/7
| |    | Customers end up phoning customer service   | 1,000 / hour
| 2 |  Newsletter sign up page down  | Customers can't sign up to newsletter   | 100$ / hour | Within 1 hour during business hours


When thinking about SLAs, you need to remember 2 things: 1) revenue impact and 2) employee life impact:
* The looser your SLAs are, the larger your revenue impact could be.
* But the tighter your SLAs are, the more impact it'll have on the on-caller's life, how attractive your on-call programme is for engineers and how costly it could be to pay engineers.

Note that SLAs aren't set in stone and should evolve with your product.

Also note that when I say *you* here, I mean the person thinking about on-call and whoever is the decision maker regarding SLAs in your organisation. It might be your product owner, your stakeholder, your CPO, your CTO, CEO. Just make sure it's clear with everyone as you might think 1 hour is good enough but your CEO might not agree.

## Organise your on-call team

Once you have a good understanding of the main incident scenarios, their impact and you know the SLAs you want to have, it's time to build an on-call programme to reach those.

At the core of an on-call programme will be your on-call team. A team made of humans.  
Your main challenge will be to make your on-call programme attractive to engineers.  
And here are some of the variables at your disposal:

**1. Who is doing on-call?**  
It could be any engineers, all engineers, only volunteers, only seniors or engineers only hired for this.  

**2. When are engineers doing on-call?**  
Within working hours? Outside of workings hours?
Remember that not everyone is able to work outside of ~9 to 6 weekdays.  
We all have different lives, and working nights or weekends might not always be possible.

**3. What are engineers responsible for?**  
Any part of the product? Even if they're not familiar with it?  
If you decide to have everyone on-call, you might scare a lot of people away by doing this, not everyone is ready to jump on an incident on a product they know little about.  
There are ways around this like having good incident playbooks, but still, it can be very stressful to be woken in the middle of the night to fix a critical system one knows nothing about.

**4. How long are the on-call shift?**  
1/2 day? 1 day? 1 week? A mix?  
Weekly are easy to arrange but can be very long.

**5. Do you compensate for on-call? By how much?**  
You could decide not to pay for on-call and decide it's just part of the job.  
But it must be transparent when you hire engineers and you might exclude great people.  
If you do decide to compensate, then you want to weigh this against the cost of incidents.  
It's possible that having an on-call team end up being more expensive than accepting downtime.  
If you pay an engineer 500$ a week of on-call but incidents are usually 1,000$ and happen rarely, you're better off without an on-call team.

**6. Can you lower your SLAs?**  
If your SLAs are really strict, you may have to review them and lower them.  
A 10 mins SLA basically means the on-caller can't really leave their desk while 1 hour SLA will give them enough time to go for a short walk or do some shopping.  
So lower SLA will make your on-call programme more attractive.

## My experience in practice

I've experienced different types of on-call teams in different types of organisations: unpaid, paid, outsourced, centralised, loose SLA, strict SLA, "you build it, you run it, you support it", etc...  
I don't think I've seen the perfect programme. On-call will always be disruptive and never perfect, no matter what the company does.

It'll always be quite divisive as well among engineers.  
Some engineers will want to support their product and be able to do it (I am one of them, at least at the moment) but others won't want to by principles or won't be able to because of that thing called personal life.  

At the end of the day, it's important to remember why you're thinking about this problem: what should happen when there's an incident, what is the impact and what can you afford?  
Setting up an on-call team is definitely not easy, especially if on-call is not engrained from day 1 in your culture and into your hiring process.

So good luck!

