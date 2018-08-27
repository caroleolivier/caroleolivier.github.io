---
layout: post
title: Accessibility week 4 - Title and input validation
date: 2018-08-27
---

Work has been mega busy so I had to miss a week of accessibility the week before last.

But last week was slightly better and although I haven't dedicated as much time as I'd like to, I still found the time to look a bit at title and alert. In this post, I'll describe what I have learnt.

### Title

First of all, by `title`, I mean the HTML title element:

> The HTML Title element `<title>` defines the document's title that is shown in a browser's title bar or a page's tab. It only contains text and tags within the element are ignored.
>
> [developer.mozilla](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title)

This is the title of my last blog post about Accessibility and React.

![HTML title example]({{ "/assets/blog/html_title.png" | absolute_url }}){:height="40%" width="40%"}

It always felt to me like a details to be honest. I never read or even look at it when I load a page. Although, thinking about it, it is super useful for navigation when multiple tabs are opened.

However, in terms of accessibility, it's really important. When a page is loaded this is the first thing that screen readers read out loud.
On the [developer.mozilla](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/title#Accessibility_concerns) page, they recommend to be super descriptive about the page. So do not neglect them!
<br/>
I even read somewhere people recommending using `title` as a way to convey alert messages. I am not really sure how it works yet to be honest, and whether the screen reader detects the change and reads out the new title. It feels a bit weird and not necessarily the right place, but we shall address this in the next section :)


### Input Validation

Originally, I was hoping to address the topic of alert on a web page (alert on forms, alert on input validation, random alerts). However, I quickly realised this is a complex topic and I have a loooooot to learn, I have barely scratched the surface!
<br/>
So, in this section I will share what I have learnt about input validation and alert.

This is the problem: how do you make an input accessible, particularly when it errors?

![Input validation with error]({{ "/assets/blog/input_validation.png" | absolute_url }}){:height="40%" width="40%"}

Here are a few things to consider:
* **Do not rely only on colours, combine it with visual indicator**
<br/> If you are colour blind you may not see it, but the text box border and the icon are red.
<br/> The warning icon helps indicating there is an error on the input.
* **Use simple and clear messaging to indicate the user there is an error**
* **For screen reader users, speak out loud!**
<br/> This was the new part to me and the one I am going to focus on here: it is called _alerting_.



#### Alerts and screen readers

Basically, when a user enters invalid data, we want to notify them. For users with good vision, we can use visual indicators like on the example above. However, for users who rely heavily on screen readers, visual indicators are not good enough. We need to give them vocal feedback. So how do we do this?


##### Alert window

Do not use alert window. There are **not** accessible (try it on your screen reader and you'll see. I tried VoiceOver on Safari and Chrome, it wasn't good!).

![Alert window]({{ "/assets/blog/alert_window.png" | absolute_url }}){:height="40%" width="40%"}


##### The role alert

One way to give immediate feedback to the user is to wrap your input into a `section` and give it the role `alert`. This indicates to the screen reader that the HTML element is a live region, i.e. will dynamically change and needs to be watched out. So when the input contains error and you display an error message, the screen reader will read out the error message, this is really cool!
<br/>
This the bit of HTML I added to make my input more accessible :)
```html
<section role="alert">
  <!-- the HTML with input and dynamically added error message -->
</section>
```

Rob Dodson (again!) made a really great video about `role="alert"` on [YouTube](https://www.youtube.com/watch?v=5lzAj1ahRSI&index=22&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&t=0s). I can only recommend it, it is just 10 minutes and incredibly informative, with very good examples.
<br/>
[developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_alert_role) also contains great information about the alert role.


#### Unanswered questions

As I thought about this problem, I was thinking about what would happen if my input was part of a bigger form and only validated on submission. If all the fields error and the screen readers read all errors one after the other, how confusing is it going to be? How should the error(s) be displayed? Each error underneath the field they are related to? Or all in one place? Should the focus be moved? Should the title of the page also contains the errors? What is the friendliest and most inclusive solution?
<br/>
I haven't answered this obviously and I haven't researched it yet. This is on my long list of things to do, but I am quite excited by the problem! Lot of thinking and learning on the horizon 🤔

<br/>

PS: about the emojis, I tested my screen reader on it, it doesn't indicate it's an emoji: 🤔 is read as _thinking face_, the name of the emoji. It is not ignored, so kind of accessible but is it really? Or am I excluding people by using emojis? I need to research the subject more.