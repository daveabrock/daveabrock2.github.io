---
date: "2020-10-24"
title: "The .NET Stacks #22: .NET 5 RC 2 ships, .NET Foundation all hands, and links"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-22-card.png
subtitle: This week, .NET 5 RC 2 ships, we talk .NET Foundation, and look around the community.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

I messaged a developer friend to see if he liked my puns, and he said "false." I told him I didn't appreciate the cyber boolean.

On tap this week:

* .NET 5 RC 2 ships
* .NET Foundation updates
* Last week in the .NET world

## 🚢 .NET 5 RC 2 ships

On Tuesday, Microsoft announced the release of [.NET 5.0 RC 2](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-rc-2/). With the general release of .NET 5 on November 10, it's the last release candidate ("near-final," as Richard Lander puts it). You can [download it here](https://dotnet.microsoft.com/download/dotnet/5.0) (and will also need the latest Visual Studio preview on [Windows](https://visualstudio.microsoft.com/vs/preview/) or [Mac](https://visualstudio.microsoft.com/vs/mac/)).

The biggest news out of the general announcement post? MSI installers are now available for ARM64 (yes!)—however, note the .NET 5 SDK [does not contain the Windows Desktop components on ARM64](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-8/#windows-arm64). As a bonus, I learned that [ClickOnce](https://docs.microsoft.com/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application?view=vs-2019) is still a thing and available for .NET Core 3.1 and .NET 5 Windows apps.

### ASP.NET Core updates for Blazor

For being so late in the release cycle, ASP.NET Core [was able to ship quite a few Blazor updates for RC 2](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-2/).

As I've [written about extensively](https://daveabrock.com/2020/09/10/blazor-css-isolation), Blazor now ships with CSS isolation—the ability to scope styles to only a single component. Previously, [when the feature was released in .NET 5 Preview 8](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/), all scoped files were bundled into a big `scoped.styles.css` file. If you were styling a lot of components, the bundle could get quite heavy. Now, Blazor produces one bundle for each referenced project or package with the format `MyProject.styles.css`. (The official Microsoft doc on CSS isolation, [which I'm writing](https://github.com/dotnet/AspNetCore.Docs/pull/19956), should be live in the next week or two.)

In addition, RC 2 comes with a bunch of Blazor tooling updates. The [pesky static port issue](https://github.com/dotnet/aspnetcore/issues/25887) was resolved, you can step out and over async methods, and can debug lazy loaded assemblies. Also, your tools can now throw warnings when using unsupported Blazor Web Assembly APIs from a browser. (This is part of a larger .NET 5 effort that annotates which APIs are supported [with a platform compatability analyzer](https://docs.microsoft.com/dotnet/standard/analyzers/api-analyzer#discover-cross-platform-issues).)

### Entity Framework Core 5 updates

Because EF Core 5 RC1 [was so feature-rich](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/whatsnew)—with many-to-many, mapping entity types to queries, event counters, `SaveChanges` interception and much more, RC2 was spent on bug fixes. (Also, if Jeremy Likness's callouts in every update haven't gotten your attention, know that EF Core 5 requires .NET Standard 2.1, meaning it won't run on .NET Standard 2.0 platforms like .NET Framework.)

Next week, I'll talk about the .NET 5 release cycles and how Microsoft is supporting them.

## 🧭 .NET Foundation "all hands" meeting recap

The .NET Foundation provided a [September/October update this week](https://dotnetfoundation.org/blog/2020/10/14/blog/posts/net-foundation-september-october-2020-update), and also [hosted an "all hands meeting"](https://www.youtube.com/watch?v=CX8wT0mO5qg) with the Board of Directors (which was live-streamed and open to all).

In the past, the Foundation has heard loud and clear that they need to work on better communication and openness. They've responded with regular updates, releasing budget info, a State of the Union update, and more. There's a proposal to change the mission statement ([and you can chime in on the open GitHub issue until October 26](https://github.com/dotnet-foundation/website/pull/388)):

>Our mission is to build and educate producers and consumers, both new and old of the .NET platform. We will grow a trusted OSS ecosystem adopted by education, commercial entities, and all users. We will lead by example creating a world-wide, healthy, vibrant, and diverse OSS community.

There was some mention of the [.NET Foundation maturity model](https://github.com/dotnet-foundation/project-maturity-model), which is currently sitting on the back burner. This model helps to improve the quality and quantity of .NET open source projects (and .NET in general). A big driver of this model is to get corporate sponsors to pay for .NET Foundation projects and there's some open work on seeing that through—how do you make the projects sustainable without lazily asking maintainers to keep working hard on them?

Rodney Littles II made a great point in saying: "There are people in the community that are asking a lot of the .NET Foundation ... We are the .NET community, what are we going to do for ourselves? We have to get past this concept of a Microsoft-driven construct."

It's easy to get skeptical about the "Microsoft-driven construct" comment—Microsoft owns .NET and has a huge financial stake in seeing it do well, so hoping for complete independence isn't realistic. As a result, when it comes to the .NET Foundation Microsoft is both an asset and a liability. But it's important for the community to not view the Foundation as something that needs to be driven by Microsoft. Yes, there will always be questions on Microsoft's independence, but at a certain point the growth of the Foundation depends on the community.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* .NET 5 RC 2 is out—[Richard Lander has the details](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-rc-2), Daniel Roth [updates us on ASP.NET Core](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-2), and Jeremy Likness [runs through EF Core](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-ef-core-5-rc2).
* Andrew Lock [runs database migrations when deploying to Kubernetes](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-7-running-database-migrations/).
* Manuel Grundner [writes a static site comment system with .NET Core and GitHub Actions](https://blog.delegate.at/2020/10/11/comment-system-on-static-sites-with-dotnet-core-and-github-actions.html).

### 📢 Announcements

* The Azure SDK team [offers an October update](https://devblogs.microsoft.com/azure-sdk/october-2020-release).
* Jimmy Bogard [releases MediatR 9.0](https://jimmybogard.com/mediatr-9-0-released).
* Rahul Bhandari [releases the .NET Core October 2020 update for reliability and other non-security fixes](https://devblogs.microsoft.com/dotnet/net-core-october-2020), and Tara Overfield [republishes the July 2020 Security Only Updates for .NET Framework to resolve a known issue that affected the original release](https://devblogs.microsoft.com/dotnet/net-framework-republishing-of-july-2020-security-only-updates) as well as [the .NET Framework October 2020 security and quality rollup updates](https://devblogs.microsoft.com/dotnet/net-framework-october-2020-security-and-quality-rollup-updates).

### 📅 Community and events

* The .NET Foundation [provides a September/October 2020 update](https://dotnetfoundation.org/blog/2020/10/14/blog/posts/net-foundation-september-october-2020-update), and also [holds an "All Hands" meeting](https://www.youtube.com/watch?v=CX8wT0mO5qg).
* Speaking of, [.NET Foundation executive director Claire Novotny talks to the .NET Docs Show](https://www.youtube.com/watch?v=RBb-iuElZxI).
* For .NET standups, Entity Framework [talks to David Pine about his new SDK](https://www.youtube.com/watch?v=KenrBgNg8jk) and [ASP.NET discusses .NET 6 planning](https://www.youtube.com/watch?v=euM0GtPpbyc).
* Microsoft rolls out [.NET Live TV](https://devblogs.microsoft.com/dotnet/dotnet-live-tv).
* GitHub shows off [its new mobile PR capabilities](https://github.blog/2020-10-14-even-better-code-review-in-github-for-mobile/).
* Hadi Hariri [talks more about his issues with Git](https://hadihariri.com/2020/10/14/a-few-words-on-git/).
* Octopus Deploy [is now a corporate sponsor of the .NET Foundation](https://octopus.com/blog/dotnet-foundation).

### 😎 ASP.NET Core / Blazor

* Richard Reedy [adds gRPC to his Blazor app](https://www.telerik.com/blogs/how-to-add-grpc-to-your-blazor-app).
* Marinko Spasojevic [uses an access token with Blazor WASM HttpClient](https://code-maze.com/using-access-token-with-blazor-webassembly-httpclient/) and also [secures Blazor WebAssembly with IdentityServer4](https://code-maze.com/how-to-secure-blazor-webassembly-with-identityserver4/).
* Claudio Bernasconi [introduces Blazor Server](https://www.claudiobernasconi.ch/2020/10/14/introduction-to-blazor-server/).
* Niels Swimberghe [pushes UI changes from Blazor Server to browser on server raised events](https://swimburger.net/blog/dotnet/pushing-ui-changes-from-blazor-server-to-browser-on-server-raised-events).
* Jon Hilton [renders a Blazor WASM components in your existing MVC/Razor Pages application](https://jonhilton.net/blazor-wasm-in-razor-pages/), and also [continues prerendering Blazor WASM with .NET 5](https://jonhilton.net/blazor-wasm-prerendering-missing-http-client/).
* Steve Fenton [writes about simple conditional updates to entities in ASP.NET Core MVC](https://www.stevefenton.co.uk/2020/10/simple-conditional-updates-to-entities-in-asp-net-core-mvc/).

### ⛅ The cloud

* Dave Brock (ahem) [deploys an Azure Static Web App using Blazor and Azure Functions](https://daveabrock.com/2020/10/13/azure-functions-static-apps-blazor).
* Abel Wang [creates the Static Web App PR Workflow for Azure App Service using Azure DevOps](https://devblogs.microsoft.com/devops/static-web-app-pr-workflow-for-azure-app-service-using-azure-devops).
* Mickaël Derriey [uses Azure Identity with Azure SQL, Graph, and Entity Framework](https://devblogs.microsoft.com/azure-sdk/azure-identity-with-sql-graph-ef).
* Scott Hanselman [migrated his blog to Azure, and the work is just beginning](https://www.hanselman.com/blog/migrating-this-blog-to-azure-its-done-now-the-work-begins).

### 📔 C#

* Matthew Jones [discusses classes and members in C#](https://exceptionnotfound.net/csharp-in-simple-terms-7-class-basics-properties-methods-access), and also [talks about methods, parameters, and arguments in C#](https://exceptionnotfound.net/csharp-in-simple-terms-6-methods-parameters-and-arguments).
* Steve Gordon [continues his series on System.Threading.Channels](https://www.stevejgordon.co.uk/dotnet-internals-system-threading-channels-unboundedchannelt-part-3).
* Carmel Eve [runs through the Decorator pattern in C#](https://endjin.com/blog/2020/10/design-patterns-in-csharp-the-decorator-pattern.html).
* Michael Shpilt [works with dynamic Queries with expressions Trees in C#](https://michaelscodingspot.com/dynamic-queries/).
* Khalid Abuhakmeh [works through switch expressions in C# 8](https://khalidabuhakmeh.com/switch-expressions-in-csharp-8).
* Marco Siccardi [writes about helpful extensions with dealing with types](https://msicc.net/some-helpful-extensions-when-dealing-with-types-in-net/).
* Jerome Laban [profiles C# 9 source generators](https://jaylee.org/archive/2020/10/10/profiling-csharp-9-source-generators.html).
* Cezary Piatek [tracks down async code smells with analyzers](https://cezarypiatek.github.io/post/async-analyzers-p1/).
* Anthony Giretti [introduces covariant returns in C# 9](https://anthonygiretti.com/2020/10/12/introducing-c-9-covariant-returns/).
* Patrick Smacchia [discusses immutable classes with C# 9 records](https://blog.ndepend.com/c9-records-immutable-classes/).

### 📗 F#

* Isaac Abraham says [you shouldn't be skeptical about F# tooling](https://www.compositional-it.com/news-blog/sceptical-about-f-tooling-these-days-you-shouldnt-be/), and also [creates Azure Functions with Farmer](https://www.compositional-it.com/news-blog/creating-azure-functions-with-farmer/).
* Istvan walks through [why he chose F# for his AWS Lambda project](https://dev.to/l1x/why-i-chose-f-for-our-aws-lambda-project-4978).
* Christian Findlay [discusses how to use F# from C#](https://christianfindlay.com/2020/10/17/how-to-use-fsharp-and-csharp/).
* Nabeel Valley [walks through scripting with F#](https://nabeelvalley.netlify.app/stdout/2020/13-10/launch-fsi-from-terminal/).
* Mariusz [discusses file order in F#](https://dev.to/klimcio/file-order-in-f-the-most-annoying-thing-for-a-beginner-38dc).
* Christina Ljungberg [unit tests with F#](https://christinaljungberg-independent.com/2020/10/15/unit-testing-with-f/).



### 🔧 Tools

* Mark Downie [steps into a specific method in Visual Studio](https://www.poppastring.com/blog/debugger-tip-step-into-a-specific-method).
* Kayla Cinnamon [offers Windows Terminal tips and tricks](https://devblogs.microsoft.com/commandline/windows-terminal-tips-and-tricks).
* Georg Dangl [runs SQL Server integration tests in .NET Core with Docker](https://blog.dangl.me/archive/running-sql-server-integration-tests-in-net-core-projects-via-docker/).
* ErikEJ discusses [EF Core Database First gotchas and workarounds](https://erikej.github.io/efcore/2020/10/12/ef-core-sqlserver-scaffolding-gotchas.html).
* Kevin Griffin [has a quick post about upgrading .NET CLI templates](https://consultwithgriff.com/how-to-upgrade-dotnet-cli-templates).
* Michael Shpilt [writes about four awesome tools for debugging .NET in production](https://oz-code.com/blog/production-debugging/4-awesome-tools-for-net-debugging-in-production).
* Steve Fenton [executes raw SQL scripts in EF Core](https://www.stevefenton.co.uk/2020/10/execute-raw-sql-scripts-in-entity-framework-core/).
* Shahed Chowdhuri [debugs multiple .NET Core projects in VS Code](https://wakeupandcode.com/debugging-multiple-net-core-projects-in-vs-code/).
* David Grace [sets up a Blazor WASM and ASP.NET Core Web API in Azure DevOps](https://www.roundthecode.com/dotnet/setup-your-blazor-wasm-and-asp-net-core-web-api-in-azure-devops).
* Paul Michaels [executes dynamically generated SQL in EF Core](https://www.pmichaels.net/2020/10/10/executing-dynamically-generated-sql-in-ef-core).
* Nicholas Blumhardt [bootstraps logging with Serilog + ASP.NET Core](https://nblumhardt.com/2020/10/bootstrap-logger/).
* Scott Hanselman [keeps his WSL Linux instances up to date automatically within Windows 10](https://www.hanselman.com/blog/keeping-your-wsl-linux-instances-up-to-date-automatically-within-windows-10).

### 📱 Xamarin

* Mitchel Sellers [adds dark theme support for Xamarin Forms Shell](https://www.mitchelsellers.com/blog/article/adding-dark-theme-support-for-xamarin-forms-shell).
* Leomaris Reyes [offers 5 tips to use the grid for layouts](https://www.syncfusion.com/blogs/post/5-tips-to-easily-use-the-grid-for-layouts-in-xamarin-forms.aspx).
* Yana Kerpecheva [chooses a time duration with Telerik TimeSpan Picker for Xamarin](https://www.telerik.com/blogs/choose-time-duration-telerik-timespan-picker-xamarin).
* Delpin Susai Raj [supports dark mode](https://xamarinmonkeys.blogspot.com/2020/10/xamarinforms-support-dark-mode.html).

### 🎤 Podcasts

* The .NET Rocks podcast [talks about migrating .NET Applications to Azure with Mike Richter](https://www.dotnetrocks.com/default.aspx?ShowNum=1709).
* The Changelog [talks to Spotify about shipping at scale](https://changelog.com/podcast/415).
* The Azure DevOps Podcast [talks about what's new in Azure App Service](http://azuredevopspodcast.clear-measure.com/stefan-schackow-on-whats-new-in-azure-app-service-episode-110).
* The Coding Blocks podcast [continues talking about the DevOps Handbook](https://www.codingblocks.net/podcast/the-devops-handbook-enable-daily-learning/).

### 🎥 Videos

* The ON.NET Show talks about [PWAs with Blazor](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-PWAs-with-Blazor), the [YARP reverse proxy](https://www.youtube.com/watch?v=1IqQkNcsqWE), and [Azure Static Web Apps](https://www.youtube.com/watch?v=bVSq1rKcW-o).
* Immo Landwerth discusses [the journey to .NET 5](https://www.youtube.com/watch?v=-kAr21i11f4) and also [.NET 5 cross-plat development](https://www.youtube.com/watch?v=A_y1gIzzRT8).
