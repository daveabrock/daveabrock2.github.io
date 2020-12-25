---
date: "2020-11-21"
title: "The .NET Stacks #26: .NET 5 has arrived, let's party"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-26-card.png
subtitle: This week, we look at the .NET Conf content and also discuss the buzz around dependency injection.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*Note: This is the published version of my free, weekly newsletter: The .NET Stacks—originally sent to subscribers on November 16, 2020. Subscribe at the bottom of this post to get the content right away!*

Happy Monday, everybody! I hope you have a wonderful week. We'll be covering a few things this morning:

* .NET 5 has arrived
* Dependency injection gets a community review
* Last week in .NET world

# 🥳.NET 5 has arrived

Well, we made it: .NET 5 is [officially here](https://devblogs.microsoft.com/dotnet/announcing-net-5-0). This week, Microsoft showed off their hard work during three busy days of .NET Conf—and with it, some [swag](https://www.dotnetconf.net/swag) including my new [default Visual Studio Code theme](https://marketplace.visualstudio.com/items?itemName=dotnetconfteam.dotnet-purple-theme). I'm starting to wonder if overhyping Blazor is possible—as great as it is, I wish Microsoft would dial it back a bit.

Anyway: we've spent the better part of six months discussing all the updates (like, [my favorite things](https://daveabrock.com/2020/11/13/dotnet-stacks-25), [Blazor's readiness](https://daveabrock.com/2020/11/06/dotnet-stacks-24), [the support model](https://daveabrock.com/2020/10/31/dotnet-stacks-23), [what's happening to .NET Standard](https://daveabrock.com/2020/09/26/dotnet-stacks-18), [EF Core 5](https://daveabrock.com/2020/09/19/dotnet-stacks-17), [app trimming](https://daveabrock.com/2020/09/12/dotnet-stacks-16), [System.Text.Json vs. Newtonsoft](https://daveabrock.com/2020/08/08/dotnet-stacks-11), and so much more).

I know you’re here for the .NET Conf links—so let me be of service. You can see all the talks [from this YouTube playlist](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oVWop1HEOml2OdqbDs6IlcI), but the highlights are below.

From the Microsoft perspective, we had the following sessions:

* The Scotts [welcome you to .NET 5](http://www.youtube.com/watch?v=o-esVzL3YLI).
* Mads Torgersen and Dustin Campbell [talk about C# 9](https://www.youtube.com/watch?v=x3kWzPKoRXc), and Phillip Carter [introduces F# 5](https://www.youtube.com/watch?v=MPlVE8WdD-0).
* Immo Landwerth and Phillip Carter [port projects to .NET 5](http://www.youtube.com/watch?v=bvmd2F11jpA).
* Jeremy Likness and Shay Rojansky [talk about Entity Framework Core 5](https://www.youtube.com/watch?v=BIImyq8qaD4).
* James Newton-King [discusses high performance with gRPC](http://www.youtube.com/watch?v=EJ8M2Em5Zzc).
* Steve Sanderson and Safia Abdalla [talk about Blazor in .NET 5](https://www.youtube.com/watch?v=Nag6u5TxjIA), and Daniel Roth and Javier Calvarro talk about [integrating it with ASP.NET Core](https://www.youtube.com/watch?v=CEjqhTGrqDY).
* Rich Lander, Jan Kotas, and Stephen Toub [dive deep on the .NET 5 runtime](https://www.youtube.com/watch?v=qJXJnop1bZ0).
* Scott Hanselman [updates his site to .NET 5, live](https://www.youtube.com/watch?v=28D_roo3cUw).
* Chris Sfanos and Dmitry Lyalin [provide a .NET desktop development update](https://www.youtube.com/watch?v=NDYcq1yKhiA).
* Claire Novotny and Layla Porter [provide a .NET Foundation update](http://www.youtube.com/watch?v=ppIBnjAdgik).
* Maddy Leger and David Ortinau [roll out Xamarin.Forms 5](https://www.youtube.com/watch?v=M7UVz82dE90).
* Glenn Condrong and David Fowler [talk about microservices with Project Tye](https://www.youtube.com/watch?v=_s8UdhGOGmY).
* Kathleen Dollard and Rainer Sigwald [get to know the .NET 5 SDK](https://www.youtube.com/watch?v=WmOCtlvNaTQ).
* Leslie Richardson talks about [debugging in Visual Studio with Mark Downie](https://www.youtube.com/watch?v=cOYFDD3tU38).
* Robert Green and Brady Gaster talk about [HTTP API development with .NET, Azure, and OpenAPI](https://www.youtube.com/watch?v=G7Le65Ln_Qk).
* Jon Galloway talks with David Wengier [about C# source generators](https://www.youtube.com/watch?v=3YwwdoRg2F4).
* Eilon Lipton [shows off Blazor Mobile Bindings](https://www.youtube.com/watch?v=qTPHAlHom20).

From the community:

* Carl Franklin [talks about application state in Blazor apps](https://www.youtube.com/watch?v=GIupo55GTro).
* Ed Charbeneau [talks about Blazor stability testing tools](https://www.youtube.com/watch?v=WdB723tIWg0).
* Vaibhav Gujral [talks about Azure Static Web Apps](https://www.youtube.com/watch?v=-hxQNjZfRYg).
* Veronika Kolesnikova [talks about ML.NET, Azure, and Xamarin](https://www.youtube.com/watch?v=WKtUkZzi0jc).
* Menaka Basker [architects cloud native apps in Azure with .NET Core](https://www.youtube.com/watch?v=espeHztMDS8).
* Bryan Hogan [discusses Polly](https://www.youtube.com/watch?v=L9_fGJOqzbM).
* Lizzy Gallagher [discusses .NET Core migration at enterprise scale](https://www.youtube.com/watch?v=C-2haqb60No).
* Nico Paez talks about [testing with extension methods and fluent interfaces](https://www.youtube.com/watch?v=mF6qQCW_fOI).
* Zaid Ajaj [builds React applications with F#](https://www.youtube.com/watch?v=a6Ct3CM_lj4).
* Eduard Keilholz [gets real-time insights from serverless solutions](https://www.youtube.com/watch?v=PDMRaNoS2Xc).
* Luis Beltran [discusses AI enrichment with Azure Cognitive Search](https://www.youtube.com/watch?v=YapSbK75_1w).
* Florian Rappl [talks about microfrontends with Blazor](https://www.youtube.com/watch?v=npff2NjVXEE).

# 💉 Dependency injection gets a community review

During the day job, I’ve been teaching infrastructure engineers how to code in C#. With plenty of experience with scripting languages like PowerShell, explaining variables, functions, and loops isn’t really a big deal. Next week, however, I’ll have to discuss dependency injection and I’m not looking forward to it. That’s not because they won’t understand it, because they’re very smart and they definitely will (eventually). It’s because it’s a head-spinning topic to beginners but is essential to learn because ASP.NET Core is centered around it.

If you aren’t familiar with dependency injection, it’s a software design pattern that helps manage dependencies [towards abstractions and not lower-level implementation details](https://docs.microsoft.com/dotnet/architecture/modern-web-apps-azure/architectural-principles#dependency-inversion). [New is glue](https://ardalis.com/new-is-glue/), after all. In ASP.NET Core, we use interfaces (or base classes) to abstract dependencies away, and “register” dependencies in a service container (most commonly IServiceProvider in the ConfigureServices method in a project’s Startup class). This allows us to “inject” services (or groups of services) only when we need them.

Right on cue, this week’s edition of .NET Twitter Drama included the [discussion](https://twitter.com/davidfowl/status/1326271656776634368) of a [Hacker News article](https://t.co/Lq0KCy0jKZ?amp=1) criticizing the ASP.NET Core DI practice as overkill. The article mirrors a lot of HN: .NET rants with some valuable feedback nested inside.

In my experience, DI in general is a difficult concept to learn initially but sets you up for success for maintaining loose coupling and testability as your project matures. However, the initial ceremony can drive people away from ASP.NET Core as developers can view it as overkill if they think their project will never need it.

I do think the conversation this week helped folks understand that working with settings is very ceremonial and can lead to confusion (just look at the options pattern). This might foster .NET team discussions about doc updates and how to provide a better experience when working with configuration. A boy can dream, anyway.

# 🌎 Last week in the .NET world

## 🔥 The Top 3

* Maybe you've heard: .NET 5 is here. Richard Lander [has the announcement](https://devblogs.microsoft.com/dotnet/announcing-net-5-0), Daniel Roth [discusses ASP.NET Core 5](https://devblogs.microsoft.com/aspnet/announcing-asp-net-core-in-net-5), and Jeremy Likness [covers EF Core 5](https://devblogs.microsoft.com/dotnet/announcing-the-release-of-ef-core-5-0).
* From the language side, Mads Torgersen [provides an update on C# 9](https://devblogs.microsoft.com/dotnet/c-9-0-on-the-record) and Phillip Carter [announces F# 5](https://devblogs.microsoft.com/dotnet/announcing-f-5).
* Ed Charbeneau [writes about Blazor stability testing tools](https://www.telerik.com/blogs/blazor-stability-testing-tools-for-bulletproof-applications).

## 📢 Announcements

* For Visual Studio: Jacqueline Widdis [writes about the Visual Studio 2019 v16.8 and v16.9 Preview 1 releases](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-8), Pratik Nadagouda [writes about the release of the Git experience in Visual Studio](https://devblogs.microsoft.com/visualstudio/announcing-the-release-of-the-git-experience-in-visual-studio), and Jon Galloway [rolls out Visual Studio 2019 for Mac version 8.8](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-for-mac-version-8-8-is-now-available).
* Peter Groenewegen [provides an update on IntelliCode suggestions](https://devblogs.microsoft.com/visualstudio/intellicode-suggestion-apply-all).
* Joey Aiello [introduces PowerShell 7.1](https://devblogs.microsoft.com/powershell/announcing-powershell-7-1).
* Angela Zhang [announces the C#/WinRT Version 1.0 with .NET 5 GA](https://blogs.windows.com/windowsdeveloper/2020/11/10/announcing-c-winrt-version-1-0-with-the-net-5-ga-release).
* Rahul Bhandari [provides the .NET Core November 2020 updates](https://devblogs.microsoft.com/dotnet/net-core-november-2020), and Tara Overfield [discusses the .NET Framework 2020 security updates](https://devblogs.microsoft.com/dotnet/net-framework-november-2020-security-and-quality-rollup-updates).
* Filip Woj [releases dotnet-script 1.0.0](https://www.strathweb.com/2020/11/dotnet-script-1-0-0-released-with-support-for-net-5-0-and-c-9/).
* Kayla Cinnamon [walks through Windows Terminal Preview 1.5](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-5-release).
* Uno [discusses .NET 5 and C# 9 support in Uno Platform 3.2](https://platform.uno/blog/uno-platform-3-2-net-5-c-9-support-and-net-5-webassembly-aot-support/).

## 📅 Community and events

With .NET Conf, no community standups so a light section except: The .NET Docs Show [codes a drone with Bruno Capuano](https://www.youtube.com/watch?v=2xeKomASV0E).

## 🚀 .NET 5

* David Hayden [writes about EF Core 5 many-to-many relationships](https://www.davidhayden.me/blog/ef-core-5-many-to-many-relationships), [ASP.NET Core 5 model binding to C# 9 record types](https://www.davidhayden.me/blog/asp-net-core-5-model-binding-to-csharp-9-record-types), and [OpenAPI and Swagger UI in ASP.NET Core 5 Web API](https://www.davidhayden.me/blog/openapi-and-swagger-ui-in-asp-net-core-5-web-api).
* Michael MacDonald asks: [does .NET 5 deliver](https://medium.com/young-coder/does-net-5-deliver-8f3f89193d21)?
* Alex Yakunin [talks about his performance gains in .NET 5 when migrating his Fusion project](https://alexyakunin.medium.com/astonishing-performance-of-net-5-7803d69dae2e).
* Mitchel Sellers [talks about what to do now that .NET 5 is here](https://www.mitchelsellers.com/blog/article/net-5-is-here-now-what).
* Miguel Bernard [sums up the .NET 5 breaking changes](https://blog.miguelbernard.com/net-5-the-breaking-changes-you-need-to-know-about/).
* Andrea Chiarelli [writes about five things you should know about .NET 5](https://auth0.com/blog/dotnet-5-whats-new/).
* David Grace [answers five questions about .NET 5](https://www.roundthecode.com/dotnet/five-questions-you-may-have-about-asp-net-core-for-dotnet-5).

## 😎 ASP.NET Core / Blazor

* Marinko Spasojevic [writes about global HTTP error handling in Blazor WebAssembly](https://code-maze.com/global-http-error-handling-in-blazor-webassembly/).
* Jon Hilton asks: [is it possible to render components "dynamically" in Blazor](https://jonhilton.net/blazor-dynamic-components/)?
* Marinko Spasojevic [writes about lazy loading in Blazor WebAssembly](https://code-maze.com/lazy-loading-in-blazor-webassembly/).
* Khalid Abuhakmeh [implements a webhook framework in ASP.NET Core](https://khalidabuhakmeh.com/implement-a-webhook-framework-with-aspnetcore).
* Mike Brind [writes about implementing a custom model binder in Razor Pages](https://www.mikesdotnetting.com/article/353/implementing-a-custom-model-binder-in-razor-pages).
* Ricardo Peres [talks about possible pitfalls with localization in ASP.NET Core](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-localization-with-shared-resources).
* Henrick Tissink [talks about path versioning for ASP.NET Core APIs](https://dev.to/htissink/asp-net-core-api-path-versioning-197o).
* Imar Spaanjaars [implements health checks in ASP.NET Core](https://imar.spaanjaars.com/611/implementing-health-checks-in-aspnet-core).

## ⛅ The cloud

* Dominique St-Amand [migrates to the new C# Azure KeyVault SDK libraries](https://www.domstamand.com/migrating-to-the-new-csharp-azure-keyvault-sdk-libraries/).
* At CodeMaze, [using Azure WebJobs in .NET apps](https://code-maze.com/azure-webjobs-in-app-service/).
* Josh Hurley [runs through .NET 5 on AWS](https://aws.amazon.com/blogs/developer/net-5-on-aws).
* Christopher Christou [explores .NET 5 with the AWS Toolkit for Visual Studio](https://aws.amazon.com/blogs/developer/exploring-net-5-with-the-aws-toolkit-for-visual-studio).
* Justin Yoo [works with Azure Functions via GitHub Actions with no publish profile](https://techcommunity.microsoft.com/t5/apps-on-azure/azure-functions-via-github-actions-with-no-publish-profile).
* Damien Bowden [implements a web app and an ASP.NET Core secure API using Azure AD](https://damienbod.com/2020/11/09/implement-a-web-app-and-an-asp-net-core-secure-api-using-azure-ad-which-delegates-to-second-api/).

## 📔 Languages

* Miguel Bernard [writes about the unknown goodies for C# 9](https://blog.miguelbernard.com/c-9-the-unknown-goodies/).
* Khalid Abuhakmeh [writes about ExceptionDispatchInfo and capturing exceptions](https://khalidabuhakmeh.com/exceptiondispatchinfo-and-capturing-exceptions).
* Carmel Eve [writes about the Facade pattern in C#](https://endjin.com/blog/2020/11/design-patterns-in-csharp-the-facade-pattern.html).
* David Hayden [writes about top-level programs in C# 9](https://www.davidhayden.me/blog/top-level-programs-in-csharp-9).
* Dominique St-Amand [talks about handling being throttled by an API in C#](https://www.domstamand.com/csharp-ways-of-handling-when-being-throttled-by-an-api/).
* Claudio Bernasconi [talks about C# 9 record types](https://www.claudiobernasconi.ch/2020/11/07/csharp-9-record-types-introduction-and-deep-dive/).
* Oren Eini [profiles to investigate performance regression](https://ayende.com/blog/192325-A/always-profile-the-case-of-the-mysterious-performance-regression) and also [writes about the cost of serializing large object graphs in JSON](https://ayende.com/blog/192324-A/always-profile-the-hidden-cost-of-serializing-large-object-graphs-to-json).
* Bohdan Stupak [uses Span of T in F#](https://www.c-sharpcorner.com/article/using-spant-in-f-sharp/).
* Mike Melanson [writes about when to choose F# over Rust](https://thenewstack.io/this-week-in-programming-when-to-choose-f-over-rust/).

## 🔧 Tools

* Ian Bebbington [writes about cross-platform app authentication with Azure AD B2C and the Uno Platform](https://ian.bebbs.co.uk/posts/UnoB2C).
* Steve Smith [generates GitHub Actions from dotnet new templates](https://ardalis.com/github-actions-from-cli/).
* Khalid Abuhakmeh [dives into NuGet history for fun and community insights](https://blog.jetbrains.com/dotnet/2020/11/09/diving-into-nuget-history-for-fun-and-community-insights/).
* Iryne Somera [discusses the best 5 tools for .NET monitoring](https://stackify.com/best-5-tools-for-net-monitoring/).
* Paul Michaels [debugs a failed API request with Fiddler](https://www.pmichaels.net/2020/11/07/debugging-a-failed-api-request-and-defining-an-authorization-header-using-fiddler-everywhere).
* Idan Shatz [discusses the different ways to debug .NET apps running as Docker microservices](https://oz-code.com/blog/production-debugging/debugging-net-applications-running-as-docker-microservices-from-intrusive-to-non-intrusive).
* Adam Bertram [compares file formats between XML, YAML, JSON, and HCL](https://octopus.com/blog/state-of-config-file-formats).

## 📱 Xamarin

* Delpin Susai Raj [writes about a custom TitleView](https://xamarinmonkeys.blogspot.com/2020/11/xamarinforms-custom-titleview.html).
* Brandon Minnick [uses immutable objects with SQLite-Net](https://codetraveler.io/2020/11/11/using-immutable-objects-with-sqlite-net/).
* Leomaris Reyes [writes about a phone dialer and sending emails and SMS](https://www.telerik.com/blogs/phone-dialer-sending-emails-sms-xamarin-forms).
* Damien Tohin Doumer Kake [works on JWT social auth with ASP.NET Core and Xamarin Essentials](https://doumer.me/jwt-social-auth-with-asp-net-core-and-xamarin-essentials/).
* David Britch [displays SVGs as TabbedPage tab icons](https://www.davidbritch.com/2020/11/display-svgs-as-tabbedpage-tab-icons-in.html).

## 🎤 Podcasts

* The Xamarin Show [talks about .NET 5](https://www.xamarinpodcast.com/81).
* The .NET Rocks podcast [talks with Billy Hollis about the ROI of good UX design](https://www.dotnetrocks.com/default.aspx?ShowNum=1713).
* The Developers Road podcast [talks with Chris Woodruff about building your skillset](https://www.developersroad.com/episodes/005-chris-woodruff/), and also [about building community with Beth Massi](https://www.developersroad.com/episodes/003-beth-massi/).
* The 6-Figure Developer podcast [talks about self-care in COVID times](https://6figuredev.com/podcast/episode-169-welcome-back-ash-self-care-in-covid-times/).
* At Technology and Friends, [David Giard talks with Scott Hanselman about productivity](http://davidgiard.com/2020/11/09/ScottHanselmanOnProductivity.aspx).

## 🎥 Videos

* Derek Comartin [talks about C# performance with Steve Gordon](https://codeopinion.com/talking-c-performance-with-steve-gordon).
* Jeff Fritz [streams about SDK, project types, and testing](https://www.youtube.com/watch?v=HQT1GOA3hg8).
* The ASP.NET Monsters [walks through Azure SQL Elastic Query](https://www.youtube.com/watch?v=IAx1nsh5-Ao).
* Data Exposed [discusses Azure SQL database deployments](https://channel9.msdn.com/Shows/Data-Exposed/Leveling-Up-Your-Azure-SQL-Database-Deployments) and also [talks about Azure Arc Enabled SQL Managed Instance](https://channel9.msdn.com/Shows/Data-Exposed/What-is-Azure-Arc-Enabled-SQL-Managed-Instance--Data-Exposed).
* Mads Kristensen [walks through the new Git experience in Visual Studio 2019](https://www.youtube.com/watch?v=UHrAg3iKoe0).
* A new Azure Enablement video series [talks about architecting successful workloads on Azure](https://channel9.msdn.com/Shows/Azure-Enablement/Architect-successful-workloads-on-Azure--Introduction-Ep-1-Well-Architected-series).
