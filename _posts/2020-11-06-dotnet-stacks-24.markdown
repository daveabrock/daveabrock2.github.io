---
date: "2020-11-06"
title: "The .NET Stacks #24: Blazor readiness and James Hickey on Coravel"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-24-card.png
subtitle: This week, Blazor readiness and a talk with James Hickey.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

RIP, [James Bond](https://www.nytimes.com/2020/10/31/movies/sean-connery-dead.html). While watching *Goldfinger* last night, I forgot about the best feature of Connery Bond films: if you're confused about the plot, the villain will explain it all to you right before the movie ends. Ah, the original "let's have retro before the sprint ends."

This week:

* Is Blazor ready for the enterprise?
* Dev Discussions: James Hickey
* Last week in the .NET world

## Is Blazor ready for the enterprise?

If you're writing .NET web apps, you've likely aware of all the Blazor hype. (If you haven't, it allows you to write web UIs in C#, where you've traditionally had to use JavaScript.) In the last two years, I've seen it morph from a cute Microsoft project to something they've deemed production-ready.

And with .NET 5—becoming generally available next week!—we're seeing a lot of great improvements with Blazor: [CSS isolation](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/#css-isolation-for-blazor-components), [JavaScript isolation](https://docs.microsoft.com/aspnet/core/blazor/call-javascript-from-dotnet?view=aspnetcore-5.0#blazor-javascript-isolation-and-object-references), [component virtualization](https://docs.microsoft.com/aspnet/core/blazor/components/virtualization), [toggle event support](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-support-for-ontoggle-event), [lazy loading](https://docs.microsoft.com/aspnet/core/blazor/webassembly-lazy-load-assemblies), [server-side prerendering](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-prerendering), and so on. These improvements help Blazor catch up to base features of leading SPA frameworks, like Vue, React, and Angular.

If you're slinging code for a decent-sized company, you [might be wondering if Blazor is ready for the enterprise](https://www.telerik.com/blogs/is-blazor-safe-enterprise-bet). Can you convince your pointy-haired bosses to use it for new, and maybe existing, apps? I think it's ready. However, this isn't an "easy yes"—it involves a nuanced answer. We'll answer this question by answering some common questions.

**Is it another Silverlight?** If you've worked in Blazor for a little while, it's a tired argument but one you'll have to answer from those not in the day-to-day .NET world. While a similar concept—using C# instead of JavaScript to build web apps—Blazor is built on web standards and is not built with proprietary tech that can be suddenly thrown away. It isn't a browser plugin like Silverlight.

**How will it help teams deliver faster?** Blazor lowers the front-end learning curve normally associated with JavaScript, and allows devs to use their language and their tools to get things done. Blazor doesn't *replace* JavaScript. However, if you're in a shop with mostly C# devs, it's an obvious productivity improvement. This isn't a quick on/off switch, as teams still need to get familiar with core SPA concepts, but the use case for .NET shops is clear.

**Is it supported with a good ecosystem?** Because Blazor lives in the .NET ecosystem, it comes with official Microsoft support just like any other product (and, last week, [we discussed .NET 5 support](https://daveabrock.com/2020/10/31/dotnet-stacks-23)). Additionally, Microsoft continues to devote significant investment in it and has a long history of backwards compatibility. The ecosystem is not as evolved as it is with Angular and React—they've had a head start—but is growing tremendously. As [Peter Vogel mentioned](https://www.telerik.com/blogs/is-blazor-safe-enterprise-bet), Blazor already has 25% of the interest that Vue has ([from Google Trends](https://trends.google.com/trends/explore?cat=31&geo=US&q=Blazor,Vue)).

**Does it perform well?** Compared to other SPA frameworks, Blazor's performance is ... fine? It'll pass the eye test with Vue and whatnot in most cases but there will be *some* waiting—Blazor Web Assembly has a large-ish download size (as its loading .NET in the browser), and Blazor Server has a network hop for each user interaction. The team has [made a lot of progress in addressing performance](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-performance-improvements), and AOT compilation is the most popular request for ASP.NET in .NET 6 (and will impact non-Blazor apps in ASP.NET as well). If you're dealing with huge amounts of data, you might want to wait for these improvements—but should be suitable in most business cases.  

## Dev Discussions: James Hickey

When designing a real production system—especially over the web—there's a lot to think about past coding features for stakeholders. For example, how do you manage events? What about scheduling tasks? What about your caching mechanism? A lot of these considerations make you wonder if they should be supplied out-of-the-box with ASP.NET Core.

James Hickey's [Coravel project is here to assist](https://github.com/jamesmh/coravel). Billed as a "near-zero config .NET Core library that makes task scheduling, caching, queuing, mailing, event broadcasting, and more a breeze," it ships with an impressive set of free features—and you can look at Coravel Pro for a professional admin panel, UI management, health metrics, and more.

I caught up with James to talk about his project, software design, and more.

![James Hickey]({{ site.url }}{{ site.baseurl }}/assets/img/james-hickey.jpg)

### As the author of Refactoring TypeScript, what are your biggest tips on keeping a codebase healthy? 

* Simplicity rules the day. If you "feel" like your solution is getting complicated, then step back or take a 15-minute walk!
* Reuse is over-hyped. It leads to coupling. Coupling more often-than-not causes trouble down the road.
* Abstract I/O from other kinds of logic. Don't do direct database or file system access from the core logic in your system. Put that behind an interface.
* If you want to get good at designing code, then get good at knowing how to build unit tests that have minimal (or no) mocking/stubbing.
* Use automated quality analyzers to fail builds when code standards are violated.
* [Fix broken windows](https://blog.codinghorror.com/the-broken-window-theory/).

### Let's talk about your Coravel project. I love your bold selling point: "Near-zero config .NET Core micro-framework that makes advanced application features like Task Scheduling, Caching, Queuing, Event Broadcasting, and more a breeze!" Can you walk through what caused you to pursue this project, how Coravel can help me, and how it works with existing .NET projects? 

Back in the day when I was building *.aspx* WebForms, I discovered the PHP framework Laravel and loved how it included all these additional features like queuing, task scheduling, and so on out of the box. When .NET Core was gaining traction, I found myself wanting some of the conveniences of Laravel but in .NET Core.

Coravel can help you get a new project up-and-running faster by including things like automated task scheduling, background task queuing, and so on out of the box without having to install extra infrastructure. It works with any .NET Core projects and is (supposed to be) super easy to install, configure and use.

### For scheduling and event management, I know a lot of folks like utilizing things like triggered Azure Functions and Service Bus and the like—is there an advantage to using Coravel over that?

One disadvantage is that Coravel doesn't (yet!) support distributed scheduling, queuing, and caching features. One advantage though, again, is you don't have to install any additional infrastructure. You can install a NuGet package, add a couple lines of code and you're good-to-go!

You can use Coravel and these other products together, too. For example, we often use a distributed service bus to integrate separate systems via async messaging. Coravel can support the internal integration within a system—like how you might emit domain events between aggregates. Doing this in-memory has certain advantages. Jimmy Bogard, for example, [has written about using in-memory domain event processing](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/). 

### I know by default, Coravel caching is in-memory (but has options to persist to a database). How does this compare to something like Redis?

Again, using Coravel you won't have to muck around with installing, configuring, and so on with that additional infrastructure. Redis is a fantastic product and I'm hoping to eventually offer providers for Coravel to hook into Redis—which could make using Redis caching, messaging, and so on even easier. I just need to find some time 😉.

### Do you view Coravel as an enhancement over the BCL, or filling in for some of its shortcomings? This is part of a broader thought about OSS's role within .NET Core.

I view Coravel as enhancements over the BCL. I wouldn't expect to see these features included in a barebones console application, for example. But when it comes to web applications, these are all features that you'll need pretty much out-of-the-gate if it's anything more than a demo application.

### I'm a fan of your "Loosely Coupled Show" that you run with Derek Comartin (sure beats the "Tightly Coupled Show")—a great spot to discuss software architecture and design. In such a fluid industry, are there any firm principles you are stubborn about?

Generally, I stick with using a feature folder approach to structuring any projects I work on. I've [written about this before](https://builtwithdot.net/blog/changing-how-your-code-is-organized-could-speed-development-from-weeks-to-days) and Derek [just wrote about it](https://codeopinion.com/organizing-code-by-feature-using-vertical-slices/). I'm a huge fan of CQRS and think it fits perfectly with the feature folders approach.

I've given a talk on these topics [at the Florida .NET meetup](https://www.youtube.com/watch?v=36G6-vwwpe8&feature=youtu.be) if anyone's interested in hearing more about it.

*Check out the [full interview at my website](https://daveabrock.com/2020/11/01/dev-discussions-james-hickey).*

## What is your one piece of programming advice?

Make sure you get lots of sleep and step outside once in a while. 😅

## 🌎 Last week in the .NET world

### 🔥 The Top 5

* Jimmy Bogard [finds a .NET 5 breaking change with IndexOf](https://jimmybogard.com/mind-your-strings-with-net-5-0).
* Jeremy Likness [asks for help planning EF Core 6.0](https://devblogs.microsoft.com/dotnet/help-us-plan-ef-core-6-0), and also [announces version 1.0 of .NET for Apache Spark](https://devblogs.microsoft.com/dotnet/announcing-version-1-0-of-net-for-apache-spark).
* James Newton-King [walks through gRPC performance improvements in .NET 5](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5).
* Damien Bowden [uses Azure Cognitive Search suggesters in ASP.NET Core and autocomplete](https://damienbod.com/2020/10/29/using-azure-cognitive-search-suggesters-in-asp-net-core-and-autocomplete/).
* Rachel Appel [builds serverless apps from Azure Functions](https://blog.jetbrains.com/dotnet/2020/10/29/build-serverless-apps-with-azure-functions/).

### 📢 Announcements

* Tara Overfield [offers the .NET Framework October update for Windows and Windows Server](https://devblogs.microsoft.com/dotnet/net-framework-october-2020-cumulative-update-preview-update-for-windows-10-2004-and-windows-server-version-2004).
* Lily Ma [outlines the Azure SDK GitHub issue support process](https://devblogs.microsoft.com/azure-sdk/github-issue-support-process).
* Leslie Richardson [talks about the future of Visual Studio extensions](https://devblogs.microsoft.com/visualstudio/the-future-of-visual-studio-extensions).


### 📅 Community and events

* For community .NET standups, we've got a [brand new game dev standup](https://www.youtube.com/watch?v=jvg57VW1V5c), Entity Framework [talks about EF Core 5 collations](https://www.youtube.com/watch?v=YF1MbSYfiRY), and ASP.NET talks about [updating Scott Hanselman's blog to .NET Core](https://www.youtube.com/watch?v=Up4Wd_wHzDg).
* The .NET Docs Show talks [with Sam Basu about Blazor and Comet for Native Mobile Apps](https://www.youtube.com/watch?v=lzVZSKcC-oY).
* You can check out a stream of this weekend's [.NET Dev Summit 2020 - Asia Pacific](https://www.youtube.com/watch?v=69cccDJ_Vec).


### 😎 ASP.NET Core / Blazor

* Dave Brock (ahem) [introduces his Blazor project](https://daveabrock.com/2020/10/26/blast-off-blazor-intro), and also [writes and tests a 404 Blazor component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page).
* Jon Hilton asks: [should your enterprise pick Angular, React, or Blazor](https://www.telerik.com/blogs/should-enterprise-pick-angular-react-blazor)?
* Marinko Spasojevic [calls JavaScript functions with C# in Blazor WebAssembly](https://code-maze.com/how-to-call-javascript-code-from-net-blazor-webassembly/).
* Scott Hanselman [uses the ASP.NET Core environment feature to manage development vs. production for any config file type](https://www.hanselman.com/blog/using-the-aspnet-core-environment-feature-to-manage-development-vs-production-for-any-config-file-type).
* Jeetendra Gund [passes multiple parameters to a GET method in ASP.NET Core MVC](https://www.telerik.com/blogs/how-to-pass-multiple-parameters-get-method-aspnet-core-mvc).
* Thomas Ardal [talks about how he does bundling and minification in ASP.NET Core](https://blog.elmah.io/how-we-do-bundling-and-minification-in-asp-net-core/).
* Filip Woj [shows off compact APIs with C# 9, .NET 5, and ASP.NET Core](https://www.strathweb.com/2020/10/beautiful-and-compact-web-apis-with-c-9-net-5-0-and-asp-net-core/).

### ⛅ The cloud

* Paul F. Wood [discusses when, why, and how to use Azure Key Vault](https://www.advancedinstaller.com/using-azure-key-vault.html).
* Matteo Prosperi [tinkers with client-side Blazor and the AWS SDK for .NET](https://www.codeproject.com/Articles/5283689/Tinkering-with-client-side-Blazor-and-the-AWS-SDK).
* Nwose Lotanna Victor [deploys an Angular app with Azure Static Web Apps](https://www.telerik.com/blogs/deploy-angular-application-with-azure-static-web-apps).
* Ben De St Paer-Gotch [sets up cloud deployments using Docker, Azure and Github Actions](https://www.docker.com/blog/setting-up-cloud-deployments-using-docker-azure-and-github-actions/).


### 📔 C#

* Matthew Jones [writes about interfaces and abstract classes in C#](https://exceptionnotfound.net/csharp-in-simple-terms-10-interfaces-and-abstract-classes).
* Munib Butt [shows off the Producer Consumer pattern in C#](https://www.c-sharpcorner.com/article/producer-consumer-pattern-in-c-sharp/).
* Tomasz Pęczek [consumes JSON Objects Stream with HttpClient](https://www.tpeczek.com/2020/10/consuming-json-objects-stream-ndjson.html).
* Khalid Abuhakmeh [generates QR codes with C#](https://khalidabuhakmeh.com/generate-qr-codes-csharp).
* Nick Gamb [authenticates with SAML in ASP.NET Core and C#](https://developer.okta.com/blog/2020/10/23/how-to-authenticate-with-saml-in-aspnet-core-and-csharp).
* Eric Potter [upgrades a .NET Framework library to .NET 5](http://humbletoolsmith.com/2020/10/23/upgrading-a-_net-framework-library-to-_net-5/).
* Jason Farrell [continues his series on unit testing](https://jfarrell.net/2020/10/25/test-series-part-2-unit-testing/).

### 🔧 Tools

* The Uno Platform Team [discusses WebAssembly tools, frameworks, and libraries for .NET devs](https://platform.uno/blog/webassembly-tools-frameworks-and-libraries-for-net-developers/).
* ErikEJ [sets the command timeout with the latest .NET SqlClient](https://erikej.github.io/sqlclient/2020/10/26/sqlclient-commandtimeout-preview.html).
* For Visual Studio: Mads Kristensen [offers tips on searching in Visual Studio](https://devblogs.microsoft.com/visualstudio/get-more-done-with-search-in-visual-studio), and Steve Smith [configures Visual Studio to name private fields with underscores](https://ardalis.com/configure-visual-studio-to-name-private-fields-with-underscore/).
* Andrew Lock [monitors Helm releases that use jobs and init containers](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-9-monitoring-helm-releases-that-use-jobs-and-init-containers/).
* Over at Code Maze, [they work with Elasticsearch in ASP.NET Core](https://code-maze.com/elasticsearch-aspnet-core/).
* Rajeev Bera [talks about Git tips and tricks](https://opensource.com/article/20/10/advanced-git-tips).
* Sanjay Modi [uses MiniProfiler in ASP.NET Core 3.1](https://procodeguide.com/programming/asp-net-core-code-profiling-using-miniprofiler/).
* Mark Seemann [talks about how to keep REST API URLs evolvable](https://blog.ploeh.dk/2020/10/26/fit-urls/).

### 📱 Xamarin

* Charlin Agramonte says: [stop doing IsVisible="true/false" to show hide views](https://xamgirl.com/stop-doing-isvisibletrue-false-to-show-hide-views-in-runtime-in-xamarin-forms/).
* Jean-Marie Alfonsi [discusses Sharpnado.Tabs 2.0](https://www.sharpnado.com/sharpnado-tabs-2-0/).
* Leomaris Reyes [replicates an event app UI](https://askxammy.com/replicating-event-app-ui-in-xamarin-forms/).

### 🎤 Podcasts

The 6-Figure Developer podcast [talks about managing cloud cost with Omry Hay](https://6figuredev.com/podcast/episode-167-manage-cloud-cost-with-omry-hay/).

### 🎥 Videos

* Kathleen Dollard [talks about the difference between .NET Core, .NET 5, and .NET Framework](https://www.youtube.com/watch?v=CL1kFrw4qEY).
* Data Exposed [talks about creating your first Azure SQL database](https://channel9.msdn.com/Shows/Data-Exposed/Creating-Your-First-Azure-SQL-Database).
* Jeff Fritz [talks about SOLID and dependency injection in C#](https://www.youtube.com/watch?v=2RKF8XTf0Fc)
