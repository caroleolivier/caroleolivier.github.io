---
layout: post
title: Thoughts about date times
date: 2019-11-10
---

Dealing with date times is one of the hardest problem a software engineer deals with.
<br/>
Many of us wish every place in the world used UTC: no timezone, no summer/winter time. It would be a better place haha.

I've come across a problem recently that made me think about what exactly happened when the clock changes.
<br/> This post is a collection of thoughts about date time changes.


## Observations

### Winter to Summer time in the UK

On 31 March 2019 at 1am in the UK the clock jumped to 2am.
<br/>Does it mean 31 March 2019 01:30am doesn't exist? 🤔

I live in London, if I had planned to meet somebody on March 31 01:30am London time, that would have been a problem!

It got me thinking, what happens in JavaScript when creating a [Date object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) around this date time.

WARNING: I live and run my code IN LONDON. And today is 10 November.
<br/>London timezone is GMT (Greenwich Median Time) during Winter which is equal to UTC
<br/>But it's BST (British Summery Time) during Summer which is 1 hour ahead of UTC: UTC+1.

This is what I get:
```
$ new Date(2019, 2, 31, 0, 30, 0):  2019-03-31T00:30:00.000Z
$ new Date(2019, 2, 31, 1, 30, 0):  2019-03-31T01:30:00.000Z
$ new Date(2019, 2, 31, 2, 30, 0):  2019-03-31T01:30:00.000Z
$ new Date(2019, 2, 31, 3, 30, 0):  2019-03-31T02:30:00.000Z
$ new Date(2019, 2, 31, 4, 30, 0):  2019-03-31T03:30:00.000Z
```

WARNING: Months are 0 indexed.

31 March 2019 00:30am is converted to 31 March 2019 00:30am UTC. It makes sense as at this time, the UK is still on GMT so follows UTC.
<br/>
31 March 2019 02:30am is converted to 31 March 2019 01:30am UTC. It makes sense as at this time, the UK is on BST, i.e. UTC+1.
<br/>
Now, as you can see, 31 March 2019 01:30am which actually doesn't exist since the clock jumps, doesn't fail and exist.
It's considered Winter time.
<br/>
Should NodeJs be smart enough and fail? Is this behaviour defined somewhere? I honestly don't know yet. To be continued... 🕵️‍


### Summer to Winter time in the UK

On 27 October 2019 at 2am in the UK the clock went backwards to 2am.
<br/>Wait, did we re-live the same hour twice? Does it mean 27 October 2019 01:30am happened twice? 🤔

Again, I live in London, if I had planned to meet somebody on March 27 01:30am London time, which 01:30am would that have meant?!

I also looked at what happens in JavaScript around this date time:

```
$ new Date(2019, 9, 27, 0, 30, 0):  2019-10-26T23:30:00.000Z
$ new Date(2019, 9, 27, 1, 30, 0):  2019-10-27T00:30:00.000Z
$ new Date(2019, 9, 27, 2, 30, 0):  2019-10-27T02:30:00.000Z
$ new Date(2019, 9, 27, 3, 30, 0):  2019-10-27T03:30:00.000Z
$ new Date(2019, 9, 27, 4, 30, 0):  2019-10-27T04:30:00.000Z
```

27 October 2019 00:30am is converted to 26 October 2019 23:30am UTC. It makes sense as at this time, the UK is still on BST, i.e. UTC+1.
<br/>
27 October 2019 02:30am is converted to 27 October 2019 02:30am UTC. It makes sense as at this time, the UK is back on GMT and follows UTC.
<br/>
Now, as you can see, 27 October 2019 01:30am is converted to 27 October 2019 00:30am UTC. It makes sense because the clock only changes at 2am *BST*. HOWEVER, after the clock has gone backwards, 27 October 2019 01:30am should then be converted to 27 October 2019 01:30am UTC.


If we just look at the UTC date times, it looks like there's a gap. There's nothing between 00:30am and 02:30am.
It's because when one says 27 October 01:30am London time, it's ambiguous! Without knowing whether one is talking about BST or GMT, one can't know what time is exactly referred to.
<br/>
Should nodeJs be smart enough and fail? Complaining it's ambiguous? I don't know either. To be continued... 🕵️‍

<br/>

So, I have a few things to think about and investigate.
<br/>What's for sure is that time zones and daylight savings are really hard.
<br/>But I'm also quite worried about what's going to happen when we get rid of it in Europe (deadline 2021). I'm worried about the legacy system with "hard-coded bug fixes" that cater for DST. I need to think about this more though!

