---
date: "2020-08-01"
title: "The .NET Stacks #10: .NET 5 taking shape, how approachable is .NET, a talk with Jeremy Likness, more!"
tags: [dotnet-stacks]
share-img: /assets/img/stacks-10-card.png
subtitle: We discuss the latest on .NET 5, the approachability of .NET, a talk with Jeremy Likness, and more!
comments: false
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

We had a tremendously busy week in .NET and it shows in this week's edition.

This week, we:

* Discuss the latest in .NET 5
* Think about what it's like to be a beginner in the .NET ecosystem
* Kick off an interview with Jeremy Likness, Microsoft's Sr. PM of .NET Data
* Take a trip around the community

Also, say 👋🏻 to my parents! They just subscribed and this is the only line they'll understand. 🤓

## .NET 5 is close, so close

This week, we hit the preview 7 release for .NET 5 (see the notes for [.NET 5 Preview 7](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-7), [EF Core 5 Preview 7](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-ef-core-5-0-preview-7), and [ASP.NET Core updates in the new preview](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-7/), as well as Steven Toub's post on [.NET 5 performance improvements](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5/)). The next release for .NET 5 will be Release 8, and then there will the two RCs (each with "go live" licenses). .NET 5 is slated for official release in early November.

What's new in these previews? A few items of note:

* Blazor WASM apps now target .NET 5
* Blazor improvements in debugging, accessibility, and performance
* In EF, a `DbContextFactory`, ability to clear the `DbContext` state, and transaction savepoints

It looks like single-file apps, the ability for .NET Core apps to be [published and distributed as a single executable file](https://github.com/dotnet/runtime/issues/36590), is coming in the next release (release 8).

## How would *you* help a beginner get started on .NET?

Dustin Gorski published a great post last week called [.NET for Beginners](https://dusted.codes/dotnet-for-beginners). It certainly isn't a quick read, but I would recommend giving it a read when you get a few minutes. He discusses why many feel that .NET isn't very approachable for newcomers. It opened my eyes: I was aware of most of his criticisms, but I've been dealing with them for so long I haven't thought about how difficult it can be for newcomers coming into .NET for the first time.

From my perspective, what I agreed most with was:

* How you have to know a lot to get started
* How feature bloat really prevents beginners from knowing the best way to do something, when there are so many ways to do it (with varying performance impacts)
* How the constant change and re-architecting of the platform makes it almost impossible to keep up

Think about it: if someone can to you and asked how to get started in .NET using a sample application, *what would you say?*

Would you start by explaining .NET Core and how it's different than .NET Framework? And mention .NET Standard as a bridge between them? Or get them excited about .NET 5? Would you also bring up C# and F# and VB and the differences between them? What about Xamarin and Mono? What about a simple web site? Should they get started with Blazor? Traditional MVC? Razor Pages? One of my biggest takeaways from the piece: *you have to know a lot of trivia to start developing in .NET.*

Feature bloat is real, especially when it comes to C#. Take, for example, the promise of immutability in C# 9 with records ([which I've written about](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records)). Records are similar to—but have immutable features over—structs. The exact reasons aren't important here (that records are meant for immutability and prevent boilerplate code) but a [valid complaint shows](https://stackoverflow.com/questions/61939693/when-to-use-c9-records): why are we introducing a new construct into the language where we could have improved on what already exists? If someone wanted to use immutability in .NET, what would you say now? Use records in C# 9? Use readonly structs? What about a tuple class? Or use F#?

This is not to say Microsoft isn't aware of all these challenges and isn't trying to improve. They definitely are! You can head over to [*try.dot.net*](https://try.dot.net/) to run C# in the browser ([with an in-browser tutorial](https://dotnet.microsoft.com/learn/dotnet/in-browser-tutorial/)). You can run code in the docs and in Microsoft Learn modules. C# 9 top-level statements take away that pesky `Main` method in console apps ([yet another shameless plug](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)).

Coming from the slow-moving days from the bloated `System.Web` namespace, I never thought I would be hearing (and agreeing) about gripes of .NET moving too fast. This is a testament to the hard work by the folks at Microsoft. Taking hints from .NET 5, I hope .NET allows developers to focus on what's important, not try to be everything to everyone, and avoid feature bloat in C#. I think a continued focus on approachability positively impacts all .NET developers.  

## Dev Discussions: Jeremy Likness, Sr. PM of Data at Microsoft

If you’ve worked on Azure or .NET for awhile—like Azure Functions and Entity Framework—you’re probably familiar with Jeremy Likness, the senior PM for .NET data at Microsoft. He’s spoken at several conferences (remember those?), [writes about various topics at his site](https://blog.jeremylikness.com/), is a familiar face on various .NET-related videos, and, as of late, can be seen in the Entity Framework community standups (and more!).

I caught up with Jeremy to talk about his path to software development and all that’s going on with Entity Framework and .NET 5. After you get through this week’s interview, I think we can all agree his path to Microsoft is both inspiring and absolutely crazy.

Jeremy was very generous with his time! Because there’s so much to cover, I’ve split this into two different parts. This week, we get to know Jeremy. Next week, we’ll get into his work and discuss Entity Framework and .NET 5.

![Jeremy Likness]({{ site.url }}{{ site.baseurl }}/assets/img/jeremy-likness.jpg)

**As a developer at heart, what kind of projects have you been tinkering with?**

My first consulting projects ... involved XAML via WPF then Silverlight. I was a strong advocate of Silverlight because I had built some very complex web apps using JavaScript and managing them across browsers was a nightmare. ... Silverlight was an implicit promise to write C# code that could run anywhere, and for many reasons probably more political than technical, the promise was not fulfilled.

I believe it has been realized with .NET Core and more specifically, Blazor. I resisted Blazor when it came out due to the existence of mature web frameworks available like Angular, React, Vue.js and Svelte, but colleagues convinced me to give it a spin and I was blown away by two things: first, how productive I could be and produce so much in a short period of time. Second, how many existing packages work with it and run inside the browser “as is.”

I’ve been building a lot of Blazor apps to explore different protocols, ways of interacting with data, application of the MVVM pattern and more. I’m working to publish some reference applications that show how to use Blazor with Entity Framework Core [and have published seven articles](https://blog.jeremylikness.com/series/blazor-and-ef-core/).

I am also diving into expressions. I [published a blog post](https://blog.jeremylikness.com/blog/dynamically-build-linq-expressions/) about parsing JSON and turning it into an expression tree to evaluate. I’ve also [written about the inverse](https://blog.jeremylikness.com/blog/look-behind-the-iqueryable-curtain/): parsing an `IQueryable` LINQ expression to pull out the various pieces. Imagine you have a queryable source that you send to a component that then applies ordering, filtering, and sorting. How do you inspect the result to verify how the query was manipulated?

**What is your one piece of programming advice?**

After decades of building software my biggest piece of advice is that your first step shouldn’t be to find the library or framework or tool, but to solve the problem.

Too often people add frameworks or tools or patterns because they’re recommended, rather than actually determining if they add value. Many times the overhead outweighs the benefit. I’m often told, “That solution isn’t right because you don’t have a business logic layer.” My response is, “So?” It’s not that I don’t see value in that layer for certain implementations, but that it’s not always necessary.

Have you worked on that project that forced an architecture so that one change involves updating five projects and the majority of them are just default “pass through” implementations? I am a fan of solving for the solution, and only then if you find some code is repeated, refactor. Don’t over-engineer or complicate. One of my favorite starting points for a solution is to consider, “What is the ideal way I’d like to code for this?”

This is just a small portion of my interview with Jeremy. There is [so much more at my site](https://daveabrock.com/2020/07/25/dev-discussions-jeremy-likness-1), including Jeremy's unusual path and a crazy story about his interview!

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* We’re getting so close to .NET 5: Jeremy Likness [announces EF Core 5 Preview 7](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-ef-core-5-0-preview-7/), Richard Lander [announces .NET 5 Preview 7](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-7/), and Microsoft also announces [ASP.NET Core updates in .NET 5 Preview 7](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-7/).
* The .NET Foundation [has begun the voting process for board elections](https://dotnetfoundation.org/blog/2020/07/20/director-election-2020-voting-starts-tomorrow).
* Jon Hilton [asks if Blazor EditForms is essential or too much magic](https://jonhilton.net/why-use-blazor-edit-forms/).

### 📢 Announcements

* Kayla Cinnamon [announces the release of Windows Terminal Preview 1.2](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-2-release/).
* Tara Overfield [announces the .NET Framework July 2020 update preview](https://devblogs.microsoft.com/dotnet/net-framework-july-2020-cumulative-update-preview/).
* It looks like [many-to-many is now working in the EF daily builds](https://github.com/dotnet/efcore/issues/19549#issuecomment-663237077).
* The Azure Service Fabric 7.1 [announced a second refresh release](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-1-second-refresh-release/ba-p/1534246).
* The Windows team [announced the first update to the Windows Package Manager](https://devblogs.microsoft.com/commandline/windows-package-manager-preview-v0-1-41821/).
* Daniel Jurek [talks about the July release of the Azure SDK](https://devblogs.microsoft.com/azure-sdk/azure-sdk-release-july-2020/).

### 📅 Community and events

* SciFiDevCon is [coming next week](https://www.scifidevcon.com/).
* We had three community standups this week: Desktop [discusses EF Core 5 updates](https://www.youtube.com/watch?v=4QnbYkQ1WNw), Entity Framework [discusses scaffolding with Handlebars](https://www.youtube.com/watch?v=6Ux7EpgiWXE), and ASP .NET [discusses web tools with Sayed Hashimi](https://www.youtube.com/watch?v=j4yIHg9l4QQ).
* The DotNet Docs Show talks [.NET desktop app development with Olia Gavrysh](https://www.youtube.com/watch?v=mWhSgasKVhE).

### 😎 Blazor

* Lee Richardson [compares Blazor WASM with Angular](http://www.leerichardson.com/2020/07/blazor-webassembly-vs-angular-client.html).
* Ramkumar Shanmugam [discusses how to use bUnit for Blazor and integrating it into your Azure pipeline](https://www.syncfusion.com/blogs/post/bunit-for-blazor-and-how-to-integrate-it-in-azure-pipeline.aspx).
* Eilon Lipton [provides a monthly update for hybrid Blazor apps in Mobile Blazor Bindings](https://devblogs.microsoft.com/aspnet/hybrid-blazor-mobile-blazor-bindings-july-update/).
* Marinko Spasojevic [discusses Blazor WebAssembly forms, form validation, and @ref directives](https://code-maze.com/blazor-webassembly-forms-form-validation/).
* Andrea Chiarelli [secures Blazor WebAssembly apps](https://auth0.com/blog/securing-blazor-webassembly-apps/).
* Marinko Spasojevic [sorts in Blazor WebAssembly and ASP.NET Core Web API](https://code-maze.com/blazor-webassembly-sorting/).
* Matthew Jones [builds Conway's Game of Life in C# and Blazor WASM](https://exceptionnotfound.net/conways-game-of-life-with-emojis-in-csharp-and-blazor-webassembly).

### 🚀 .NET Core

* Michal Bialecki [merges migrations in EF 5](http://www.michalbialecki.com/2020/07/24/merging-migrations-in-entity-framework-core-5/).
* David Grace [discusses creating your own logging provider to log to text files in .NET Core](https://www.roundthecode.com/dotnet/create-your-own-logging-provider-to-log-to-text-files-in-net-core).
* Thomas Levesque [works through ASP.NET Core 3, IIS, and empty HTTP headers](https://thomaslevesque.com/2020/07/23/aspnet-core-iis-and-empty-http-headers/).
* Carl Rippon [does integration testing on ASP.NET Core Web API controllers with SQL](https://www.carlrippon.com/integration-testing-on-asp.net-web-api-controllers-with-a-sql-backend/).
* Michal Bialecki [adds EF Core 5 migrations to a .NET 5 project](http://www.michalbialecki.com/2020/07/20/adding-entity-framework-core-5-migrations-to-net-5-project).
* Jon P. Smith [talks tips and techniques for configuring EF Core](https://www.thereformedprogrammer.net/ef-core-in-depth-tips-and-techniques-for-configuring-ef-core/).
* Jason Gaylord [talks about adding Newtonsoft.JSON back to .NET Core 3.1 and later](https://www.jasongaylord.com/blog/2020/07/17/adding-newtonsoft-json-to-services).
* David Grace [uses hosted services in ASP.NET Core to create a "most viewed" background service](https://www.roundthecode.com/dotnet/hosted-services-asp-net-core-create-a-most-viewed-background-service).

### ⛅ The cloud

* Damien Bowden [waits for Azure Durable Functions to complete](https://damienbod.com/2020/07/24/waiting-for-azure-durable-functions-to-complete/).
* Steve Fenton [talks about how to start and stop an Azure App Service on a schedule with Azure Logic Apps](https://www.stevefenton.co.uk/2020/07/start-and-stop-an-azure-app-service-on-a-schedule-with-azure-logic-apps/).
* Angie Doyle [provides a monthly update on the Azure Portal](https://techcommunity.microsoft.com/t5/azure-portal/azure-portal-july-2020-update/ba-p/1538786).
* Chris Noring [learns durable functions with .NET Core and C#](https://techcommunity.microsoft.com/t5/apps-on-azure/learn-durable-functions-with-net-core-and-c-stateful-serverless/ba-p/1533559).
* Jason Gaylord [creates an Alexa skill using .NET Core and Azure](https://www.jasongaylord.com/blog/2020/07/21/creating-alexa-skill-using-dotnet-azure).
* Daniel Krzyczkowski [talks event sourcing with Azure SQL and EF Core](https://daniel-krzyczkowski.github.io/Event-Sourcing-With-Azure-SQL-And-Entity-Framework-Core/).
* Aaron Powell [continues working with GraphQL on Azure](https://www.aaron-powell.com/posts/2020-07-21-graphql-on-azure-part-2-app-service-with-dotnet/).
* Microsoft appears to be [working on an Azure-powered cloud PC service](https://www.zdnet.com/article/microsoft-is-working-on-an-azure-powered-cloud-pc-service/#ftag=RSSbaffb68).
* Steve Gordon [introduces Docker ECS integration for AWS](https://www.stevejgordon.co.uk/dotnet-on-aws-introducing-docker-ecs-integration).
* Itay Podhajcer [deploys a serverless RabbitMQ cluster on Azure with .NET](https://www.pulumi.com/blog/rabbitmq-azure/).
* Damien Bowden [uses Azure Key Vault and managed identities with Azure Functions](https://damienbod.com/2020/07/20/using-key-vault-and-managed-identities-with-azure-functions/).
* Azure Tips and Tricks [has a new article about Azure Functions and secure configuration with Azure Key Vault](https://microsoft.github.io/AzureTipsAndTricks/blog/tip271.html).
* Over at AWS, Steve Roberts [talks about AWS X-Ray for tracing distributed .NET apps](https://aws.amazon.com/blogs/developer/a-new-more-simplified-setup-for-x-ray-tracing-of-net-applications/).

### 📔 Languages

* Josef Ottosson [talks about how C# 9 records will change his life](https://josef.codes/how-csharp-records-will-change-my-life/).
* Derek Comartin [wants you to stop throwing exceptions and start being explicit](https://codeopinion.com/stop-throwing-exceptions-start-being-explicit).
* Khalid Abuhakmeh [has fun with LINQ expression visitors](https://khalidabuhakmeh.com/linq-expression-visitors).
* Dave Brock [goes on a C# 9 scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt).
* Jeremy Likness [looks behind the IQueryable curtain](https://blog.jeremylikness.com/blog/look-behind-the-iqueryable-curtain/).
* Jonathan Allen [talks about lambda improvements with C# 9](https://www.infoq.com/news/2020/07/CSharp-Lambdas).
* Anand Gupta [dives deep on .NET garbage collection](https://hackernoon.com/net-garbage-collection-here-we-go-gl1q3uui?source=rss).
* Jonathan Allen [talks about range operators and pattern-matching in C# 9](https://www.infoq.com/news/2020/07/CSharp-9-Range-Patterns).
* Ian Griffiths [talks about supporting older runtimes with C# 8 nullable references](https://endjin.com/blog/2020/07/dotnet-csharp-8-nullable-references-supporting-older-runtimes.html).
* Jonathan Channon [talks about applicatives and custom operators in F#](https://blog.jonathanchannon.com/2020-07-17-understanding-fsharp-applicatives-custom-operators/).

### 🔧 Tools

* Khalid Abuhakmeh [writes Xamarin.Mac Apps With JetBrains Rider](https://khalidabuhakmeh.com/write-xamarin-mac-apps-with-jetbrains-rider).
* Michael Shpilt [offers some tricks on working with the Immediate Window in Visual Studio](https://michaelscodingspot.com/visual-studio-immediate-window/).
* Adam Storr [loves Resharper 2](https://adamstorr.azurewebsites.net/blog/why-i-love-resharper-2).
* Joseph Guadagno [debugs Azure Function Event Grid triggers locally with JetBrains Rider](https://www.josephguadagno.net/2020/07/20/debugging-azure-function-event-grid-trigger-locally-with-jetbrains-rider).
* Over at the NDepend blog, [some tips for working with Visual Studio files and layouts](https://blog.ndepend.com/10-visual-studio-files-and-layout-productivity-tips/).
* j2i.net does a review [of the Windows Terminal Preview](https://blog.j2i.net/2020/07/18/windows-terminal-preview/).
* Scott Hanselman [explores RepoDB, a .NET open source hybrid ORM library](https://www.hanselman.com/blog/ExploringTheNETOpenSourceHybridORMLibraryRepoDB.aspx).

### 📱 Xamarin

* David Ortinau [draws UI with Xamarin.Forms shapes and paths](https://devblogs.microsoft.com/xamarin/xamarin-forms-shapes-and-paths/).
* Stefan Nenchev [builds a sample app with Xamarin.Forms, Telerik UI, MVVMFresh, and EF Core](https://www.telerik.com/blogs/todo-sample-application-xamarin-forms-telerik-ui-mvvmfresh-microsoft-entityframeworkcore).
* Charlin Agramonte [works through multilingual support in Xamarin.Forms](https://devblogs.microsoft.com/xamarin/mastering-multilingual-in-xamarin-forms/).
* Rendy Del Rosario [creates a Xamarin Binding Library for iOS And Android](https://www.xamboy.com/2020/07/20/creating-a-xamarin-binding-library-for-ios-and-android-part-1/).
* Nick Randolph [says: Xamarin.Forms is not just a XAML platform](https://nicksnettravels.builttoroam.com/dotnetmaui-not-a-xaml-platform).
* The Xamarin Podcast [talks about Xamarin.Forms 4.7](https://devblogs.microsoft.com/xamarin/xamarin-podcast-inverter-converter/).

### 🎤 Podcasts

* The .NET Rocks podcast [has a lively discussion with Sebastien Lambla about Microsoft's role in the OSS ecosystem](https://www.dotnetrocks.com/default.aspx?ShowNum=1697).
* The Microsoft 365 Developer Podcast [discusses using Blazor to create Microsoft Teams tabs](https://www.m365devpodcast.com/e/using-blazor-to-create-microsoft-teams-tabs-with-thomy-golles/).
* The 6-Figure Developer podcast [talks with Edward Thomson about GitHub Actions](https://6figuredev.com/podcast/episode-153-github-actions-with-edward-thomson/).
* The Coding Blocks Podcast [talks about architecting for low-risk releases](https://www.codingblocks.net/podcast/the-devops-handbook-architecting-for-low-risk-releases/).
* At Technology and Friends, David Giard [talks with Dave Pine about .NET 5 and the Docs team](http://davidgiard.com/2020/07/20/DavePineOnNET5AndTheDocsTeam.aspx).
* The Azure DevOps Podcast [talks with Jimmy Bogard about AutoMapper and MediatR](http://azuredevopspodcast.clear-measure.com/jimmy-bogard-on-automapper-and-mediatr-episode-98).


### 🎥 Videos

* The Xamarin Show [discusses shapes, paths, and app themes in 4.7](https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-47-Shapes-Paths-and-App-Themes).
* Data Exposed [talks about automated backups for Azure SQL](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Automated-Backups-Part-1).
