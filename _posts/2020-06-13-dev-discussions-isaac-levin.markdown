---
date: "2020-06-13"
title: "Dev Discussions - Isaac Levin talks PresenceLight"
subtitle: Microsoft's Isaac Levin talks about his PresenceLight project.
share-img: /assets/img/isaac-card.png
tags: [dotnet-stacks, dev-discussions]
---

{: .box-note}
This is the full interview from my discussion with Isaac Levin in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away!

If you watched Scott Hanselman's [keynote at Microsoft Build](https://mybuild.microsoft.com/sessions/871ef73f-f04a-405b-a0fa-01d7433067d1?WT.mc_id=-blog-scottha) (you know, the "self-hosted" one), he showed off changing room lighting to your status in Teams (or Slack, Skype, etc.) and matching any background colors to your smart lights with the click of a button!

This was brought to you by PresenceLight, a WPF app developed by Microsoft's Isaac Levin. I caught up with Isaac to talk about his path to Microsoft, the development of the app, and advice for developers.

For more information on PresenceLight, check out the [GitHub repo](https://github.com/isaacrlevin/PresenceLight) (where you can [find the releases](https://github.com/isaacrlevin/PresenceLight/releases)), Isaac's [blog post](https://www.isaaclevin.com/post/presence-light/), and of course you can always [contact him on Twitter](https://twitter.com/isaacrlevin).

![Isaac Levin]({{ site.url }}{{ site.baseurl }}/assets/img/isaac-levin.jpg)

**Can you walk me through your path to working at Microsoft?**

I graduated college in 2010 with no real interest in joining Microsoft, mostly due to imposter syndrome. After eight years bouncing around web developer jobs and consulting, I went to Build, where I met up with an old colleague who had just received an offer to join. He was an MVP and told me all the great things Microsoft had been doing. It piqued my interest.

A few months later, by chance, I was contacted by a recruiter and eventually joined in January of 2018. My first role was a customer-facing developer advocate-type role, part of Microsoft Services (not straight consulting). Then, in November 2019, I moved into my current role as Product Marketing Manager of end-to-end technical content for Developer Tools and DevOps. My job focuses around telling the complete developer story to Azure through content and demos.

**What motivated you to develop PresenceLight?**

> PresenceLight was something I had been thinking of for a bit. I wrote about it in my [blog](https://www.isaaclevin.com/post/presence-light), but mostly it centered around there being no real solution to push your Teams status (called Presence) to a smart light.

There are solutions that are tethered to your machine via USB or a Bluetooth dongle, but nothing that runs independently. When the Microsoft Graph team made Presence available from an API, I started working on a solution pretty quickly.  

**I was under the impression, from how this was introduced in Scott Hanselman's Build keynote, that the PresenceLight's main draw is syncing Philips Hue bulbs with your Microsoft Teams status. Are there more capabilities than that?**

The keynote was actually focused around LIFX lights, as he used those and we partnered with them on a few other things during Build. PresenceLight currently supports Hue, LIFX, YeeLight, and Xiaomi. I would love there to be more. If anyone has some other smart lights they want to see, I recommend they send a PR in.

**Can you walk through a quick high-level architecture of your solution?**

The solution is a WPF application running on .NET Core 5, which is in preview. In summary: the end user opens the app and gets prompted to log in to Microsoft 365. My application gets a token from Azure Active Directory and makes subsequent requests to the Graph API to get the user's profile info and presence. The app polls the API based on the user settings and broadcasts that presence to every enabled light. Check out more in-depth things at the [wiki on GitHub](https://github.com/isaacrlevin/PresenceLight/wiki).

**What was the hardest part of getting this up and running?**

The hardest part was setting up and configuring the auth. Because I work for a very large company, the process to create a tenant at the time was challenging (IT review and such). But now, the Presence API no longer requires admin consent, drastically lowering the barrier to entry. Also, I had never built a real WPF app before, so that was a fun experience.

**Any future plans for PresenceLight? Are you going to Blazor all the things?**

Right now, WPF PresenceLight is basically done, and I'm accepting new functionality via issues or pull requests. One of the things I realized very quickly is that WPF running on Windows is a bit of a blocker for lots of developers.

I wanted to create a solution that could run on a Mac or Linux, and I asked a few friends on the .NET team for some thoughts. I came to the idea if I had an ASP.NET Worker running - think of a Windows service or a Linux daemon - that does polling and light broadcasting, I wouldn't need a UI always open, and wouldn't have UI thread blocking issues to worry about.

My original POC was just that, with a settings file to manage configuration. I quickly realized that was bad UX. I built a server-side Blazor front-end that would act as the login mechanism to Azure AD and allow the user to manage configuration, with all the lifting being done by the Worker. Because both are ASP.NET Core projects, they can actually run in the same project, so there was one endpoint to use.

I'm also leveraging .NET Core 5 single file executable publishes, which allow the built payload to be very minimal. Right now, you can get a build from the Release page and it will just work with Windows, Mac, Linux, and WSL2.

I see the cross-platform version to be a more technical path for PresenceLight, as the WPF app is easy to install and run. It is available on almost every package deployment frameworks and it just works.

Right now, I'm working on how to step up the experience. A lot of technical folks like Raspberry Pis, and I have a working example that I use at home to run PresenceLight on a Raspberry Pi over my local network. I'm still trying to figure out a way to make this distributable, as right now everything is tied to a particular IP address, and forcing folks to do that is a bad experience. After I figure that out, who knows?

**What is your one piece of programming advice?**

The one piece of advice I have is to not worry about your quality as a developer. For a long time in my career (and still to this day) I don't think I am any good. But honestly, whatever you think isn't probably true. Are there bad developers? Of course. But there are far more good developers to think they are bad.

The only way you fix this is to alter your mindset, and write code and learn.

> Take an opportunity to learn new things, watch dev streamers, read docs, whatever you can to learn. Slowly you will realize the only thing that was causing your imposter syndrome was in your head.
