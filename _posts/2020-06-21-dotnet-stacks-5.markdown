---
date: "2020-06-20"
title: "The .NET Stacks #5: gRPC-Web, play with C# 9, .NET Foundation, community roundup!"
tags: [dotnet-stacks]
comments: false
subtitle: A rundown on gRPC-web and how to play with the C# 9 bits today!
share-img: /assets/img/stacks-5-card.png
---

This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.
{: .box-note}

On tap this week:

- gRPC-Web for .NET
- Try out C# 9 in LinqPad
- Checking in on the .NET Foundation
- Community roundup

## gRPC-Web for .NET officially released this week

Microsoft [announced this week](https://devblogs.microsoft.com/aspnet/grpc-web-for-net-now-available/) that gRPC-Web for .NET is officially available, after offering it in preview in January. Before discussing what problem it solves, let's step back with a quick primer on gRPC for the uninitiated.

gRPC is a high-performance Remote Procedure Call (RPC) framework, that is language-agnostic to boot (see [the gRPC site](https://grpc.io/).) If this concept is new to you, you are probably wondering one of two things (or both): (1) how does this compare to HTTP APIs, and (2) how is this different than WCF, which I have previously associated with RPC calls?

### How is gRPC different than HTTP APIs?

Your typical workflow of retrieving data over the wire likely involves HTTP APIs, such as REST, with either JSON or XML. Here's a rundown of some key differences:

- REST APIs offer optional schema/contract support using OpenAPI/Swagger, while it is required in gRPC using a *.proto* definition. This file defines the contact of your gRPC services and messages. You've likely noticed that REST is open to a lot of ... interpretation, and a contract model is a huge benefit.
- While existing API models use regular HTTP, gRPC is designed for HTTP/2 and the performance benefits it provides, such as compression and multiplexing. While HTTP APIs can use HTTP/2, it is not designed for it by default like gRPC is.
- gRPC offers bi-directional streaming, while HTTP APIs only offer client or server streaming.
- gRPC offers first-class code generation support, by sharing the *.proto* file between client and server implementations, while HTTP APIs require additional tooling and OpenAPI support.

### How is gRPC different than WCF?

So, to answer the second question, how is this different than your experience with WCF? The main use case for RPC is coding with coordination between the client and the server with the idea of a single platform without a networking dependency. 

With these benefits, you can see gRPC coming in handy for microservices in need of efficiency gains and/or point-to-point real-time requirements. Where gRPC shines over WCF - in addition to what I outlined above - is not requiring the dated SOAP protocol, no .NET language dependency, and the Protobuf serialization model (an extremely efficient binary message format).

### gRPC-Web for .NET: resolving browser support issue

Of course, gRPC doesn't come without weaknesses. Human readability is an issue, as gRPC messages are encoded by default. But the real limitation is the limited browser support, if at all. The HTTP/2 support is awesome, but there isn't a browser today that can control requests over the Web for gRPC clients. Browsers do not require HTTP/2 and aren't mature enough to support it yet. This opens up a need to tooling that provides *some* gRPC browser support.

This is the need that gRPC-Web for .NET fills. gRPC-Web provides a JavaScript client that all modern browsers support, and a server proxy. The client, in turn, calls the proxy and the proxy forwards on the gRPC requests - allowing you to prevent hacking some nginx magic.

Your browser needs are likely high these days, with SPAs and Blazor, so this provides a lot of benefits. Try it out by [checking out the announcement](https://devblogs.microsoft.com/aspnet/grpc-web-for-net-now-available/) and going through the [documentation](https://docs.microsoft.com/aspnet/core/grpc/browser?view=aspnetcore-3.1), and a [sample app](https://github.com/grpc/grpc-dotnet/tree/master/examples#browser).

### Play with C# 9 today

I [played around with C# 9 a little bit this week](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp). C# 9 is slated for release with .NET 5 in November 2020. Lucky for the community, it's a lot easier to play with the preview bits. You can [now use the LinqPad tool](https://twitter.com/linqpad/status/1273191238087225345)! Just enable a checkbox in your settings, and you're good to go. You don't even need to install anything. Give it a shot and let me know what you think.

My main takeaway is: things are getting a lot more functional and immutable.

### Checking in with the .NET Foundation

I don't think I'm going out on a limb by saying Microsoft is going through a rough patch with the OSS community right now. The progress in Microsoft embracing OSS in the last decade, and their reputation, took a hard hit with the issues surrounding AppGet - even the biggest Microsoft fans would probably admit the communication and openness was severely lacking, and a weekend blog post [skirting around things](https://devblogs.microsoft.com/commandline/winget-install-learning/) didn't seem to help. It's triggered several community responses [like this](https://twitter.com/hhariri/status/1267066538504445952). For those of us in the community who have backed Microsoft's efforts, it's a little sad.

This seems to have placed renewed focus on the .NET Foundation, showcased as *an independent, non-profit organization established to support an innovative, commercially friendly, open-source ecosystem around the .NET platform*. A big benefit here is community outreach, and, as Microsoft being a corporation due to shareholders, they obviously have some interest there. With board elections coming up, it's definitely high time to make an impact (with the caveats/analysis [written here](https://seankilleen.com/2020/06/thoughts-on-the-net-foundations-revised-election-process/)).

While community folks are encouraged to participate, I'm also hoping this is Microsoft's opportunity to become more open, honest, and communicative with the developer community.

## Community roundup

We had a light week this week, but still have some good stuff below from the community.

### From Microsoft

- Leslie Richardson continues [debugging async code with parallel stacks for tasks](https://devblogs.microsoft.com/visualstudio/debugging-async-code-parallel-stacks-for-tasks/).
- gRPC-Web for .NET [is now officially released](https://devblogs.microsoft.com/aspnet/grpc-web-for-net-now-available/).
- Some advice on [how to optimize your Azure costs](https://azure.microsoft.com/blog/optimize-your-azure-costs-to-help-meet-your-financial-objectives/).
- A cool study of [machine learning on Azure for baseball decision analysis](https://techcommunity.microsoft.com/t5/azure-global/machine-learning-on-azure-for-baseball-decision-analysis/ba-p/1474902).
- A walkthrough of [what's in Xamarin.Forms 4.7](https://devblogs.microsoft.com/xamarin/xamarin-forms-4-7/).

### Blog posts

- Rick Strahl discusses [adding additional MIME mappings to the static file provider](https://weblog.west-wind.com/posts/2020/Jun/12/Adding-Additional-Mime-Mappings-to-the-Static-File-Provider).
- Shahed Chowdhuri walks through [ASP.NET Core 3.1 validation](https://wakeupandcode.com/validation-in-asp-net-core-3-1/) and [the Worker Service](https://wakeupandcode.com/worker-service-in-net-core-3-1/).
- Sam Basu talks about [7 things to enjoy in MAUI](https://www.telerik.com/blogs/7-things-to-enjoy-maui-and-dotnet-maui).
- Anthony Giretti [introduces C# 9 init-only properties](https://anthonygiretti.com/2020/06/16/introducing-c-9-init-only-properties/) and [records](https://anthonygiretti.com/2020/06/17/introducing-c-9-records/).
- Andrew Lock adds [host filtering to Kestrel in ASP.NET Core](https://andrewlock.net/adding-host-filtering-to-kestrel-in-aspnetcore/).
- Dave Brock [talks about feature flags with Azure App Configuration](https://daveabrock.com/2020/06/17/use-feature-flags-azure-app-config) and [tours C# 9](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp).
- Andy Leonard [introduces the Azure Data Factory REST API](https://andyleonard.blog/2020/06/an-introduction-to-azure-data-factory-rest-api/).
- Daniel Hindrikes talks about [TinyMvvm and Xamarin.Forms](https://danielhindrikes.se/index.php/2020/06/10/get-started-with-tinymvvm/).
- Gary Woodfine on [the continuous integration check-in dance](https://garywoodfine.com/the-continuous-integration-check-in-dance/).
- Joseph Guadagno [protects an ASP.NET Core Web API with Microsoft Identity Platform](https://www.josephguadagno.net/2020/06/12/protecting-an-asp-net-core-api-with-microsoft-identity-platform).
- Mark Heath discusses which database to use in [your Azure serverless app](https://markheath.net/post/azure-serverless-database).
- Patrick Smacchia offers some [Visual Studio Solution Explorer productivity tips](https://blog.ndepend.com/10-visual-studio-solution-explorer-productivity-tips/).

### Podcasts/videos

- At the Visual Studio toolbox, Scott Hunter talks .NET 5 in [two](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/NET-with-Scott-Hunter-Part-1) [parts](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/NET-with-Scott-Hunter-Part-2), and the EF Core [in-depth series continues](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Entity-Framework-Core-In-Depth-Part-3).
- ON.NET talks about [tracing](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Tracing) and [performance testing techniques](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Performance-Testing-Techniques).
- The Complete Developer Podcast talks about [REST anti-patterns](https://completedeveloperpodcast.com/episode-255/).
- The ASP.NET community standup discusses [performance infrastructure](https://www.youtube.com/watch?v=QIDxGbSJhBE&t=4s).
- This week, [two](https://www.youtube.com/watch?v=Ot8PTydQi2k) [different](https://www.youtube.com/watch?v=t-X09mGPvNM) GitHub quick reviews.
- The Xamarin Show talks [Resizetizer.NT for cross-platform images](https://channel9.msdn.com/Shows/XamarinShow/Cross-platform-Images-Simplified-with-ResizetizerNT--The-Xamarin-Show).
- All of this week's Blazor 2020 sessions [are on YouTube](https://www.youtube.com/watch?v=XoizucRjxgU&feature=youtu.be).
- The Azure Functions team [caught up on things](https://www.youtube.com/watch?v=8jzvH-iqcEo).

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
