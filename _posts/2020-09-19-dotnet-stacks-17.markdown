---
date: "2020-09-19"
title: "The .NET Stacks #17: EF Core 5, Blazor + CSS, and community!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-17-card.png
subtitle: EF Core 5 is done, Blazor CSS isolation, and more!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Just a reminder that in your methods, [try to write less than 914 "using var" statements](https://github.com/dotnet/roslyn/issues/47591).

We're talking about a lot of little things this week:

* EF Core 5 is done
* Blazor CSS isolation
* The curious case of unit testing
* September is F#-tember at *The .NET Stacks*
* Community links

## EF Core 5 is done

This week, Arthur Vickers proclaimed that [Entity Framework Core 5.0 is done](https://twitter.com/ajcvickers/status/1304140668579708928), pending any issues or bug fixes. Arthur says the team has also spent the last two weeks squashing bugs, writing better exception messages, and improving the docs.

You can [check out the official plan](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/plan), but I'll be looking forward to the following features:

* [Many-to-many nav properties](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/plan#many-to-many-nav) and [full many-to-many support](https://github.com/dotnet/efcore/issues/10508)
* [Table-per-type inheritance mapping](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/plan#table-per-type-tpt-inheritance-mapping)
* [Filtered includes](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/plan#filtered-include) and [split includes](https://github.com/dotnet/efcore/issues/20892)

You can [check out the daily builds today](https://github.com/dotnet/efcore/blob/release/5.0-rc2/docs/DailyBuilds.md).

## Blazor CSS isolation

With the latest .NET preview, the Blazor team released CSS isolation ([among many other things](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/)). With limited documentation available, [I wrote about it](https://daveabrock.com/2020/09/10/blazor-css-isolation).

The gist: CSS isolation allows you to scope your CSS to your components. Instead of loading all your styles unnecessarily and incurring CSS bleed when dealing with multiple components and libraries, you can say: *I only want these styles to be associated with my component*. There's a lot of flexibility, too—you can use the `::deep` combinator in your scoped CSS to pass styles down to child components and working with CSS preprocessors is a snap.

CSS isolation in Blazor is a build-time step—this means there's no reliance on JavaScript or .NET code, you can easily integrate with existing tooling, and there's no performance hit if you aren't using it. However, this also means you're forced to recompile to see any new CSS changes. This will force you into [using `dotnet watch`](https://docs.microsoft.com/aspnet/core/tutorials/dotnet-watch?view=aspnetcore-3.1) if you haven't already—hitting F5 in Visual Studio every time is no way to go through life.

I'll be writing the official Microsoft reference doc on it, so if you want anything covered that isn't in my post or the [GitHub issue for the doc](https://github.com/dotnet/AspNetCore.Docs/issues/19360), let me know.

## The curious case of unit testing

My thinking on unit testing has been shifting a bit in the last six months or so. The timing couldn't be better: I'm seeing a lot of chatter about unit testing both inside and outside of the .NET developer community.

Back in early July, Alexey Golub [wrote a popular piece called *Unit Testing is Overrated*](https://tyrrrz.me/blog/unit-testing-is-overrated). It aligned with a lot of what I was thinking about—placing your *primary focus* on unit tests isn't always valuable. This is not to say you shouldn't value them, or champion them, or appreciate them. Quite the opposite. It's when we decouple so much while saying "we need to make the code testable" where, in the end, the only value is to make unit testing work. In the end, are we just testing our mocks?

Instead of spending a ridiculous amount of time setting up mocks that don't replicate much of a complex software system, why not rely more on integration tests? This opinion would be blasphemy just ten (or even five) years ago. After all, unit tests should be quick, repeatable, and pure (no side effects). With the rise of containerization and better cloud infrastructure, the "integration tests are slow and flaky" argument is less of an issue.

To that end: last month, Khalid Abuhakmeh wrote [*Secrets of a .NET Professional*](https://khalidabuhakmeh.com/secrets-of-a-dotnet-professional), in which he writes this (which led to a [spirited Twitter discussion](https://twitter.com/JanVanRyswyck/status/1295063028757680133)):

>The systems we build have many complex external dependencies. The hubris of thinking we could mock away decades of investment in a database, web service, or any other technology has been the source of many frustrating arguments. In my opinion, one good integration test is worth 1,000 unit tests... With the rise of containerization, the pain of writing integration tests has never been lower.

For me, my thought is that unit testing is crucial for testing and verifying core (and pure) business logic. But you need to test against your dependencies, and many times the best bang for your buck comes with integration testing—as long as the tests are reasonably fast and cheap. As for me, I'm no longer obsessing over testing every single layer of an MVC app (as an aside, Andrew Lock [wrote a nice post that questions the value of unit testing APIs and MVC controllers](https://andrewlock.net/should-you-unit-test-controllers-in-aspnetcore/)).

Our industry gets dogmatic, so opinions like these can get heated. But to me, it doesn't really seem like a controversial opinion to take a nuanced approach to testing so we can find the most value in our hectic days. Containerization has revolutionized our industry—so let's take advantage of it.

## September is F#-tember at *The .NET Stacks*

Do you work in F#? Or are you a C# developer and intrigued by its possibilities but haven't found time to dive in? As C# has "borrowed" a lot of functional programming paradigms, you might be wondering about tradeoffs between C# "with functional bits" and straight F#. I've got you covered.

Next week, I'm interviewing Isaac Abraham, author of *[Get Programming with F#](https://www.manning.com/books/get-programming-with-f-sharp)*. Soon after, I'll post an interview with Phillip Carter, the PM for F# at Microsoft. Stay tuned for some great conversations.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Michael Shpilt [discusses assembly versioning and "DLL hell" in .NET](https://michaelscodingspot.com/dotnet-dll-hell/).
* The .NET Docs Show [talks with Jon Skeet](https://www.youtube.com/watch?v=3i8zDtpw-kQ).
* Andrew Lock [continues his k8s series: this time, configuring resources with YAML manifests](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-2-configuring-resources-with-yaml-manifests/).

### 📢 Announcements

* Nish Anil [announces a free e-book on Blazor for ASP.NET Web Forms developers](https://devblogs.microsoft.com/aspnet/blazor-aspnet-webforms-ebook).
* Timothé Larivière [introduces Fabulous, an OSS framework for mobile and desktop apps using functional programming](https://devblogs.microsoft.com/xamarin/fabulous-functional-app-development).
* Pierre Boulay [discusses how in the latest Windows Insider build, you can attach and mount a physical disk inside WSL 2](https://devblogs.microsoft.com/commandline/access-linux-filesystems-in-windows-and-wsl-2).
* GitHub announced [an integration with Microsoft Teams](https://github.blog/2020-09-10-announcing-the-github-integration-with-microsoft-teams/).
* Rahul Bhandari [announces the release of the .NET Core September 2020 security update](https://devblogs.microsoft.com/dotnet/net-core-september-2020), and Tara Overfield [announces the September 2020 security and quality updates for .NET Framework](https://devblogs.microsoft.com/dotnet/net-framework-september-2020-security-and-quality-rollup-updates).
* The .NET Docs team [shows off what's new from the month of August](https://docs.microsoft.com/en-us/dotnet/whats-new/2020-08).
* Visual Studio Codespaces [is consolidating into GitHub Codespaces](https://devblogs.microsoft.com/visualstudio/visual-studio-codespaces-is-consolidating-into-github-codespaces/).

### 📅 Community and events

We had three community standups: Languages & Runtime [explores miscellaneous topics](https://www.youtube.com/watch?v=XU3-xVtqJy4), Machine Learning [talks SciSharp](https://www.youtube.com/watch?v=ngvR-BNQsJE), and ASP.NET [talks about Microsoft.Identity.Web](https://www.youtube.com/watch?v=hxDli4imREE).

### 😎 Blazor

* Dave Brock (ahem) [walks through CSS isolation in Blazor](https://daveabrock.com/2020/09/10/blazor-css-isolation). 
* Peter Vogel [compares performance options among Telerik DataGrid, JavaScript, and Blazor](https://www.telerik.com/blogs/comparing-performance-telerik-datagrid-javascript-blazor-code), and also [works with local storage in a Blazor PWA](https://visualstudiomagazine.com/articles/2020/09/08/blazor-pwa-local-storage.aspx).
* Karl Shifflett [works with a Blazor WASM GraphQL client](https://oceanware.wordpress.com/2020/09/08/blazor-wasm-graphql-client/).
* Julio Sampaio [gets started with Blazor](https://www.red-gate.com/simple-talk/dotnet/c-programming/first-steps-with-blazor/).

### 🚀 .NET Core

* Nickolas Fisher [uses Okta to migrate an ASP.NET Framework to ASP.NET Core](https://developer.okta.com/blog/2020/09/09/aspnet-migration-dotnet-core).
* Vladimir Pecanac [creates a custom config provider in ASP.NET Core](https://code-maze.com/aspnet-configuration-creating-custom-provider/).
* David Grace asks: [is Entity Framework Core quicker than Dapper?](https://www.roundthecode.com/dotnet/entity-framework/is-entity-framework-core-quicker-than-dapper).
* Michał Białecki [works with views in EF Core 5](http://www.michalbialecki.com/2020/09/09/working-with-views-in-entity-framework-core-5/).
* Tomasz Pęczek [works with HTTP trailers in ASP.NET Core](https://www.tpeczek.com/2020/09/little-known-aspnet-core-features-http.html).
* Mark Heath [migrates from ASP.NET to ASP.NET Core](https://markheath.net/post/migrate-aspnet-core).

### ⛅ The cloud

* Damien Bowden [secures Azure Functions using an Azure virtual network](https://damienbod.com/2020/09/10/securing-azure-functions-using-an-azure-virtual-network/).
* Seth Juarez [fixes mixed-content and CORS issues at ML Model inference time with Azure Functions](https://www.sethjuarez.com/2020/09/10/cors-and-mixed-content-with-azure-functions/).
* Jason Farrell [continues his work on Azure Durable Functions](https://jfarrell.net/2020/09/05/durable-functions-part-4-analyze-and-download/).
* Siva Ramani and Naveen Balaraman [deploy a .NET Core Web API container to Amazon EKS](https://aws.amazon.com/blogs/developer/build-and-deploy-net-core-webapi-container-to-amazon-eks-using-cdk-cdk8s).
* Jamie Maguire [adds speech to a chatbot using the Bot Framework and Azure Speech Services](http://www.jamiemaguire.net/index.php/2020/09/05/adding-speech-capability-to-your-chatbot-using-bot-framework-and-azure-speech-services/).

### 📔 Languages

* Patrick Lioi [talks about evolving an open source test framework with .NET](https://opensource.com/article/20/9/testing-net-fixie).
* Suresh M [talks about understanding C# anonymous types](https://www.syncfusion.com/blogs/post/understanding-csharp-anonymous-types.aspx).
* Lawrence Hecht [talks about how Visual Basic is lingering on](https://thenewstack.io/visual-basic-lingers-on/).
* Thomas Claudius Huber [discusses target-typed new expressions in C# 9](https://www.thomasclaudiushuber.com/2020/09/08/c-9-0-target-typed-new-expressions/).
* Andrew Nosenko [works on async subroutines with C# 8 and IAsyncEnumerable](https://dev.to/noseratio/asynchronous-coroutines-with-c-8-0-and-iasyncenumerable-2e04).

### 🔧 Tools

* Ian Miell [talks about the many ways to undo a commit in Git](https://zwischenzugs.com/2020/09/10/five-ways-to-undo-a-commit-in-git/).
* Matthew Jones [uses a Dapper base repository in C#](https://exceptionnotfound.net/using-a-dapper-base-repository-in-c-to-improve-readability).
* ErikEJ uses [EF Core Power Tools to rename entities and properties when reverse engineering a database](https://erikej.github.io/efcore/2020/09/07/ef-core-power-tools-renaming-advanced.html).
* Jason Gaylord [adds Azure architecture icons to diagrams](https://www.jasongaylord.com/blog/2020/09/06/azure-architecture-icons).
* Adam Storr [uses Project Tye to run dependent services with ASP.NET Core](https://adamstorr.azurewebsites.net/blog/using-project-tye-to-run-dependent-services-for-use-with-aspnetcore).

### 📱 Xamarin

* Jean-Marie Alfonsi [walks through the TaskLoaderCommand](https://www.sharpnado.com/taskloadercommand-asynccommand/).
* Vicente Gerardo Guzmán Lucio [creates custom renderers for a control](https://www.syncfusion.com/blogs/post/how-to-create-custom-renderers-for-a-control-in-xamarin-forms.aspx).
* James Montemagno [quickly discusses modal navigation](https://devblogs.microsoft.com/xamarin/xamarin-forms-shell-quick-tip-modal-navigation/).

### 🎤 Podcasts

* The .NET Rocks podcast [gets started with Xamarin](https://www.dotnetrocks.com/default.aspx?ShowNum=1704).
* The Xamarin Podcast [talks about the Surface Duo, Android startup times, and Xamarin.Essentials](https://www.xamarinpodcast.com/77).
* The RunAsRadio podcast [gets snarky about the cloud with Corey Quinn](http://www.runasradio.com/default.aspx?ShowNum=724).
* The Stack Overflow Podcast discusses [how developers can become successful writers](https://the-stack-overflow-podcast.simplecast.com/episodes/how-developers-can-become-successful-writers-QkFip8NF).
* The Merge Conflict podcast [discusses Blazor](https://www.mergeconflict.fm/218).
* The .NET Core Podcast [talks about IoT and .NET Core with Pete Gallagher](https://dotnetcore.show/episode-59-iot-and-net-core-with-pete-gallagher/).
* The Loosely Coupled Show [talks about loosely-coupled monoliths](https://www.youtube.com/watch?v=xODJrIYDxA4).

### 🎥 Videos

* The Visual Studio Toolbox [talks about the new Git experience](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/The-New-Git-Experience).
* Data Exposed [talks about Azure SQL Edge](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Edge-Demo-Renewable-Energy), and also [works with Power BI and Azure Synapse](https://channel9.msdn.com/Shows/Data-Exposed/Analyzing-your-Data-Lake-with-Power-BI-and-Azure-Synapse).
* At the Maintainers, [Shawn Wildermuth talks with Dennis Doomen about FluentAssertions](http://wildermuth.com/2020/09/10/The-Maintainers-Dennis-Doomen-and-FluentAssertions).
* The AI Show [discusses OSS framework support in the Azure Machine Learning service](https://channel9.msdn.com/Shows/AI-Show/OSS-Framework-Support-in-Azure-Machine-Learning-Service).
* The ON.NET Show [uses .NET microservices with Steeltoe](https://channel9.msdn.com/Shows/On-NET/NET-Microservices-with-Steeltoe).
