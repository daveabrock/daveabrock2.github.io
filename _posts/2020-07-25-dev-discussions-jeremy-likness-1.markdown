---
date: "2020-07-25"
title: "Dev Discussions - Jeremy Likness (1 of 2)"
subtitle: We get to know Jeremy Likness, the senior PM of .NET Data at Microsoft.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/likness-1-card.png
---

This is the full interview from my discussion with Jeremy Likness in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away!
{: .box-note}

If you've worked on Azure or .NET for awhile—like Azure Functions and Entity Framework—you're probably familiar with Jeremy Likness, the senior PM for .NET data at Microsoft. He's spoken at several conferences (remember those?), [writes about various topics at his site](https://blog.jeremylikness.com/), is a familiar face on various .NET-related videos, and, as of late, can be seen in the Entity Framework community standups (and more!).

I caught up with Jeremy to talk about his path to software development and all that's going on with Entity Framework and .NET 5. After you get through this week's interview, I think we can all agree his path to Microsoft is both inspiring and absolutely crazy.

Jeremy was very generous with his time! Because there's so much to cover, I've split this into two different parts. This week, we get to know Jeremy. Next week, [we'll get into his work and discuss Entity Framework and .NET 5](https://daveabrock.com/2020/08/01/dev-discussions-jeremy-likness-2).

![Jeremy Likness]({{ site.url }}{{ site.baseurl }}/assets/img/jeremy-likness.jpg)

**Can you walk me through your path to Microsoft?**

My desire to be a programmer started when I was 7 years old back in 1981. At home and bored, I found a manual for our [TI-99/4A computer](https://en.wikipedia.org/wiki/Texas_Instruments_TI-99/4A) that ran at a blazing 3 MHz (nope, not GHz) with 16 kilobytes (nope, not a typo) of memory. I typed in a BASIC program, ran it, and was blown away. It felt like magic and I realized that this was something I could master.

> I pursued a Computer Science degree in college but ended up dropping out for mental health reasons. I believed what everyone told me: that without a degree I might as well give up on a career, so I focused on fast food, retail, hospitality, and even worked in a pool hall for a year.

My personal break came when I interviewed for a job at an insurance company and mentioned that I speak Spanish. They hired me as a Spanish-speaking claims representative. I was always competitive so I tried to close the most claims in the department, but the “green screen” software running on an AS/400 (now called iSeries) would often crash.

We’d call tech support, and they’d come over and basically break into a command line and restart the app. I decided to close the loop and fix it myself, and when IT figured out what I was doing, they called me up. **I expected to get disciplined but instead was offered a job in IT.** It was a night shift position mostly focused on reloading printer cartridges in refrigerator-sized printers, but that gave me time to study the language they used, RPG, and eventually earn a spot on the programming team.

I worked at various positions and had job titles ranging from Technical Team Lead to Director of IT. I even owned my own company for a few years. It was an online fitness business that I scaled up doing online coaching, delivering seminars and selling CDs and books.

I sold the company when I had the opportunity to join a startup focused on custom hotspot portals back in 2006. My first day there, the CEO took me to Ikea, bought a desk then dropped me off at a loft in downtown Atlanta. “Find a spot you like, assemble the desk and get to work.” The company later pivoted to mobile device management.

After five years of startup mode working 12-hour days and most weekends, I had to make a change. I left the company with nothing vested and they were acquired a few years later for $1.5 billion. I was very proud of the infrastructure and product I helped build to position them for the acquisition, but also extremely happy to have my health, time, and sanity back.

I went into consulting at a company named Wintellect that was entirely remote. I went from not being home for weeks at a time to being home all the time, and working over 80 hours a week to 40-hour weeks with bonuses if I had to work more. This strengthened my relationship with my wife and daughter and taught me the importance of balance. I was invited by another local Atlanta company to build their application consulting practice and joined as their director of application development. Over three years, I built up a multi-million dollar practice and had the opportunity to hire and mentor some amazing developers.

I was very happy in that role and regularly ignored recruiting emails, but one came in from Microsoft and I couldn’t ignore it. **I watched the positive cultural transformation under Satya Nadella over social media and just a week earlier had told my wife, “Some great things are happening at Microsoft.” The timing was perfect, so I thought, “It can’t hurt to try.”**

I replied and started a series of interviews. I waited a few weeks to receive the feedback that I interviewed well, but they were looking for someone with a lot more open source contributions. My work at the time was largely proprietary enterprise line of business apps. I decided it wasn’t meant to be and moved on… only to receive a call a week later about another role. I interviewed for that role and felt good about it, but didn’t hear back for several weeks. I was finally told that my interview went great but due to budget adjustments, the position was no longer open.

Clearly I was not meant to work at Microsoft, right? Then I received a third call about a new developer advocate role. The first call was all about why I thought I would be a good fit and why I was willing to leave a role as Director of Application Development to become an individual contributor. I explained it was always a dream of mine to work at Microsoft and that the role felt custom fit for me – after all, I was already advocating to raise awareness about our app dev services at my current company. “Great, we’ll schedule an interview.”

At the time I planned a trip to Italy with my wife and some close friends to celebrate our 20-year anniversary. Microsoft wanted two days to interview me. I reported directly to the CEO and knew it would be strange to request two extra days off at the last minute. I had one extra day after the trip to adjust to the time zone change, so I asked the recruiter if they could squeeze the interviews into a single day and make it happen on that specific day. To my surprise, they said, “Yes.”

> I flew back to Atlanta from Italy, helped my wife get to her shuttle to head home, then went back into the airport to fly to Seattle. I landed at midnight, was in my hotel by 1 am and back up at 5 am to start a day of interviews. It was the best decision I ever made because I landed the role! I was renewed for my 8th year as an MVP just 8 days before my start date on July 10th, 2017.

I wrote a [fun post spanning my career](https://blog.jeremylikness.com/blog/2016-02-28_30-years-of-hello-world/) and also [wrote a post about my recent transition to the PM role](https://blog.jeremylikness.com/blog/new-role-dotnet-data-pm/).

**As a developer at heart (your blog is called "Developer for Life", after all) what kind of projects have you been tinkering with?**

My first consulting projects at Wintellect involved XAML via WPF then Silverlight. I was a strong advocate of Silverlight because I had built some very complex web apps using JavaScript and managing them across browsers was a nightmare. It is a common misconception that JavaScript was “different”, when in reality it was the implementation of the HTML interface or DOM that caused us grief in the earlier days. 

Silverlight raised productivity orders of magnitude and XAML made it possible to work in parallel with designers. It was a game-changer and I dedicated time to learn it inside and out. Silverlight was an implicit promise to write C# code that could run anywhere, and for many reasons probably more political than technical, the promise was not fulfilled. 

I believe it has been realized with .NET Core and more specifically, Blazor. 

> I resisted Blazor when it came out due to the existence of mature web frameworks available like Angular, React, Vue.js and Svelte, but colleagues convinced me to give it a spin and I was blown away by two things: first, how productive I could be and produce so much in a short period of time. Second, how many existing packages work with it and run inside the browser “as is."

Want to build a Markdown engine? No worries, take your pick of libraries to pull in. For personal learning (and as a result, education for others) I’ve been building a lot of Blazor apps to explore different protocols, ways of interacting with data, application of the MVVM pattern and more. I’m working to publish some reference applications that show how to use Blazor with Entity Framework Core [and have published seven articles](https://blog.jeremylikness.com/series/blazor-and-ef-core/).

I am also diving into expressions. I [published a blog post](https://blog.jeremylikness.com/blog/dynamically-build-linq-expressions/) about parsing JSON and turning it into an expression tree to evaluate. I've also [written about the inverse](https://blog.jeremylikness.com/blog/look-behind-the-iqueryable-curtain/): parsing an `IQueryable` LINQ expression to pull out the various pieces. Imagine you have a queryable source that you send to a component that then applies ordering, filtering, and sorting. How do you inspect the result to verify how the query was manipulated?

My other projects are related to products I work with. I’m working on reference samples for using EF Core with Blazor, WPF, and WinForms. I’m also starting to explore big data and will be looking at projects that use .NET for Spark.

**What is your one piece of programming advice?**

After decades of building software my biggest piece of advice is that **your first step shouldn’t be to find the library or framework or tool, but to solve the problem.**

> Too often people add frameworks or tools or patterns because they’re recommended, rather than actually determining if they add value. Many times the overhead outweighs the benefit. I’m often told, “That solution isn’t right because you don’t have a business logic layer.” My response is, “So?” It’s not that I don’t see value in that layer for certain implementations, but that it’s not always necessary.

Have you worked on that project that forced an architecture so that one change involves updating five projects and the majority of them are just default “pass through” implementations? I am a fan of solving for the solution, and only then if you find some code is repeated, refactor. Don’t over-engineer or complicate. One of my favorite starting points for a solution is to consider, “What is the ideal way I’d like to code for this?”

For example, I am building a Blazor MVVM implementation. For testing the filtering and sorting that the view model applies, I would love to pass an `IQueryable<Entity>` then do something like this...

```csharp
Assert.That(query.CalledMethod(nameof(Enumerable.Take)).WithValue(5))
```

...to verify that the view model applied the right extensions for paging. I start with what is easy to read and understand, then work backwards to provide an API that satisfies it.

As another example, when you raise property change notifications, you often have dependencies. If I have `Quantity` and `CostPerItem`, then `TotalCost` changes whenever `Quantity` or `CostPerItem` does. 

I’d love to say ...

```csharp
TotalCost.DependsOn(CostPerItem).AndAlso(Quantity)
```

... so I start with that, then build the appropriate interfaces to make it work. That in my opinion leads to readable, maintainable code.

*You can follow Jeremy Likness [at his site](https://blog.jeremylikness.com/) and [on Twitter](https://twitter.com/jeremylikness).*
