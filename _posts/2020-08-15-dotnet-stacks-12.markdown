---
date: "2020-08-15"
title: "The .NET Stacks #12: Azure DevOps or GitHub Actions, .NET Foundation results, community links!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-12-card.png
subtitle: We discuss Microsoft CI/CD options, find out who's on the .NET Foundation board, and look at the busy week in the .NET community!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday, everyone! Current status: still wondering if I'm [just a JSON pusher](https://twitter.com/thomas_k_r/status/1291076179693363206) (**warning**: link has a naughty word).

This week, we:

* Discuss how Azure DevOps and GitHub Actions will be ironed out
* Discuss the .NET Foundation election results
* Check out a busy week in the community

## GitHub Actions or Azure DevOps?

Hopefully by now, in 2020, you know that [friends don't let friends right-click publish](https://damianbrady.com.au/2018/02/01/friends-dont-let-friends-right-click-publish/). There are so many tools you can use to work with [continuous integration and continuous deployment](https://www.infoworld.com/article/3271126/what-is-cicd-continuous-integration-and-continuous-delivery-explained.html) pipelines—I'd like to talk about two under Microsoft's ownership: Azure DevOps and GitHub Actions.

As a .NET developer, you're probably familiar with [Azure DevOps](https://azure.microsoft.com/services/devops/) (previously Team Foundation Server). It's got a decade of battle-tested use in enterprises, with well-featured CI/CD pipelines. If you want best-in-class Microsoft tooling with either your on-prem or cloud deployments, it's always been a clear choice.

The lines blurred a little when, in 2018, [Microsoft acquired GitHub](https://news.microsoft.com/announcement/microsoft-acquires-github/). GitHub, traditionally known for being the community's preferred way to share code, was now bought by a company invested in open source development and an interest in bringing Microsoft-ness to new audiences. GitHub's own CI/CD solution, GitHub Actions, is getting a lot of well-deserved recognition. And with [npm joining GitHub](https://github.blog/2020-03-16-npm-is-joining-github/), GitHub will definitely help improve experiences with dependency management. To further blur the lines, a lot of folks don't know about their GitHub Enterprise offering, an on-prem solution that even has integration capabilities with Active Directory!

So what does this say for the future of Microsoft CI/CD pipelines? As a long-term strategy, Microsoft can't expect to run two similar products with internal competition. I actually [wondered about this not so long ago](https://twitter.com/daveabrock/status/1253462315397251074) and was referred to an [insightful conversation that occurred on the Azure Podcast with GitHub PM Sasha Rosenbaum](http://azpodcast.com/post/Episode-321-GitHub). 

She agreed that, yes, the investments will be geared toward GitHub and its vibrant community and long-term GitHub will probably win out. This isn't to say that you need to migrate in 6 months or anything—she even mentioned a 5-year timeframe—but the future is definitely with GitHub Actions. As such, her recommendation was that there's nothing wrong with using Azure DevOps today, but if you're starting a new project to look into using GitHub. 

Until the time comes when Microsoft has one CI/CD tool, you'll likely see GitHub Actions build out with a stronger feature set. It's already robust—and with its CI capabilities are right there with Azure DevOps—but has some work to do until it's on-par with its continuous deployment functionality (release pipelines, gates, advanced permissions, and whatnot).

## .NET Foundation Board of Directors election results are in

This week, we found out who [would serve on the .NET Foundation Board of Directors](https://dotnetfoundation.org/blog/2020/08/04/net-foundation-board-of-directors-election-results-2020). Out of 17 nominees, these six folks came out on top (with Beth Massi on the 7th spot as Microsoft's one appointed board member):

* [Rodney Littles, II](https://twitter.com/rlittlesii)
* [Javier Lozano](https://twitter.com/jglozano)
* [Layla Porter](https://twitter.com/LaylaCodesIt)
* [Jeff Strauss](https://twitter.com/jeffreystrauss)
* [Bill Wagner](https://twitter.com/billwagner)
* [Shawn Wildermuth](https://twitter.com/shawnwildermuth)

It's great to see an entirely new Board and am looking forward to seeing what comes from these fresh perspectives.

## 🌎 Last week in the .NET world

### The Top 3

* The .NET Foundation [Board of Directors election results are in](https://dotnetfoundation.org/blog/2020/08/04/net-foundation-board-of-directors-election-results-2020).
* Jon Hilton [makes a responsive navbar with Blazor and Tailwind](https://jonhilton.net/responsive-blazor-navbar/).
* Andrew Lock [goes deep on IHttpClientFactory](https://andrewlock.net/exporing-the-code-behind-ihttpclientfactory/).

### 📢 Announcements

* Microsoft announced [the release of Visual Studio 2019 v16.7 and v16.8 Preview 1](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-7-releases/). With v16.7, [IntelliCode helps to spot repetitions in your code](https://devblogs.microsoft.com/visualstudio/making-repeated-edits-easier-with-intellicode-suggestions/). Other VS news: Gabrielle Crevecoeur [announces an Angular Language Service for Visual Studio](https://devblogs.microsoft.com/visualstudio/angular-language-service-for-visual-studio/) and [Visual Studio 2019 for Mac version 8.7 is now available](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-for-mac-version-8-7-is-now-available/).
* You can now [sign up for the private preview for Visual Studio Codespaces](https://devblogs.microsoft.com/cppblog/sign-up-for-the-private-preview-of-visual-studio-support-for-codespaces/).
* You can now [choose the initial branch name for new repos in Azure Repos](https://devblogs.microsoft.com/devops/azure-repos-default-branch-name/).
* Okta announced [a developer certification program](https://developer.okta.com/blog/2020/08/03/developer-certification).
* Both [Microsoft](https://cloudblogs.microsoft.com/opensource/2020/08/03/microsoft-joins-open-source-security-foundation/) and [GitHub](https://github.blog/2020-08-03-github-joins-the-open-source-security-foundation/) have joined the Open Source Security Foundation.
* The OpenTelemetry [.NET SDK has reached beta](https://medium.com/opentelemetry/opentelemetry-net-beta-released-e1b070f0a5bc).

### 📅 Community and events

* Three community standups this week: Xamarin [talks about Forms 4.8](https://www.youtube.com/watch?v=4W6vXC6v-J8), Entity Framework talks about [the EF Core in Depth video series](https://www.youtube.com/watch?v=b-zTazj2vuI), and ASP.NET Core [has an update on Project Tye](https://www.youtube.com/watch?v=r0Z02t_7gm8).
* The .NET Docs Show [talks with Chase Aucoin](https://www.youtube.com/watch?v=UShy1RGCK0E).
* Troy Hunt [plans on open sourcing Have I Been Pwned](https://www.troyhunt.com/im-open-sourcing-the-have-i-been-pwned-code-base/).

### 😎 Blazor

* Marinko Spasojevic [works on Blazor WASM registration functionality with ASP.NET Core Identity](https://code-maze.com/blazor-webassembly-registration-aspnetcore-identity/) and also [uses AuthenticationStateProvider in Blazor WASM](https://code-maze.com/authenticationstateprovider-blazor-webassembly/).
* Peter Vogel [creates a progressive web app with Blazor Web Assembly](https://visualstudiomagazine.com/articles/2020/08/03/blazor-pwa.aspx).

### 🚀 .NET Core

* David Grace [logs data changes in EF Core](https://www.roundthecode.com/dotnet/entity-framework/log-data-changes-in-entity-framework-core-create-entities-dbcontext) and also [adds Google auth to an ASP.NET Core application](https://www.roundthecode.com/dotnet/how-to-add-google-authentication-to-a-asp-net-core-application).
* Atul Warade [works on background tasks in ASP.NET Core](https://www.c-sharpcorner.com/article/implement-background-task-using-backgrounservice-class-in-asp-net-core/).
* Derek Comartin [gets started with Apache Kafka with .NET Core](https://codeopinion.com/getting-started-apache-kafka-with-net-core/).
* Martijn Boland [uses AppText for easy ASP.NET Core localization](https://blogs.taiga.nl/martijn/2020/08/03/easy-asp-net-core-localization-with-apptext/).
* Khalid Abuhakmeh [handles HTTP status codes with Razor Pages](https://khalidabuhakmeh.com/handle-http-status-codes-with-razor-pages).
* ErikEJ [calls stored procedures with OUTPUT params with FromSqlRaw in EF Core](https://erikej.github.io/efcore/2020/08/03/ef-core-call-stored-procedures-out-parameters.html).
* Shahed Chowdhuri [has rolled out his ASP.NET Core 3.1 A-Z e-book](https://wakeupandcode.com/release-asp-net-core-3-1-a-z-ebook/).

### ⛅ The cloud

* Kevin Griffin [loves Azure Static Web Apps](https://consultwithgriff.com/i-love-azure-static-web-apps).
* Sacha Barber [looks at Azure Event Hubs with Kafka](https://sachabarbs.wordpress.com/2020/08/04/azure-event-hubs-with-kafka/).
* Dror Helper [creates a .NET AWS serverless app using API Gateway with API Key](https://helpercode.com/2020/08/03/creating-an-net-aws-serverless-application-using-api-gateway-with-api-key/).
* Chris Pietschmann [discusses some tips and tricks for the Azure Cloud Shell](https://build5nines.com/azure-cloud-shell-tips-and-tricks/).
* Damien Bowden [works on monitoring and diagnostics in Azure Durable Functions](https://damienbod.com/2020/08/01/azure-durable-functions-monitoring-and-diagnostics/).

### 📔 Languages

* Khalid Abuhakmeh [talks about preprocessor directives in C#.](https://khalidabuhakmeh.com/csharp-preprocessor-directives).
* Ian Griffiths [continues his C# nullable references series, discussing when methods don't return](https://endjin.com/blog/2020/08/dotnet-csharp-8-nullable-references-when-methods-dont-return.html).
* Rami Honig [has a guide for learning C# and .NET in 2020](https://oz-code.com/blog/net-c-tips/ultimate-2020-guide-learning-debugging-c-net).
* Brian Lagunas [warns against async void in C#](https://brianlagunas.com/why-is-async-void-bad-and-how-do-i-await-a-task-in-an-object-constructor-in-c/).
* Oren Eini [explains the use of default(object)](https://ayende.com/blog/191585-B/what-is-default-object-used-for).
* Jiří Činčura [looks at the best way to create an empty collection in C#](https://www.tabsoverspaces.com/233833-best-way-to-create-an-empty-collection-array-and-list-in-csharp-net).
* Carmel Eve [talks about how to fully initialize types in constructors with C# nullable using the async factory pattern](https://endjin.com/blog/2020/08/fully-initialize-types-in-constructor-csharp-nullable-async-factory-pattern.html).
* Maxime Brugidou [discusses moving .NET to Linux at scale](https://medium.com/criteo-labs/moving-net-to-linux-at-scale-d8ff49b42661).

### 🔧 Tools

* Gunnar Peipman [builds ASP.NET Core applications on Visual Studio Codespaces and VS Code](https://gunnarpeipman.com/aspnet-core-visual-studio-codespaces/).
* Justin Yoo [sets up Visual Studio Codespaces for .NET Core](https://techcommunity.microsoft.com/t5/apps-on-azure/configuring-codespaces-for-net-core-development/ba-p/1565330).
* Munib Butt [has a primer on getting started with LinqPad](https://www.c-sharpcorner.com/article/learn-to-use-linqpad/).
* Rick Strahl [uses .NET Core tools for reusable and shareable tools and apps](https://weblog.west-wind.com/posts/2020/Aug/05/Using-NET-Core-Tools-to-Create-Reusable-and-Shareable-Tools-Apps).
* Matthias Koch [implements and debugs custom MSBuild tasks](https://ithrowexceptions.com/2020/08/04/implementing-and-debugging-custom-msbuild-tasks.html).
* Mike Hadlow [restores from an Azure Artifacts NuGet feed from inside a Docker build](http://mikehadlow.blogspot.com/2020/08/restoring-from-azure-artifacts-nuget.html).
* Gérald Barré enforces [async code good practices using a Roslyn analyzer](https://www.meziantou.net/enforcing-asynchronous-code-good-practices-using-a-roslyn-analyzer.htm).
* Scott Hanselman works through [an easy way to SSHG into Bash and WSL2 on Windows 10 from an external machine](https://www.hanselman.com/blog/THEEASYWAYHowToSSHIntoBashAndWSL2OnWindows10FromAnExternalMachine.aspx).
* Charles Flatt [offers an in-depth look at .NET Framework NuGet packages](https://www.softwaremeadows.com/posts/net_framework_nuget_packages_-_versioning__dependency_resolution__and/).
* Shawn Wildermuth [talks about Pinger, his app that pings a range of IP addresses](http://wildermuth.com/2020/08/02/NET-Core-Console-Apps---A-Better-Way).
* Matt Lacey [clears up a thing or two about nuget.exe](https://www.mrlacey.com/2020/08/fixing-really-common-misunderstanding.html).
* Dan Clarke [works on cleaner tests with XUnit's IAsyncLifetime and expression-bodied members](https://www.danclarke.com/cleaner-tests-with-iasynclifetime).
* Justin Chen [shows off semantic highlighting in the VS Code PowerShell Preview extension](https://devblogs.microsoft.com/powershell/semantic-highlighting-in-the-powershell-preview-extension-for-visual-studio-code/).

### 📱 Xamarin

* Vicente Gerardo Guzmán Lucio [talks about new features in Xamarin.Forms 4.7](https://www.syncfusion.com/blogs/post/new-features-in-xamarin-forms-4-7-multibinding-light-and-dark-modes.aspx).
* Leomaris Reyes [works on Excel file creation](https://askxammy.com/getting-started-with-excel-files-creation-in-xamarin-forms/).
* Daniel Hindrikes [talks about OneTime bindings](https://danielhindrikes.se/index.php/2020/08/03/you-only-need-an-onetime-binding/).
* Microsoft has a handy list [of Xamarin-related .NET virtual events in August](https://devblogs.microsoft.com/xamarin/august-dotnet-virtual-events/).
* Charlin Agramonte [shows off show/hide password functionality using EventTrigger](https://xamgirl.com/show-hide-password-using-eventtrigger-in-xamarin-forms/).
* Over at the Okta blog, Giorgi Dalakishvili [works through Xamarin.Essentials and Web Authenticator](https://developer.okta.com/blog/2020/07/31/xamarin-essentials-webauthenticator).

### 🎤 Podcasts

Over at the .NET Rocks podcast, [they discuss adding identity to your applications](https://www.dotnetrocks.com/default.aspx?ShowNum=1699).

### 🎥 Videos

* Data Exposed [talks about transitioning your SQL Server skills to Azure SQL](https://channel9.msdn.com/Shows/Data-Exposed/Learn-Azure-SQL).
* A new [Azure SQL for Beginners video series launched](https://channel9.msdn.com/Series/Azure-SQL-for-Beginners/Introduction-to-Azure-SQL-for-beginners-1-of-61).
* Scott Hanselman [warns you about git push --force](https://www.youtube.com/watch?v=dgOpnebZkRo).
* The ON.NET Show [continues talking about packaging and deploying .NET Core for Linux](https://channel9.msdn.com/Shows/On-NET/Packaging-and-deploying-NET-Core-for-Linux-Part-2).
* From .NET Conf, a bunch: [Cecil Phillip adds DAPR to .NET microservices](https://www.youtube.com/watch?v=g-gOlkD9lKs), [David Dieruf works on .NET microservices with Steeltoe](https://www.youtube.com/watch?v=3meYereHHtM), [Shayne Boyer builds and debugs microservices with Kubernetes and Visual Studio](https://www.youtube.com/watch?v=98nIvg7ne7Q), [Elton Stoneman evolves .NET Framework monoliths with .NET 5 and Kubernetes](https://www.youtube.com/watch?v=Wbjh4T-cdv8), and [James Newton-King builds high-performance microservices with gRPC and .NET](https://www.youtube.com/watch?v=HVq4TstHCEs).
