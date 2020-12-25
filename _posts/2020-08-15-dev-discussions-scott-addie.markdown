---
date: "2020-08-15"
title: "Dev Discussions - Scott Addie"
subtitle: In the latest interview, we talk with Scott from Microsoft.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/addie-card.png
---

This is the full interview from my discussion with Scott Addie in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away!
{: .box-note}

Have you worked in ASP.NET Core? If so, you have surely come across Scott Addie, whether you know about it or not. For over three years, Scott has worked as a content developer at Microsoft—publishing documentation on the framework, APIs, ebooks, blog posts, and Learn modules, and more.

He's also been active in the developer community—both before and after joining Microsoft—and you can find him as a co-host on [The .NET Docs Show](https://dotnetdocs.dev/), a weekly Twitch stream.

I recently caught up with Scott to talk about his work in the community, his career, .NET 5, and what he's working on.

![Scott Addie]({{ site.url }}{{ site.baseurl }}/assets/img/keynote-scott.jpg)

**Walk us through your role and how you got there in your career.**

For over three years, I've been a content developer on the *.NET, Web Development, & Languages* team of the Developer Relations division. My primary responsibility is to produce technical content focused on teaching ASP.NET Core. I'm provided creative freedom in how to best accomplish that goal. Ultimately, the ability to effectively communicate complex ideas, in both verbal and written forms, is critical to my success in this role.

Content I've produced includes:

* Conceptual documentation on framework features
* API reference documentation
* eBooks
* Blog posts
* Interactive, browser-based courses
* Breakout sessions & workshops for conferences, code camps, & user groups
* Conference keynotes
* Webinars
* Live streams
* Podcasts

You'll find much of this content on Microsoft and third-party properties such as docs.microsoft.com, Channel 9, Learn, Learn TV, dev.to, Twitch, and YouTube.

### The journey to the mothership

I'm glad you asked this question. I promised myself I'd write about the journey at some point. Now I have an excuse. To understand how I got here, let's recount the transformative events. Many people helped me get there, so I want to give credit where it's due.

Rewind to the year 2013. I was building line-of-business apps in the financial services industry and was searching for ways to differentiate myself. .NET was a battle-tested, yet tired technology, but it allowed me to make a decent living. .NET was being used to build almost all new apps at the company. My employer was willing to cover the cost of a .NET certification, so I pursued it. Nine months of 2013 were spent tirelessly studying for and taking exams to earn my Microsoft Certified Solutions Developer (MCSD) certification.

One day while having lunch with some colleagues in the cafeteria, the witty banter turned to the topic of developer conferences. Conferences were foreign to me, but all the talk about Microsoft's TechEd event excited me. The prospects of networking opportunities with like-minded attendees and access to engineers building .NET and Visual Studio were too good to pass up. This opportunity would also help me better prepare for the MCSD exams. I returned to my desk and told my manager I wanted to discuss TechEd at our upcoming 1-on-1. After several hours of persuasion, my manager approved the projected expenses and agreed to send me to TechEd 2013 in New Orleans. Shout out to Ruth Hands, my manager at the time. Ruth continually went to bat for me. She was supportive of my ideas and was an accelerant for my career.

On the flight to New Orleans later that year, I studied the session schedule and noticed countless "developer celebrities". I built a schedule of sessions to attend that week. While at the conference, I attended breakout sessions from John Papa, Daniel Roth, Julie Lerman, Rowan Miller, and many others. I also attended an "Ask the Experts" session facilitated by Julie Lerman on the topic of Entity Framework. The conference's content was amazing! After a week of breakout sessions, I felt more prepared than ever for my next MCSD exam. I was mesmerized by the presenters' skill in delivering technical content to such a large and diverse audience. Conference speaking is what I wanted to do one day.

![TechEd 2013 badge]({{ site.url }}{{ site.baseurl }}/assets/img/teched-2013-badge.jpg)

Skip ahead to February 2014. I drove to the local testing center and completed the last of three exams for the MCSD certification. I passed! Equipped with a plethora of fresh .NET knowledge from the certification, I was inspired to share what I learned. With the birth of my daughter the following month, I became a father! It was a relief to earn my certification just weeks before her birth. The next couple of months would be challenging as I navigated parenting and struggled through sleep deprivation. After spending a few days with my daughter, there was a strong desire to kick my career into high gear to support her. I began to ponder what other changes I could make to differentiate myself.

It was early January 2015. A friend at work, named Matt Frye, stopped by my desk unannounced (as he always did). Matt mentioned a local .NET user group called *MADdotNET*. Matt attended some meetings in the past and encouraged me to attend. The January meeting's topic was KnockoutJS. John Papa had inspired me to use KnockoutJS in his TechEd 2013 session. January marked my first user group meeting. I was hooked. It was after that meeting that I volunteered to give a talk at *MADdotNET*. April 1 was the first available date. I figured the attendance would be low for two reasons: I was new to the speaking circuit and was speaking on April Fools' Day.

34 people were in attendance for [my talk about using JavaScript task runners with ASP.NET MVC](https://www.meetup.com/MADdotNET/events/219252815/). To say I was nervous would be an understatement. I felt sick to my stomach, and I had absolutely no idea what I was doing. The feedback at the end of the meeting was encouraging. Several folks in the audience came up afterwards and personally thanked me for teaching them something new. The organizer, Lance Larsen, also provided much needed positive reinforcement and invited me back to speak again. To keep the momentum alive, I volunteered to deliver the same talk at another local user group. This group was a bit smaller, focused on mobile development, and was run by Matt Soucoup and Matt Snyder. Through Matt Soucoup, I met other passionate .NET community members, including Mitch Muenster, who later became a Xamarin MVP.

I caught the conference bug in 2013 and the speaking bug in 2015. If you've caught either one, then you already understand that they're highly addictive substances. For the next couple years, I actively participated in both. A highlight from the early speaking years included meeting folks like Ashley Grant, Cecil Phillip, Jeremy Likness, and Rachel Appel at South Florida Code Camp. Another highlight was getting interviewed by Seth Juarez on the main stage at That Conference 2016:

![That Conference 2016 interview with Seth]({{ site.url }}{{ site.baseurl }}/assets/img/that-conference-2016.png)

While attending That Conference 2016, I stopped by an open spaces session about authoring for Pluralsight. Course authoring was another avenue I wanted to explore. I met David Berry, an accomplished Pluralsight author from the area, and Adam Mumma, a Pluralsight acquisitions editor. As an aside, David now works on another content team at Microsoft. After chatting with both David and Adam, I decided to audition for Pluralsight. After spending two weeks creating my demo, I decided this opportunity wasn't for me. Talking into a microphone with no audience didn't give me the adrenaline rush that's inherent to conference speaking. I called it quits after giving it a fair shot.

My speaking progression took me from user groups to code camps to regional conferences. Through attending conferences and writing talks, I discovered some indispensable tools: Twitter and GitHub. At the time, Twitter was solely a tool for me to follow industry luminaries. In fact, I created a Twitter account in 2013 to stay in touch with folks I'd met and seen speak at TechEd 2013. GitHub became the tool I used for uploading slide decks for my talks and sharing with attendees.

In the beta releases of ASP.NET 5 (the precursor to ASP.NET Core), I invested some time reading the Microsoft docs to better understand ASP.NET 5. I wanted to build conference talks on the topic and introduce it to my team at work once the product had reached general availability. It was during this period of learning that I delivered a talk at the inaugural MKE DOT NET conference. MKE DOT NET was organized by a small team of people at Centare, including Steve Hicks. Steve was a kickball teammate, mentor in my first job out of college, and introduced me to David Pine. As yet another aside, this David is also now a member of my team at Microsoft. You could say I'm partially responsible for the `DavidOverflowException` at Microsoft.

As my knowledge of the ASP.NET 5 framework grew, I discovered gaps in the documentation and project templates. I wanted to address them to validate my understanding. A small documentation contribution, to what was then called ASP.NET 5, was my debut in open-source contributions on GitHub. I started a blog to share any information that didn't belong in the official documentation. Over the course of several months, I became a top contributor to the ASP.NET 5 documentation repository on GitHub. The .NET Foundation expressed their gratitude for my docs contributions with a care package, hand-written note, and challenge coin:

![.NET Foundation thank you note]({{ site.url }}{{ site.baseurl }}/assets/img/dotnet-foundation-note.jpg)

My blogging and involvement on GitHub led me to collaborating with brilliant folks all over the world. Mads Kristensen read a blog post I wrote about Webpack and contacted me about collaborating on a Visual Studio extension called *Webpack Task Runner*. We spent some time working on that extension and a few others, the most popular of which was *npm Task Runner*. While building a talk on ASP.NET 5 with Visual Studio Code, I discovered the Yeoman generator for ASP.NET 5 templates. My curiosity of this tool led me to collaborating on GitHub with Sayed Hashimi, Shayne Boyer, and Piotr Blazejewicz. At the time, I was just learning Git. I began submitting pull requests and made myself look like a fool a couple of times. They provided coaching to get me up-to-speed. Over the course of a few weeks working with them, I learned a ton and started to feel more confident with Git.

There was another time when I was working with Mads Kristensen on the JSON schema store project. While hacking on a JSON schema, I discovered a bug in Visual Studio's JSON editor. I reported it to Mads, and he introduced me to Mike Lorbetske. Mike was an engineer on the `dotnet new` templating experience and worked with Mads on the JSON editor in Visual Studio. Mike arranged a call to discuss the defect in greater detail. At some point in the call, we stopped "talking shop", and he asked "Have you ever considered coming to work for Microsoft?". I hadn't considered myself Microsoft material at that point, so I didn't have a great answer. Mike's question got me thinking. It was time to explore that idea further and to make more intentional career decisions moving forward.

Through my developer community contributions, I was nominated for the Microsoft MVP program. And 2016 marked my first year as an MVP! Others in the program told me that the biggest perk of the MVP program is the annual MVP Summit on the Redmond campus. Based on this feedback, I made it a point to attend. At the 2016 MVP Summit, I met up with Mads Kristensen. We debated the best approach for adding Yarn support to the *npm Task Runner* extension. I would later implement that Yarn support on the flight home from the summit. I ran into Mike Lorbetske in between sessions, who introduced me to his teammate Sean Peters. By the way, they're both Wisconsinites and know Steve Hicks. What a small world!

I received an email from Martin Woodward asking if I planned to be in a certain room that afternoon at that summit. No context was provided. I simply replied, "yes". When that time came, I took a seat in the session. Thousands of other MVPs were packed into the room, and impostor syndrome hit me hard. I was surrounded by industry veterans I had looked up to for years. The session kicked off, and Scott Hanselman took the stage. The following slide was displayed:

![MVP Summit 2016]({{ site.url }}{{ site.baseurl }}/assets/img/mvp-summit-2016.jpg)

I was blown away when Scott recognized me as one of five most significant contributors. I didn't feel as though my contributions were that significant, but it felt amazing to get some recognition. That recognition made me push myself even harder, which led to my renewal in the MVP program for another year. More community contributions were made, and more networking with Microsoft employees took place.

Between 2013 and 2017, I shifted from a silent lurker to an active participant on Twitter. I also became more involved in developer community groups on LinkedIn. And in April 2017, I received two messages that would forever change my career. A Twitter and a LinkedIn message came from Rick Anderson and Scott Cate, respectively, about jobs at Microsoft. After a few phone calls, I scheduled a full day of interviews on the Redmond campus. I wasn't totally sure what I was getting myself into. I was, however, certain that I enjoyed developer relations work. Continuing to fund my conference travel out of my own pocket and spending nights and weekends writing talks was unsustainable. Why not pursue a job that allows me to do this stuff?

On the campus, I met with folks from all over the Developer Relations division (then called APEX). My last interview was to take place with Jeff Sandquist in his office. I arrived at Jeff's office, where I found him chatting with Seth Juarez. Seth recognized me from the That Conference 2016 interview and gave me a high five. It felt good to see another familiar face.

Skip ahead a week or so to Build 2017, and I received a call from the Microsoft recruiter with an offer to join my current team. I accepted the offer after confirming that the talent acquisition team had the right Scott.

**What do you miss most about no conferences? Can any of what you miss be replaced remotely?**

The "hallway track" is what I miss the most. Many conferences have time slots during which there are no topics of interest. The hallway track is the undocumented, unconference meeting to fill that void. Friendships are forged, new ideas are exchanged, career opportunities arise, and problems are solved. Some of my most actionable customer feedback was harvested in this track.

Unfortunately, the hallway track can't be replaced in a virtual setting. There's something about attendees being in the same physical location and practicing the [Pac-Man Rule](https://www.ericholscher.com/blog/2017/aug/2/pacman-rule-conferences/) that makes this exciting. This pandemic era brings countless distractions at home that can degrade the quality of the conversations that occur in a virtual format.

David Pine, Cam Soper, and myself launched a [Twitch stream](https://dotnetdocs.dev/) in February of this year to fill some of the void. In each episode, there's a "Hallway Track" segment in which we have an informal discussion with a guest. Live viewers can come and go and actively participate in the Twitch chat.

**What kind of projects are you hacking away at now?**

Bill Wagner, Maxime Rouiller, and myself have been collaborating on a tool that generates "What's New" pages for conceptual docsets hosted on *docs.microsoft.com.* It's a .NET Core console app written in C# that uses the GitHub GraphQL API to process pull requests. I plan to distribute the tool internally as a .NET Core global tool. The idea is to provide customers with a summary of what changed in a particular product's documentation during a specific time period. For example, the ASP.NET Core documentation publishes "What's New" pages on a monthly cadence. At the time of writing, the concept has been adopted for the following products:

* [.NET](https://docs.microsoft.com/dotnet/whats-new)
* [ASP.NET Core](https://docs.microsoft.com/aspnet/core/whats-new)
* [Azure DevOps](https://docs.microsoft.com/azure/devops/release-notes/docswhatsnew)
* [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/whats-new-docs)
* [Xamarin](https://docs.microsoft.com/xamarin/whats-new)

I'm actively working with a handful of other teams, including C++, Visual Studio, and SQL, to roll out these pages. There's a sizeable features backlog for this tool. Some feature ideas include a "What's New" hub page and an RSS feed.

**How is the docs team prepping for .NET 5? I see a lot of content landing page reorgs, is that related?**

[Visual Studio Magazine](https://visualstudiomagazine.com/articles/2020/07/29/net-5-wave.aspx) recently wrote about the .NET 5 content reorganization initiative. In a nutshell, .NET 5's focus is consolidation. Given that theme, our documentation must adapt to the modern .NET vision. I own the .NET web development area and am collaborating with others on cross-cutting .NET fundamentals content, such as the *[Microsoft.Extensions](https://docs.microsoft.com/dotnet/api/?view=dotnet-plat-ext-3.1&term=Microsoft.Extensions)* family of APIs for configuration, dependency injection, and so on. In some areas, you'll begin to see less emphasis on product names, particularly for content targeted at new customers. If a customer wants to build a web application with .NET, they shouldn't have to know that ASP.NET is the product name. I'm tasked with solving this problem for the ASP.NET content.

**My approach has always been: hit up the docs for just-in-time knowledge, or quick learning, and Learn modules for in-depth and hands-on content. Is that Microsoft's approach?**

The team offers content for folks at every level in their journey. If you're a seasoned ASP.NET Core developer, the [conceptual content](https://docs.microsoft.com/aspnet/core/) and [API reference content](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-3.1) are great resources. If you're new to .NET, the [Learn .NET](https://dotnet.microsoft.com/learn) page is a better starting point. If you want to learn more about using ASP.NET Core with Azure, [Microsoft Learn](https://docs.microsoft.com/learn/browse/?expanded=dotnet&products=aspnet-core) is a good place to start. In many Learn modules, an Azure subscription isn't required to test drive Azure services with .NET. You're provided an Azure Cloud Shell instance and a set of instructions that can often be completed in under an hour. Gamification is an important aspect of Learn. As you complete modules, you earn achievement badges. Virtual trophies are awarded for the completion of learning paths.

**Over at Build, a few folks discussed Learn TV. What is it and what can we expect from it?**

Learn TV is a Microsoft-owned video content platform that offers 24 hours of programming, 7 days a week. If you tuned in to the recent [.NET Conf: Focus on Microservices](https://focus.dotnetconf.net/agenda) event, you may have noticed that it streamed live on Learn TV. Additionally, Learn TV streams live shows from the Developer Relations organization, such as **The .NET Docs Show** and **92 & Pike**. It's still early days for Learn TV, hence the "preview" label on its landing page. Golnaz Alibeigi is working with a small team of folks to evolve it in to a world-class product. I'll defer to Golnaz for specific plans.

**I know you work on a lot of Learn modules. What are you working on now?**

Nish Anil, Cam Soper, and myself are authoring a .NET microservices learning path in Microsoft Learn. The learning path is based on the eBook [.NET Microservices: Architecture for Containerized .NET apps](https://docs.microsoft.com/dotnet/architecture/microservices/) and is expected to consist of seven modules. At the time of writing, the following modules have been published:

* [Create and deploy a cloud-native ASP.NET Core microservice](https://docs.microsoft.com/learn/modules/microservices-aspnet-core/)
* [Implement resiliency in a cloud-native ASP.NET Core microservice](https://docs.microsoft.com/learn/modules/microservices-resiliency-aspnet-core/)

Next up is a microservices module focused on DevOps with GitHub Actions. The learning path project board is [available for anyone to see](https://github.com/dotnet/AspNetCore.Docs/projects/68).

**What is your one piece of programming advice?**

My advice is to ask probing questions and challenge the status quo. Regardless of your organizational rank, ask why your team operates the way they do. Ask why certain technology choices were made. The answers might surprise you. Equipped with those answers, you can begin to understand existing processes and formulate improvements. If you have an idea, don't be afraid to speak up. Everyone brings a unique perspective to the table.

*You can follow Scott Addie [on Twitter](https://twitter.com/Scott_Addie).*
