---
date: "2020-05-31"
title: "The .NET Stacks #2: Project Tye, YARP, news, and community!"
tags: [dotnet-stacks]
comments: false
subtitle: Details on Project Tye, talking about YARP, and community links!
share-img: /assets/img/stacks-2-card.png
---

This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.
{: .box-note}

Happy Monday, everyone. I have to be honest, it feels awkward diving right into .NET after the last week or so, especially considering what's happening in America right now. I'll just say this before I get to it: I hope everyone is okay. 

## Recovering from Build

Build was great, but it was impossible trying to keep up with all the announcements. It's always great to geek out on new advancements in the .NET ecosystem, but I knew I couldn't focus on everything. Despite that, I've been able to unwind from the madness and think about two updates that developers might have missed but deserve some attention: Project Tye and YARP.

### Project Tye

When designed well, microservices are great. At the risk of sounding like a Gartner consultant, they help you with scalability, data isolation, allow teams to pick the best technology for the job (but not all jobs), and enable small and focused teams to write loosely coupled services that can be updated and deployed easily. 

But: let's say that you've decided to quit your job and become Amazon's newest competitor. You write up a quick web app and some services as REST APIs—an account management service. A shipping service. An inventory service. You also implement the [proxy design pattern](https://dzone.com/articles/why-proxies-are-important-for-microservices) and create an API gateway to handle communication between these services and your web application. Because you love .NET, you've coded all this in a .NET Core MVC web app with four Core APIs behind it. You pat yourself on the back, fire off an email to your (former) boss to let him know you're going to be rich, then call it a night.

In the morning, you start debugging your services and quickly wonder if you can recall that email. You've just signed up for managing the infrastructure and development lifecycle of five applications—and as a result, debugging can be a real pain. And you haven't even considered how you will deploy and manage your distributed application.

This is where [Project Tye comes in](https://github.com/dotnet/tye), giving us hope for a better way: an experimental .NET Foundation project that promises to make "developing, testing, and deploying microservices and distributed applications easier."

The goals of Project Tye are:

- Simplifying the development of microservices by providing the capability to run multiple services with a single command, as well as providing containers for dependencies and providing address discovery using simple conventions
- Automating deployment of .NET applications to Kubernetes by generating manifests with minimal configuration using a single configuration file

This project is in experimental mode but that also means you can make a big impact by getting involved. Check out the [Microsoft blog post](https://devblogs.microsoft.com/aspnet/introducing-project-tye/) and the [GitHub repository](https://github.com/dotnet/tye) for details. As for me, I'm excited about a simplified development process and looking forward to not having to run multiple Visual Studio applications to nail down an issue.

### YARP

Do you use a reverse proxy? If you aren't familiar with them, a few common use cases/scenarios:

- **Security**: a big use case, it takes requests and performs authentication/authorization
- **Load balancing**: a server can sit in front of your backend servers and distribute requests efficiently
- **Optimization**: they can manage compressing inbound/outbound data and provide caching, offloading intensive tasks from your core application servers

YARP—which stands for YARP: A Reverse Proxy—is a new project that is focused on creating a reverse proxy server. According to the README at [the YARP repo](https://github.com/microsoft/reverse-proxy), Microsoft saw a lot of their internal teams either building a proxy or asking for one, so they got to work on a common solution.

If you're wondering "why not stick with what I have now, like nginx?" Feel free, Microsoft says, but this is suited for those who want a super-fast proxy server using infrastructure from the ASP.NET and .NET ecosystems. David Fowler, Microsoft's partner software architect, promises to provide [faster performance than nginx](https://twitter.com/davidfowl/status/1253392545343606785). Proxies like nginx can be configuration headaches, for sure, so providing a simplified configuration model using .NET assets will definitely be big selling points.

For more details, check out the [Microsoft blog post](https://devblogs.microsoft.com/dotnet/introducing-yarp-preview-1/) and the [GitHub repository](https://github.com/microsoft/reverse-proxy). Much like with Tye, it is early and your feedback is appreciated and encouraged.

## Odds and ends

This week, we were able to see the results of the [2020 Stack Overflow Developer Survey](https://insights.stackoverflow.com/survey/2020#overview). About 65,000 developers took part and this is what we learned:

- Rust is again the "most loved" language—despite most people having no experience in it. TypeScript came in at #2, and C# at #8.
- ASP.NET Core was the #1 "most loved" Web framework at 70.7%, outranking React, Vue, Express, and Gatsby, and also the #1 "most loved" other framework

--

The .NET Foundation provided their [April/May 2020 update this week](https://dotnetfoundation.org/blog). It's worth checking out the update to see some new projects.

A few highlights:

- **Docker.DotNet**, a library to interact with Docker Remote API endpoints in your .NET applications
- **Python.NET**, a package that gives Python programmers nearly seamless integration with the .NET 4.0+ CLR on Windows and Mono runtime on Linux and OSX
- **FlubuCore**, a cross platform build and deployment automation system that allows you to define your build and deployment scripts in C# using an intuitive fluent interface

--

There's been plenty of demos on Blazor and all its capabilities, but I really recommend going through the latest [ASP.NET standup](https://www.youtube.com/watch?v=onI2_Q0wrdM), where the team showcases a demo app that isn't just a typical "hello world" app that gets your feet wet. It's called [CarChecker](https://github.com/SteveSandersonMS/CarChecker) and shows off authentication, offline support, localization, browser storage, and more. It even has image recognition from ML.NET! Highly recommended.

## Community roundup

### Announcements

- Microsoft [updates us on experimental mobile Blazor bindings](https://devblogs.microsoft.com/aspnet/announcing-experimental-mobile-blazor-bindings-may-update/).
- Over at the Microsoft Open Source blog, they announced [cloud-native workflows with Dapr and Logic Apps](https://cloudblogs.microsoft.com/opensource/2020/05/26/announcing-cloud-native-workflows-dapr-logic-apps/).
- The Service Bus Explorer in the Azure Portal is [now in preview](https://azure.microsoft.com/updates/sesrvice-bus-explorer/).
- .NET desktop apps is [getting some love from GitHub Actions](https://devblogs.microsoft.com/dotnet/continuous-integration-workflow-template-for-net-core-desktop-apps-with-github-actions/).
- Microsoft Learn has a [new module on cloud-native ASP.NET Core microservices](https://docs.microsoft.com/learn/modules/microservices-aspnet-core/).

### Blog posts

- Scott Hanselman [develops on Docker with Visual Studio Container Tools and WSL2](https://www.hanselman.com/blog/DevelopingOnDockerWithTheNewAndImprovedVisualStudioContainerToolsAndWSL2.aspx).
- Anthony Giretti talks about [enhancing the new Windows Terminal with posh-git](https://anthonygiretti.com/2020/05/24/visual-studio-2019-new-windows-terminal-has-arrived-how-to-enhance-it-with-posh-git-for-git-usage/).
- Konrad Kokosa has [.NET async/await in a simple picture](https://tooslowexception.com/net-asyncawait-in-a-single-picture/).
- Shahed Chowdhuri talks through [unit testing in ASP.NET Core 3.1](https://wakeupandcode.com/unit-testing-in-asp-net-core-3-1/).
- Jeremy Likness [uses Blazor WASM for a serverless ComosDB solution authenticated by Azure Active Directory](https://blog.jeremylikness.com/blog/azure-ad-secured-serverless-cosmosdb-from-blazor-webassembly/).
- AWS talks about [10 years of building .NET](https://aws.amazon.com/blogs/developer/10-years-of-building-net-on-aws/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+AwsDeveloperBlog+%28AWS+Developer+Blog%29).
- Matthias Koch discusses [nullable reference types in C# 8](https://blog.jetbrains.com/dotnet/2020/05/26/nullable-contexts-nullable-attributes/).
- Jason Roberts uses Microsoft.FeatureManagement to [conditionally render HTML](https://dontcodetired.com/blog/post/Conditional-HTML-Rendering-with-Microsoft-Feature-Flags-(MicrosoftFeatureManagement)).
- Steve Gordon [architects a cloud-native service with .NET and AWS](https://www.stevejgordon.co.uk/architecting-a-cloud-native-service-with-dotnet-and-aws).
- Jesse Liberty [sends an alert from the view model](http://jesseliberty.com/2020/05/27/an-alert-from-the-viewmodel/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+JesseLiberty-SilverlightGeek+%28Jesse+Liberty%29).
- Vladimir Vozar walks though [API versioning in ASP.NET Core 3.x](https://exceptionnotfound.net/overview-of-api-versioning-in-asp-net-core-3-0).
- Julio Sampaio talks about [an alternative to Razor templating](https://www.red-gate.com/simple-talk/dotnet/net-development/creating-templates-with-liquid-in-asp-net-core/).

### Podcasts/videos

- The Syntax podcast has a quick chat about [supporting IE11](https://traffic.libsyn.com/secure/syntax/Syntax251.mp3).
- The Visual Studio folks walkthough [debugging managed async code](https://www.youtube.com/watch?v=aVEug50YpaM).
- The ASP.NET Monsters discusses [staged rollouts with Microsoft.FeatureManagement](https://www.youtube.com/watch?v=ptiki5yhWDU).
- Over at ON.NET, they talk [MVC and Razor Pages](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-MVC-and-Razor-Pages?WT.mc_id=DX_MVP4025064) as well as [Blazor](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Blazor).
- The .NET Rocks podcast [discusses building apps with Uno](http://www.dotnetrocks.com/default.aspx?ShowNum=1689).
- A couple .NET community standups: a [Blazor release party](https://www.youtube.com/watch?v=onI2_Q0wrdM), and [Build updates for desktop](https://www.youtube.com/watch?v=vjIQ1xKU5xw).
- A .NET design review [for a few APIs](https://www.youtube.com/watch?v=jhibZCgzB9o).

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
