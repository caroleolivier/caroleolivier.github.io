---
layout: post
title: Accessibility week 2 - Sort code input
date: 2018-08-05
---

I am at the end of week 2 of my accessibility journey. This week I learnt quite a few things while working on a sort code input. In case you are not familiar with UK sort code, a sort code is a bank's identifier, it looks like this: 12-34-56.

### Fieldset and Legend for grouping controls

The sort code input I worked on was split into 3 boxes: one box for each pair of numbers. This is fine but it is important to keep the 3 inputs related, they are after all, part of the same input, i.e. the sort code.
<br />
One way to group inputs using semantic HTML is to use a `fieldset` as a container and a `legend` to indicate what the fieldset is for. You can find more information about fieldset and legend on [W3's website](https://www.w3.org/WAI/tutorials/forms/grouping/). It looks like this:

``` html
<fieldset>
    <legend>Sort Code</legend>
    <input type="number" id="firstTwoDigits">
    <input type="number" id="secondTwoDigits">
    <input type="number" id="thirdTwoDigits">
</fieldset>
```

I played a bit with `fieldset` and `legend` with Voice Over (the Mac screen reader) on Chrome. I must say I was disappointed by the whole experience, Voice Over on Chrome doesn't say anything about groups of elements when it encounters a `fieldset` and `legend`. It is better on Safari though. This is one quirk of screen readers, each screen reader has a favourite browser. Voice Over is better with Apple's own browser: Safari.


### Don't move focus automatically

One noticeable feature the sort code input had, was its ability to automatically move focus between input boxes. By this, I mean that entering the first two digits of the sort code into the first input field would automatically trigger a focus jump to the second input field (and same between second and third input field).

Many websites move focus automatically between input fields to make them user-friendly. I have seen this behaviour often for inputs such as dates when they are split into day, month and year.

It may be okay for many users but it can be disorientating for others. I have done some research and it doesn't look like there is a definite consensus on this from the community. I have found this article on [bbc.co.uk](https://www.bbc.co.uk/guidelines/futuremedia/accessibility/mobile/forms/managing-focus) that recommends avoiding it.
<br />
I personally don't like when focus is changed automatically: I either end up one box away from where I should be (because I use the tab key to jump to the next input but the focus was automatically moved so I didn't need to...). Or I want to double check what I have entered but my focus has moved and I have to come back.
<br />
So as a rule, I have decided to not change focus programmatically on user input.


### Watch out for duplicated ids

Screen readers rely quite heavily on the HTML `id` attribute (for instance to know about the connection between a label and an input box). If your tree contains more than one element with the same ids, the screen reader will read out all the related elements. So elements wrongly labelled with the same ids will mess up the information read by screen readers! I won't go into more details about this problem now, but it is important to remember. I will try to dedicate a week to labelling where this problem will be clearly highlighted.


### What's next?

Next week, I am not sure yet what I will be focusing (haha) on. There are so many things to learn! But I'll carry on picking simple things to look at and fix and learn by doing :)