---
title: "Stop Disabling the Submit Button"
date: 2022-07-16T15:55:38+0000
description: "When your form is invalid and the submit button is disabled, it's doing more harm than good."
tags:
  - a11y
  - forms
draft: true
---

Before I get into this, I want to make clear that, like all things in software development and in life, there are no hard rules. Everything depends on the context that you're working in. It may make sense for you to do the opposite of what I suggest. That's fine! My thoughts on this are shaped by my own experience for the specific applications I've worked on. As always, weigh the options in front of you and choose the best one for _your task_. All I ask is that you consider these points next time you feel compelled to disable a submit button.

Developing forms for the web is hard. You have to worry about:

- accessibility
- styling
- your choice of form validation method (whether to use JS or the native browser tools)
- maintenance costs
- how a user interacts with a field, and how that field interacts with other fields
- and the list goes on...

All of which are complex issues in their own right! However, there is one choice that I argue is almost always applicable: when your user hasn't filled out all fields or has entered invalid data, **leave the submit button enabled**.

Why?

# It's a bad user experience, especially for large forms

Consider that you're filling out a form on the web. You get through each field, entering what you think is correct. Then, you get to the end of the form. Great! Except... the submit button is disabled, and there's nothing telling you what you did wrong. Not great.

One way to mitigate this is to provide feedback as the user fills out the form (either on-blur or on-change). This will work to catch any mistakes they make _for the fields that interact with_, but not the fields they don't. Then, what if they missed a field? Yes, you can clearly indicate that certain fields are required, but that's more cognitive load on the user to search through your form to find empty required fields. These problems get exacerbated the larger your form is and the more inter-field validation rules that are in play.

Your users don't want to fill out forms, they want to do the thing your product allows them to do. Adding more cognitive load makes it harder to use and will either drive your users away or make them resent using your application.

# It makes your forms less accessible

Accessibility is important, regardless of what kind of application you're building. There is a spectrum for how important it is to your application. On the lower importance end, it allows you to reach more users and create a more equitable web. On the higher importance end, it prevents your organization from being the target of lawsuits. Where you fall in this spectrum depends on your application.

When you disable a button, it hides it from the browser's accessibility API. Several types of accessibility tools use these APIs to interact with web pages. If you disable the submit button, users will not be aware what they can even do with the form until the form is valid.

**TODO: provide example of how to check this in Firefox**

# It's not a meaningful safeguard against your application taking in bad data

For web applications, the recommended ["best practice"](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation) is to perform validation of user inputs in two places: in the browser (client-side) and on your backend (server-side). The client-side focuses on providing helpful messages in a user-friendly way, but can be circumvented easily. The server-side is meant to be the last line of defense before your data is processed (entered into a database, sent to a third-party, etc.), and is harder to circumvent because the developers control the it. If you are developing any type of web application, you _must_ do both.

If your application does both of those things, then disabling the submit button is not preventing anything: your client/server validations are. Additionally, since disabling happens on the client-side, it can still be circumvented via browser tools.

# It is a maintenance cost with no value added

Every line of code and every feature we add is a maintenance burden. We have to ensure (to the best of our ability) that new functionality not only works, but that it doesn't introduce regressions into our software. So, we must be selective about what features we include in our applications to strike the right balance between maintenance burden and value adds for our users. "Maintenance burden" is not a tangible thing; there's no unit of "maintenance" that we can optimize for. 

However, maintenance translates into something that _is_ tangible: time. It takes developers more time to add new features or fix bugs the more code they have to wade through. The more time it takes developers to add features, the more money being spent on their salaries _and_ potential lost income that the changes would have brought in.

How can we reduce the maintenance cost of our applications? By cutting out the things that provide little or no value. As I've argued in the previous sections, there is no value added for the end user if the submit button is disabled. If there's no value, don't do it! Without it, developers and product managers can focus on the functionality users come to their application for instead of addressing issues that serve no purpose.

# Caveats

I know the title doesn't qualify "Don't Disable the Submit Button" with "when the form is invalid", but that's a mouthful. There are perfectly acceptable use cases to disable submit buttons that fall outside the bounds I've talked about so far. One example that comes to mind is to indicate that the form has been submitted, but is waiting on a response. In this case, it's perfectly reasonable to disable the submit button in this case to prevent submissions from occurring multiple times. 

I can't think of any more caveats, but let me know on [Twitter](https://twitter.com/jfreedman0212) if you do!

# Resources

If you'd like to learn more about form design, I highly recommend Adam Silver's book [Form Design Patterns](https://formdesignpatterns.com/). It's not too long and chock-full of practical examples. It also had easy to follow code examples that are framework-agnostic. I was not sponsored by the book, I just really like it and it has informed some of my opinions on form design.
