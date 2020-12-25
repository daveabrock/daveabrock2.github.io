---
date: "2020-05-24"
title: "The .NET Stacks #1: Microsoft Build, announcements galore!"
tags: [dotnet-stacks]
comments: false
subtitle: We discuss all of the Microsoft Build news!
share-img: /assets/img/stacks-1-card.png
---

{: .box-note}
This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.

Welcome to the very first issue of *The .NET Stacks*! I appreciate all of you for being my first subscribers—even before it was even launched. If you have any suggestions, you can simply reply to this email or [hit me up on Twitter](https://twitter.com/dotnetstacks). And if you like what you see, I would love for you to [share with others](https://www.dotnetstacks.com) as well. Let's get started!

## 3 things to keep you informed

The three things you need to know to keep you current and impress your colleagues (the details of which are in this newsletter):

1. Blazor Web Assembly support is now live! This gives you the ability to debug C# in the browser and use one language for your *entire* development experience (while you may need to use some interop libraries). Need to work on the front-end, but don't have a team with a good JavaScript skillset? Are you a one-person shop too overwhelmed to learn JavaScript? Well, there you go.
1. With .NET 5, to be released in November 2020, comes the vision of "One .NET," including Xamarin! A core tenet is producing one .NET runtime and framework that can be used everywhere.
1. C# 9 is coming along, which brings a ton of improvements like init-only properties and records (among many, many other things)

## Microsoft Build

As you may have heard, Microsoft Build occurred this week—from the comfort of our homes. The real question is: will we ever want to go back, after having this go so seamlessly?

All the content is available for on-demand viewing on the Build site and Channel 9. Here are the keynotes:

- [Empowering every developer, with Satya Nadella](https://mybuild.microsoft.com/sessions/23912de2-1531-4684-b85a-d57ac30af09e?source=sessions)
- [Every developer is welcome, with Scott Hanselman and guests](https://mybuild.microsoft.com/sessions/871ef73f-f04a-405b-a0fa-01d7433067d1?source=sessions)
- [Azure: Invent with purpose](https://mybuild.microsoft.com/sessions/80ec2639-35c3-462b-8155-1ef52c29310c?source=sessions)
- [Building the tools to work and learn, with Rajesh Jha and guests](https://mybuild.microsoft.com/sessions/828faeb1-b24f-427f-bfce-078b8c0f4fd5?source=sessions)

On top of the keynotes, there were several valuable break-out sessions that you can [watch at your convenience](https://mybuild.microsoft.com/). Of course, a ton of announcements/news:

- The new Windows Terminal is [live with the 1.0 release](https://devblogs.microsoft.com/commandline/windows-terminal-1-0/). Who thought you could have fun with the terminal? 
- The official Windows Package Manager is [now in preview](https://devblogs.microsoft.com/commandline/windows-package-manager-preview/). 
- C# 9 is [really coming along](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/), and [so is F# 5](https://devblogs.microsoft.com/dotnet/f-5-update-for-net-5-preview-4/).
- Not new, but definitely showcased by Scott Hanselman and others at Build: [PowerToys](https://github.com/microsoft/PowerToys), a set of powerful utilities for Windows.
- Preview 4 of .NET 5 is [now available](https://devblogs.microsoft.com/dotnet/announcing-net-5-preview-4-and-our-journey-to-one-net/). .NET 5, the culmination of the "One .NET" mission, will be released in November. 
- Microsoft [released .NET Multi-Platform App UI (.NET MAUI)](https://devblogs.microsoft.com/dotnet/introducing-net-multi-platform-app-ui/), the evolution of Xamarin.Forms into unified .NET (in .NET 6). This promises the ability to write code for any device from a single codebase.
- C# in the browser is finally GA, [with Blazor WebAssembly support](https://devblogs.microsoft.com/aspnet/blazor-webassembly-3-2-0-now-available/)! It's turned into a mature ecosystem very quickly.
- While announced before Build, they showcased Visual Studio Codespaces (formerly Visual Studio Online), which allows you to [work with fully-configured environments on the fly](https://devblogs.microsoft.com/visualstudio/introducing-visual-studio-codespaces/), even from the browser.
- Azure Static Web Apps have rolled out (in preview), and [looks quite promising](https://azure.microsoft.com/services/app-service/static/).
- Learn TV [is now live](https://docs.microsoft.com/learn/tv/).

## Thought of the week

For many of us who have been writing .NET for awhile, we can easily split Microsoft into two companies: the Gates/Ballmer years ("OId Microsoft"), with its resistance to open source in favor of world dominance and Microsoft lock-in; and two, the kindler/gentler Microsoft who strives to be open to all, in favor of driving business to its Azure offerings ("New Microsoft").

When New Microsoft announced the Windows Package Manager, it was met with quite a bit of skepticism from the community, and caused one spirited GitHub discussion. Leaders in the space, like Chocolatey, don't seem to be worried yet, but some folks in the community are asking: why is this needed, with so many community projects out there that already solve this problem (or are further along)? Microsoft points at the issues delivering a native app as well as security considerations.

Microsoft's stance of "if you are happy with what you use, great" comes off to some as a little naive, considering how the remnants of Old Microsoft still linger to some folks. As for the community, some patience would help without jumping to conclusions, as Microsoft has open-sourced this and is open to feedback—and New Microsoft has a track record of working with developers, not against them.

## From the community

This section is a bit brief because of all the Build announcements, but alas:

- A couple .NET design reviews this week: [connection abstractions](https://www.youtube.com/watch?v=C5W7f10eX3Q) and [ARM](https://www.youtube.com/watch?v=4JGzVdL4mkI&t=6232s).
- Ben Foster talks about [binding and validating enums for ASP.NET Core](https://benfoster.io/blog/binding-validating-enums-aspnet-core/).
- Jason Roberts [illustrates the power of feature filters in the Microsoft.FeatureManagement library](https://dontcodetired.com/blog/post/Microsoft-Feature-Flags-Controlling-Features-with-Feature-Filters-(MicrosoftFeatureManagement)).
- Thomas Levesque [talks hash codes](https://thomaslevesque.com/2020/05/15/things-every-csharp-developer-should-know-1-hash-codes/).
- Shahed Chowdhuri [discusses tag helper authoring in ASP.NET Core 3.1](https://wakeupandcode.com/tag-helper-authoring-in-asp-net-core-3-1/).
- Thomas Ardal shows [how to implement always signed in with ASP.NET Core and Azure](https://blog.elmah.io/implementing-always-signed-in-with-asp-net-core-and-azure/).
- Steve Fenton talks about [simplified console apps with C# 9](https://www.stevefenton.co.uk/2020/05/csharp-9-simplified-console-apps/).

## What are you building?

Care to show off what you're building? Reach out at hey@dotnetstacks.com, and it might be featured in a future newsletter.

## Tweet of the week

![tweet of the week]({{ site.url }}{{ site.baseurl }}/assets/img/tweet-of-week.png)

## Feedback?

I would love to hear any feedback you have for The .NET Stacks! For a little while, I'll be trying some new things—as with software, some might work and some might not. My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter](https://www.dotnetstacks.com) as well. I'll talk to you next week!
