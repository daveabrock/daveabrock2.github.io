---
date: "2020-08-29"
title: "The .NET Stacks #14: Checking in on NuGet changes, many-to-many in EF Core, community roundup, and more!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-14-card.png
subtitle: We discuss many-to-many in EF Core 5, check in with upcoming NuGet changes, and look at the busy week in the .NET community!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday. Do you ever [watch a satire and think it's a documentary](https://www.youtube.com/watch?v=y8OnoxKotPQ)?

This week, we'll:

* Get excited about many-to-many in EF Core
* Take a look at some current and upcoming NuGet changes
* Check in on the community

## Many-to-many in EF Core 5

So this is exciting: many-to-many support is now included in the Entity Framework Core daily builds, and the team [spent this week's community standup showing it off](https://www.youtube.com/watch?v=W1sxepfIMRM).

A big part of this work includes the concept of skip navigations, or many-to-many navigation properties. For example, here's a basic model that assigns links in a newsletter (what can I say, it's fresh on my mind) and is a good use case for many-to-many and is ... [heavily inspired and/or outright stolen](https://github.com/dotnet/efcore/issues/19003) from the GitHub issue:

```csharp
public class Link
{
    public int LinkId { get; set; }
    public string Url { get; set; }
    public string Description { get; set; }

    public List<LinkTag> LinkTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }
    public string Description { get; set; }

    public List<LinkTag> LinkTags { get; set; }
}

public class LinkTag
{
    public int LinkId { get; set; }
    public Link Link { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }

    public DateTime LinkCreated { get; set; }
}
```

So, to load a link and its tags, I'd need to use two navigation properties, and refer to the joining table when performing queries:

```csharp
var linksAndTags
    = context.Links
        .Include(e => e.LinkTags)
        .ThenInclude(e => e.Tag)
        .ToList();
```

With many-to-many in EF Core 5, I can skip over the join table and use direct navigation properties. We can instead get a little more direct:

```csharp
public class Link
{
    public int LinkId { get; set; }
    public string Description { get; set; }

    public List<Tag> Tags { get; set; } // Skips right to Tag
    public List<LinkTag> LinkTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }
    public string Description { get; set; }

    public List<Link> Links { get; set; } // Skips right to Link
    public List<LinkTag> LinkTags { get; set; }
}
```

Look how easy querying is now:

```csharp
var linksAndTags 
    = context.Links
        .Include(e => e.Tags)
        .ToList();
```

In the standup, Arthur Vickers mentioned an important fact that's easy to overlook: while the release timing is similar, EF Core 5 is not bound to .NET 5. It targets .NET Standard 2.1, so can be used in any .NET Core 3.*x* apps (and of course is available for use in .NET 5). Take a look at [the docs](https://docs.microsoft.com/dotnet/standard/net-standard) to learn more about .NET Standard compatibility.

## Checking in on NuGet changes

This week, the .NET Tooling community standup [brought in the NuGet team](https://www.youtube.com/watch?v=fijQkqWssn0) to talk about what they're working on. They mentioned three key things: UX enhancements to *nuget.org*, improvements to package compatibility in Visual Studio, and better README support for package authors.

The team has been working on improving the experience on nuget.org—in the last few weeks, they've blogged about how you can [use advanced search](https://devblogs.microsoft.com/nuget/advanced-search-on-nuget-org/) and also [view dependent packages](https://devblogs.microsoft.com/nuget/view-dependent-packages-on-nuget-org/). While I prefer to work with packages in my IDE, you can hit up *nuget.org* for a better view, but it's hard to filter through the noise. The new filtering options are a welcome improvement: I can filter by dependencies, tools, templates, relevance, downloads, and prereleases. Whether I value stability or finding out what's new, the experience is a lot better now.

I was most interested to hear about tooling support, as that's how a lot of us use NuGet most. The team discussed upcoming changes to floating version support. Floating version support means you're using the latest version of a range you specify: for example, if you say you want version `2.*` of a package, and it rolled out `1.0`, `2.0`, and `2.1` versions, you're using `2.1`. Soon, you'll be able to specify this in the `Version` field in the NuGet UI—preventing you from hacking away from the project file. As a whole, the `Version` field will allow more flexibility and not just be a drop-down.

Lastly, if you author NuGet packages, README links will no longer be a post-package upload but built right into the Visual Studio Package Properties. That will allow you to easily have your package documentation on *nuget.org*, and the NuGet UI in Visual Studio will have a link to it.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Matt Mitchell [does a deep dive on how .NET builds and ships](https://devblogs.microsoft.com/dotnet/a-deep-dive-into-how-net-builds-and-ships/).
* Jeremy Likness [inspects and mutates IQueryable expression trees](https://blog.jeremylikness.com/blog/inspect-and-mutate-iqueryable-expression-trees/).
* Matthew Jones [uses conditional C# LINQ clauses to make a multiple-input search engine](https://exceptionnotfound.net/using-conditional-csharp-linq-clauses-to-make-a-multiple-input-search-engine).

### 📢 Announcements

* Phillip Carter [is looking for feedback on the .NET project system in Visual Studio](https://devblogs.microsoft.com/dotnet/help-us-improve-visual-studio-project-tooling-for-net-core/).
* Jon Gallant [talks about the August release of the Azure SDKs](https://devblogs.microsoft.com/azure-sdk/azure-sdk-release-august-2020/), and Jianghao Lu [shows us what's new in the Azure Identity release](https://devblogs.microsoft.com/azure-sdk/azure-identity-august-2020-ga/).
* Azure Cosmos DB serverless [is now in preview](https://azure.microsoft.com/updates/serverless-offer-for-azure-cosmos-db-now-in-public-preview/).
* You can now [view dependent packages on nuget.org](https://devblogs.microsoft.com/nuget/view-dependent-packages-on-nuget-org/).
* AWS SDK for .NET v3.5 [is generally available](https://aws.amazon.com/blogs/developer/aws-sdk-for-net-v3-5-now-generally-available).
* Mads Kristensen [announces his new stream, where he writes or updates Visual Studio extensions](https://devblogs.microsoft.com/visualstudio/live-coding-visual-studio-extensions/).

### 📅 Community and events

* The .NET Docs Show [talks with Rachel Appel](https://www.youtube.com/watch?v=d7wtGgaNeS4).
* We had three community standups this week: .NET tooling [talks about NuGet improvements](https://www.youtube.com/watch?v=fijQkqWssn0), Entity Framework [talks many-to-many](https://www.youtube.com/watch?v=W1sxepfIMRM), and ASP.NET [talks .NET 5 stuff](https://www.youtube.com/watch?v=IQLEjj7v4io).

### 😎 Blazor

Jon Hilton [compares Blazor and Vue](https://www.telerik.com/blogs/blazor-vs-vue-web-developers), and also [works with client-side Blazor](https://www.telerik.com/blogs/how-to-work-with-client-side-blazor).

### 🚀 .NET Core

* Tomasz Pęczek [supports encrypted content encoding in .NET Core](https://www.tpeczek.com/2020/08/supporting-encrypted-content-encoding.html).
* The Code Maze blog [works with IncludeMembers and custom projections with AutoMapper in ASP.NET Core](https://code-maze.com/automapper-net-core-custom-projections/), [discusses how to implement SignalR automatic reconnect with Angular](https://code-maze.com/signalr-automatic-reconnect-option/), and [adds gRPC to an ASP.NET Core project and MongoDB](https://code-maze.com/grpc-aspnet-mongodb/).
* Gunnar Peipman [logs NHibernate SQL to ASP.NET Core loggers](https://gunnarpeipman.com/aspnet-core-nhibernate-log-sql/).
* Damien Bowden [works through symmetric and asymmetric encryption in .NET Core](https://damienbod.com/2020/08/19/symmetric-and-asymmetric-encryption-in-net-core/).
* Andrew Lock [controls the IHostedService execution order in ASP.NET Core 3.x](https://andrewlock.net/controlling-ihostedservice-execution-order-in-aspnetcore-3/).
* ErikEJ [maps and uses SQL Server stored procedures with EF Core Power Tools](https://erikej.github.io/efcore/2020/08/10/ef-core-power-tools-stored-procedures.html), and [uses SQL Server Compact 4 with .NET Core 3.1 on Windows](https://erikej.github.io/sqlce/2020/08/17/netcore-sql-compact.html).


### ⛅ The cloud

* Matias Quaranta [works with HttpClientFactory in the Azure Cosmos DB .NET SDK](https://devblogs.microsoft.com/cosmosdb/httpclientfactory-cosmos-db-net-sdk/).
* At the Azure Community Blog, [a look at Dapr](https://techcommunity.microsoft.com/t5/azure-developer-community-blog/dapr-a-small-revolution-in-the-microservices-world/ba-p/1595397).
* Will Velida [explores Azure Cosmos DB serverless](https://dev.to/willvelida/exploring-azure-cosmos-db-serverless-46j3).
* Dominique St-Amand [uses path-based routing in Azure Application Gateway with Azure WebApps](https://www.domstamand.com/path-based-routing-in-azure-application-gateway-with-azure-webapps/).
* Damien Bowden [secures Azure Functions using API keys](https://damienbod.com/2020/08/17/securing-azure-functions-using-api-keys/).
* Jason Farrell [keeps going on Azure Durable Functions, with approving the upload](https://jfarrell.net/2020/08/16/durable-functions-part-3-approve-the-upload/).
* Daniel Krzyczkowski [uses event sourcing with Azure Cosmos SB change feed and Azure Functions](https://daniel-krzyczkowski.github.io/Event-Sourcing-With-Azure-Cosmos-Db-Change-Feed/).

### 📔 Languages

* Thomas Claudius Huber [talks about top-level statements in C# 9](https://www.thomasclaudiushuber.com/2020/08/18/c-9-top-level-statements-or-should-i-say-hey-wheres-the-main-method/).
* Anthony Giretti [makes options immutable in ASP.NET Core 5 and C# 9](https://anthonygiretti.com/2020/08/19/asp-net-core-5-make-your-options-immutable/), and [looks at native-sized integers in C# 9](https://anthonygiretti.com/2020/08/19/introducing-c-9-native-sized-integers/).
* Khalid Abuhakmeh [uses the select drop-down in ASP.NET Razor](https://khalidabuhakmeh.com/use-select-dropdown-in-aspdotnet-razor).
* Roland Weigelt [creates a thumbnail for video and image files in C#](https://weblogs.asp.net/rweigelt/ways-to-create-a-thumbnail-for-video-and-image-files-in-c).
* Gunnar Peipman [uses Structuremap in legacy ASP.NET MVC apps](https://gunnarpeipman.com/aspnet-mvc-structuremap-dependency-injection/).

### 🔧 Tools

* Trevor Sullivan [works with Visual Studio Code and Error Lens](https://trevorsullivan.net/2020/08/19/write-code-faster-using-visual-studio-code-error-lens/).
* Matthew MacDonald discusses [the design patterns programmers *really* use](https://medium.com/young-coder/the-design-patterns-programmers-really-use-c2e7790a900e).
* Dave Brock (ahem) [takes a look at Project Tye for microservices development](https://daveabrock.com/2020/08/19/microservices-with-tye-1).
* Mathankumar Rajendran [develops an ASP.NET Core app using VS Code](https://www.syncfusion.com/blogs/post/how-to-develop-an-asp-net-core-application-using-visual-studio-code.aspx).
* Mark Seemann says [unit testing is fine](https://blog.ploeh.dk/2020/08/17/unit-testing-is-fine/).
* Vinicius Apolinario [extracts a web app from IIS to create a new container image](https://techcommunity.microsoft.com/t5/itops-talk-blog/journey-of-an-app-how-to-extract-a-web-app-from-iis-to-create-a/ba-p/1590189).
* Rami Honig [discusses the top 10 open source NuGet tools for .NET development](https://oz-code.com/blog/net-c-tips/top-10-open-source-nuget-tools-net-development).

### 📱 Xamarin

* James Montemagno [talks about app first run detection with Xamarin.Essentials](https://devblogs.microsoft.com/xamarin/first-run-xamarin-essentials/).
* Matthew Leibowitz [works through templated controls](https://dotnetdevaddict.co.za/2020/08/16/templated-controls-in-xamarin-forms/).
* Jonathan Peppers [profiles Xamarin.Android startup](https://devblogs.microsoft.com/xamarin/performance-xamarin-android-apps/).
* David Britch [connects to localhost over HTTPS for (Xamarin) Android release builds](https://www.davidbritch.com/2020/08/connecting-to-localhost-over-https-for.html).
* Jesse Liberty [walks through the MonkeyCache library](http://jesseliberty.com/2020/08/16/monkeycache-step-by-step).

### 🎤 Podcasts

* At Developer Tea, [4 things you have to leave behind as a beginning engineer](https://developertea.simplecast.com/episodes/4-things-you-have-to-leave-behind-as-a-beginning-engineer-NlYLyLFT).
* The Stack Overflow podcast asks: [should managers of developers ever make technical decisions?](https://the-stack-overflow-podcast.simplecast.com/episodes/should-managers-of-developers-ever-make-technical-decisions-KhG_a_wb).
* The .NET Rocks podcast [talks with Philip Carter about F#](https://www.dotnetrocks.com/default.aspx?ShowNum=1701).
* The Azure DevOps Podcast [talks to Brady Gaster about SignalR and more](http://azuredevopspodcast.clear-measure.com/brady-gaster-on-signalr-and-more-episode-102).
* The Coding Blocks Podcast [keeps working on the DevOps Handbook, talking about anticipating problems](https://www.codingblocks.net/podcast/the-devops-handbook-anticipating-problems/).
* The 6 Figure Developer podcast [talks about BlazorCMS and SpeakerMeet](https://6figuredev.com/podcast/episode-157-blazorcms-speakermeet/).
* The No Dogma podcast [talks to Mads Torgersen about C# 9](https://no-dogma-podcast-301aeb97.simplecast.com/episodes/145-mads-torgersen-c-9-b6PqWza0).

### 🎥 Videos

* The ON.NET Show [adds a little Swagger to OData](https://channel9.msdn.com/Shows/On-NET/Adding-a-little-Swagger-to-OData).
* Data Exposed [improves query performance by managing statistics in Azure SQL](https://channel9.msdn.com/Shows/Data-Exposed/Improve-Query-Performance-by-Managing-Statistics-in-Azure-SQL).
* The Xamarin Show [talks about the new Drawing API](https://www.youtube.com/watch?v=hGrqgir4gqs).
* Brian Lagunas [asks whether to use ConfigureAwait True or False](https://brianlagunas.com/which-do-i-use-configureawait-true-or-false/).
