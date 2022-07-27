---
layout: post
title: The cost of a code change
date: 2021-07-28
---

Each line of code a developer writes has a cost.
<br>And I don't think we (developers) are naturally good at estimating it.
<br>**So whenever we want to make a code change, we really need to ask ourselves whether it's worth it**.

A few notes:

* Being worth it
<br> It'll depend on the team, the time, the situation.
<br>I'll touch on this in a later post.
<br>Here I'll focus on the things that happen when one does a code change.

* Code change
<br>I refer to code change as anything that'll lead and follow a modification to your code base (e.g. a new feature, a refactor).

### Making a code change is similar to baking a cake 🍰

**There's more to a code change than writing code.**

Similarly to:

**There's more to baking a cake than mixing the ingredients.**


When you bake a cake, you first think about your cake. What type of cake you’re gonna bake, the ingredients you need.
<br>_Dev thinking about that refactoring work they’ve been wanting to do for a while_
<br>Then, you make your cake. You weigh flour, eggs, milk, you chop almonds, cut bananas.
<br>_Dev writing code_
<br>You learn how to use your new egg mixer, you read the manual, figure out what speed to use.
<br>_Dev learning a new tech/library_
<br>In-between you get interrupted to answer an urgent friend’s call.
<br>_Slack message, system alerts_
<br>Once done you go back to baking, spend time remembering where you left off.
<br>Then, you put it in the oven, check on it regularly, make sure it’s not burning.
<br>_Checking in after deployment_
<br>In the meantime, you clean up and do the dishes.
<br>_Follow up code change to clean some leftovers todos_
<br>Finally, you invite your friend to eat it.
<br>_Sprint review, demo to colleagues_
<br>You share your recipe and your tips for making it a success.
<br>Maybe you even write down the tweaks you made or the things you’ve learnt.
<br>_Write documentation_

It turns out writing code is (un?)surprisingly similar.
Even very trivial code changes go through a similar (although shorter) version of this.



### What's the hidden cost of a code change?

With this (great) baking analogy in mind, let's take a look at what's involved in making a code change.

#### 🤔 Thinking & brainstorming

The cost behind a code change starts with the person thinking about it.

New features are likely to come from data analysis, user research or an innovation project.
<br>Other changes like performance improvement or introducing a new library come from a dev thinking and researching the idea: they think, they read, they experiment, they chat to colleagues. Even a tiny change often involves these steps.

And this is not free.


#### 🤓 Learning

Next cost is learning.

It can be very small: for instance you read a part of the codebase you weren’t familiar with.
<br>Or it can be big: you need to get familiar with a new library or tech you’ve never used before.
<br>It’s very rare not to learn anything at all when making a code change.
<br>Copy change or config change might be the only time when you really don’t learn anything.
<br>And so you spend time up-skilling and learning.
<br>(That’s one of the beauties of our job. We never stop learning)

But, again, not free.


#### ✍🏾 Writing

Writing is the most obvious one.

But even when we talk about writing, we write more lines of code than strictly necessary.
<br>You write a new feature, you refactor code.
<br>But you also write tests: unit tests, integration tests, end-to-end tests. Since you want to make sure what you write is robust and won’t break in the future.
<br>You may also write documentation.
<br>Maybe you’ll also create new monitors/alerts to make sure all is working as expected.
<br>There are various bits that may need refactoring or updating.

Many code change, no matter how small, will have this overhead.

#### Review & Peer learning

That cost is super interesting. It involves other people.
<br>Some sort of lateral cost.

You're the one making the code change but it needs to be reviewed and understood by a minimum number of your teammates.
<br>They switch context, they read your code, they ask questions.
<br>Maybe they need to learn about this new tech or library you’ve introduced.
<br>It’s possible you’ve even set up a quick meeting to introduce them to this new thing you’re writing.
<br>This cost can be exponential.
<br>I've seen cases where a new cool library was introduced and we all had to learn how to use it, etc... and honestly for no valid reasons other than somebody decided it was cool ⛔


#### 👩‍⚕️ Future & Care

Once something is written and deployed 🎉, it's gonna need care.
<br>It may need maintenance.
<br>It’ll be read by many other devs.
<br>They may have to up-skill and learn that new library you’ve added.
<br>You may have introduced bugs that need fixing. Or things will have changed and will need to be adjusted.
<br>And finally one day a dev will have to decommission/migrate/delete your change.

Any code change has an ongoing cost.

### Conclusion

I believe it's our duty, as developers, to ask ourselves whether what we do, suggest, propose is really worth it for the team and the company.
<br>And not just for the present but for the future too.
<br>The code you write will be your legacy. It'll very likely outlive your time at the company. Make it count.
