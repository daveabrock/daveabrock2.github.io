---
date: "2020-11-28"
title: "The .NET Stacks #27: Giving some 💜 to under-the-radar ASP.NET Core 5 features"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-27-card.png
subtitle: This week, we look at some under-the-radar ASP.NET Core 5 features, and look around the community.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*Note: This is the published version of my free, weekly newsletter, The .NET Stacks. It was originally sent to subscribers on November 23, 2020. Subscribe at the bottom of this post to get the content right away!*

Happy Monday to you all. With .NET 5 out the door and the holidays approaching, things aren't as crazy. To that end, I wanted to check in on some ASP.NET Core 5 features that might have flown under the radar—and as always, a busy week with our wonderful .NET community.

## Giving some 💜 to under-the-radar ASP.NET Core 5 features

As you may have heard, [.NET 5 is officially here](https://devblogs.microsoft.com/dotnet/announcing-net-5-0/). Last week, [we geeked out with .NET Conf](https://daveabrock.com/2020/11/21/dotnet-stacks-26). For .NET web developers, Microsoft is pushing Blazor on you *hard*. And for good reason: it's the future of ASP.NET and a game-changer in many ways. Bringing C# to the browser is a big deal.

As a result, Blazor is getting a great deal of the ASP.NET Core 5 attention. It's important to note that there are still tons of ASP.NET Core developers that aren't moving to Blazor for a variety of reasons. To be clear, I'm not saying that Microsoft is neglecting these developers—they are not. Also, since Blazor resides in the ASP.NET Core ecosystem, many Blazor-related enhancements help ASP.NET Core as a whole!

However, with all the attention paid to Blazor, a lot of other ASP.NET Core 5 enhancements may have flown under your radar. So this week, I'd like to spend some time this week focusing on non-Blazor ASP.NET Core 5 improvements.

(Also, while it's very much in flight, the .NET team [released the initial ASP.NET Core 6 roadmap](https://github.com/dotnet/aspnetcore/issues/27883) for your review.)

### ⛅ Azure app service supports latest .NET version automatically

You may have noticed that Azure App Service supported .NET 5 on Day 1—meaning when you eagerly downloaded the SDK, you were able to deploy your web apps to Azure App Service. This is thanks to an Early Runtime Feature, which now allows for automatic support of subsequent .NET releases as soon as they're released. You no longer have to patiently wait for latest-version support.

Before diving in, though, [get familiar with how it all works](https://aka.ms/app-service-early-access).

### 🆕 Model binding works with C# 9 record types

With [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), immutability is making a big imprint in the C# ecosystem. This can allow you to greatly simplify your need for "data classes"—a big use case is with models that contain properties and no behavior.

ASP.NET Core 5 now [can be used for model binding in MVC controllers or Razor Pages](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#model-binding-and-validation-with-c-9-record-types). I [wrote about it last week](https://daveabrock.com/2020/11/18/simplify-api-models-with-records). Look at how much simpler I can make things.

![Our title bar in action]({{ site.url }}{{ site.baseurl }}/assets/img/api-models.png)

### ✅ API improvements

I did discuss this a few weeks ago—and [also wrote about it last week](https://daveabrock.com/2020/11/16/httprepl-openapi-swagger-netcoreapis)—but when you create a Web API project in ASP.NET Core 5 (either from Visual Studio or the `dotnet` CLI), [OpenAPI is enabled by default](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#web-api). This includes adding the Swashbuckle package into your middleware automatically, which allows you to use Swagger UI out-of-the-box. In Visual Studio, just [hit F5 to explore your APIs](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#web-api).

Also, the `FromBody` attribute [now supports options for optional properties](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#custom-handling-of-authorization-failures), and [new JSON extension methods](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#custom-handling-of-authorization-failures) can help you write lightweight APIs.

### 🚀 HTTP/2 and gRPC performance improvements

In .NET 5, gRPC has been given the first-class treatment. If you [aren't familiar](https://docs.microsoft.com/aspnet/core/grpc/?view=aspnetcore-5.0), gRPC is a contract-first, open-source remote procedure call (RPC) framework that enables real-time streaming and end-to-end code generation—and leaves a small network footprint with its binary serialization.

With .NET 5, gRPC gets the [highest requests per second after Rust](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/). This is thanks to [reduced HTTP/2 in Kestrel](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/#http-2-allocations-in-kestrel), improvements [with reading HTTP headers](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/#http-2-allocations-in-kestrel), and support for [Protobuf message serialization](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/#protobuf-message-serialization).

We are anxiously awaiting Azure App Service and IIS support. Keep an eye on [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/9020) for updates.

### 🔐 AuthN and AuthZ improvements

There's been quite a few improvements with authentication and authorization.

First, Microsoft has developed a [new `Microsoft.Identity.Web` library](https://docs.microsoft.com/dotnet/api/microsoft.identity.web?view=azure-dotnet-preview) that allows you to handle authentication with Azure Active Directory. You can use this to access Azure resources in an easier way, including with Microsoft Graph. This [is supported](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#azure-active-directory-authentication-with-microsoftidentityweb) in ASP.NET Core 5.

Joseph Guadagno has written [some nice posts](https://www.josephguadagno.net/2020/08/29/working-with-microsoft-identity-configure-local-development) on the topic—and as a long-time subscriber of *The .NET Stacks*, you can trust his judgment. 😉

You can also [allow anonymous access to an endpoint](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#allow-anonymous-access-to-an-endpoint), [provide custom handling of authorization failures](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#custom-handling-of-authorization-failures), and [access authorization when using endpoint routing](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#custom-handling-of-authorization-failures).

### 📦 Container performance enhancements

Before .NET 5, when you published a Dockerfile you needed to pull the whole .NET Core SDK *and* the ASP.NET Core image. Now, the download size for the SDK is greatly reduced and the runtime image download is almost eliminated (only pulling the manifest). You can check out the GitHub issue to see [improvement numbers for various environments](https://github.com/dotnet/dotnet-docker/issues/1814#issuecomment-625294750).

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Mika Dumont [provides an update on .NET productivity (Roslyn)](https://devblogs.microsoft.com/dotnet/whats-new-in-net-productivity).
* The .NET Docs Show [talks to Irina Scurtu about REST in .NET](https://www.youtube.com/watch?v=xwh83W2VWlA).
* Abel Wang [continues writing about pulling off a Static Web App PR workflow for Azure App Service using Azure DevOps](https://devblogs.microsoft.com/devops/static-web-app-pr-workflow-for-azure-app-service-using-azure-devops-pt-2-but-what-if-my-code-is-in-github).

### 📢 Announcements

* GitHub accounts [are now integrated into Visual Studio 2019](https://devblogs.microsoft.com/visualstudio/github-accounts-are-now-integrated-into-visual-studio-2019).
* Microsoft has released [WinUI 3 Preview 3](https://blogs.windows.com/windowsdeveloper/2020/11/17/announcing-winui-3-preview-3).
* GitHub Desktop [now has split diffs](https://github.blog/2020-11-17-introducing-split-diffs-in-github-desktop/).
* AWS [announces end of support for the AWS SDK for .NET version 2](https://aws.amazon.com/blogs/developer/announcing-the-end-of-support-for-the-aws-sdk-for-net-version-2/).
* Jon Douglas [introduces NuGet 5.8](https://devblogs.microsoft.com/nuget/getting-started-with-nuget-5-8).
* Clément Habinshuti [introduces the OData Web API authorization library](https://devblogs.microsoft.com/odata/introducing-the-odata-web-api-authorization-library).
* Jimmy Bogard [updates his vertical slice example to .NET 5](https://jimmybogard.com/vertical-slice-example-updated-to-net-5).

### 📅 Community and events

* The Overflow [writes about the complexities and rewards of open sourcing corporate software products](https://stackoverflow.blog/2020/11/17/the-complexities-and-rewards-of-open-sourcing-corporate-software-products/).
* For community standups this week, Tooling [talks to Stephen Toub about .NET 5 performance](https://www.youtube.com/watch?v=XahjibK6SLs), Entity Framework [has an EF Core 5 community panel](https://www.youtube.com/watch?v=AkqRn2vr1lc), and [ASP.NET talks about bUnit with Egil Hansen](https://www.youtube.com/watch?v=LjGCPaP8DH8).
* The .NET Foundation [wants your feedback](https://dotnetfoundation.org/about/survey), and there might be prizes involved.

### 😎 ASP.NET Core / Blazor

* Dave Brock (hi!) [simplifies ASP.NET Core API models with C# 9 records](https://daveabrock.com/2020/11/18/simplify-api-models-with-records), and also [works with OpenAPI, Swagger UI, and HttpRepl in ASP.NET Core 5](https://daveabrock.com/2020/11/16/httprepl-openapi-swagger-netcoreapis).
* The Code Maze blog [sends an SMS with ASP.NET Core](https://code-maze.com/send-sms-aspnetcore/).
* Khalid Abuhakmeh [resolves multiple types in ASP.NET Core](https://khalidabuhakmeh.com/resolve-multiple-types-in-aspnetcore).
* Imar Spaanjaars [uses standard health checks in ASP.NET Core](https://imar.spaanjaars.com/612/using-standard-health-checks-and-building-your-own-in-aspnet-core), and also [unit tests authorization on controllers](https://imar.spaanjaars.com/616/unit-testing-authorization-on-your-controllers).
* Marinko Spasojevic [writes about localization in Blazor WebAssembly apps](https://code-maze.com/localization-in-blazor-webassembly-applications/), and also [works with Blazor WebAssembly component virtualization](https://code-maze.com/blazor-webassembly-component-virtualization-with-web-api/).
* Kristoffer Strube [calls anonymous C# functions from JS in Blazor WebAssembly](https://blog.elmah.io/call-anonymous-c-functions-from-js-in-blazor-wasm/).
* Shaun C. Curtis [offers an alternative to routing in Blazor](https://www.codeproject.com/Articles/5286205/An-Alternative-to-Routing-in-Blazor-2).
* Ricardo Peres [discusses pitfalls when returning a custom service provider from ConfigureServices](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-returning-a-custom-service-provider-from-configureservices).
* Mark Heath [uses OpenAPI auto-generated clients in ASP.NET Core](https://markheath.net/post/openapi-autogen-aspnetcore).
* Roland Weigelt [compares SignalR in .NET Core 3.1 vs. .NET 5](https://weblogs.asp.net/rweigelt/tiny-difference-big-consequences-reloaded-signalr-in-net-core-3-1-vs-net-5).

### ⛅ The cloud

* Damien Bowden [uses the Microsoft Graph API in ASP.NET Core](https://damienbod.com/2020/11/20/using-microsoft-graph-api-in-asp-net-core/).
* Azure Tips & Tricks [writes about which database to use in your next Azure Functions app](https://microsoft.github.io/AzureTipsAndTricks/blog/tip295.html).
* Mark Heath [writes about simple messaging with Rebus and Azure Storage queues](https://markheath.net/post/rebus-azure-storage-queues).
* Joe Guadagno [adds .NET 5 support to Azure App Service](https://www.josephguadagno.net/2020/11/17/adding-dotnet5-support-to-azure-app-service).
* Niels Swimberghe [queries top requested URL's using Kusto and Log Analytics in Azure Application Insights
](https://swimburger.net/blog/azure/querying-top-requested-urls-using-kusto-and-log-analytics-in-azure-application-insights).
* David Hayden [publishes an ASP.NET Core Web API to Azure API Management](https://www.davidhayden.me/blog/publish-asp-net-core-web-api-to-azure-api-management-services).

### 📔 C# posts

* Konrad Kokosa [writes about 6 less popular facts about C# 9 records](https://tooslowexception.com/6-less-popular-facts-about-c-9-records/).
* David Hayden [writes about recursive Fibonacci and memoization in C#](https://www.davidhayden.me/blog/recursive-fibonacci-and-memoization-in-csharp).
* Patrick Smacchia [explains C# index and range operators](https://blog.ndepend.com/c-index-and-range-operators-explained/).

### 📗 F# posts

* Timothé Larivière [writes about Fabulous](https://devblogs.microsoft.com/xamarin/fabulous-going-beyond-hello-world).
* Alican Demirtas [writes about `nameof` support in F# 5](https://www.compositional-it.com/news-blog/support-for-nameof-in-f-5).
* Angel Munoz writes about [monoliths in F# and JS](https://dev.to/tunaxor/good-ol-monoliths-1foo), as well [as working with Giraffe](https://dev.to/tunaxor/looking-at-the-giraffe-18mo).

### 🔧 Tools

* Matt Lacey [gets festive with Visual Studio Extensions](https://www.mrlacey.com/2020/11/a-festive-introduction-to-visual-studio.html).
* Derek Comartin [talks about the complexity of caching](https://codeopinion.com/the-complexity-of-caching).
* David Walsh [removes untracked files in Git](https://davidwalsh.name/git-remove-untracked-files).
* Scott Hanselman [asks you to update dotnet-outdated because it's outdated](https://www.hanselman.com/blog/your-dotnet-outdated-is-outdated-update-and-help-keep-your-net-projects-up-to-date).
* Mark Seemann [redirects legacy URLs](https://blog.ploeh.dk/2020/11/16/redirect-legacy-urls/).
* Andrew Lock [offers tips, tricks, and edge cases when deploying to k8s](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-12-tips-tricks-and-edge-cases/).
* Khalid Abuhakmeh [works with Uno Platform and Rider](https://blog.jetbrains.com/dotnet/2020/11/16/working-with-uno-platform-and-rider/).

### 📱 Xamarin

* Benjamin Askren [says goodbye to Xamarin.Forms](https://medium.com/@ben_12456/goodbye-xamarin-forms-f41723fb9fe1).
* Leomaris Reyes [works with experimental flags](https://askxammy.com/being-friends-with-the-experimental-flags/).

### 🏴‍☠️ Other finds

* David McCarter [says the "full-stack" developer is a myth](https://www.c-sharpcorner.com/article/the-full-stack-developer-is-a-myth-in-2020/).
* The API Evangelist [uses API mocks as part of an API-first workflow](http://apievangelist.com/2020/11/17/using-api-mocks-as-part-of-an-apifirst-workflow/).

### 🎤 Podcasts

* The Software Engineering Daily podcast [talks about GraphQL at GitHub with Marc-Andre Giroux](https://softwareengineeringdaily.com/2020/11/20/graphql-at-github-with-marc-andre-giroux).
* The .NET Rocks Podcast [talks with Aaron Stannard about Microsoft open source](https://www.dotnetrocks.com/default.aspx?ShowNum=1714).
* On Software Engineering Radio, [Julie Lerman talks about ORMs and EF](https://www.se-radio.net/2020/11/episode-435-julie-lerman-on-object-relational-mappers-and-entity-framework/).
* The Merge Conflict podcast [discusses .NET Conf and the Apple M1 event](https://www.mergeconflict.fm/228).

### 🎥 Videos

* The Visual Studio Toolbox [discusses the .NET object allocation tracking tool](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Performance-Profiling--NET-Object-Allocation-Tracking-Tool).
* Brian Lagunas [fixes an issue with Blazor drop-down not working with InputSelect](https://brianlagunas.com/blazor-cascading-dropdown-not-working-with-inputselect/).
* The Azure Enablement series [introduces the Cloud Adoption Framework for Azure](https://channel9.msdn.com/Shows/Azure-Enablement/Overview-of-Cloud-Adoption-Framework-for-Azure--Introduction-Ep1-Cloud-Adoption-Framework).
* Jeff Fritz [talks about new features in C# 9](https://www.youtube.com/watch?v=htB7ohlaYKk).
* ON.NET talks about [service discovery with Steeltoe](https://www.youtube.com/watch?v=ZsVDgEOoSEU), and also [Steeltoe and Azure Spring Cloud](https://www.youtube.com/watch?v=jWIA6vltQuc).
