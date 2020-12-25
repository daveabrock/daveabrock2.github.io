---
date: "2020-08-22"
title: "The .NET Stacks #13: .NET 5 Preview 8 and Blazor, interview with Scott Addie, community links!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-13-card.png
subtitle: We discuss Blazor and .NET 5 Preview 8, talk with Scott Addie, and look at the busy week in the .NET community!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday to you! Here's what we have for this week:

* How Blazor is drastically improving in preview 8 of .NET 5
* A discussion with Scott Addie
* A trip around the community

## .NET 5 Preview 8 ♥'s Blazor

In a few weeks, we'll see preview 8 of .NET 5, the *last* preview before the release candidates, and it'll be filled with a ton of Blazor improvements for performance and tooling. In [this week's ASP.NET community standup](https://www.youtube.com/watch?v=KRNd8JDRqRc), the team showed off a lot of these new features that folks have been asking for.

The biggest takeaway is that, with preview 8, Blazor is using the .NET 5 runtime and libraries—and with it, its improvements. Previously, Blazor Web Assembly was using Mono libraries—which seemed a little off, when thinking about the "One .NET" mission of .NET 5. Using the .NET 5 runtime now accounts for a ton of performance gains: the team reports that arbitrary CPU code in .NET 5 is around 30% faster (think dictionaries and string comparisons), JSON serialization and deserialization looks to be about twice as fast, and Blazor component rendering is anywhere from 2x to 4x faster.

Microsoft has traditional C# devs sold on Blazor—one toolchain, one language, much awesomeness. We get it. To really make an impact, and welcome other folks to .NET, Blazor will need to have features that folks demand from their other SPA frameworks and libraries like Angular and React (or other tools in that space). For example, if I ask a JS expert (and .NET outsider) to try a new SPA competitor, and there's no auto-refresh, it's hard to be taken seriously.

With preview 8, we're seeing quite a few of these features. Here's three of them.

### CSS isolation

When working with components, the idea is that they can be portable, organized elements. If I have a `<DatePicker>` component, for example, I should be able to drop it and its styles wherever I need. Until preview 8, I would have to style my components in some CSS file somewhere else, and bring in that style sheet, and worry about maintaining that and dealing with cascading impacts throughout my site.

No more. Now, with CSS isolation, you can make life easier on yourself. By using a naming convention like `myComponent.razor.css`, Blazor will bind the CSS to your component with no imports needed! And since that's only bound to your component you can simplify your styles. Instead of declaring something like `<table class="myComponentStyle">` just style a `table` in your component's stylesheet and done!

*What about JS isolation?* It isn't ready now, but it's on the way.

### Lazy loading

Let's say you have parts of your app that aren't accessed every time. For example, reporting that's generally accessed once a month. With lazy loading, you can load any assemblies only when needed. In your routing configuration, you can inject a lazy loading service and specify which assemblies to load and when. No more customized tree shaking.

### Auto-refresh

The instant feedback loop is a big reason why so many people are drawn to front-end development. I can make a change, click Save (or not even Save!) and my changes are reflected immediately. This is thanks for incremental build tooling running in the background.

.NET has its [own incremental file watcher](https://docs.microsoft.com/aspnet/core/tutorials/dotnet-watch?view=aspnetcore-3.1), `dotnet watch`, that not a lot of people know about. Blazor will use `dotnet watch` behind the scenes to provide the auth-refresh capability.

There's much more coming in preview 8, so definitely [check out the video if you're so inclined](https://www.youtube.com/watch?v=KRNd8JDRqRc)—and watch this space for more details when it arrives.

## Dev Discussions: Scott Addie

Have you worked in ASP.NET Core? If so, you have surely come across Scott Addie, whether you know about it or not. For over three years, Scott has worked as a content developer at Microsoft—publishing documentation on the framework, APIs, ebooks, blog posts, and Learn modules, and more.

He’s also been active in the developer community—both before and after joining Microsoft—and you can find him as a co-host on The .NET Docs Show, a weekly Twitch stream.

I recently caught up with Scott to talk about his work in the community, his career, .NET 5, and what he’s working on.

![Scott Addie]({{ site.url }}{{ site.baseurl }}/assets/img/keynote-scott.jpg)

**Walk us through your role.**

For over three years, I’ve been a content developer on the .NET, Web Development, & Languages team of the Developer Relations division. My primary responsibility is to produce technical content focused on teaching ASP.NET Core. I’m provided creative freedom in how to best accomplish that goal. Ultimately, the ability to effectively communicate complex ideas, in both verbal and written forms, is critical to my success in this role.

**What kind of projects are you hacking away at now?**

Bill Wagner, Maxime Rouiller, and myself have been collaborating on a tool that generates "What's New" pages for conceptual docsets hosted on *docs.microsoft.com.* It's a .NET Core console app written in C# that uses the GitHub GraphQL API to process pull requests. I plan to distribute the tool internally as a .NET Core global tool. The idea is to provide customers with a summary of what changed in a particular product's documentation during a specific time period. For example, the ASP.NET Core documentation publishes "What's New" pages on a monthly cadence. At the time of writing, the concept has been adopted for the following products:

* [.NET](https://docs.microsoft.com/dotnet/whats-new)
* [ASP.NET Core](https://docs.microsoft.com/aspnet/core/whats-new)
* [Azure DevOps](https://docs.microsoft.com/azure/devops/release-notes/docswhatsnew)
* [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/whats-new-docs)
* [Xamarin](https://docs.microsoft.com/xamarin/whats-new)

I'm actively working with a handful of other teams, including C++, Visual Studio, and SQL, to roll out these pages. There's a sizeable features backlog for this tool. Some feature ideas include a "What's New" hub page and an RSS feed.

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

*This is just a small portion of my interview with Scott! [Head over to my site for more](https://daveabrock.com/2020/08/15/dev-discussions-scott-addie).*

## 🌎 Last week in the .NET world




### 🔥 The Top 3

* Khalid Abuhakmeh [discusses the secrets of a .NET professional](https://khalidabuhakmeh.com/secrets-of-a-dotnet-professional).
* Andrew Lock [talks about DI scopes in IHttpClientFactory message handlers](https://andrewlock.net/understanding-scopes-with-ihttpclientfactory-message-handlers/).
* Damien Bowden [does retry error handling for activities and orchestrations in Azure Durable Functions](https://damienbod.com/2020/08/11/retry-error-handling-for-activities-and-orchestrations-in-azure-durable-functions/).

### 📢 Announcements

* Tara Overfield [runs through .NET Framework August 2020 security and quality updates](https://devblogs.microsoft.com/dotnet/net-framework-august-2020-security-and-quality-rollup-updates/), and Rahul Bhandari [discusses August 2020 security updates for .NET Core](https://devblogs.microsoft.com/dotnet/net-core-august-2020/).
* Microsoft and Chris Pietschmann [have released a 2020 developer's guide to Azure](https://build5nines.com/free-ebook-developers-guide-to-azure-2020-edition/).
* It looks like nuget.org [now supports advanced search](https://devblogs.microsoft.com/nuget/advanced-search-on-nuget-org/).

### 📅 Community and events

* A very hard day [at Mozilla, and the community at large](https://blog.mozilla.org/blog/2020/08/11/changing-world-changing-mozilla/).
* The .NET Docs Show has an *amazing* conversation with [Layla Porter about .NET Core 3.1 and Twilio](https://www.youtube.com/watch?v=GTIsjQq-GXU).
* Two community standups this week: the languages team [talks IoT, API analyzers, and more](https://www.youtube.com/watch?v=EU3c0yz_cwE), and ASP.NET [talks about Blazor updates in .NET 5](https://www.youtube.com/watch?v=KRNd8JDRqRc).

### 😎 Blazor

* Marinko Spasojevic [walks through role-based auth with Blazor WebAssembly](https://code-maze.com/blazor-webassembly-role-based-authorization/).
* Both [Lee Richardson](https://www.codeproject.com/Articles/5276055/Blazor-WebAssembly-vs-Angular-Client-Side-Clash) and [Jon Hilton](https://www.telerik.com/blogs/blazor-vs-angular-web-developers) compare Angular with Blazor WASM.

### 🚀 .NET Core

* Hamid Khan [works with CORS in .NET Core](https://www.c-sharpcorner.com/article/cors-in-dotnet-core/).
* Over at CodeMaze, [how to send client-specific messages using SignalR](https://code-maze.com/how-to-send-client-specific-messages-using-signalr/).
* Derek Comartin [migrates to .NET Core](https://codeopinion.com/migrating-to-net-core-mission-complete).
* David Grace [continues logging data changes in EF Core](https://www.roundthecode.com/dotnet/entity-framework/log-data-changes-in-entity-framework-core-service-integration-testing-with-api).
* Steve Gordon [talks about best practices when integration testing ASP.NET Core apps](https://www.stevejgordon.co.uk/integration-testing-asp-net-core-applications-best-practices).
* Karthik Anandan [shields an ASP.NET MVC web app with Content Security Policy](https://www.syncfusion.com/blogs/post/shield-your-asp-net-mvc-web-applications-with-content-security-policy-csp.aspx).
* Jeremy Wells [walks through TDD and exception handling with xUnit in ASP.NET Core](https://medium.com/swlh/tdd-and-exception-handling-with-xunit-in-asp-net-core-f9ffe5dde800).
* Mark Seemann [writes an ASP.NET Core URL builder](https://blog.ploeh.dk/2020/08/10/an-aspnet-core-url-builder/).
* Ryan Palmer [walks through configuration, secrets, and KeyVault with ASP.NET Core](https://www.compositional-it.com/news-blog/configuration-secrets-and-keyvault-with-asp-net-core/).
* Matthew Soucoup [authenticates an ASP.NET Core web app with Microsoft.Identity.Web](https://codemilltech.com/authenticate-a-asp-net-core-web-app-with-microsoft-identity-web/).

### ⛅ The cloud

* Jason Farrell [uploads a file using Azure Durable Functions](https://jfarrell.net/2020/08/13/durable-functions-part-2-the-upload/).
* Rishit Mishra [secures web app secrets through Azure Key Vault](https://www.red-gate.com/simple-talk/dotnet/net-development/securing-web-application-secrets-through-azure-key-vault/).
* Mitch Denny [discusses packaging, tools, and repo structure for the Azure SDKs](https://devblogs.microsoft.com/azure-sdk/azure-sdk-packaging-tools-and-repository-structure/).
* Kevin Griffin [redirects with Azure Static Web Apps](https://consultwithgriff.com/how-to-redirect-with-azure-static-web-apps).
* Abhijeet Jadhav [queries the Microsoft Graph API using C#](https://www.c-sharpcorner.com/article/delta-queries-in-microsoft-graph-using-c-sharp/).

### 📔 Languages

* Dave Brock (ahem) [learns another few things about C# 9 records](https://daveabrock.com/2020/08/14/records-spec).
* Khalid Abuhakmeh [builds HTML with C#](https://khalidabuhakmeh.com/building-html-with-csharp).
* Vishal Saroopchand [compares Go and C#](https://devblogs.microsoft.com/premier-developer/go-and-csharp-comparison/).
* Raymond Chen [discusses hidden constraints on the result type in Concurrency Runtime tasks](https://devblogs.microsoft.com/oldnewthing/20200812-00/?p=104075).
* Urs Enzler [talks about F# equality](https://www.planetgeek.ch/2020/08/12/our-journey-to-f-equality-for-free/) and also [happy path coding with F#](https://www.planetgeek.ch/2020/08/11/our-journey-to-f-happy-path-coding/).
* Jonathan Channon [discusses F# type aliases](https://www.softwarepark.cc/blog/2020/8/7/understanding-f-type-aliases).
* Anthony Giretti [extends partial methods in C# 9](https://anthonygiretti.com/2020/08/10/introducing-c-9-extending-partial-methods/).

### 🔧 Tools

* Scott Hanselman [shows off free books for learning about cloud-native .NET apps](https://www.hanselman.com/blog/FreeBooksForLearningAndGettingStartedWithCloudNativeNETApps.aspx).
* Michael Shpilt [offers five Visual Studio productivity tips](https://michaelscodingspot.com/productivity-tips-in-visual-studio/).
* Josef Ottosson [includes referenced projects in dotnet pack](https://josef.codes/dotnet-pack-include-referenced-projects/).
* Mads Kristensen [shows off Presentation Mode in Visual Studio](https://devblogs.microsoft.com/visualstudio/use-visual-studio-in-presentation-mode/).

### 📱 Xamarin

* Leomaris Reyes [explores the new Xamarin.Forms grid structure](https://www.telerik.com/blogs/exploring-new-grid-structure-xamarin-forms) and also [replicates a streaming UI app](https://askxammy.com/replicating-streaming-ui-app-in-xamarin-forms/).
* Dobrinka Yordanova [works on Mobile Blazor Bindings for Xamarin](https://www.telerik.com/blogs/telerik-mobile-blazor-bindings-for-xamarin).
* Theodora Tataru [builds a college diary in Xamarin.Forms](https://devblogs.microsoft.com/xamarin/college-diary-xamarin-theodora-tataru/).
* Jake Kirsch [talks about the new Xamarin.Forms 4.8 release](https://devblogs.microsoft.com/xamarin/xamarinforms-4-8-gradients-brushes/).
* Nick Randolph [discusses the ItemsPanel](https://nicksnettravels.builttoroam.com/xaml-basics-itemspanel).

### 🎤 Podcasts

* The Xamarin Podcast discusses [brushes, gradients, and shapes](https://www.xamarinpodcast.com/76).
* The .NET Core Show [talks to Luis Quintanilla about ML.NET](https://dotnetcore.show/episode-57-ml-net-with-luis-quintanilla/).


### 🎥 Videos

* Data Exposed [talks about Azure SQL capacity planning](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Capacity-Planning-Overview).
* The ON.NET Show [uses Redis as a .NET Core data store](https://channel9.msdn.com/Shows/On-NET/Using-Redis-as-a-NET-Core-Data-Store).
* The Visual Studio Toolbox [works through source generators in C#](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Source-Generators-in-CSharp).
