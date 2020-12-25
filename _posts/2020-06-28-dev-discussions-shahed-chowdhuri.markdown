---
date: "2020-06-28"
title: "Dev Discussions - Shahed Chowdhuri talks about his ASP.NET Core A-Z blog series"
subtitle: We talk with Shahed Chowdhuri about ASP.NET Core blogging and more!
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/shahed-card.png
---

This is the full interview from my discussion with Shahed Chowdhuri in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away!
{: .box-note}

As a huge fan of the Marvel universe, Microsoft's Shahed Chowdhuri must feel like Captain America after publishing the last post of his [ASP.NET Core A-Z blog series](https://wakeupandcode.com/aspnetcore/#aspnetcore2020) this week. In these posts, he wrote 26 ASP.NET Core posts in 26 weeks, from A-Z (Authentication & Authorization to Zero-Downtime Web Apps). This was all using a single sample project!

I talked with Shahed about his path to Microsoft, the evolution of the blog series, his side projects and interests and, of course, which Marvel character sums him up best.

You can reach out to Shahed [on Twitter](https://twitter.com/shahedC) and follow his site, [Wake Up and Code](https://wakeupandcode.com/).

![Shahed Chowdhuri]({{ site.url }}{{ site.baseurl }}/assets/img/shahed-chowdhuri.jpg)

**What got you into software development, and Microsoft? What are you doing for Microsoft these days?**

Although I took two and a half years to complete a four-year degree in Civil and Environmental Engineering, I also taught myself how to build websites and software applications while in college. I worked on my personal website and also additional websites for the Civil Engineering department, the International Students Association, and my dorm.

I realized that I could build something, see results instantly, fix issues, and then see my changes right away. I got an internship as a web application developer during my second (and final) summer vacation, and then got a full-time job offer upon graduation.

I joined Microsoft as a Tech Evangelist 6+ years ago, as I was already using my spare time for public speaking on various topics. My role has changed in recent years, and I’m currently working with our enterprise customers to collaborate on software development projects and solve real-world business problems.

**What made you take on the original ASP.NET Core A-Z [blog series](https://wakeupandcode.com/aspnetcore/#aspnetcore2019) in 2019?**

During my evangelism days, I would mostly use my site to publish my PowerPoint slides after a meetup or conference. But I wouldn’t get a lot of traffic on the site, because I didn’t have many recent articles. As my role changed within Microsoft, I really wanted to give something back to the community again, in the form of technical writing and code samples.

In the fall of 2018, I was helping a colleague with some .NET code for a customer project (file upload into Azure Blob Storage) during a company hackathon. Instead of emailing him the code sample with an explanation, I decided to write a blog post and publish the code in a GitHub repository. To lead into this blog post, I also worked on a “Hello World” article, that was appropriately titled “[Hello, ASP.NET Core!](https://wakeupandcode.com/hello-asp-net-core/)” so that I could help new developers get started with ASP.NET Core.

As a result, this was the birth of a mini series to end 2018 with a bang. As you can see in the list of article titles below, the first letter of each post spells out Happy New Year.

- **H**ello, ASP .NET Core!
- **A**zure Blob Storage from ASP .NET Core File Upload
- **P**ages in ASP .NET Core: Razor, Blazor and MVC Views
- **P**rotocols in ASP .NET Core: HTTPS and HTTP/2
- **Y**our Web App Secrets in ASP .NET Core
- **N**etLearner – ASP .NET Core Internet Learning Helper
- **E**F Core Migrations in ASP .NET Core
- **W**atching for File Changes in ASP .NET Core
- **Y**our First Razor UI Library with ASP .NET Core
- **E**xploring .NET Core 3.0 and the Future of C# with ASP .NET Core
- **A**PI Controllers in ASP .NET Core
- **R**eal-time ASP .NET Core Web Apps with SignalR

Over a span of 12 weeks from October to December 2018, I published these seemingly randomly-selected topics on ASP.NET Core. Eventually, I [revealed the hidden message “HAPPY NEW YEAR” just before the new year](https://wakeupandcode.com/aspnetcore/#aspnetcore2018). Knowing that I couldn’t pull off the same trick twice, I decided go with a new series in 2019.

There are 52 weeks in a year, 26 weeks in half a year. This was the perfect opportunity to [cover 26 different topics from A-Z in 2019](https://wakeupandcode.com/aspnetcore/#aspnetcore2019), since there are also 26 letters in the alphabet.

**As you kicked off a new version of the series with an approach to feature everything in a single project, did you come across anything unexpected or interesting?**

Before 2019, I wasn’t sure if I could pick a topic for each letter if I stuck to ASP.NET Core. I was debating whether I should cover HoloLens, Bot Framework, Machine Learning and various other topics throughout the series. But I narrowed it down to topics that would be relevant to an ASP.NET Core developer, and also reviewed my list with [Jon Galloway](https://twitter.com/jongalloway?lang=en). (Jon is a a Sr. PM on the Visual Studio Mac team who was formerly on the .NET Team, and still continues to run the .NET Community Standup livestream broadcast.)

The 2019 series included code snippets that were all over the place. For the 2020 series, I decided to start with a real-world open-source web app ([NetLearner](https://github.com/shahedc/NetLearnerApp)) and build upon it week after week. I made sure that the web app included similar functionality across multiple web projects (MVC, Razor Pages, Blazor) with a shared library for core/infrastructure code. I realized that I needed to provided some context for the NetLearner repository, so I also published a [“2020 Prelude” series in late 2019](https://wakeupandcode.com/aspnetcore/#aspnetcore2020prelude). This mini-series provided some information on how to share code across multiple web apps, how to get started with the latest stable version of ASP .NET Core (v3.1) and what NetLearner is all about.

- [ASP.NET Core code sharing between Blazor, MVC and Razor Pages](https://wakeupandcode.com/asp-net-core-code-sharing-between-blazor-mvc-and-razor-pages/)
- [Hello ASP.NET Core v3.1!](https://wakeupandcode.com/hello-asp-net-core-v3-1/)
- [NetLearner on ASP.NET Core 3.1](https://wakeupandcode.com/netlearner-on-asp-net-core-3-1/)

**Other than ASP.NET Core, what other Microsoft technologies excite you/do you like to tinker with?**

There are so many! Before joining Microsoft, I had used Visual Studio and C# paired with XNA to build video games for Windows and XBox 360 in my spare time.

I also used my ASP.NET skills to publish a free Sales Data Analyzer tool to help other indie developers better understand their worldwide sales data.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Back when XBLIG Marketplace was available, I released 2 silly games for Xbox 360 in 2011, followed by a sales data analyzer and basic starter kit in 2012, for dev community. <br><br>Briefly mentioned in Xbox magazine, March 2014: <br><br>PDF: <a href="https://t.co/NNkujH1g59">https://t.co/NNkujH1g59</a> <a href="https://t.co/rvAvHRATBI">pic.twitter.com/rvAvHRATBI</a></p>&mdash; Shahed Chowdhuri @ Microsoft (@shahedC) <a href="https://twitter.com/shahedC/status/1118407088282177536?ref_src=twsrc%5Etfw">April 17, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Over the years, I’ve had a chance to learn and tinker with ASP.NET Core, Bot Framework, Cognitive Services, Desktop apps, Entity Framework, Functions (serverless), game development with Unity, HoloLens mixed reality, IoT, JavaScript, Kinect, Logic Apps, Machine Learning…oh wait, this is turning into another A-Z series!

**Do you have any *other* specific projects you've been working on that you want to show off?**

Yes! I’ve been working on some early concepts for a cinematic universe visualizer. The goal is to connect the dots between all the various movies, shows, cast and crew that are a part of each cinematic universe, starting with Marvel’s MCU.

**What kind of Marvel character are you?**

I am Groot.

*(Ed. Note: I am hereby required to show off Baby Groot.)*

<iframe width="560" height="315" src="https://www.youtube.com/embed/Hrimfgjf4k8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br />

**What is your one piece of programming advice?**

One piece of advice I would give anyone is to always keep learning. That doesn’t necessarily mean that you should learn every new programming language or framework that pops up on your radar. It could mean that you may want to dig deeper into a language you’ve already been working with. It could be some business skills that you want to pick up, to run your own software business or work as a consultant to help others run theirs.
