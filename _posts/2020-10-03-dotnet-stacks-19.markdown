---
date: "2020-10-03"
title: "The .NET Stacks #19: An Ignite recap and F# with Phillip Carter"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-19-card.png
subtitle: This week, we run through Ignite and also talk with Phillip Carter about F# from the Microsoft perspective.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

This week, we'll review Ignite 2020, talk with Phillip Carter about F# from the Microsoft perspective, and check out what's going on around the community.

## Ignite 2020 is a wrap

Build 2020 seems like so long ago, wasn't it? I covered it [in our very first issue](https://daveabrock.com/2020/05/24/dotnet-stacks-1) four months ago. This week, Microsoft put on Ignite 2020—virtually, of course. If you've followed .NET over the last few months, you didn't see a lot of surprises from the developer perspective—but there's still some good stuff to go through.

Let's see: .NET 5 RC [is now available](https://aka.ms/AA9g2s4) (but as a loyal reader, you knew that), Azure Communication Services [is now in preview](https://aka.ms/AA9gi1k) (with developers asking what that means for Twilio), [Visual Studio 2019 support for GitHub Codespaces is now in beta](https://aka.ms/AA9n0mf), there's [new capabilities for Azure Logic Apps](https://aka.ms/AA9g2qj), and Azure Cosmos DB [now has a serverless option](https://aka.ms/AA9gi0z). There's more to see, depending on your focus—so check out the [Ignite 2020 Book of News for the full treatment](https://news.microsoft.com/ignite-2020-book-of-news/).

You can [check out the sessions on-demand now](https://myignite.microsoft.com/sessions). Here's some that should be popular with .NET developers:

* [The future of .NET is .NET 5](https://myignite.microsoft.com/sessions/9c81f93e-2a2b-4f1c-91fd-4523a9959045) and a follow-up [Ask the Experts session with the .NET team](https://myignite.microsoft.com/sessions/0b186c68-9e6d-4dae-a85c-ef861b80fd6e)
* [Migrate, Modernize .NET applications on Azure](https://myignite.microsoft.com/sessions/ddc55aa4-c115-4377-9051-c0a7494499a1) and [two](https://myignite.microsoft.com/sessions/c0150e76-d436-40ef-bc3a-500c519f5eb3) [different](https://myignite.microsoft.com/sessions/779edf29-117c-4ea7-be97-26c3bb265176) Ask the Experts sessions
* [Are we there yet? App Development in Azure with Scott Hanselman and Friends](https://myignite.microsoft.com/sessions/1b0a0c40-053e-498f-a1e3-c907a56fb535)
* [.NET App Modernization and Migration from End to end, using data migration tools and Azure SQL](https://myignite.microsoft.com/sessions/41e9066a-cb2f-4585-a0ed-73e75b01a851)

In case it isn't completely obvious, these events are sales pitches for Azure—and the long game for developer productivity beats AWS and GCP by a country mile.

Would you like to get started in minutes without worrying about device specs or mind-numbing setup? Try [GitHub Codespaces](https://github.com/features/codespaces), a containerized environment in the cloud. Would you like to deploy your repo to the cloud in seconds? Easy. When you own the developer tools, the cloud platform, and the de-facto code sharing service in the industry, the "see how easy that was?" model is really paying off.

## Dev Discussions: Phillip Carter

Last week, we [talked to Isaac Abraham about F# from a C# developer's perspective](https://daveabrock.com/2020/09/19/dev-discussions-isaac-abraham). This week, I'm excited to get more F# perspectives from Phillip Carter. Phillip is a busy guy at Microsoft but a big part of his role as a Program Manager is overseeing F# and its tooling.

In this interview, we talk to Phillip about Microsoft F# support, F# tooling (and how it might compare to C#), good advice for learning F#, and more.

![Phillip Carter]({{ site.url }}{{ site.baseurl }}/assets/img/phillip-carter.jpg)

## From the build system to the tooling, is F# a first-class citizen in Visual Studio and other tools like C# is? If I'm a C# dev coming over, would I be surprised about things I am used to?

For anyone who uses Visual Studio to primarily edit code, then F# may take some getting used to, but most of the things you’re used to using are present. In that sense, it is first class. Project integration, semantic colors, IntelliSense, tooltips (more advanced than those in C#), some refactorings and analyzers, and so on.

C# has objectively more IDE features than F#, and the number of refactorings available in F# tooling absolutely pales in comparison to C# tooling. Some of these come down to how each language works, though, so it’s not quite so simple as “F# could just have XYZ features that C# has.” But overall, I think people tend to feel mostly satisfied by the kinds of tooling available to them.

It’s often claimed that F# needs less refactoring tools because the language tends to guide programmers into one way to do things, and the combination of the F# type system and language design lends itself towards the idiom, “if it compiles, it works right.” This is mostly true, though I do feel like there are entire classes of refactoring tools that F# developers would love to use, and they’d be unique to F# and functional programming.

## What's the biggest hurdle you see for people trying to learn F#, especially from OO languages like C# or Java?

I think that OO programming in mainstream OO languages tends to over-emphasize ceremony and lead to overcomplicated programs. A lot of people normalize this and then struggle to work with something that is significantly simpler and has less moving parts.

When you *expect* something to be complicated and it’s simple, this simplicity almost feels like it’s mocking you, like the language and environment is somehow smarter than you. That’s certainly what I felt when I learned F# and Haskell for the first time.

Beyond this, I think that the biggest remaining hurdles simply are the lack of curly braces and immutability. It’s important to recall that for many people, programming languages are strongly associated with curly braces and they can struggle to accept that a general-purpose programming language can work well without them.

C, C++, Java, C#, and JavaScript are the most widely used languages and they all come from the same family of programming languages. Diverging greatly from that syntax is a big deal and shouldn’t be underestimated. This syntax hurdle is less of a concern for Python programmers, and I’ve found that folks with Python experience are usually warmer to the idea of F# being whitespace sensitive. Go figure!

The immutability hurdle is a big one for everyone, though. Most people are trained to do “place-oriented programming”—put a value in a variable, put that variable in a list of variables, change the value in the variable, change the list of variables, and so on. Shifting the way you think about program flow in terms of immutability is a challenge that some people never overcome, or they prefer not to overcome because they hate the idea of it. It really does fundamentally alter how you write a program, and if you have a decade or more of training with one way to write programs, immutability can be a big challenge.

## We're seeing a lot of F# inspiration lately in C#, especially with what's new in C# 9 (with immutability, records, for example). Where do you think the dividing line is between C# with FP and using F#? Is there guidance to help me make that decision?

I think the dividing line comes down to two things: what your defaults are and what you emphasize.

C# is getting a degree of immutability with records. But normal C# programming in any context is mutable by default. You can do immutable programming in C# today, and C# records will help with that. But it’s still a bit of a chore because the rest of the language is just begging you to mutate some variables.

They’re called *variables* for a reason! This isn’t a value judgement, though. It’s just what the defaults are. C# is mutable by default, with an increasing set of tools to do some immutability. F# is immutable by default, and it has some tools for doing mutable programming.

I think the second point is more nuanced, but also more important. Both C# and F# implement the .NET object system. Both can do inheritance, use accessibility modifiers on classes, and do fancy things with interfaces (including interfaces with default implementations). But how many F# programmers use this side of the language as a part of their normal programming? Not that many. OOP is possible in F#, but it’s just not emphasized. F# is squarely about functional programming first, with an object programming system at your disposal when you need it.

On the other hand, C# is evolving into a more unopinionated language that lets you do just about anything in any way you like. The reality is that some things work better than others (recall that C# is not immutable by default), but this lack of emphasis on one paradigm over the other can lead to vastly different codebases despite being written in the same language. 

Is that okay? I can’t tell. But I think it makes identifying the answer to the question, “how should I do this?” more challenging. If you wanted to do functional programming, you could use C# and be fine. But by using a language that is fairly unopinionated about how you use it, you may find that it’s harder to “get it” when thinking functionally than if you were to use F#. Some of the principles of typed functional programming may feel more difficult or awkward because C# wasn’t built with them in at first. Not necessarily a blocker, but it still matters.

*To read the entire interview, [head on over to my site](https://daveabrock.com/2020/09/26/dev-discussions-phillip-carter).*

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Aaron Powell [announces Blazor support for Azure Static Web Apps](https://devblogs.microsoft.com/aspnet/azure-static-web-apps-with-blazor).
* Isaac Abraham [continues his series on how to become an F# adopter](https://www.compositional-it.com/news-blog/how-to-become-an-f-adopter-part-3/).
* Jeremy Likness [runs EF Core queries on SQL Server from Blazor WebAssembly](https://blog.jeremylikness.com/blog/run-efcore-queries-against-sql-from-blazor-webassembly/).

### 📢 Announcements

* Jamshed Damkewala [provides the latest details on .NET Core releases and support](https://devblogs.microsoft.com/dotnet/net-core-releases-and-support).
* Kayla Cinnamon [announces the release of Windows Terminal Preview 1.4](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-4-release).
* Demitrius Nelon [announces a new preview of Windows Package Manager](https://devblogs.microsoft.com/commandline/windows-package-manager-preview-v0-2-2521).
* Nikisha Reyes-Grange [announces new serverless and analytics capabilities](https://devblogs.microsoft.com/cosmosdb/ignite-2020-new-serverless-and-analytics-capabilities-announced).
* Jacqueline Widdis [shows off new features in Visual Studio 2019 v16.8 Preview 3.1](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-8-preview-3-1), and Angelos Petropoulos [announces the ability to use GitHub Actions with Visual Studio](https://devblogs.microsoft.com/visualstudio/using-github-actions-in-visual-studio-is-as-easy-as-right-click-and-publish).
* The Bridge to Kubernetes tool (previously Local Process with Kubernetes) [is now in GA](https://devblogs.microsoft.com/visualstudio/bridge-to-kubernetes-ga).
* You can now [build Xamarin.iOS apps using iOS 14 and Xcode 12](https://devblogs.microsoft.com/xamarin/ios-14-and-xcode-12-xamarin-ios).

### 📅 Community and events

* Now with Ignite 2020 in the books, [you can check out all the sessions](https://myignite.microsoft.com/sessions).
* Just two standups with Ignite going on: Desktop [talks about OSS dev with WinForms](https://www.youtube.com/watch?v=LT8ntgw8b3c), and [Machine Learning talks about Blazor WASM and ML.NET with .NET 5](https://www.youtube.com/watch?v=HBEGPANJnr4).
* The .NET Docs Show [talks about mobile development with Matt Soucoup](https://www.youtube.com/watch?v=pzZHJ60uqsw).

### 😎 Blazor / ASP.NET Core

* Shaun Curtis works on a Blazor database app: he [talks about using UI controls with his Blazor database application](https://www.codeproject.com/Articles/5280090/Building-a-Database-Application-in-Blazor-Part-4-U), then   [updates his Blazor database app to use CRUD list operations](https://www.codeproject.com/Articles/5280391/Building-a-Database-Application-in-Blazor-Part-5-V).
* Daniel Jimenez Garcia [compares PWA options for Blazor, ASP.NET Core, Vue.js, and Angular](https://www.dotnetcurry.com/aspnet-core/progressive-web-apps-blazor-vuejs-angular).
* Marinko Spasojevic [creates real-time charts with Blazor and SignalR](https://code-maze.com/creating-blazor-webassembly-signalr-charts/).
* Kristoffer Strube [wraps JavaScript libraries in Blazor WebAssembly](https://blog.elmah.io/wrapping-javascript-libraries-in-blazor-webassembly-wasm/).
* Khalid Abuhakmeh [offers a jump start for localization in ASP.NET Core](https://khalidabuhakmeh.com/aspdotnet-core-localization-jump-start) and also [uses ASP.NET Core middleware to remember request culture](https://khalidabuhakmeh.com/remember-asdotnet-request-culture-using-middleware).
* David Grace [works through Facebook logins in ASP.NET Core](https://www.roundthecode.com/dotnet/allow-your-users-to-login-to-your-asp-net-core-app-through-facebook).

### ⛅ The cloud

* Matias Quaranta [optimizes bandwidth in the Azure Cosmos DB .NET SDK](https://devblogs.microsoft.com/cosmosdb/enable-content-response-on-write).
* Jason Gaylord [analyzes pull request information using GitHub webhooks and Azure Logic Apps](https://www.jasongaylord.com/blog/2020/09/24/github-pull-request-webhook-logic-app).
* Tom Kerkhove [discusses the value of running Azure Logic Apps anywhere](https://blog.tomkerkhove.be/2020/09/22/why-running-azure-logic-apps-anywhere-is-a-game-changer/).
* Damien Bowden [works on securing Azure Functions using Azure AD JWT auth for user access tokens](https://damienbod.com/2020/09/24/securing-azure-functions-using-azure-ad-jwt-bearer-token-authentication-for-user-access-tokens/).
* Mark Heath [discusses serverless materialized views](https://markheath.net/post/serverless-materialized-views).
* Scott Hanselman talks about [Blazor WebAssembly on Azure Static Web Apps](https://www.hanselman.com/blog/BlazorWebAssemblyOnAzureStaticWebApps.aspx).
* A nice piece on [creating and using the Azure Service Bus in .NET Core](https://www.pmichaels.net/2020/09/19/creating-and-using-an-azure-service-bus-in-net-core).

### 📔 C#

* Matthew Jones [writes about the C# type system](https://exceptionnotfound.net/csharp-in-simple-terms-1-the-type-system).
* Vladimir Khorikov [uses domain purity to get the current date and time](https://enterprisecraftsmanship.com/posts/domain-model-purity-current-time/).
* Carmel Eve [discusses a simple pattern for using System.CommandLine with dependency injection](https://endjin.com/blog/2020/09/simple-pattern-for-using-system-commandline-with-dependency-injection.html).
* Ian Griffiths [discusses nullable references and serialization in C# 8](https://endjin.com/blog/2020/09/dotnet-csharp-8-nullable-references-serialization.html).
* Jason Roberts [discusses using approval tests to write tests more quickly](http://dontcodetired.com/blog/post/Approval-Tests-Write-Tests-More-Quickly).
* Patrick Smacchia [shows off his top 10 new APIs in .NET 5](https://blog.ndepend.com/top-10-net-5-0-new-apis/).
* Jamie Maguire [responds to phone calls using Twilio and C#](http://www.jamiemaguire.net/index.php/2020/09/19/responding-to-phone-calls-using-twilio-and-c/).

### 📗 F#

* Tyson Williams [discusses the Bottom type](https://tysonwilliams.coding.blog/2020-09-21_bottom_type_in_fsharp).
* Chester Burbidge [writes a serverless app on AWS using F# and Fable](https://chester.codes/serverless-apps-aws-fsharp-fable/).
* Dom Raniszewski [uses dependency injection in F# Azure Functions](https://purple.telstra.com/blog/dependency-injection-in-fsharp-azure-functions).

### 🔧 Tools

* Gouri Sohoni [discusses source control in Azure DevOps best practices](https://www.dotnetcurry.com/devops/source-control-azure-devops-best-practices).
* Tim Heuer [talks about how to use GitHub Codespaces with .NET Core](https://devblogs.microsoft.com/dotnet/using-github-codespaces-with-net-core).
* Andrew Lock [creates a Helm chart of an ASP.NET Core app](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-4-creating-a-helm-chart-for-an-aspnetcore-app/).
* Nicholas Blumhardt [injects services into Serilog filters, enrichers, and sinks](https://nblumhardt.com/2020/09/serilog-inject-dependencies/).
* Dan Clarke [compares .NET mocking libraries](https://www.danclarke.com/comparing-dotnet-mocking-libraries).
* Scott Hanselman [runs through cross-platform diagnostic tools for .NET Core](https://www.hanselman.com/blog/CrossPlatformDiagnosticToolsForNETCore.aspx).
* ErikEJ offers tips [for making the most of EF Core with Azure SQL](https://erikej.github.io/efcore/sqldb/2020/09/21/efcore-azure-sql-db.html).

### 📱 Xamarin

* Scott Kuhl [looks at Xamarin.Forms dual screen support](https://medium.com/@scottkuhl/dual-screen-in-xamarin-forms-33962b765c9).
* Charlin Agramonte [works with the Scratch View](https://xamgirl.com/scratch-view-in-xamarin-forms/).
* Leomaris Reyes [gets device and display information data with Xamarin Essentials](https://www.telerik.com/blogs/getting-device-display-information-data-xamarin-essentials).
* Leomaris Reyes [works with data caching](https://askxammy.com/learning-to-use-data-caching-in-xamarin-forms/).

### 🎤 Podcasts

* The Microsoft Cloud Show [talks about Microsoft 365 and Ignite 2020](https://www.microsoftcloudshow.com/podcast/Episodes/377-microsoft-365-ignite-2020-cvp-jeff-teper/).
* .NET Rocks [talks Dapr with Haishi Bai](https://www.dotnetrocks.com/default.aspx?ShowNum=1706).
* The 6-Figure Developer podcast [talks about C# 9 and .NET 5 with LaBrina Loving](https://6figuredev.com/podcast/episode-162-c-9-and-net-5-with-labrina-loving/).
* The DevTalk podcast [talks with Jon Dick about moving to .NET MAUI (Xamarin)](https://kerry.lothrop.de/devtalk-46-jon-dick/).
* The .NET Core Podcast talks [about Uno with Jérôme Laban](https://dotnetcore.show/episode-60-uno-platform-with-jerome-laban/).

### 🎥 Videos

* The AI Show [introduces Metrics Advisor](https://channel9.msdn.com/Shows/AI-Show/Introducing-Metrics-Advisor).
* Data Exposed [secures an Azure SQL database by setting the minimal TLS version](https://channel9.msdn.com/Shows/Data-Exposed/How-to-Secure-Azure-SQL-Database-by-Setting-Minimal-TLS-Version).
