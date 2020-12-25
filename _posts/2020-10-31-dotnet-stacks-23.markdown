---
date: "2020-10-31"
title: "The .NET Stacks #23: .NET 5 support, migration tools, and links"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-23-card.png
subtitle: This week, .NET 5 RC 2 ships, we talk .NET Foundation, and look around the community.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Get yourself a teammate that looks at your code [like Prince William looks at KFC](https://twitter.com/kieraneverson/status/1318602963921883138).

Here's what we have this week:

* Understand the release cycles of .NET
* AWS releases open-source migration tool
* An update on ASP.NET Core feedback for .NET 6
* Last week in the .NET world

# ⏲ Understand the .NET release cycles and support

It's hard to believe that two weeks from tomorrow, .NET 5 will be generally available. Week by week, we've been poking around the improvements between general .NET, ASP.NET Core and Blazor, and EF Core 5, which are all released together now. While the release candidates offer production licenses, on November 10 things will get real.

To that end, you might be wondering when future releases will occur, how Microsoft is supporting .NET 5, and what that means for older versions of .NET Core and .NET Framework. (This isn't as sexy as Blazor, *I suppose*, but is important to understand as you plan your app's future.)

In terms of future releases, new major releases will be released every November.

![.NET release schedule]({{ site.url }}{{ site.baseurl }}/assets/img/release-schedule.png)

As you can see, "LTS" and "Current" support will alternate between releases. As a refresher, let's walk through those.

* **Long Term Support (LTS) releases** - supported for a minimum of three years (or 1 year after the next LTS ships, whatever is longer). The current LTS releases are .NET Core 2.1 (with end of support on August 21, 2021), and .NET Core 3.1 (December 3, 2022).
* **Current releases** - supported for 3 months after the next major or minor release ships

.NET 5 is a "Current" release. Typically, you'd have three months to upgrade to 5.1 but Microsoft says [they're no longer doing point releases](https://twitter.com/terrajobst/status/1315032612914618368). When it comes to .NET 6, it'll be LTS (supported for three years after general availability)—giving you a less hands-on approach. There's a [Twitter discussion](https://twitter.com/terrajobst/status/1315029878001950720) on the benefits and drawbacks.

Microsoft has published an [official .NET 5 support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) if you want to explore this further.

**What about .NET Framework?** While .NET Framework is not dead, [it's basically done](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/). All new features will be built on .NET Core. With .NET Framework 4.8 being the last major release for .NET Framework, it'll only be supported with reliability and security fixes. You aren't being forced to move .NET Framework apps to .NET Core, and there are no plans to remove .NET Framework from Windows, but you won't see any feature improvements.

# 🔨 AWS open-sources .NET Core migration tool

This week, Amazon Web Services (AWS) [open-sourced](https://github.com/aws/porting-assistant-dotnet-client/blob/main/README.md) their [Porting Assistant for .NET](https://aws.amazon.com/porting-assistant-dotnet/) tool, which assists you in [porting your .NET Framework apps to .NET Core on Linux](https://visualstudiomagazine.com/articles/2020/10/21/aws-net-porting-tool.aspx). It was first released in July.

AWS [says their differentiator](https://aws.amazon.com/blogs/aws/announcing-the-porting-assistant-for-net/) is assessing the entire tree of package dependencies and functionality like incompatible APIs—it uses solution files as the starting point, preventing the need to analyze individual binary files. Interestingly, this single tool appears to be more robust than Microsoft's migration tools. 

This begs the question: what does Microsoft offer? For their part, Microsoft has a [comprehensive document about migrating from .NET Framework to .NET Core](https://docs.microsoft.com/dotnet/core/porting/). They also have a [list of tools you can utilize](https://docs.microsoft.com/dotnet/core/porting/tools).

* The [.NET Portability Analyzer](https://docs.microsoft.com/dotnet/standard/analyzers/portability-analyzer), which analyzes your code and provides a report about how portable your code is between Framework and Core
* The [.NET API Analyzer](https://docs.microsoft.com/dotnet/standard/analyzers/api-analyzer), a Roslyn analyzer that detects compatibility in cross-platform libraries
* The [Platform Compatibility Analyzer](https://docs.microsoft.com/dotnet/standard/analyzers/platform-compat-analyzer), another Roslyn analyzer that informs devs when using platform-specific APIs from call sites where API might not be supported
* A [`try-convert` utility](https://www.nuget.org/packages/try-convert/), a .NET Core global tool that helps convert from the `project.json` model to the `.csproj` model. Microsoft makes clear it is not supported in any way.

It reminds me of the old American football adage: if you have two quarterbacks, you have none. I've heard a common question: shouldn't Microsoft be leading this charge since ... they own .NET and are the most familiar with the platform? Why haven't they invested in robust migration tooling? I see a few factors at play here.

Considering .NET is a free product, the Microsoft play is to get you on Azure. What's to prevent you from using their awesome conversion tool and heading over to the competition? From AWS's standpoint, they're quite incentivized to do this: the sell for .NET devs is hard enough and they need to make things as easy as possible. If you couple that with the complexities of this tooling, Microsoft seems to be a little leery of making any big investments in that space.

# 📓 An update on ASP.NET Core feedback for .NET 6

A few weeks ago, I [wrote about](https://daveabrock.com/2020/10/16/dotnet-stacks-21) the [GitHub issue](https://github.com/dotnet/aspnetcore/issues/26625) you can visit to chime in on the future of ASP.NET Core for .NET 6. I'd like to call out [a comment by  Muhammad Rehan Saeed](https://github.com/dotnet/aspnetcore/issues/26625) that outlines core ASP.NET issues that *aren't Blazor related*.

While Blazor is awesome and the future of ASP.NET Core, there are tons of ASP.NET developers who aren't using it for various reasons and are fine with using MVC or Razor Pages. If you're interested in providing feedback on those issues, look at Muhammad's comment—personally, I'm interested in [HTTP/3 support](https://github.com/dotnet/aspnetcore/issues/15271), [streaming API support for MVC](https://github.com/dotnet/aspnetcore/issues/11558), and [C# 8 nullable reference type support](https://github.com/dotnet/aspnetcore/issues/5680).

# 🌎 Last week in the .NET world

## 🔥 The Top 3

* David Ramel [talks about how AWS has open-sourced the tool for porting .NET framework apps to .NET Core](https://visualstudiomagazine.com/articles/2020/10/21/aws-net-porting-tool.aspx).
* Khalid Abuhakmeh [builds a Blazor farm animal soundboard](https://blog.jetbrains.com/dotnet/2020/10/22/building-a-blazor-farm-animal-soundboard/).
* Microsoft Edge announced new [preview builds for Linux](https://blogs.windows.com/msedgedev/2020/10/20/microsoft-edge-dev-linux).

## 📢 Announcements

* OData now supports [.NET 5](https://devblogs.microsoft.com/odata/asp-net-odata-8-0-preview-for-net-5).
* My Blazor CSS isolation Microsoft doc [has been published](https://docs.microsoft.com/aspnet/core/blazor/components/css-isolation?view=aspnetcore-5.0).
* Git 2.29 [is out](https://github.blog/2020-10-19-git-2-29-released/).
* Tara Overfield [provides an October 2020 .NET Framework update](https://devblogs.microsoft.com/dotnet/net-framework-october-2020-cumulative-update-preview-update).
* Jon Skeet [tours the .NET Functions Framework](https://codeblog.jonskeet.uk/2020/10/23/a-tour-of-the-net-functions-framework), a framework he built using Google Cloud Functions in C#.


## 📅 Community and events

* The .NET Foundation [joins the Open Source Initiative's Affiliate Program](https://opensource.org/node/1090).
* GitHub announced [the npm public roadmap and a new feedback process](https://github.blog/2020-10-22-introducing-the-npm-public-roadmap-and-a-new-feedback-process/).
* We had three .NET community standups this week: .NET tooling [discusses `dotnet` templates](https://www.youtube.com/watch?v=zh-1x2gXfQw), Machine Learning [talks ML.NET in VS Code](https://www.youtube.com/watch?v=ghqz3VO_Qo4), and ASP.NET [talks about distributed tracing](https://www.youtube.com/watch?v=PtN5y7rF90Y).
* The .NET Docs Show talks about [Reactive Extensions for UI frameworks with Rodney Littles II](https://www.youtube.com/watch?v=xndK7E3oCd4).

## 😎 ASP.NET Core / Blazor

* Dave Brock (ahem) [improves rendering performance with Blazor component virtualization](https://daveabrock.com/2020/10/20/blazor-component-virtualization).
* Safia Abdalla [talks about `ComponentBase` in Blazor](https://safia.rocks/blog/combing-through-component-base/).
* Peter Vogel asks: [is Blazor safe for the enterprise?](https://www.telerik.com/blogs/is-blazor-safe-enterprise-bet)
* Marinko Spasojevic [works with attribute-based access control with Blazor WebAssembly and IdentityServer4](https://code-maze.com/atribute-based-access-control-blazor-webassembly-identityserver4/), and also [integrates Blazor WebAssembly role-based security with IdentityServer4](https://code-maze.com/blazor-webassembly-role-based-security-with-identityserver4/).
* Brian Lagunas [uses NPM packages in Blazor](https://brianlagunas.com/using-npm-packages-in-blazor/).
* Tim Heuer [filters a Bootstrap table in C# and Blazor](https://timheuer.com/blog/filtering-data-table-with-blazor/).
* Niels Swimberghe [works on real-time apps with Blazor Server and Firestore](https://swimburger.net/blog/dotnet/real-time-applications-with-blazor-server-and-firestore), and also [configures ServicePointManager.SecurityProtocol through AppSettings](https://swimburger.net/blog/dotnet/configure-servicepointmanager-securityprotocol-through-appsettings).
* Damian Hickey [nests applications in ASP.NET Core 3.1](https://dhickey.ie/2020/10/20/aspnet-core-3-nested-apps/).
* Khalid Abuhakmeh [adds headers to a response in ASP.NET 5](https://khalidabuhakmeh.com/add-headers-to-a-response-in-aspnet-5).

## ⛅ The cloud

* Over at Code Maze, [they deploy an ASP.NET Core Web API to Azure API apps](https://code-maze.com/deploying-aspnet-core-web-api-azure-api-apps/).
* David Grace [continues creating and configuring a SQL Server database and ASP.NET Core Web API in Azure](https://www.roundthecode.com/dotnet/create-configure-sql-server-database-and-asp-net-core-web-api-in-azure).
* Damien Bowden [uses encrypted access tokens in Azure with Microsoft.Identity.Web and Azure app registrations](https://damienbod.com/2020/10/22/using-encrypted-access-tokens-in-azure-with-microsoft-identity-web-and-azure-app-registrations/), and also [implements a full-text search using Azure Cognitive Search in ASP.NET Core](https://damienbod.com/2020/10/19/implement-a-full-text-search-using-azure-cognitive-search-in-asp-net-core/).
* Szymon Kulec [offers tips for improving Azure Functions performance](https://blog.scooletz.com/2020/10/19/improving-Azure-Functions-performance).
* Daniel Krzyczkowski [monitors solutions with Azure Application Insights](https://daniel-krzyczkowski.github.io/Monitor-Azure-Solution-With-Azure-Application-Insights/), and also [manages release flow using Azure DevOps pipelines](https://daniel-krzyczkowski.github.io/Manage-Release-Flow-Using-Pipelines-In-Azure-DevOps/).
* ~~Hulk Hogan~~ Joe Guadagno [authenticates an API secured with Azure AD, Microsoft Identity, and React Native](https://www.josephguadagno.net/2020/10/24/working-with-microsoft-identity-react-native-client).

## 📔 C#

* Mads Torgersen [talks about the direction of C#](https://www-techrepublic-com.cdn.ampproject.org/c/s/www.techrepublic.com/google-amp/article/c-designer-torgersen-why-the-programming-language-is-still-so-popular-and-where-its-going-next/).
* Matthew Jones [talks about inheritance and polymorphism in C#](https://exceptionnotfound.net/csharp-in-simple-terms-9-inheritance-and-polymorphism), and also [works with structs and enums in C#](https://exceptionnotfound.net/csharp-in-simple-terms-8-structs-and-enums).
* David Hayden [talks about C# 9 records](https://www.davidhayden.me/blog/csharp-9-record-type), and also [discusses C# 9 init-only properties](https://www.davidhayden.me/blog/csharp-9-init-only-properties).
* Claudio Bernasconi [installs and uses C# 9 in Visual Studio 2019](https://www.claudiobernasconi.ch/2020/10/21/install-and-use-csharp-9-in-visual-studio-2019/).
* Carmel Eve [talks about the adapter pattern in C#](https://endjin.com/blog/2020/10/design-patterns-in-csharp-the-adapter-pattern.html).
* Ian Griffiths [warns against defeating Roslyn warnings with empty strings](https://endjin.com/blog/2020/10/dotnet-csharp-8-nullable-references-empty-strings.html).
* Anthony Giretti [discusses static anonymous functions in C# 9](https://anthonygiretti.com/2020/10/21/introducing-c-9-static-anonymous-functions/), and also [runs through local function attributes in C# 9](https://anthonygiretti.com/2020/10/19/introducing-c-9-attributes-on-local-functions/).
* Matthew MacDonald [runs through C# throughout its 20-year history](https://medium.com/young-coder/c-sharp-language-changes-from-1-0-to-9-0-b2282e8e30fd).
* Jeremy Smith [clears up some things about C# abstract classes and interfaces](https://jeremybytes.blogspot.com/2020/10/abstract-classes-vs-interfaces-in-c.html).

## 📗 F#

* Alican Demirtas [discusses JSON serialization in F#](https://www.compositional-it.com/news-blog/json-serialisation-in-f/).
* CompositionalIT [released a dojo to help you write SAFE applications](https://github.com/CompositionalIT/SAFE-Dojo).
* Patrick Tone [walks through using `bind` in F#](https://patrickt.one/2020/10/20/in-a-bind-with-fsharp.html).

## 🔧 Tools

* Scott Hanselman [talks about how you shouldn't break URLs](https://www.hanselman.com/blog/dont-ever-break-a-url-if-you-can-help-it).
* Joab Jackson [writes how the Dapr runtime is almost production-ready](https://thenewstack.io/the-dapr-distributed-runtime-nears-production-readiness/).
* Jason Gaylord [queries Snowflake using .NET](https://www.jasongaylord.com/blog/2020/10/22/query-snowflake-using-dotnet).
* Derek Comartin [discusses defining service boundaries with loosely coupled systems](https://codeopinion.com/defining-service-boundaries-by-splitting-entities).
* Andrew Lock [runs database migrations using jobs and init containers](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-8-running-database-migrations-using-jobs-and-init-containers/).
* Tyler Hakes [compares GitHub Actions and Azure DevOps](https://www.7pace.com/blog/azure-devops-vs-github).

## 📱 Xamarin

* Leomaris Reyes [adds shortcuts with Xamarin Essentials](https://askxammy.com/adding-shortcuts-with-xamarin-essentials/).
* Delpin Susai Raj [works on a network speed monitor](https://xamarinmonkeys.blogspot.com/2020/10/xamarinforms-network-speed-monitor.html).
* Glenn Versweyveld [works on custom visual state triggers and control templates](http://depblog.weblogs.us/2020/10/19/xamarin-forms-custom-visual-state-triggers-and-control-templates/).

## 🎤 Podcasts

* The Xamarin Podcast [talks about seeing AI](https://www.xamarinpodcast.com/80).
* The .NET Rocks podcast [talks about the .NET Foundation with Layla Porter](https://www.dotnetrocks.com/default.aspx?ShowNum=1710).
* The Azure Podcast [discusses through Azure Spring Cloud](http://azpodcast.azurewebsites.net/post/Episode-351-Azure-Spring-Cloud).

## 🎥 Videos

* The ON.NET Show talks about [event-driven apps on Kubernetes with KEDA](https://www.youtube.com/watch?v=RMGcaUjBk90).
* The Xamarin Show [talks about Xamarin.Forms 5](https://www.youtube.com/watch?v=ttF80UnrJAg).
* Michael Gilliland [uses SAFE in F# to create a billiards app](https://www.youtube.com/watch?v=mUs6elEwo_U&ab_channel=MichaelGilliland).
* Almir Mesic [talks about writing F# and Azure Functions apps for production](https://www.youtube.com/watch?v=nxuSMtWGZjc&ab_channel=NDCConferences).
