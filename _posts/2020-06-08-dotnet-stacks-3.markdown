---
date: "2020-06-08"
title: "The .NET Stacks #3: Native feature flags, local Kubernetes, community roundup!"
tags: [dotnet-stacks]
comments: false
subtitle: Let's talk feature flags and Kubernetes in Visual Studio.
share-img: /assets/img/stacks-3-card.png
---

This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.
{: .box-note}

In this week's issue we will be talking about:

- Native .NET feature flags
- Working with Kubernetes clusters locally in Visual Studio
- Community roundup

## Native .NET feature flags

Have you implemented feature flags (or feature toggles) in your applications? They're a great way to easily enable and disable system features and behaviors without changing code. They can generate complexity if not managed well, but the benefits are great when it comes to A/B testing, gradual rollouts, and managing long-living branches. Martin Fowler has a [good language-agnostic overview on the topic](https://martinfowler.com/articles/feature-toggles.html).

Until somewhat recently, to get this capability in .NET you had to either roll your own solution or depend on an external library such as LaunchDarkly and Esquio. You can now use native Microsoft libraries to accomplish much of this functionality. Microsoft provides the Microsoft.FeatureManagement library (supported by .NET Standard 2.0, meaning compatibility with .NET Framework!) and Microsoft.FeatureManagement.AspNetCore. I've noticed a lot of folks in the community are writing about it (including [yours truly](https://daveabrock.com/2020/06/07/custom-filters-in-core-flags.html)!), as it's easy to use and has a pretty solid feature set. The external libraries still hold value with their advanced functionality and support but if you need something to accomplish most of what you need, all on top of ASP.NET configuration, you've got it

The libraries are open-sourced, where you can see much of the benefits:

- Pre-packaged feature filters, and the ability to write your own with the IFeatureManager interface
- Easy service registration in ASP.NET Core middleware
- Ability to gate features at the controller/action level with FeatureGate
- Support for a `<feature>` tag helper to get away from the @if Razor syntax and to assist with conditional rendering
- HttpContext support
- Ability to execute flags if a set of flags are specified, and the ability to check for missing feature filters
For details, check out [the docs](https://docs.microsoft.com/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core?tabs=core2x).

## New Visual Studio feature: Local Process with Kubernetes

Microsoft rolled out a new preview of Visual Studio 2019 this week ([16.7 Preview 2](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-7-preview-2/), to be exact - if you aren't aware, you can install these with regular Visual Studio side-by-side). It's easy to gloss over another preview, but the VS team has introduced a new feature feature: [Local Process with Kubernetes](https://devblogs.microsoft.com/visualstudio/introducing-local-process-with-kubernetes-for-visual-studio%E2%80%AF2019/).

Kubernetes, love it as we might, is Latin for "overly complicated." (It's true, you don't need to Google it.) Even if you have a solid workflow down, it involves updating your source code, building a container image, and deploying to your cluster - and forcing you to manage Dockerfile and Kubernetes manifest files. If you have a lot of microservices, each with their own data stores, it's a real, actual headache. 

Using Local Process with Kubernetes, you connect your machine to your Kubernetes cluster and don't need to compile all your dependencies every single time. Environment variables, connection strings, and the like are inherited by the local microservice code. Under the covers, the feature redirects traffic between your connected cluster and your development machine.

It looks promising. If you try it out, let me know how it goes (you can reply to this email, or the Twitter links are below).

(You can also use this feature [in Visual Studio Code](https://docs.microsoft.com/azure/dev-spaces/how-to/local-process-kubernetes-vs-code).)

## Odds and ends

Authentication and security is not easy, and a pain point for those with simple scenarios in ASP.NET Core. It is designed to be standard-based, but the learning curve is real. The [community is listening](https://twitter.com/HumanCompiler/status/1269042512997396480).

--

Need a Blazor fix? Register for [Blazor Day on June 18](https://blazorday.net/planning).

## Community roundup

### From Microsoft

- As mentioned above, Microsoft released [Visual Studio v16.7 Preview 2](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-7-preview-2/), and with it comes a new [Local Process with Kubernetes feature](https://devblogs.microsoft.com/visualstudio/introducing-local-process-with-kubernetes-for-visual-studio%E2%80%AF2019/).
- Microsoft provides updates for [Azure CosmosDB's API for MongoDB](https://devblogs.microsoft.com/cosmosdb/build-2020-recap-whats-new-in-azure-cosmos-dbs-api-for-mongodb/).
- Andrew Clinick writes about the [WinGet/AppGet confusion](https://devblogs.microsoft.com/commandline/winget-install-learning/). However we got here, I think Microsoft can do better than a Saturday morning blog post that skirts around the concerns from the OSS community.

### Blog posts

- Damien Bowden discusses [using an ASP.NET Core API with Azure Active Directory auth and user access token](https://damienbod.com/2020/05/29/login-and-use-asp-net-core-api-with-azure-ad-auth-and-user-access-tokens/).
- A busy week for Telerik, as their folks write about [needing a Blazor ecosystem](https://www.telerik.com/blogs/what-blazor-needs-an-ecosystem), [Fiddler for Xamarin devs](https://www.telerik.com/blogs/fiddler-for-xamarin-developers), and [testing in production with Fiddler](https://www.telerik.com/blogs/test-in-production-with-fiddler).
- A quick note about [dynamic payloads in ASP.NET Core](https://weblogs.asp.net/ricardoperes/dynamic-payloads-in-asp-net-core).
- Andrew Lock walks through [customizing the ASP.NET Core default UI without editing PageModels](https://andrewlock.net/customising-aspnetcore-identity-without-editing-the-pagemodel/).
- Jeremy Likness [dynamically builds LINQ expressions](https://blog.jeremylikness.com/blog/dynamically-build-linq-expressions/).
- Derek Comartin [configures errors and warnings in C#](https://codeopinion.com/configuring-errors-and-warnings-in-c/).
- Brady Gaster talks about downr 3.0, a Markdown-powered blogging engine - [now updated for ASP.NET Core 3.1 and Blazor WebAssembly](https://bradygaster.com/posts/introducing-downr-3).
- Karthik Chintala [breaks down how ASP.NET Core processes a request](https://coderethinked.com/how-does-asp-net-core-processes-a-request/).
- Ken Dale [noticed that System.Text.Json settings don't roundtrip](https://rimdev.io/default-system-text-json-settings-dont-roundtrip-serialize-deserialize-through-test-server/).
- Rikam Palkar [walks through an async/await example](https://www.c-sharpcorner.com/article/asynchronous-programming-with-async-await/).
- Jason Gaylord talks about [his experiences installing the latest .NET 5 bits](https://www.jasongaylord.com/blog/2020/06/04/dotnet-5-preview-4-and-visual-studio-2019) and Visual Studio 2019 previews and also [creating your first Azure Static Web App](https://www.jasongaylord.com/blog/2020/06/01/creating-your-first-azure-static-web-app).
- Dominique St-Amand [discusses live notifications from Azure Key Vault to Slack](https://www.domstamand.com/live-notifications-from-an-azure-keyvault-to-your-slack/).
- James McCaffrey [clusters non-numeric data using C#](https://visualstudiomagazine.com/articles/2020/06/03/clustering-non-numeric-data.aspx).
- Over at the Stack Overflow blog, some [insights on the popularity of Kubernetes](https://stackoverflow.blog/2020/05/29/why-kubernetes-getting-so-popular/).
- Jon P. Smith breaks down the [anatomy of database reads in Entity Framework Core](https://www.thereformedprogrammer.net/ef-core-in-depth-what-happens-when-ef-core-reads-from-the-database/).

### Podcasts/videos

- .NET Rocks [talks testing Blazor apps](https://www.dotnetrocks.com/default.aspx?ShowNum=1690).
- The Azure podcast [talks about Azure Edge Zones](http://azpodcast.azurewebsites.net/post/Episode-332-Azure-Edge-Zones).
- Keith Beller discusses [managing cloud-ready .NET app secrets in Visual Studio](https://devblogs.microsoft.com/premier-developer/managing-cloud-ready-net-app-secrets-in-visual-studio/).
- ON.NET talks about [SameSite cookie security](https://www.youtube.com/watch?v=HJ_cfB77454).
- A stream of [GitHub Quick Reviews](https://www.youtube.com/watch?v=VE3T-ckgZuI), and [two](https://www.youtube.com/watch?v=H3obznO9_uo) [more](https://www.youtube.com/watch?v=P1w3Tc7Oyqk) .NET design reviews.

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
