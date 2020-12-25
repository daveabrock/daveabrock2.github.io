---
date: "2020-06-14"
title: "The .NET Stacks #4: EF Core, PresenceLight, community roundup!"
tags: [dotnet-stacks]
comments: false
subtitle: We check in on EF Core and talk with Isaac Levin about his PresenceLight project!
share-img: /assets/img/stacks-4-card.png
---

{: .box-note}
This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.

A busy issue this week, as we discuss:

- Checking in on Entity Framework Core
- PresenceLight with Isaac Levin
- Community roundup

## Checking in on Entity Framework Core

The Entity Framework team had a [community standup this week](https://www.youtube.com/watch?v=OWuP_qOYwsk). I believe the EF standups are new and it's a great way for the community to get more visibility into what the team is working on - as EF is a crucial piece of the .NET ecosystem with a very demanding audience (this is data access, after all).

They chatted with Erik Ejlskov Jensen - known in the community as ErikEJ - and he showed off his EF Core Power Tools. These tools allow GUI-based EF Core support in Visual Studio. The use cases include reverse engineering existing databases, creating migrations, and creating diagrams of your models. Because the EF team is focused on cross-platform support, their tooling is focused on a streamlined command-line interface (CLI)/PowerShell toolset. As such, with no native GUI support for EF Core in Visual Studio, this fills a great need - and [with almost 90k downloads](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EFCorePowerTools), you may already be familiar.

The team also previewed some new EF Core 5.0 bits, slated for release when .NET 5.0 ships in November. They discussed using ToLog(), a quick-and-dirty way to get up and running with basic EF logging with no logging dependencies required.

When EF Core was released with a limited feature set of EF6 capabilities, they've been swamped with developers screaming, with varying levels of politeness, "when will *this* be in EF Core?" (I admit, I took a turn myself.) I do think it's important to point out that with engaging with the community, the team is giving the community a better idea of what's on the radar. For example, if you look at the [EF Core 5.0 plan](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/plan), you'll see the top 3 most requested features are being addressed:

- Many-to-many navigation properties ([community discussion](https://github.com/dotnet/efcore/issues/1368))
- Filtered includes ([community discussion](https://github.com/dotnet/efcore/issues/1833))
- Table-per-type inheritance mapping ([community discussion](https://github.com/dotnet/efcore/issues/2266))

## Dev discussions: Isaac Levin

*Introducing a new feature where I talk to folks from Microsoft and across the community about what they're working on.*

If you watched Scott Hanselman’s [keynote at Microsoft Build](https://mybuild.microsoft.com/sessions/871ef73f-f04a-405b-a0fa-01d7433067d1?WT.mc_id=-blog-scottha) (you know, the “self-hosted” one), he showed off changing room lighting to your status in Teams (or Slack, Skype, etc.) and matching any background colors to your smart lights with the click of a button! This was brought to you by PresenceLight, a WPF app developed by Microsoft’s Isaac Levin. I caught up with Isaac to talk about his path to Microsoft, the development of the app, and advice for developers.

![Isaac Levin]({{ site.url }}{{ site.baseurl }}/assets/img/isaac-levin-newsletter.jpg)

**What motivated you to develop PresenceLight?**

PresenceLight was something I had been thinking of for a bit. I wrote about it in my [blog](https://www.isaaclevin.com/post/presence-light), but mostly it centered around there being no real solution to push your Teams status (called Presence) to a smart light. There are solutions that are tethered to your machine via USB or a Bluetooth dongle, but nothing that runs independently. When the Microsoft Graph team made Presence available from an API, I started working on a solution pretty quickly.

**Can you walk through a quick high-level architecture of your solution?**

The solution is a WPF application running on .NET Core 5, which is in preview. In summary: the end user opens the app and gets prompted to log in to Microsoft 365. My application gets a token from Azure Active Directory and makes subsequent requests to the Graph API to get the user’s profile info and presence. The app polls the API based on the user settings and broadcasts that presence to every enabled light. Check out more in-depth things at the [wiki on GitHub](https://github.com/isaacrlevin/PresenceLight/wiki).

**Any future plans for PresenceLight? Are you going to Blazor all the things?**

One of the things I realized very quickly is that WPF running on Windows is a bit of a blocker for lots of developers. I wanted to create a solution that could run on a Mac or Linux...and I came to the idea if I had an ASP.NET Worker running ...that does polling and light broadcasting, I wouldn’t need a UI always open, and wouldn’t have UI thread blocking issues to worry about.

I built a server-side Blazor front-end that would act as the login mechanism to Azure AD and allow the user to manage configuration, with all the lifting being done by the Worker. Because both are ASP.NET Core projects, they can actually run in the same project, so there was one endpoint to use.

I’m also leveraging .NET Core 5 single file executable publishes, which allow the built payload to be very minimal. Right now, you can get a build from the [Releases page](https://github.com/isaacrlevin/PresenceLight/releases) and it will just work with Windows, Mac, Linux, and WSL2.

**What is your one piece of programming advice?**

The one piece of advice I have is to not worry about your quality as a developer. For a long time in my career (and still to this day) I don’t think I am any good. But honestly, whatever you think isn’t probably true. Are there bad developers? Of course. But there are far more good developers to think they are bad.

The only way you fix this is to alter your mindset, and write code and learn. Take an opportunity to learn new things, watch dev streamers, read docs, whatever you can to learn. Slowly you will realize the only thing that was causing your imposter syndrome was in your head.

For more information on PresenceLight, check out the [GitHub repo](https://github.com/isaacrlevin/PresenceLight) (where you can [find the releases](https://github.com/isaacrlevin/PresenceLight/releases)), Isaac’s [blog post](https://www.isaaclevin.com/post/presence-light/), and of course you can always [contact him on Twitter](https://twitter.com/isaacrlevin).

*Check out the [full interview at my site](https://daveabrock.com/2020/06/13/dev-discussions-isaac-levin)*.

## Odds and ends

Microsoft introduced this week a ["Web Live Preview" Visual Studio extension](https://devblogs.microsoft.com/aspnet/introducing-web-live-preview/) for .NET Framework customers to get that fine, fine "hot reload" functionality. If it goes well, we may see it working with .NET Core and Blazor as well.

--

ASP.NET Core topped the TechEmpower performance charts for [plaintext responses per second](https://www.techempower.com/benchmarks/#section=data-r19&hw=ph&test=plaintext).

--

The .NET Foundation [announced their 2020 election this week](https://dotnetfoundation.org/blog/2020/06/08/announcing-net-foundation-elections-2020).

## Community roundup

### From Microsoft

- Microsoft announces [preview 5 of EF Core 5.0](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-5-0-preview-5/).
- Not to be outdone, [preview 5 of .NET 5.0](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-5/) and [June updates for .NET Core](https://devblogs.microsoft.com/dotnet/net-core-june-2020-updates-2-1-19-and-3-1-5/).
- Leslie Richardson discusses how [she debugs async code in Visual Studio](https://devblogs.microsoft.com/visualstudio/how-do-i-debug-async-code-in-visual-studio/).
- Microsoft announced Distributed Tracing for Azure Functions...[on Medium](https://medium.com/@tsuyoshiushio/durable-functions-distributed-tracing-71426fe2246f)?

### Blog posts

- Wael Kdouh shows [how to detect unsupported browsers in a Blazor WebAssembly app](https://medium.com/@waelkdouh/how-to-detect-unsupported-browsers-under-a-blazor-webassembly-application-bc11ab0ee015).
- Leomaris Reyes talks about [the evolution of Xamarin into MAUI](https://www.telerik.com/blogs/time-to-evolve-net-multi-platform-app-ui-maui).
- Ricardo Peres starts a [series on ASP.NET Core OData](https://weblogs.asp.net/ricardoperes/asp-net-core-odata-part-1).
- Logesh Palani shows [how to convert an image to base-64 in Xamarin (and the other way around](https://logeshpalani.blogspot.com/2020/06/how-to-convert-base-64-to-image-in.html)).
- At the Stack Overflow blog, [why developers who use Rust love it](https://stackoverflow.blog/2020/06/05/why-the-developers-who-use-rust-love-it-so-much/).
- Darren Hale provides some thoughts on [F# and Azure Functions](https://darrensnotebook.blogspot.com/2020/06/f-and-azure-functions.html).
- Konrad Kokosa discusses [await false & await true](https://tooslowexception.com/await-false-await-true/).
- Matthew Jones plays Tic-Tac-Toe with [Blazor WebAssembly, C#, and .NET Core](https://exceptionnotfound.net/using-blazor-webassembly-and-csharp-to-play-tic-tac-toe-in-dotnet-core).
- Chase Aucoin [authenticates an AWS Lambda function in C#](https://developer.okta.com/blog/2020/06/08/serverless-lambda-functions-csharp).
- Jason Gaylord [walks through Azure Feature Flags in an ASP.NET Razor Pages app](https://www.jasongaylord.com/blog/2020/06/08/adding-azure-feature-flags-to-aspnet-razor-pages-app). 
- Jon Hilton [compares Blazor and React](https://www.telerik.com/blogs/blazor-vs-react-web-developers).
- Andrew Lock talks about [setting global authorization policies in ASP.NET Core 3.x](https://andrewlock.net/setting-global-authorization-policies-using-the-defaultpolicy-and-the-fallbackpolicy-in-aspnet-core-3/).

### Podcasts/videos

- At the Coding Blocks podcast, [how to navigate a code review](https://www.codingblocks.net/podcast/googles-engineering-practices-how-to-navigate-a-code-review/).
- A nice list from Microsoft's Michael Crump of [on-demand Azure app dev videos from Build 2020](https://microsoft.github.io/AzureTipsAndTricks/blog/tip266.html).
- Steve Smith talks about [.NET Foundation with Claire Novotny](https://weeklydevtips.com/episodes/net-foundation-with-guest-claire-novotny-JmEDt8jU).
- The ASP.NET Monsters talk about [feature management and Azure App Configuration](https://www.youtube.com/watch?v=_UOdA7vgqWE).
- ON.NET talks about [endpoint routing in ASP.NET Core](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Endpoint-Routing?WT.mc_id=DX_MVP4025064).
- A few community standups this week: for [Xamarin](https://www.youtube.com/watch?v=6mGQlkkrO8g) and [Entity Framework](https://www.youtube.com/watch?v=OWuP_qOYwsk).

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
