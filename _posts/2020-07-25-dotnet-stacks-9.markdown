---
date: "2020-07-25"
title: "The .NET Stacks #9: Project Coyote, new Razor editor, and more!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-9-card.png
subtitle: We discuss Project Coyote, a new Visual Studio Razor editor, and more!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

So, what do you think? I changed newsletter providers and got a new look (thanks to [@comfycoder](https://twitter.com/comfycoder) for the logo help). I hope you like it. It works on my machine, and I hope it works on yours too. 😎

Any suggestions? [Hit me up on Twitter](https://twitter.com/daveabrock) or just reply to this e-mail. I'm all ears on what content you'd like—I want to make this the best part of your Monday!

This week we're talking about debugging async with Coyote, an exciting new Razor editor in Visual Studio, a fun look at unit testing, and more!

## Debug async issues faster with Coyote

Isn't async programming fun? It's so much fun, you can probably relate to this. (If you've spent months on a single concurrency issue, it might also make you cry.)

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Roses are red<br>And so are you<br>Violets are blue<br>Asynchronous operations are great</p>&mdash; I Am Devloper (@iamdevloper) <a href="https://twitter.com/iamdevloper/status/1096059902101868544?ref_src=twsrc%5Etfw">February 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

All jokes aside, it's a concurrent world and we just live in it. Our customers *demand* it. In this microservice-friendly, cloud-first world, we want things fast and efficient at the lowest cost possible. In the cloud, minimizing cost requires high throughput, which requires high concurrency. If you haven't pulled your hair out from a concurrency/threading issue, to that I say: welcome to our field. I hope you had a nice graduation party.

We know this is hard, and that's why we rely on the .NET framework to handle most of the magic for us. With the [Task Parallel Library](https://docs.microsoft.com/dotnet/standard/parallel-programming/task-parallel-library-tpl) (TPL) and async/await, we don't have to worry about thread scheduling, state management, or other hard low-level details.

However, we also get flexibility with APIs like [Task.Run](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run?view=netcore-3.1) that give us the power to queue work—if we don't know exactly what we're doing, we can get in loads of trouble. Even with high code coverage, powerful integration testing, and stress testing, we still know we're not too far from a concurrency issue that will cost our customers (and our employers) tons of time and money.

Meet Coyote, a set of tools that help you find these seemingly impossible-to-reproduce bugs quicker. (It is an [evolution of the P# project](https://github.com/p-org/PSharp), and is moving out of research mode.)

How does this work? After you write code that uses the Coyote programming model—the familiar task-based model in in preview—you can run `coyote test`, where the magic begins. When you run this, Coyote manages task scheduling and explores all the complexities of your code. Coyote runs tests repeatedly with "different scheduling choices" each time. If a bug is found, Coyote reports a reproducible bug trace, which can be replayed until you find the bug.

Coyote has been put through its paces for [various Azure services](https://microsoft.github.io/coyote/case-studies/azure-batch-service) and promises integration with unit testing frameworks and minimal overhead. Is it worth it for internal CRUD apps? Maybe not. But if you are operating at tremendous scale where concurrency issues keep you up at night, it might be a game-changer.

There's so much to take in! Check out the resources for more details on Project Coyote.

* [Project Coyote site](https://microsoft.github.io/coyote/)
* [Extreme programming meets systematic testing using Coyote](https://cloudblogs.microsoft.com/opensource/2020/07/14/extreme-programming-meets-systematic-testing-using-coyote/)
* [GitHub repo](https://github.com/microsoft/coyote/)
* [ON.NET - Reliable Async Systems with Coyote](https://channel9.msdn.com/Shows/On-NET/Reliable-Async-Systems-with-Coyote-Part-1)

## New Razor editor for Visual Studio in preview

It seems like we're constantly getting news of a new Visual Studio preview feature, and this week is no exception. If you download Visual Studio 2019 16.7 Preview 4 (got all that?) you can [use a brand new Razor editor](https://devblogs.microsoft.com/aspnet/new-experimental-razor-editor-for-visual-studio/) to assist your local development with Blazor, MVC, and Razor Pages.

Most of us know Razor but if you don't: it's a templating language (using HTML and, most frequently, C#) you use to define how you dynamically render content in your ASP.NET app. With Blazor, you can reuse UI components using *.razor* files. Razor in Visual Studio comes with IntelliSense, completions, and the like. Why a new editor, then?

Today, Visual Studio does a lot of tricks behind the scenes with projection buffers for the different languages it supports. For example, if you have C# and HTML in a Razor file, Visual Studio reads the Razor file from disk, sends the code to a Razor buffer, a C# buffer, and an HTML buffer. Finally, each buffer uses its own independent language service for each language you use. *Oof.*

Why should you care? If you've signed a lifetime contract to use Visual Studio and you're a team of one, fine—but if you want extensibility with Codespaces or Live Share in VS, you're in trouble. All these dependencies require a lot of work to coordinate and can hinder the speed of future improvements.

To address this, Microsoft has been working on a Razor Language Server that implements the features you know and love through a common protocol. Wherever the extension exists (VS, VS Code, wherever), it coordinates with the Razor Language Server.

This solution is currently used for Razor support in VS Code, and will be used for Codespaces and LiveShare for Visual Studio. And now, you can try the VS implementation today (in preview). It has some rough edges, but I look forward to see how it performs. I do know Razor mostly works well but can exhibit some inconsistent performance. Will this help?

## 🌎 Last week in the .NET world

I'm trying a new format this week so you can sift through the links easier.

### 🔥 The Top 3

* Michael Shpilt [discusses how assemblies load in C# and .NET](https://michaelscodingspot.com/assemblies-load-in-dotnet/).
* Daniel Roth [discusses a new experimental Razor editor for Visual Studio](https://devblogs.microsoft.com/aspnet/new-experimental-razor-editor-for-visual-studio/).
* Steve Gordon [looks at how the ASP.NET Core middleware pipeline is built](https://www.stevejgordon.co.uk/how-is-the-asp-net-core-middleware-pipeline-built).

### 📢 Announcements

* Microsoft [announces the release of WinUI 3 Preview 2](https://blogs.windows.com/windowsdeveloper/2020/07/15/announcing-winui-3-preview-2).
* Take a look at [the .NET Core](https://devblogs.microsoft.com/dotnet/net-core-july-2020/) and [.NET Framework](https://devblogs.microsoft.com/dotnet/net-framework-july-2020-security-and-quality-rollup-updates/) security updates for July 2020.
* We have [a new version of the Azure CLI, v 2.9](https://techcommunity.microsoft.com/t5/azure-tools/introducing-azure-cli-v2-9-with-improved-performance-and-user/ba-p/1522141).
* Microsoft [introduces Coyote](https://cloudblogs.microsoft.com/opensource/2020/07/14/extreme-programming-meets-systematic-testing-using-coyote/).
* Microsoft [introduces C# markup for Xamarin.Forms](https://devblogs.microsoft.com/xamarin/c-sharp-markup-for-xamarin-forms/).
* Ruben Rios [walks through authentication experience improvements in Visual Studio 2019 16.6](https://devblogs.microsoft.com/visualstudio/improving-the-authentication-experience-for-enterprises-leveraging-conditional-access-policies/).
* Stephen Toub [does a rundown of performance improvements coming in .NET 5](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5/).

### 📅 Community and events

* All the .NET Foundation nominees [were interviewed this week](https://www.youtube.com/playlist?list=PL1rZQsJPBU2Qjz-agkBKHon6BRnqlsoPN).
* The *.NET Conf - Focus on Microservices* event is on July 30, [and the sessions and speakers are live](https://focus.dotnetconf.net/).
* The DotNet Docs Show [talks to Jeremy Sinclair](https://www.youtube.com/watch?v=Crd-sP7ZJEU).
* Two standups this week: [Visual Studio](https://www.youtube.com/watch?v=vtXLzbsbF7E) and [ASP.NET](https://www.youtube.com/watch?v=QuAdLWrv4HA).

### 😎 Blazor

* Marco Dalla Libera [walks through the MVVM pattern in Blazor for state management](https://www.syncfusion.com/blogs/post/mvvm-pattern-in-blazor-for-state-management.aspx).
* Marinko Spasojevic [walks through Blazor WASM searching with ASP.NET Core web API](https://code-maze.com/blazor-webassembly-searching/), as well as [pagination](https://code-maze.com/blazor-webassembly-pagination/).

### 🚀 .NET Core

* David Grace [creates CRUD API endpoints with ASP.NET Core and Entity Framework](https://www.roundthecode.com/dotnet/create-crud-api-endpoints-with-asp-net-core-entity-framework).
* Neel Bhatt [offers a primer on built-in dependency injection in ASP.NET Core](https://neelbhatt.com/2020/07/13/how-does-the-built-in-dependency-injection-work-on-asp-net-core/).
* The Pro Code Guide [discusses microservices with ASP.NET Core 3.1](https://procodeguide.com/programming/microservices-asp-net-core) and [reads configuration values in ASP.NET Core](https://procodeguide.com/programming/read-configuration-values-asp-net-core).
* Michal Bialecki [adds EF Core 5 to an existing database](http://www.michalbialecki.com/2020/07/17/adding-an-entity-framework-core-5-to-an-existing-database).
* Jason Farrell [uses the Management API with .NET Core 3.1](https://jfarrell.net/2020/07/10/using-the-management-api-with-net-core-3-1/).
* Khalid Abuhakmeh [sorts data with ASP.NET Core and query strings](https://khalidabuhakmeh.com/sort-data-with-aspnet-core-and-query-strings).
* Rick Strahl [handles SPA fallback paths in a generic ASP.NET Core server](https://weblog.west-wind.com/posts/2020/Jul/12/Handling-SPA-Fallback-Paths-in-a-Generic-ASPNET-Core-Server).
* A nice discussion [on some Entity Framework 3.1 gotchas](https://www.pmichaels.net/2020/07/11/entity-framework-3-1-gotchas).
  
### ⛅ The cloud

* Steve Gordon [pushes a .NET Docker image to Amazon ECR](https://www.stevejgordon.co.uk/dotnet-on-aws-pushing-a-dotnet-docker-image-to-amazon-ecr).
* Aaron Powell [starts a series on GraphQL on Azure](https://www.aaron-powell.com/posts/2020-07-13-graphql-on-azure-part-1-getting-started/).
* Daniel Krzyczkowski [discusses async messaging with Azure Service Bus](https://daniel-krzyczkowski.github.io/Asynchronous-Messaging-With-Azure-Service-Bus/).
* Ed Freeman [uses Azure Key Vault for encryption in C#](https://endjin.com/blog/2020/07/using-azure-key-vault-for-encryption-in-csharp-a-simple-tutorial.html).

### 📔 Languages

* Dave Brock [talks about target typing and covariant returns in C# 9](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants), and [answers some C# 9 questions](https://daveabrock.com/2020/07/17/c-sharp-9-q-and-a).
* Jay Krishna Reddy [has some tips to write clean C# code](https://www.c-sharpcorner.com/article/tips-to-write-clean-c-sharp-code/).
* Matthias Koch [bypasses polymorphism with reflection in .NET](https://ithrowexceptions.com/2020/07/13/bypassing-polymorphism-with-reflection-in-dotnet.html).
* Ian Griffiths [discusses DisallowNull in C# 8](https://endjin.com/blog/2020/07/dotnet-csharp-8-nullable-references-more-type-system-transcendence-with-disallownull.html).
* Urs Enzler [discusses his journey to learning F# basics](https://www.planetgeek.ch/2020/07/14/our-journey-to-f-learning-the-basics-of-f/).
* Ian Russell [begins a series on web programming in F#](https://www.softwarepark.cc/blog/2020/7/13/introduction-to-web-programming-in-f-with-giraffe-part-1).

### 🔧 Tools

* Patrick Smacchia [offers his 10 Visual Studio navigation productivity tips](https://blog.ndepend.com/10-visual-studio-navigation-productivity-tips/).
* Adam Bertram [customizes the Microsoft Terminal](https://petri.com/how-to-customize-the-microsoft-terminal).

### 📱 Xamarin

* Leomaris Reyes [learns about SwipeView](https://www.telerik.com/blogs/learning-swipeview-xamarin-forms).
* David Ortinau [discusses new app themes](https://devblogs.microsoft.com/xamarin/app-themes-xamarin-forms/).
* Delpin Susai Raj [debugs WebView](https://xamarinmonkeys.blogspot.com/2020/07/xamarinforms-debugging-webview.html).
* Selva Ganapathy Kathiresan [discusses the Xamarin.Forms signature pad control](https://www.syncfusion.com/blogs/post/the-all-new-xamarin-forms-signature-pad-control-is-here.aspx).
* Leomaris Reyes [replicates real estate property details](https://askxammy.com/replicating-real-estate-property-details-ui-in-xamarin-forms/).
* Mark Allibone [creates a login flow with Xamarin Forms Shell](https://mallibone.com/post/xamarin-forms-shell-login).
* Brandon Minnick [improves CollectionView scrolling](https://codetraveler.io/2020/07/12/improving-collectionview-scrolling/).
* Daniel Hindrikes [builds a styleable content button using PancakeView](https://danielhindrikes.se/index.php/2020/07/14/building-a-styleable-content-button-with-pancakeview/) *and* [releases a new version of TinyMvvm](https://danielhindrikes.se/index.php/2020/07/13/tinymvvm-2-4-1/).

### 🎤 Podcasts

* The Stack Overflow Podcast asks: [is Scrum making you a worse engineer?](https://the-stack-overflow-podcast.simplecast.com/episodes/is-scrum-making-you-a-worse-engineer-eozLH_Cq)
* The .NET Rocks podcast [discusses C# 9 with Mads Torgersen](https://www.dotnetrocks.com/default.aspx?ShowNum=1696).
* The .NET Core Podcast [talks with Alexey Golub about integrating with external APIs](https://dotnetcore.show/episode-55-working-with-external-apis-with-alexey-golub/).

### 🎥 Videos

* Data Exposed [discusses Azure SQL and IoT](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-and-IoT).
* ON.NET discusses reliable async systems with Coyote in [two](https://channel9.msdn.com/Shows/On-NET/Reliable-Async-Systems-with-Coyote-Part-2) [parts](https://channel9.msdn.com/Shows/On-NET/Reliable-Async-Systems-with-Coyote-Part-1).
* The ASP.NET Monsters [discuss record types in C# 9](https://www.youtube.com/watch?v=fEQbwwcoBuo).
* Data Exposed [talks about Advanced Threat Protection in Azure SQL](https://channel9.msdn.com/Shows/Data-Exposed/The-Benefits-of-Advanced-Threat-Protection-in-Azure-SQL-Database).
