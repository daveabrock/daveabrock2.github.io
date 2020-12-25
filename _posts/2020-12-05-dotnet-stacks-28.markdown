---
date: "2020-12-05"
title: "The .NET Stacks, #28: The future of MVC and themes of .NET 6"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-28-card.png
subtitle: This week, we look at the future of APIs in ASP.NET Core MVC and the "themes" of .NET 6.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*Note: This is the published version of my free, weekly newsletter, The .NET Stacks. It was originally sent to subscribers on November 30, 2020. Subscribe at the bottom of this post to get the content right away!*

Happy Monday to all. This week, we'll be discussing:

- The future of ASP.NET Core MVC APIs
- Check out the "themes" of .NET 6
- .NET Q&A: Not the Overflow you're looking for
- Last week in the .NET world

## The future of ASP.NET Core MVC APIs

This week, David Fowler [stopped by the ASP.NET community standup](https://www.youtube.com/watch?v=d9Bjg31VuHw) to talk about ASP.NET Core architecture. As you'd expect, it was quite informative. We learned how the team is addressing MVC, ASP.NET Core's exception to the rule.

As ASP.NET Core has kept an eye on lightweight and modular services, MVC isn't exactly a shining example of this concept. As with a lot of Microsoft tech, there are a lot of historical reasons for this—when thinking about MVC for ASP.NET Core, a lot of time was spent merging APIs and MVC web bits into one platform so performance analysis wasn't given the light of day.

If you develop APIs with MVC, then, you're paying a lot up front. When a lot of use cases are using it for CRUD operations over HTTP, do you need all the extension filters, formatters, and so on? Why pay for all of MVC if you aren't using it? As said in the standup, we're now looking at a "framework for framework authors" where all the abstractions aren't used by 99% of developers.

Fowler and team are working on something called "Project Houdini"—an effort to "make MVCs disappear." Don't worry, MVC will always be around, but the effort revolves pushing MVC productivity features to the core of the stack, and not being a special citizen. Along the way, performance should improve. Fowler showed off a way to generate imperative APIs for you at compile time using source generation (allowing you to keep the traditional MVC controller syntax).

As a result, you're shipping super-efficient code much closer to the response pipe. And when AOT gets here, it'll benefit from runtime performance and treeshaking capabilities. Stay tuned: there's a lot to wrap your head around here, with more information to come.

## Check out the "themes" of .NET 6

It's a fun time in the .NET 6 release cycle—there's a lot of design and high-level discussions going on, as it'll be 11 months before it actually ships. You can definitely look at things like the [.NET Product Roadmap](https://github.com/dotnet/core/blob/master/product-roadmap/current.md), but .NET PM Immo Landwerth has [built a *Themes of .NET* site](https://themesof.net/), which shows off the GitHub themes, epics, and stories slotted for .NET 6. (As if you had to ask, yes, [it's built with Blazor](https://github.com/terrajobst/themesof.net).)

As [Immo warns](https://twitter.com/terrajobst/status/1331821633787424769), this is all fluid and not a committed roadmap—it *will* change frequently as the team's thinking evolves. Even at this stage, I thought it was interesting to filter by the Priority 0 issues. These aren't guarantees to make it in, but it *is* what the team currently is prioritizing the highest.

📲 With Xamarin coming to .NET 6 [via MAUI](https://devblogs.microsoft.com/dotnet/introducing-net-multi-platform-app-ui/), there's predictably [a lot of Priority 0 work](https://github.com/dotnet/xamarin/issues/2) outlined here—from [managing mobile .NET SDKs](https://github.com/dotnet/xamarin/issues/4), [improving performance](https://github.com/dotnet/xamarin/issues/10), and [using .NET 6 targets](https://github.com/dotnet/xamarin/issues/16).

🏫 I'm happy to see an epic around appealing to new developers and students, and there are epics around [teaching entire classes in .NET Notebooks in VS Code](https://github.com/dotnet/core/issues/5466) and [making setup easier](https://github.com/dotnet/core/issues/5467). They're also prioritizing democratizing machine learning with stories for [using data when training in the cloud](https://github.com/dotnet/machinelearning-modelbuilder/issues/556) and [better data loading options](https://github.com/dotnet/machinelearning-modelbuilder/issues/642).

🌎 Blazor developers are anxious for [enabling ahead-of-time (AOT) compilation](https://github.com/dotnet/runtimelab/issues/248), with stories around [compiling .NET apps into WASM](https://github.com/dotnet/runtime/issues/44316) and [AOT targeting](https://github.com/dotnet/runtime/issues/43390).

✅ Acknowledging carryover from .NET 5 promises, there are stories for [building libraries for HTTP/3](https://github.com/dotnet/runtime/issues/43546) and *confidently* [generating single file apps for supported target platforms](https://github.com/dotnet/runtime/issues/43540). There's also [more on app trimming](https://github.com/dotnet/runtime/issues/43543) planned, especially [when using `System.Text.Json`](https://github.com/dotnet/runtime/issues/1568). 

📈 As promised, a lot to improve the inner-loop performance—plans for the [BCL to support hot reloading](https://github.com/dotnet/runtime/issues/45023) (and [for Blazor](https://github.com/dotnet/aspnetcore/issues/5456) to support it), [improving MSBuild performance](https://github.com/dotnet/msbuild/issues/5876), and [an improved experience for Xamarin devs](https://github.com/dotnet/xamarin/issues/13).

There's so much to look through if you've got the time, and things are going to move around a lot—there's a ton of XL-sized Priority 0 issues, for example—but it's always nice to geek out.

## Microsoft Q&A for .NET: Not the Overflow you're looking for

This week, Microsoft [announced](https://devblogs.microsoft.com/dotnet/announcing-microsoft-q-and-a-for-dotnet/) Microsoft Q&A for .NET. The Q&A experience, a replacement for Microsoft's older forum platforms like MSDN and TechNet, was [first rolled out last October](https://docs.microsoft.com/teamblog/introducing-microsoft-qanda) (and [went GA in May 2020](https://docs.microsoft.com/teamblog/microsoft-qna-ga?WT.mc_id=launchblog-blog-learn-tv)). The inevitable question: "Why not just use and/or buy Stack Overflow?" I find it interesting that this week's post didn't mention what every developer was thinking, but I've explored [Microsoft's thoughts from last year](https://docs.microsoft.com/answers/articles/388/microsoft-qa-frequently-asked-questions.html):

>We love Stack Overflow. We will continue supporting our customers who ask questions there. In the future, we will introduce a feature that points askers on Microsoft Q&A to relevant answers from Stack Overflow ... However, Stack Overflow has specific criteria about what questions are appropriate for the community and Microsoft Q&A will have a more open policy regarding this. More importantly, via Microsoft Q&A we can create unique experiences that allow us to provide the highest level of support for our customers.

In Microsoft's defense, [the line](https://docs.microsoft.com/answers/questions/773/why-not-just-use-stack-overflow.html) "I see too many questions on SO that I believe are viable in any normal support scenario, but get closed and downvoted because people didn't follow the rules" is all too familiar. There's a lot of value in a Microsoft-focused platform centered around answers, not reputation.

In addition, Microsoft is looking to own the experience with additional support channels, badges, and integration with other Microsoft services. I don't think Stack Overflow is getting nervous—the Q&A UX is similar to the MSDN forums, and that's not a compliment—but the platform is there if you want to try it. I'll be curious to see how it evolves.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Andrew Lock [applies the MVC design pattern to Razor Pages](https://andrewlock.net/aspnetcore-in-action-2e-applying-the-mvc-design-pattern-to-razor-pages/).
* Khalid Abuhakmeh [introduces Entity Framework Core 5](https://blog.jetbrains.com/dotnet/2020/11/25/getting-started-with-entity-framework-core-5/).
* Rick Strahl [warns against .NET Core runtime bitness for IIS installs when upgrading to .NET 5](https://weblog.west-wind.com/posts/2020/Nov/25/Watch-out-for-NET-Core-Runtime-Bitness-for-IIS-Installs).

### 📢 Announcements

* Jayme Singleton [wraps up .NET Conf 2020](https://devblogs.microsoft.com/dotnet/dotnetconf-2020-recap).
* James Montemagno [announces Microsoft Q&A for .NET](https://devblogs.microsoft.com/dotnet/announcing-microsoft-q-and-a-for-dotnet/).
* Maoni Stephens [writes about the new GetGCMemoryInfo API in .NET 5](https://devblogs.microsoft.com/dotnet/the-updated-getgcmemoryinfo-api-in-net-5-0-and-how-it-can-help-you/).
* Klaus Loeffelmann [writes about VB WinForms apps in .NET 5 and Visual Studio 16.8](https://devblogs.microsoft.com/dotnet/visual-basic-winforms-apps-in-net-5-and-visual-studio-16-8/?WT.mc_id=DOP-MVP-4025064).
* Tara Overfield [provides a .NET Framework November 2020 update](https://devblogs.microsoft.com/dotnet/net-framework-november-2020-cumulative-update-preview).
* Azure Pipelines [is replacing the "View YAML" experience](https://devblogs.microsoft.com/devops/replacing-view-yaml).

### 📅 Community and events

A light week because of the US Thanksgiving Holiday, so just one community standup—for ASP.NET, [David Fowler joins to talk about ASP.NET Core architecture](https://www.youtube.com/watch?v=d9Bjg31VuHw).

### 😎 ASP.NET Core / Blazor

* Dave Brock [isolates and test service dependencies in Blazor](https://daveabrock.com/2020/11/22/blast-off-blazor-service-dependencies).
* Ricardo Peres [works with inline images in ASP.NET Core](https://weblogs.asp.net/ricardoperes/inline-images-with-asp-net-core) and also pitfalls of [DI lifetime validation in ASP.NET Core](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-dependency-injection-lifetime-validation?WT.mc_id=DOP-MVP-4025064).
* Shaun C. Curtis [runs Blazor WASM and Server in a single project on a single site](https://www.codeproject.com/Articles/5287009/Blazor-WASM-and-Server-in-a-Single-Project-running).
* Khalid Abuhakmeh [writes about JS isolation, modules, and dynamic C# with Blazor](https://khalidabuhakmeh.com/blazor-javascript-isolation-modules-and-dynamic-csharp).
* Jason Farrell [uses scoped dependencies in ASP.NET Core](https://jfarrell.net/2020/11/23/using-scoped-dependencies/).
* Imar Spaanjaars [improves his ASP.NET Core site's e-mail capabilities](https://imar.spaanjaars.com/614/improving-your-aspnet-core-sites-e-mailing-capabilities).
* Marinko Spasojevic [writes about Blazor CSS isolation](https://code-maze.com/css-isolation-in-blazor-applications/) and also [works with custom validation in Blazor WebAssembly](https://code-maze.com/custom-validation-in-blazor-webassembly/).
* David Hayden [generates a client for ASP.NET Core Web API using OpenAPI](https://www.davidhayden.me/blog/generate-client-for-asp-net-core-web-api-using-openapi).
* David Grace [writes about four useful tips when using the xUnit Test Server in ASP.NET Core](https://www.roundthecode.com/dotnet/asp-net-core-web-api/four-useful-tips-using-asp-net-core-testserver-in-xunit).

### 🚀 .NET 5

* Alex Yakunin [provides more updates on his performance improvements working with .NET 5](https://medium.com/swlh/astonishing-performance-of-net-5-more-data-5cdc8d821e8c).
* Rick Strahl [upgrades his apps and libraries to .NET 5](https://weblog.west-wind.com/posts/2020/Nov/24/Upgrading-to-NET-Core-50).
* Davide Benvegnù [talks about why .NET 5 is perfect for DevOps](https://dev.to/n3wt0n/net-5-is-perfect-for-devops-4afd).
* Scott Hanselman [makes a self-contained WinForms app with .NET 5 from the command-line](https://www.hanselman.com/blog/how-to-make-a-winforms-app-with-net-5-entirely-from-the-command-line-and-publish-as-one-selfcontained-file).

### ⛅ The cloud

* Dave Brock uses [Azure Functions, Azure Storage blobs, and Cosmos DB to copy images from public URLs](https://daveabrock.com/2020/11/25/assets/img-azure-blobs-cosmos).
* Adrian Hall [announces Azure Mobile Apps v4.2.0 for .NET](https://devblogs.microsoft.com/xamarin/azure-mobile-apps-updates).
* Tim Sander [writes about the difference between null and undefined in Azure Cosmos DB](https://devblogs.microsoft.com/cosmosdb/difference-between-null-and-undefined).
* Mark Heath [writes about Azure Durable Entities](https://markheath.net/post/durable-entities-what-are-they-good-for).
* AWS had an outage, [and an interesting writeup about it](https://aws.amazon.com/message/11201/).

### 📔 Languages

* Gergely Sinka [installs .NET Core apps on Linux in 5 minutes](https://developer.okta.com/blog/2020/11/25/how-to-install-dotnetcore-on-linux).
* Thomas Levesque [continues writing about C# 9 records as strongly-typed IDs](https://thomaslevesque.com/2020/11/23/csharp-9-records-as-strongly-typed-ids-part-2-aspnet-core-route-and-query-parameters/).
* Johnson Manohar [writes about 4 steps to export Excel files to JSON using C#](https://www.syncfusion.com/blogs/post/export-excel-files-to-json-using-c-sharp.aspx).
* Vladimir Khorikov [uses C# 9 records as DDD value objects](https://enterprisecraftsmanship.com/posts/csharp-records-value-objects/).
* Khalid Abuhakmeh [consumes SOAP APIs in .NET Core](https://khalidabuhakmeh.com/consuming-soap-apis-in-dotnet-core).
* Prashant Pathak [slices in F# and writes about how it's different in F# 5](https://www.compositional-it.com/news-blog/slicing-in-f-and-how-its-a-bit-better-in-f-5).

### 🔧 Tools

* Scott Hanselman [shows off the Spectre.Console project](https://www.hanselman.com/blog/spectreconsole-lets-you-make-beautiful-console-apps-with-net-core).
* David Grace [uses ASP.NET Core's TestServer in xUnit](https://www.roundthecode.com/dotnet/asp-net-core-web-api/asp-net-core-testserver-xunit-test-web-api-endpoints).
* Matt Ward [writes about NuGet support in Visual Studio for Mac 8.8](https://lastexitcode.com/blog/2020/11/21/NuGetSupportInVisualStudioMac8-8/).
* Jesse Liberty [continues his series on Git](http://jesseliberty.com/2020/11/22/get-git-part-2/).
* Nada Rifki [writes about 9 new 2020 browser features](https://www.telerik.com/blogs/9-best-new-2020-browser-features-you-didnt-know).

### 📱 Xamarin

* Daniel Hindrikes [works on GitHub Actions and Cake for TinyMvvm](https://danielhindrikes.se/index.php/2020/11/26/github-actions-for-tinymvvm/).
* Rendy Del Rosario [works with background distance-based location updates on iOS](https://www.xamboy.com/2020/11/26/background-distance-based-location-updates-on-ios/).
* Charlin Agramonte [simplifies bindable properties with type converters in Xamarin Forms](https://xamgirl.com/simplifying-bindable-properties-with-type-converters-in-xamarin-forms/).
* Antonio Liccardi [talks about new features with Xamarin.Forms 5.0](https://www.infoq.com/news/2020/11/xamarin-forms-5-improvements/).
* Leomaris Reyes [writes about what's new in Xamarin.Essentials 1.6](https://www.telerik.com/blogs/exploring-whats-new-xamarin-essentials-1-6).
* James Montemagno [enables C# 9 in Xamarin and .NET Standard projects](https://montemagno.com/enabling-c-9-in-xamarin-net-standard-projects/).
* Delpin Susai Raj [uses a file browser](https://xamarinmonkeys.blogspot.com/2020/11/xamarinforms-file-browser.html).

### 🎤 Podcasts

* Shawn Wildermith talks to [the .NET Rocks podcast](https://www.dotnetrocks.com/default.aspx?ShowNum=1715) and [Herding Code](https://herdingcode.com/herding-code-243-shawn-wildermuth-on-his-new-film-hello-world) about his new *Hello World* film.
* The Stack Overflow Podcast [discusses how Big Tech is getting cozy with CS departments](https://the-stack-overflow-podcast.simplecast.com/episodes/big-tech-is-getting-cozy-with-computer-science-departments-82K23tP3).
* The 6 Figure Developer podcast [talks to Jeremy Sinclair](https://6figuredev.com/podcast/episode-171-jeremy-sinclair-windows-insider-net-on-arm/).
* The MS Dev Show [talks to Scott Hunter about .NET 5](https://msdevshow.com/2020/11/dot-net-5-with-scott-hunter/).
* Technology and Friends [talks to Omkar Naik on Microsoft Cloud for Health Care](http://davidgiard.com/2020/11/23/OmkarNaikOnMicrosoftCloudForHealthCare.aspx).
* The Unhandled Exception Podcast [talks with Pete Gallagher about teaching kids to code](https://unhandledexceptionpodcast.com/posts/0004-petegallagher/).
* The Work Item podcast [talks to Christos Matskas](https://theworkitem.com/blog/interview-christos-matskas/).
