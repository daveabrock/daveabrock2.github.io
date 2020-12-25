---
date: "2020-07-12"
title: "The .NET Stacks #7: Azure SDKs, testing, community roundup, more!"
tags: [dotnet-stacks]
comments: false
subtitle: A rundown on new Azure SDKs, test automation, community roundup, and more!
share-img: /assets/img/stacks-7-card.png
---

This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.
{: .box-note}

On tap this week:

- A look at the Azure SDKs
- What's your test automation maturity level?
- The evolution (and optics) of how far .NET has come
- Community roundup

## A look at the Azure SDKs

If you work with Azure SDKs, have you looked into the improvements lately in the last 6-9 months? (You can catch up quickly with a [quick video Microsoft pushed out last month](https://www.youtube.com/watch?v=38RYIx7a2M4).)

Here's the gist: before the new SDKs were released, each Azure team created their own SDKs in their own repositories, with their own CI process, with varying levels of OS support, language support, and so on. The new SDKs promise new common guidelines, centralized systems and repositories, and consistent security and support. These SDKs deliver language support for .NET, Java, Python, and JavaScript/TypeScript, with a promise of more languages to come. Most importantly, packages have been released to all the common package managers, meeting developers where they are—instead of asking them to conform to the whims of each API.

Microsoft has a [landing page of sorts](https://azure.github.io/azure-sdk/) where you can review releases, feature updates, and API information.

## What's your test automation maturity level?

From the Applitools blog, Angie Jones published a nice piece on [how to measure your test automation maturity](https://applitools.com/blog/measure-your-test-automation-maturity). While this is tech-agnostic, it's worth a mention in this space as I found it valuable for me to think more critically about my own testing approach. 

I found it interesting that, in her research, she found that 60% of the teams no longer have a distinction between development and QA engineers. Score one for end-to-end ownership and the DevOps movement.

As you read the article, you can give your team its own testing maturity score based on:

- Your ability to automate unit tests, web tests, API tests, security tests, performance tests, and accessibility tests
- If you test across browsers, devices, and screen sizes
- If tests are executed in your CI/CD pipeline
- If you use feature flags ([shameless plug for my series on .NET feature toggles](https://daveabrock.com/2020/05/24/introducing-feature-management-copy))

What was your score?

## The evolution (and optics) of how far .NET has come

If you spend any time looking at #dotnet Twitter, this week was an interesting one. Billy Collins (@BlazorGuy) [tweeted](https://twitter.com/BlazorGuy/status/1279092538490736640), *"What's up with non-.NET developers thinking C# is a Windows only, corporate bloatware language? It's not 2005 anymore!"* This caused [another discussion](https://twitter.com/davidfowl/status/1279538339780063232) spearheaded by ASP.NET Core architect David Fowler. Microsoft is definitely having to strike a fine balance of attracting modern development, yet supporting enterprise (legacy) customers.

The big challenge Microsoft still faces is trying to get developers who haven't worked on .NET in awhile—and have perhaps ditched it for faster, cross-platform solutions—that .NET has a cutting-edge platform again, and has been for the last 5 years. Their persistence to keep the .NET/ASP.NET name on every iteration of the platform has, I believe, done Microsoft a disservice. Let's say I ditched .NET in the old Framework days and only have a passing interest in .NET. If I hear things like .NET Core, .NET Standard, and .NET 5, do I believe things have changed without researching more?

As Fowler [tweeted](https://twitter.com/davidfowl/status/1279545670894936065), *"This is why timing and marketing is super important in product development. Not only is it hard to erase search history for something that’s been around for 20 years, but you have to deal with the impressions people has years ago about a “new product” with the same name. At the same time, you want to leverage the existing ecosystem so you’re not starting from scratch."*

With all that said, [.NET 5 promises a unified framework](https://devblogs.microsoft.com/dotnet/introducing-net-5/) and a stable, predictable release schedule—with a new major version every November. In many ways, this is a realization of a vision Microsoft developed many years ago with a unified, fast, cross-platform framework. And people can [run apps without a separate deploy of .NET Core](https://github.com/dotnet/runtime/issues/36590)! These are exciting times. Hopefully, the community perception continues to improve as a result.

## Community roundup

### From Microsoft

- Phillip Carter [fills us in on an F# and F# 5 June update](https://devblogs.microsoft.com/dotnet/f-5-and-f-tools-update-for-june/).
- Charlotte Yarkoni [discusses new investments in Microsoft Learn](https://blogs.microsoft.com/blog/2020/07/01/skilling-for-the-future-new-investments-in-microsoft-learn/).
- James Montemagno [offers a quick tip for easy back navigation for Xamarin.Forms](https://devblogs.microsoft.com/xamarin/xamarin-forms-shell-back-navigation/).
- Shayne Boyer works through [microservices with .NET Core, Project Tye, and GitHub Actions](https://techcommunity.microsoft.com/t5/apps-on-azure/building-a-path-to-success-for-microservices-and-net-core/ba-p/1502270).
- Azure Data Lake Storage static websites [are now in preview](https://azure.microsoft.com/en-us/updates/static-website-for-azure-data-lake-storage-now-in-public-preview/).
- Azure Functions [now has an extension for Dapr](https://cloudblogs.microsoft.com/opensource/2020/07/01/announcing-azure-functions-extension-for-dapr/).
- An announcement [for Pylance, Python support in Visual Studio Code](https://devblogs.microsoft.com/python/announcing-pylance-fast-feature-rich-language-support-for-python-in-visual-studio-code/).

### Blog posts

- Thomas Levesque [exposes a custom type as a JSON string in ASP.NET Core](https://thomaslevesque.com/2020/06/27/exposing-custom-type-as-json-string-in-asp-net-core-api/).
- Mark Heath [lists and downloads the contents of a GitHub repo in C#](https://markheath.net/post/list-and-download-github-repo-cs).
- Anthony Giretti [writes a light API with ASP.NET Core](https://anthonygiretti.com/2020/06/29/nano-services-with-asp-net-core-or-how-to-build-a-light-api/).
- Ian Griffiths talks about [the C# 8 NotNull attribute](https://endjin.com/blog/2020/06/dotnet-csharp-8-nullable-references-notnull).
- Joydip Kanjilal works with [GraphQL APIs in ASP.NET Core 3.1](https://www.red-gate.com/simple-talk/dotnet/net-development/building-and-consuming-graphql-api-in-asp-net-core-3-1/).
- Shahed Chowdhuri wraps up his ASP.NET Core A-Z series [with zero-downtime web apps](https://wakeupandcode.com/zero-downtime-web-apps-for-asp-net-core-3-1/).
- Changhui Xu [load balances an ASP.NET Core web app with Nginx and Docker](https://codeburst.io/load-balancing-an-asp-net-core-web-app-using-nginx-and-docker-66753eb08204).
- Siddharth Patel diagnoses [targeting deprecated APIs and cross-platform issues](https://www.techblogcity.com/2020/06/27/net-api-analyzer-targeting-deprecated-apis-and-cross-platform-issues/).
- Matthew Jones continues his series about [building Minesweeper in Blazor WebAssembly](https://exceptionnotfound.net/minesweeper-in-blazor-webassembly-part-2-the-blazor-component/).
- Brandon Minnick talks about [updating to Azure Functions v3 from Visual Studio](https://techcommunity.microsoft.com/t5/apps-on-azure/updating-to-azure-functions-v3-in-visual-studio/ba-p/1499785).
- Microsoft offers [three ways to customize your Windows terminal](https://blogs.windows.com/windowsdeveloper/2020/06/30/3-ways-to-customize-your-windows-terminal/?WT.mc_id=DX_MVP4025064).
- AWS has a [new porting assistant for .NET](https://aws.amazon.com/blogs/aws/announcing-the-porting-assistant-for-net), and also [discusses environment variables with .NET Core and Elastic Beanstalk](https://aws.amazon.com/blogs/developer/environment-variables-with-net-core-and-elastic-beanstalk/).
- Matthew Robbins [simplifies grids in Xamarin.Forms](https://www.mfractor.com/blogs/news/simplifying-grids-in-xamarin-forms).
- Joseph Guadagno debugs a [pesky "no route matches the supplied values" error](https://www.josephguadagno.net/2020/07/01/no-route-matches-the-supplied-values).
- Khalid Abuhakmeh discusses [Azure Functions on macOS with JetBrains Rider](https://khalidabuhakmeh.com/azure-functions-on-macos-with-jetbrains-rider).
- Vladimir Pecanac talks [string concatenation in C#](https://code-maze.com/different-ways-concatenate-strings-csharp/).
- Cezary Piatek discusses [the magical methods in C#](https://cezarypiatek.github.io/post/methods-with-special-signature/).
- Matthias Koch talks about [cross-platform setup and bootstrapping in .NET](https://ithrowexceptions.com/2020/07/01/road-to-cross-platform-setup-and-bootstrapping-in-dotnet.html).
- Muhammad Rehan Saeed discusses [the easiest way to version NuGet packages](https://rehansaeed.com/the-easiest-way-to-version-nuget-packages/).
- David Grace [uses SignalR in ASP.NET Core and React to send messages](https://www.roundthecode.com/dotnet/using-signalr-in-asp-net-core-react-to-send-messages).
- Simon Bisson talks about [future-proofing .NET app dev with Uno](https://www.roundthecode.com/dotnet/using-signalr-in-asp-net-core-react-to-send-messages).

### Podcasts/videos

- Visual Studio Toolbox [continues going in-depth on EF Core](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Entity-Framework-Core-In-Depth-Part-6).
- The DotNet Docs Show talks about [securing .NET apps with Heather Downing](https://www.youtube.com/watch?v=GY5SQMYN_9o).
- For community standups: Xamarin [discusses the 4.7 release](https://www.youtube.com/watch?v=FZmPwX1oRU8), [.NET has a Q&A](https://www.youtube.com/watch?v=PRbk13u_-Mg), and ASP.NET has [a bunch of random stuff](https://www.youtube.com/watch?v=EtJ8jkm6o1o).
- ON.NET [talks about Worker templates](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Getting-started-with-the-Worker-templates) and [deploys microservices to Azure Container Instances](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Deploying-your-Microservice-to-Azure-Container-Instances).
- Leslie Richardson and friends [start a series on finding code in Visual Studio](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Finding-Code-Part-1).
- At the Azure Podcast, [a Microsoft Q&A](http://azpodcast.azurewebsites.net/post/Episode-336-Microsoft-QA1).
- The Xamarin Show [goes beyond MVU basics](https://channel9.msdn.com/Shows/XamarinShow/FSharp-Fabulous-Beyond-MVU-Basics).
- .NET Rocks [discusses nDepend with Patrick Smacchia](https://www.dotnetrocks.com/default.aspx?ShowNum=1694).

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
