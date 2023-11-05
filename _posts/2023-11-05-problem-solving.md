---
layout: post
title: How I approach problems and solve them
date: 2023-11-05
---

This post is the rough process I follow when I approach a problem and how I solve it.

It's quite simple:
1. I define the **problem** in its **context**
2. I list **potential solutions** with their pros and cons
3. I make a **plan**
4. I **deliver the solution**

Let's pick an example:  
You start a job at a new company and you're asked to build a cash balance report (example completely made up).

## 1. Defining the problem in its context

### What?

I first ask myself what domain I am operating in.  
A cash balance report could mean many things.
Sometimes I know, if it's a domain I am familiar with, sometimes I am dont or am unsure and so I speak to:
* stakeholders
* colleagues
* the Internet
* ChatGPT

In any case, I don't hesitate to **ask the obvious**: what do you mean by cash balance? Report?

Words can mean different things to different people and are very context dependent.  
Making sure everyone is aligned is important.

For instance here, cash balance report could mean building a solution that pulls bank account balances every day and creates a pdf that can be downloaded. But maybe it means something else?


### Why? Who?

You've been asked to build a cash balance report which is a problem to solve for your stakeholders BUT it's not the "root problem": why do I actually need to build a cash balance report? what exact problem is it solving? who is it for?

Sometimes, we're approached with a problem that's actually a solution.  
Asking why until you answer the "root why" is important.


And so again:
**I speak to my stakeholders** to see whether what I've been asked to do really is the best thing we can do for the customer/company.  
If what they want is to be compliant and therefor need to store a monthly pdf cash balance then what I've been asked makes sense.  
But if what they want is to know when to move money between their bank accounts, we're not solving their problem to the best of our ability: we generate a report and then what? a human has to check it and then they have to do something and then... that's very inefficient. Maybe we could implement a notification system whereby if a balance goes under/over a threshold we notify the customers. 

So the better I understand the actual problem and its broader context, the better the solutions I'll propose will be.

> **The outcome of this phase should be a simple problem and context statement**.


## 2. Potential Solutions

Once the problem is well defined and the context is clear, it's time to look at solutions.

### How? When?

Every problem has a multitude of solutions and its context is what we'll determine the best solution right now.  
The main variables I look at are:
* Complexity
* Time
* Cost

In the case of the cash balance report we could imagine something like:

1. Manually build a report in Google Sheet and manually sent it every month  
If you have 2 customers, and it takes 30 mins every month, it could be a good first solution: low complexity, low time, low cost.  
If you already have a 1,000 customers then probably not.

2. Use a no-code solution  
Simple but potential security risk, compliance risk, etc...  
Depending on the context, it could be a good first solution.

3. Build an in-house pdf generation report and email sending solution  
The most expensive and complex solution.  
A good one if you're well established, has the resources and want total control over the report generation, email template, etc...  


For each solution I also give a super rough estimates.  
This helps the discussion with the stakeholders. They might dream of solution 3. but once the cost is factored in then 1. might be the chosen one.

This is also where I do all the engineering thinking: architecture diagram, prototyping.

> **The outcome of this phase is an agreement with the stakeholders on what solution(s) to implement**.


## 3. Planning

Once the favourite solution is decided, I go deeper into the details of planning.  
(Note that it's possible we decided to go for 1 then 2 and then 3 and want to deliver them incrementally over the next 6 months.)

At this stage what I do is:
* break down the work into small pieces
* capture potential unknowns (to prioritise these first)
* track everything in a tool like Jira or Linear
* give a better estimates of delivery

> **The outcome of this phase is a list of tasks to do with a timeline**.

## 4. Delivery

It's time to do the work.

Depending on the project it might just be 1 person or a whole team.  
So they will be more or less orchestrating/project management needed.  
I won't go into the details at this stage, this deserves its own blog post :)

> **The outcome of this phase is the problem solved :)**


## A few more practical tips

### Document

As I start working on a project, I write down as much as I can.  
A lot will end up in a "graveyard", but it helps me 1) clarify my thinking and 2) make sure I don't forget anything.  
In practice I have a living doc (Google Doc, Notion, Confluences, Notes) and a living Miro board where I can sketch things.

My living doc looks like this:

> 1- Why/Context?
> This is why I am doing this work.
> Here are some links.
> 
> 2- Proposed solutions  
> Solution A  
> Solution B
> 
> 3- Planning
> 
> Graveyard


## Visualise the flow

Whenever I solve a problem, I execute the solution in my head over and over again.  
I put myself in my clients shoes:
> I open the report page  
> I select the dates, the account  
> I click on this button, the report loads, etc...  

This could make me realise things like "should we put a limit on date selection?"

## Brainstorm

All along the way, I ask for help and speak to my colleagues to check I am not going somewhere crazy.  
I scheduled quick brainstorm sessions, architecture walkthroughs.  
I ask questions on public communication channels to make sure everyone see them and read them if they're interested.


And voilà.  
This is how I solve problems at the moment, maybe it'll change in the future, it'll certainly keep on improving and getting better :)