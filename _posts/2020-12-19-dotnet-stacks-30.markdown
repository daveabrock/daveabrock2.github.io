---
date: "2020-12-19"
title: "The .NET Stacks #30: 🥂 See ya, 2020"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/see-ya-2020.png 
subtitle: This week, we wrap up 2020 with some news, a look back, and a coding tip.
readtime: true
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*Note: This is the published version of my free, weekly newsletter, The .NET Stacks. It was originally sent to subscribers on December 14, 2020. Subscribe at the bottom of this post to get the content right away!*

👋 Happy Monday, everyone! This is the very last issue of 2020. Every year, I try to take the last few weeks off to decompress and unplug—considering how 2020 went, this year is no exception.

## 📰 News and announcements

GitHub Universe 2020 occurred this week, and [Shanku Niyogi has all the details](https://github.blog/2020-12-08-new-from-universe-2020-dark-mode-github-sponsors-for-companies-and-more/). (As .NET developers, the gap between .NET and GitHub will continue to narrow as Microsoft will [pivot to GitHub Actions as its long-term CI/CD solution](https://daveabrock.com/2020/08/15/dotnet-stacks-12#github-actions-or-azure-devops).)

The GitHub site itself stole the show. Dark Mode is now supported natively—you can delete your favorite browser extensions that do this for you. Also, the front page is a site to behold. Just hover over a globe to see commits in real-time from across the world:

![The GitHub globe]({{ site.url }}{{ site.baseurl }}/assets/img/gh-globe.png)

As for actual development features, we now see the ability to auto-merge PRs, discussions, CD support. Also, companies can now invest in OSS with [the GitHub Sponsors program](https://github.com/sponsors).

What does this Sponsors news mean for the .NET OSS ecosystem? That is a complicated question. [.NET PM Immo Landwerth wrote about it](https://github.com/dotnet-foundation/ecosystem-growth/blob/main/docs/theme.md#grow-the-set-of-trusted-libraries-that-arent-controlled-by-microsoft):

>Today, we're usually reactive when it comes to library & framework investments. By the time we know there is a need for a library we routinely research existing options but usually end up rolling our own, because nothing fits the bill as-is and we either don't have the time or we believe we wouldn't be able to successfully influence the design of the existing library. This results in a perception where Microsoft "sucks the air" out of the OSS ecosystem because our solutions are usually more promoted and often tightly integrated into the platform, thus rendering existing solutions less attractive. Several maintainers have cited this as a reason that they gave up or avoid building libraries that seem foundational. 

>To avoid this, we need to start engaging with owners of existing libraries and work with them to increase their quality ... it should become a general practice. We should even consider an ongoing work item to actively investigate widely used libraries and help them raise the quality or tighten their integration into the .NET developer experience. 

Check out the full discussion here:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Since GitHub extended the concept of sponsoring: I&#39;m not convinced that this is the right fix for the .NET ecosystem at large. Thoughts? <a href="https://t.co/8660CXWQpl">https://t.co/8660CXWQpl</a> <a href="https://t.co/qKmzNkvKQn">pic.twitter.com/qKmzNkvKQn</a></p>&mdash; Immo Landwerth (@terrajobst) <a href="https://twitter.com/terrajobst/status/1337587757086986241?ref_src=twsrc%5Etfw">December 12, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

___

Claire Novotny wrote [two](https://devblogs.microsoft.com/dotnet/improving-debug-time-productivity-with-source-link/) [posts](https://devblogs.microsoft.com/dotnet/producing-packages-with-source-link/) this week about using Source Link with Visual Studio and Visual Studio Code. [Source Link](https://github.com/dotnet/sourcelink) has been around a few years, but is now getting the full treatment. If you aren't familiar, it's "a language and source-control agnostic system for providing first-class source debugging experiences for binaries." This helps solve issues when you can't properly debug an external library (or a .NET library).

Considering most library code is openly available from a GitHub HTTP request, debugging external dependencies is a lot easier these days. If a library has enabled Source Link, you can work with the code as you would with yours: setting breakpoints, checking values, and so on. I can see benefit personally in something like `System.Text.Json` or `Newtonsoft.Json`—it'll be easier to pinpoint any silly serialization issues I may or may not be causing.

Claire's first post [shows off the value of using Source Link](https://devblogs.microsoft.com/dotnet/improving-debug-time-productivity-with-source-link/), but for this to work a library's PDB files need to be properly indexed. Her second post shows [how to add Source Link to your projects](https://devblogs.microsoft.com/dotnet/producing-packages-with-source-link/).

___

Speaking of Visual Studio, Jacqueline Widdis [rolls out Visual Studio 2019 v16.9 Preview 2](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-9-preview-2/). We're seeing a few quality-of-life improvements—`using` directives are automatically inserted when pasting types, and VS automatically adds semicolons when creating objects and invoking methods.

Speaking of quality-of-life, the Visual Studio team [is aware](https://twitter.com/PratikN/status/1336440895692914690) of the underwhelming reception to the Git integration updates added with v16.8. They have a [survey out](https://t.co/L8b2rSVjMt?amp=1) to understand why a lot of folks are turning off the feature. They're listening, so fill out the survey to have your voice heard.

___

Xin Shi [discusses Infer#, which brings you interprocedural static analysis capabilities](https://devblogs.microsoft.com/dotnet/infer-interprocedural-memory-safety-analysis-for-c) to .NET. It currently helps you detect null dereferences and resource leaks. They're working on race condition detection and thread safety violations.

___

Igor Velikorossov announced [what's new in the Windows Forms runtime for .NET 5](https://devblogs.microsoft.com/dotnet/whats-new-in-windows-forms-runtime-in-net-5-0/). They include a new `TaskDialog` control, `ListView` enhancements, upgrades to `FileDialog`, and performance and accessibility improvements.

___

Anthony Chu [announces that .NET 5 support in Azure Functions is now in early preview](https://techcommunity.microsoft.com/t5/apps-on-azure/net-5-support-on-azure-functions/ba-p/1973055). For this to happen, .NET 5 functions need to run in an out-of-process language worker separate from the Azure Functions runtime. As a result, a .NET 5 app runs differently than a 3.1 one; you build an executable that imports the .NET 5 language worker as a NuGet package. Check out [the repository readme](https://github.com/Azure/azure-functions-dotnet-worker-preview) for the full details. The .NET 5 worker will be generally available in early 2021.

In other Azure news, Richard Park [announced the new Azure Service Bus client libraries](https://devblogs.microsoft.com/azure-sdk/november-2020-servicebus-ga/). Following [the guidelines for new Azure SDKs](https://github.com/Azure/azure-sdk/blob/master/README.md), it's a culmination of months of work and the post includes details on how its making working with core Service Bus functionality quite a bit easier.

Also, the November 2020 release of the Azure SDKs include the Service Bus updates, as well as updates for Form Recognizer, Identity, and Text Analytics.

___

Microsoft released a couple new Learn modules: one on [building projects with GitHub](https://docs.microsoft.com/learn/paths/build-community-driven-projects-github/), and another with [an introduction to PowerShell](https://docs.microsoft.com/learn/modules/introduction-to-powershell).

___

Kubernetes 1.20 has been released. The [release notes are very infra-heavy](https://kubernetes.io/blog/2020/12/08/kubernetes-1-20-release-announcement/), but one thing to note is the Dockershim deprecation, which [we talked about last week](https://daveabrock.com/2020/12/12/dotnet-stacks-29#-kubernetes-is-deprecating-docker--what).

___

Okta has [released a new CLI](https://developer.okta.com/blog/2020/12/10/introducing-okta-cli). With one `okta start` command, it looks like it can register you for a new account (if you don't have one) and work with a sample ASP.NET Core MVC application.

## Dev tip: nested tuple deconstruction

I'm a fan of deconstructing C# objects [using tuples](https://docs.microsoft.com/dotnet/csharp/language-reference/builtin-types/value-tuples). Did you know you can nest them? Even though it's been around awhile, I didn't.

David Pine shows us how:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Hey C# developers, have you ever used nested tuple deconstruction? Check it out -- when deconstructing one object that supports deconstruction, you can nest construction all the way down. <a href="https://twitter.com/hashtag/csharp?src=hash&amp;ref_src=twsrc%5Etfw">#csharp</a> <a href="https://twitter.com/hashtag/dotnet?src=hash&amp;ref_src=twsrc%5Etfw">#dotnet</a> <a href="https://twitter.com/dotnet?ref_src=twsrc%5Etfw">@dotnet</a><a href="https://t.co/h5R7tsU8es">https://t.co/h5R7tsU8es</a> <a href="https://t.co/5I6mKRka6b">pic.twitter.com/5I6mKRka6b</a></p>&mdash; David Pine 🌲 (@davidpine7) <a href="https://twitter.com/davidpine7/status/1337483995207118849?ref_src=twsrc%5Etfw">December 11, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## 📆 A 2020 .NET Stacks recap

It's been a busy 2020 at *The .NET Stacks*, even if we've only been around since the end of May. What's happened since then? Let's recap.

The first issue [covered Microsoft Build](https://daveabrock.com/2020/05/24/dotnet-stacks-1). We [talked about Project Tye and YARP](https://daveabrock.com/2020/05/31/dotnet-stacks-2). We discussed [native .NET feature flags and local k8s dev in Visual Studio](https://daveabrock.com/2020/06/08/dotnet-stacks-3), [EF Core](https://daveabrock.com/2020/06/14/dotnet-stacks-4), [gRPC-Web and C# 9](https://daveabrock.com/2020/06/20/dotnet-stacks-5), [Blazor Mobile Bindings](https://daveabrock.com/2020/07/05/dotnet-stacks-6), [Azure SDKs and test automation](https://daveabrock.com/2020/07/12/dotnet-stacks-7), and how [C# 9 has become more functional](https://daveabrock.com/2020/07/18/dotnet-stacks-8). Then, we [discussed Project Coyote and a new Razor editor](https://daveabrock.com/2020/07/25/dotnet-stacks-9), [.NET's approachability](https://daveabrock.com/2020/08/01/dotnet-stacks-10), [Newtonsoft's new rule and using async void](https://daveabrock.com/2020/08/08/dotnet-stacks-11), and [what the future of Azure DevOps looks like](https://daveabrock.com/2020/08/15/dotnet-stacks-12).

Then, we covered [.NET 5 Blazor improvements](https://daveabrock.com/2020/08/22/dotnet-stacks-13), [NuGet changes and many-to-many in EF Core 5](https://daveabrock.com/2020/08/29/dotnet-stacks-14), [C# source generators](https://daveabrock.com/2020/09/05/dotnet-stacks-15), [app trimming](https://daveabrock.com/2020/09/12/dotnet-stacks-16), [Blazor CSS isolation](https://daveabrock.com/2020/09/19/dotnet-stacks-17), the [fate of .NET Standard](https://daveabrock.com/2020/09/26/dotnet-stacks-18), and [a Microsoft Ignite recap](https://daveabrock.com/2020/10/03/dotnet-stacks-19). We [introduced route-to-code and talked about what's happening with IdentityServer](https://daveabrock.com/2020/10/09/dotnet-stacks-20), [talked about Azure Static Web Apps](https://daveabrock.com/2020/10/16/dotnet-stacks-21), [got excited about .NET 5 and the .NET Foundation](https://daveabrock.com/2020/10/24/dotnet-stacks-22), and [talked about how .NET 5 support works](https://daveabrock.com/2020/10/31/dotnet-stacks-23).

After that, we [talked about Blazor's production readiness](https://daveabrock.com/2020/11/06/dotnet-stacks-24), [C# 9 records](https://daveabrock.com/2020/11/13/dotnet-stacks-25), and [celebrated the official release of .NET 5](https://daveabrock.com/2020/11/21/dotnet-stacks-26). We [gave some love to ASP.NET Core 5](https://daveabrock.com/2020/11/28/dotnet-stacks-27), talked about the [future of APIs in ASP.NET Core MVC](https://daveabrock.com/2020/12/05/dotnet-stacks-28), and [talked about how to switch controllers to route-to-code](https://daveabrock.com/2020/12/12/dotnet-stacks-29). And now, we're here.

Along the way, we've had some wonderful interviews from community leaders. We discussed [the PresenceLight project](https://daveabrock.com/2020/06/13/dev-discussions-isaac-levin), [writing about ASP.NET Core from A-Z](https://daveabrock.com/2020/06/28/dev-discussions-shahed-chowdhuri), [the Azure community](https://daveabrock.com/2020/07/10/dev-discussions-michael-crump), [Entity Framework Core](https://daveabrock.com/2020/07/25/dev-discussions-jeremy-likness-1), [the Microsoft docs](https://daveabrock.com/2020/08/15/dev-discussions-scott-addie), [ML.NET](https://daveabrock.com/2020/08/29/dev-discussions-luis-quintanilla-1), F# and functional programming (from [Microsoft](https://daveabrock.com/2020/09/26/dev-discussions-phillip-carter) and [the community](https://daveabrock.com/2020/09/19/dev-discussions-isaac-abraham)), and [Coravel](https://daveabrock.com/2020/11/01/dev-discussions-james-hickey). I've got interviews coming from LaBrina Loving, Layla Porter, Cecil Phillip, and Steve Sanderson—and welcome any suggestions for future interview subjects.

I started this newsletter as an experiment to keep my mind sharp during the pandemic, and I'm happy to see how much its grown. I hope you enjoy it, but I'm always open to feedback about how I can make it better for you. I'll have a survey in early 2021, but if you have anything to share don't hesitate to reach out.

See you in 2021! I hope you and your family have a safe and relaxing holiday season.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Chris Woodruff [implements an effective architecture for ASP.NET Core Web API](https://medium.com/rocket-mortgage-technology-blog/implementing-an-effective-architecture-for-asp-net-core-web-api-254f95b8a434).
* Martin Ullrich [talks about MSBuild tips and tricks](https://blog.jetbrains.com/dotnet/2020/12/11/tips-tricks-to-improve-your-net-build-setup-with-msbuild-webinar-recording/).
* Andrew Lock [uses action results and content negotiation with "route-to-code" APIs](https://andrewlock.net/using-action-results-and-content-negotiation-with-route-to-code/).

### 📅 Community and events

* The .NET Open Source Days [is taking place on December 18](https://www.eventbrite.com/e/dot-net-open-source-days-virtual-conference-tickets-130081873385).
* The .NET Docs Show [talks to our Marvel-ous friend Shahed Chowdhuri](https://www.youtube.com/watch?v=P4wj8C2C78c) about .NET, C#, and Azure.
* For the .NET community standups: ASP.NET [talks about Material.Blazor](https://www.youtube.com/watch?v=yzLDvQ-bOw8), Machine Learning discusses [submissions for the Virtual ML.NET Hackathon](https://www.youtube.com/watch?v=mw66ZzYhP1M), and Languages & Runtime [talk about Infer#](https://www.youtube.com/watch?v=cIB4gxqm6EY).

### 🌎 Web development

* Jon Hilton [writes about dark mode for web apps using Blazor and Tailwind CSS](https://jonhilton.net/blazor-tailwind-dark-mode/).
* Kevin Griffin [writes about managing SignalR ConnectionIds](https://consultwithgriff.com/signalr-connection-ids/).
* John Gathogo [passes OData query options in the request body](https://devblogs.microsoft.com/odata/passing-odata-query-options-in-the-request-body).
* Marinko Spasojevic [writes about user registration with Angular and ASP.NET Core Identity](https://code-maze.com/user-registration-angular-aspnet-identity/), and also [writes about custom validators and handling errors with Angular and ASP.NET Core Identity](https://code-maze.com/custom-validators-and-handling-errors-with-angular-and-asp-net-core-identity/).
* David Grace [installs ASP.NET Core in a .NET 5 app on Ubuntu 20.04](https://www.roundthecode.com/dotnet/asp-net-core-web-hosting/how-to-install-an-asp-net-core-in-net-5-app-on-ubuntu-20-04).
* Ricardo Peres [writes about being cautious with async file uploads in ASP.NET Core](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-async-file-uploads).

### ⛅ The cloud

* Chris Noring [writes about managing .NET feature flags locally and with Azure](https://techcommunity.microsoft.com/t5/apps-on-azure/no-recompile-no-redeploy-managing-features-flags-in-net-core/ba-p/1975999).
* Tom Kerkhove [monitors Azure Service Bus topic subscriptions](https://blog.tomkerkhove.be/2020/12/11/monitoring-azure-service-bus-topic-subscriptions/).
* Gergely Sinka [deploys a .NET Core app to all the clouds](https://developer.okta.com/blog/2020/12/09/dotnet-cloud-host-publish).
* Simon Timms [writes about remote debugging for Azure Functions](https://oz-code.com/blog/azure/remote-debugging-for-azure-functions-can-be-a-breeze-2).
* Rami Honig [kicks off a series on using MongoDB with C#](https://oz-code.com/blog/net-c-tips/how-to-mongodb-in-csharp-part-1).
* Damien Bowden [uses multiple APIs in Angular and ASP.NET Core with Azure AD authentication](https://damienbod.com/2020/12/08/using-multiple-apis-in-angular-and-asp-net-core-with-azure-ad-authentication/).
* Abhijit Jana [works with Azure Communication Services](https://abhijitjana.net/2020/12/07/azure-communication-services/).

### 📔 Languages

* Raymond Chen [parses ETL traces with the EventLogReader](https://devblogs.microsoft.com/oldnewthing/20201210-00/?p=104536).
* Julie Lerman [writes about domain-driven design in C#](https://www.pluralsight.com/blog/software-development/domain-driven-design-csharp).
* Dave Brock [uses local function attributes with C# 9](https://daveabrock.com/2020/12/09/local-function-attributes-c-sharp-9).
* Arthur Casals [talks to Phillip Carter about what's new in F#](https://www.infoq.com/articles/fsharp-5-interview-phillip-carter).
* Khalid Abuhakmeh [writes about common use cases for .NET reflection](https://khalidabuhakmeh.com/common-usecases-for-dotnet-reflection).
* Thomas Levesque [continues his series on using C# 9 records as strongly-typed IDs](https://thomaslevesque.com/2020/12/07/csharp-9-records-as-strongly-typed-ids-part-3-json-serialization/).
* Carmel Eve [writes about the Proxy pattern in C#](https://endjin.com/blog/2020/12/design-patterns-in-csharp-the-proxy-pattern.html).
* Niels Swimberghe [warns you about rethrowing your exceptions the wrong way](https://swimburger.net/blog/dotnet/rethrowing-your-exceptions-wrong-in-dotnet-could-erase-your-stacktrace).
* Diogo Souza [creates a CRUD app with Suave and F#](https://www.red-gate.com/simple-talk/dotnet/net-development/creating-your-first-crud-app-with-suave-and-f/).
* Thomas Ardal [predicts Die Hard fans with ML.NET and C#](https://blog.elmah.io/predicting-die-hard-fans-with-ml-net-and-csharp/).
* Tomasz Pęczek [writes about cryptography improvements in .NET 5](https://www.tpeczek.com/2020/12/cryptography-improvements-in-net-5.html).

### 🔧 Tools

* Scott Hanselman [writes about customizing your PowerShell prompt with PSReadLine](https://www.hanselman.com/blog/you-should-be-customizing-your-powershell-prompt-with-psreadline).
* David Ramel [writes about the community requesting Visual Studio for Linux](https://visualstudiomagazine.com/articles/2020/12/09/vs-linux.aspx).
* Lee Briggs [writes about embracing Kubernetes](https://www.pulumi.com/blog/embrace-kubernetes-part1/).
* Rachel Appel [uses C# 9 records and init-only properties in ReSharper and Rider 2020.3](https://blog.jetbrains.com/dotnet/2020/12/07/use-c-9-records-and-init-only-properties-in-resharper-and-rider-2020-3/).
* Dave Brock [automates a Markdown links page (this one!) with Pinboard and C#](https://daveabrock.com/2020/12/07/make-link-list-with-markdown-and-with-c-sharp).
* Xiaoping Wu [uses EF Core 5.0 in .NET Core 3.1 with MySQL](https://www.c-sharpcorner.com/article/tutorial-use-entity-framework-core-5-0-in-net-core-3-1-with-mysql-database-by2/).

### 📱 Xamarin

* James Montemagno [writes about 5 essential NuGet packages for new Xamarin projects](https://montemagno.com/essentials-nugets-for-new-xamarin-projects/).
* Leomaris Reyes [writes about the RadPopUp control](https://www.telerik.com/blogs/meet-radpopup-control-xamarin-forms).
* Denys Fiediaiev [loads an idiom-specific storyboard in Xamarin.iOS with MvvmCross](https://prin53.medium.com/storyboard-xamarin-ios-mvvmcross-e01fed0879f8).

### 🎤 Podcasts

* The Xamarin Podcast [wraps up 2020 and looks ahead to 2021](https://www.xamarinpodcast.com/83).
* The Complete Developer Podcast [discusses dependencies in unit testing](https://completedeveloperpodcast.com/dependencies-in-unit-testing).
* .NET Rocks [talks to Georgia Nelson about building a TwitchBot in Blazor](https://www.dotnetrocks.com/default.aspx?ShowNum=1717).
* The 6 Figure Developer podcasts [talks to Egil Hansen about bUnit](https://6figuredev.com/podcast/episode-173-bunit-a-blazor-testing-library-w-egil-hansen/).
* Serverless Chats [talks about statefulness and serverless with Rodric Rabbah](https://www.serverlesschats.com/78/).

### 🎥 Videos

* Visual Studio Toolbox [uses an existing .NET Core project template](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Use-an-Existing-NET-Core-Project-Template), and also [creates .NET Core projects with the command line](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Create-NET-Core-Projects-with-the-Command-Line).
* Scott Hanselman [talks about basic home networking](https://www.youtube.com/watch?v=lp_CJba4RmU).
* The ASP.NET Monsters [work through single-file applications in .NET 5](https://www.youtube.com/watch?v=Eetj3CmeBQI).
* The ON.NET Show [talks to James Newton-King about gRPC-Web](https://www.youtube.com/watch?v=UV-VnlcpDhU), [introduces microservice patterns](https://www.youtube.com/watch?v=zW4INO353Xg), and [discusses GraphQL schema design](https://www.youtube.com/watch?v=1VEMqTw8xSo).

___

![Go home](https://media.giphy.com/media/j5ncTQKEIROne/source.gif)
