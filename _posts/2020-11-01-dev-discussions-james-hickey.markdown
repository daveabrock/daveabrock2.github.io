---
date: "2020-11-01"
title: "Dev Discussions - James Hickey"
tags: [dotnet-stacks, dev-discussions]
subtitle: We talk to James Hickey about his Coravel project and software design.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/hickey-card.png
---

This is the full interview from my discussion with James Hickey in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com)!
{: .box-note}

When designing a real production system—especially over the web—there's a lot to think about past coding features for stakeholders. For example, how do you manage events? What about scheduling tasks? What about your caching mechanism? A lot of these considerations make you wonder if they should be supplied out-of-the-box with ASP.NET Core.

James Hickey's [Coravel project is here to assist](https://github.com/jamesmh/coravel). Billed as a "near-zero config .NET Core library that makes task scheduling, caching, queuing, mailing, event broadcasting, and more a breeze," it ships with an impressive set of free features—and you can look at Coravel Pro for a professional admin panel, UI management, health metrics, and more.

I caught up with James to talk about his project, software design, and more.

(You can reach out to James Hickey [through Twitter](https://twitter.com/jamesmh_dev), browse to the [Coravel GitHub repo](https://github.com/jamesmh/coravel), and [check out his site](https://www.blog.jamesmichaelhickey.com/).)

![James Hickey]({{ site.url }}{{ site.baseurl }}/assets/img/james-hickey.jpg)

## What are you up to these days? What's your main gig like?

I'm currently working full-time as a senior .NET developer at a small SaaS company—this will be changing come the new year 😉.

I'm mostly working on building a mobile API and application using .NET Core and Ionic/Angular for the product. There's also a lot of refactoring involved—hurray for legacy technology!

I also do some contract stuff on the side.

## As the author of Refactoring TypeScript, what are your biggest tips on keeping a codebase healthy? 

* Simplicity rules the day. If you "feel" like your solution is getting complicated, then step back or take a 15-minute walk!
* Reuse is over-hyped. It leads to coupling. Coupling more often-than-not causes trouble down the road.
* Abstract I/O from other kinds of logic. Don't do direct database or file system access from the core logic in your system. Put that behind an interface.
* If you want to get good at designing code, then get good at knowing how to build unit tests that have minimal (or no) mocking/stubbing.
* Use automated quality analyzers to fail builds when code standards are violated.
* [Fix broken windows](https://blog.codinghorror.com/the-broken-window-theory/). 

## Let's talk about your Coravel project. I love your bold selling point: "Near-zero config .NET Core micro-framework that makes advanced application features like Task Scheduling, Caching, Queuing, Event Broadcasting, and more a breeze!" Can you walk through what caused you to pursue this project, how Coravel can help me, and how it works with existing .NET projects? 

Back in the day when I was building *.aspx* WebForms, I discovered the PHP framework Laravel and loved how it included all these additional features like queuing, task scheduling, and so on out of the box. When .NET Core was gaining traction, I found myself wanting some of the conveniences of Laravel but in .NET Core.

Coravel can help you get a new project up-and-running faster by including things like automated task scheduling, background task queuing, and so on out of the box without having to install extra infrastructure. It works with any .NET Core projects and is (supposed to be) super easy to install, configure and use.

## For scheduling and event management, I know a lot of folks like utilizing things like triggered Azure Functions and Service Bus and the like—is there an advantage to using Coravel over that?

One disadvantage is that Coravel doesn't (yet!) support distributed scheduling, queuing, and caching features. One advantage though, again, is you don't have to install any additional infrastructure. You can install a NuGet package, add a couple lines of code and you're good-to-go!

You can use Coravel and these other products together, too. For example, we often use a distributed service bus to integrate separate systems via async messaging. Coravel can support the internal integration within a system—like how you might emit domain events between aggregates. Doing this in-memory has certain advantages. Jimmy Bogard, for example, [has written about using in-memory domain event processing](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/). 

## I know by default, Coravel caching is in-memory (but has options to persist to a database). How does this compare to something like Redis?

Again, using Coravel you won't have to muck around with installing, configuring, and so on with that additional infrastructure. Redis is a fantastic product and I'm hoping to eventually offer providers for Coravel to hook into Redis—which could make using Redis caching, messaging, and so on even easier. I just need to find some time 😉.

## Do you view Coravel as an enhancement over the BCL, or filling in for some of its shortcomings? This is part of a broader thought about OSS's role within .NET Core.

I view Coravel as enhancements over the BCL. I wouldn't expect to see these features included in a barebones console application, for example. But when it comes to web applications, these are all features that you'll need pretty much out-of-the-gate if it's anything more than a demo application.

## I'm a fan of your "Loosely Coupled Show" that you run with Derek Comartin (sure beats the "Tightly Coupled Show")—a great spot to discuss software architecture and design. In such a fluid industry, are there any firm principles you are stubborn about?

Generally, I stick with using a feature folder approach to structuring any projects I work on. I've [written about this before](https://builtwithdot.net/blog/changing-how-your-code-is-organized-could-speed-development-from-weeks-to-days) and Derek [just wrote about it](https://codeopinion.com/organizing-code-by-feature-using-vertical-slices/). I'm a huge fan of CQRS and think it fits perfectly with the feature folders approach.

I've given a talk on these topics [at the Florida .NET meetup](https://www.youtube.com/watch?v=36G6-vwwpe8&feature=youtu.be) if anyone's interested in hearing more about it.

Otherwise, the tips I had for one of the other questions apply to this question too 👍.

## What is your one piece of programming advice?

Make sure you get lots of sleep and step outside once in a while. 😅