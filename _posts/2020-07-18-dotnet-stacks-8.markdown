---
date: "2020-07-18"
title: "The .NET Stacks #8: functional C# 9, .NET Foundation nominees, Azure community, more!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-8-card.png
subtitle: We discuss functional programming in C# 9, .NET Foundation updates, and more!
---

This is an archive of my weekly (free!) newsletter, -The .NET Stacks-. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.
{: .box-note}

On tap this week:

- C# 9: a functionally better release
- The .NET Foundation nominees are out!
- Dev Discussions: Michael Crump
- Community roundup

## C# 9: a functionally better release

I've been writing a lot about C# 9 lately. *No, seriously: a lot.* This week I went a little nuts with three posts: I talked about [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), [pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching), and [top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs). I've been learning a ton, which is always the main goal, but what's really interesting is how C# is starting to blur the lines between object-oriented and functional programming. Throughout the years, we've seen some FP concepts visit C#, but I feel this release is really kicking it up a notch.

In the not-so-distant past, discussing FP and OO meant putting up with silly dogmatic arguments that they have to be mutually exclusive. It isn't hard to understand why: traditional concepts of OO constructs are grouping data and behavior (state) in single mutable objects, and FP draws a hard line between data and behavior in the name of purity and minimizing side effects (immutability by default).

So, typically as a .NET developer, this left you with two choices: C#, .NET's flagship language, or F#, a wonderful functional language that is concise (no curlies or semi-colons and great type inference), convenient (functions as first-class objects), and has default immutability.

However, this is no longer a binary choice. For example, let's look at a blog post from a few years ago that [maps C# concepts to F# concepts](https://devblogs.microsoft.com/dotnet/get-started-with-f-as-a-c-developer/).

- **C#/OO has variables, F#/FP has immutable values**. C# 9 init-only properties and records bring that ability to C#.
- **C# has statements, F# has expressions**. C# 8 introduced switch expressions and enhanced pattern matching, and has more expressions littered throughout the language now.
- **C# has objects with methods, F# has types and functions**. C# 9 records are also blurring the line in this regard.

So here we are, just years after wondering if F# will ever take over C#, we see people wondering the exact opposite as Isaac Abraham asks: [will C# replace F#](https://www.compositional-it.com/news-blog/will-csharp-replace-fsharp/)? (Spoiler alert: no.)

There is definitely pushback in the community from C# 8 purists, to which I say: *why not both?* You now have the freedom to "bring in" the value of functional programming, while doing it in a familiar language. You can bring in these features, along with C#'s compatibility. These changes will not break the language. And if they don't appeal to you, you don't have to use them. (Of course, mixing FP and OO in C# is not always graceful and is definitely worth mentioning.)

This isn't a C# vs F# rant, but it comes down to this: is C# with functional bits "good enough" because of your team's skillset, comfort level, and OO needs? Or do you need a clean break, and immutability by default? As for me, I enjoy seeing these features gradually introduced. For example, C# 9 records allow you to build immutable structures but the language isn't imposing this on you for all your objects. You need to opt in.

A more nuanced question to ask is: will C#'s functional concepts ever overpower the language and tilt the scales in FP's direction? Soon, I’ll be interviewing Phillip Carter (the PM for F# at Microsoft) and am curious to hear what he has to say about it. Any questions? Let me know soon and I’ll be sure to include them.

## The .NET Foundation nominees are out

This week, the .NET Foundation [announced the Board of Director nominees for the 2020 campaign](https://dotnetfoundation.org/about/election/candidates). I am familiar with most of these folks (a few are subscribers, hi!)—it's a very strong list and you probably can't go wrong with anyone. I'd encourage you to look at the list and all their profiles to see who you'd like to vote for (if you are a member). If not, you can [apply for membership](https://dotnetfoundation.org/member/become-a-member). Or, if you're just following the progress of the foundation, that's great too.

I know I've talked a lot about the Foundation lately, but this is an important moment for the .NET Foundation. The luster has worn off and it's time to address the big questions: what exactly is the Foundation responsible for? Where is the line between "independence" and Microsoft interests? When OSS projects collide with Microsoft interests, what is the process to work through it? And will the Foundation commit itself to open communication and greater transparency?

As for me, these are the big questions *I hope* the nominees are thinking about, among other things.

## Dev Discussions: Michael Crump

If you've worked on Azure, you've likely come across Michael Crump's work. He started [Azure Tips and Tricks](http://azuredev.tips), a collection of tips, videos, and talks—if it's Azure, it's probably there. He also runs a popular Twitch stream where he talks about various topics.

I caught up with Michael to talk about how he got to working on Azure at Microsoft, his work for the developer community, and his programming advice.

![Michael Crump]({{ site.url }}{{ site.baseurl }}/assets/img/michael-crump.png)

**My [crack team of researchers](https://google.com) tell me that you were a former Microsoft Silverlight MVP. Ah, memories. Do you miss it?**

Ah, yes. I was a Microsoft MVP for 4 years, I believe. I spent a lot of time working with Silverlight because, at that time, I was working in the medical field and a lot of our doctors used Macs. Since I was a C# WinForms/WPF developer, I jumped at the chance to start using those skillsets for code that would run on PCs and Macs.

**Can you walk me through your path to Microsoft, and what you do at Microsoft now?**

I started in Mac tech support because after graduating college, Mac tech support agents were getting paid more than PC agents (supply and demand, I guess!). Then, I was a full-time software developer for about 8 years. I worked in the medical field and created a calculator that determined what amount of vitamins our pre-mature babies should take.

Well, after a while, the stress got to me and I discovered my love for teaching and started a job at Telerik as a developer advocate. Then, the opportunity came at Microsoft for a role to educate and inspire application developers. So my role today consists of developer content in many forms, and helping to set our Tier 1 event strategy for app developers.

**Tell us a little about Azure Tips and Tricks. What motivated you to get started, and how can people get involved?**

Azure Tips and Tricks was created because I'd find a thing or two about Azure, and forget how to do it again. It was originally designed as something *just for me* but many blog aggregators starting picking up on the posts and we decided to go big with it—from e-books, blog posts, videos, conference talks and stickers.

The easiest way to contribute is by clicking on the **Edit Page** button at the bottom of each page. You can also go to *[http://source.azuredev.tips](http://source.azuredev.tips)* to learn more.

**What made you get into Twitch? What goes on in your channel?**

I loved the ability to actually code and have someone watch you and help you code. The interactivity aspect and seeing the same folks come back gets you hooked.

The [stream](https://twitch.tv/mbcrump) is broken down into three streams a week:

- Azure Tips and Tricks, every Wednesday at 1 PM PST (Pacific Standard Time, America)
- Live Interviews with Developers, every Friday at 9 AM PST (Pacific Standard Time, America)
- Live coding/Security Sunday streams, Sundays at 10:30 AM PST (Pacific Standard Time, America)

**What is your one piece of programming advice?**

I actually published [a list of my top 12 things every developer should know](https://dev.to/mbcrump/12-things-every-software-developer-should-be-doing-in-2020-5hbp).

My top one would probably be to learn a different programming language (other than your primary language). Simply put, it broadens your perspective and permits a deeper understanding of how a computer and programming languages work.

*This is only an excerpt of my talk with Michael. Read the [full interview over at my website](https://daveabrock.com/2020/07/10/dev-discussions-michael-crump).*

## Community roundup

An *extremely* busy week, full of great content!

### Microsoft

#### Announcements

- AKS now supports [confidential workloads](https://azure.microsoft.com/updates/azure-kubernetes-service-aks-now-supports-confidential-workloads-through-dcsv2-skus-preview/).
- The Edge team [announces the storage access API](https://blogs.windows.com/msedgedev/2020/07/08/introducing-storage-access-api).
- Microsoft [introduces the Text Analytics for Health APIs](https://techcommunity.microsoft.com/t5/azure-ai/introducing-text-analytics-for-health/ba-p/1505152).
- Pratik Nadagouda [talks about updates to the Git experience in Visual Studio](https://devblogs.microsoft.com/visualstudio/exciting-new-updates-to-the-git-experience-in-visual-studio/).
- Eric Boyd [shows off new Azure Cognitive Services capabilities](https://azure.microsoft.com/blog/azure-ai-build-missioncritical-ai-apps-with-new-cognitive-services-capabilities/).
- The .NET Foundation [has the nominees set for the 2020 campaign](https://dotnetfoundation.org/blog/2020/07/07/kicking-off-campaign-2020).

#### Videos

- The Visual Studio Toolbox [begins a series on performance profiling](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Performance-Profiling-Part-1-An-Introduction) and [continues their series on finding code in Visual Studio](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Finding-Code-Part-2).
- The Xamarin Show [discusses App Center and App Insights](https://channel9.msdn.com/Shows/XamarinShow/App-Center-plus-App-Insights?WT.mc_id=DX_MVP4025064).
- Data Exposed [continues their "why Azure SQL is best for devs" series](https://channel9.msdn.com/Shows/Data-Exposed/Why-Azure-SQL-is-Best-for-Developers-Part-2).
- So many community standups: we have the [Languages & Runtime one](https://www.youtube.com/watch?v=IVJgrLTG1WM), [Entity Framework](https://www.youtube.com/watch?v=5Oow3LlFjTQ), and [ASP.NET Core](https://www.youtube.com/watch?v=sYTH_xYH3iA).

#### Blog posts

- Jason Freeberg [continues his Zero to Hero with App Service series](https://techcommunity.microsoft.com/t5/apps-on-azure/zero-to-hero-with-app-service-part-3-releasing-to-production/ba-p/1506722).
- Miguel Ramos [dives into WinUI 3 in desktop apps](https://blogs.windows.com/windowsdeveloper/2020/07/07/a-deep-dive-into-winui-3-in-desktop-apps).
- Jayme Singleton [runs through the .NET virtual events in July](https://devblogs.microsoft.com/xamarin/virtual-events-july/).
- Leonard Lobel [highlights the Azure CosmosDB change feed](https://devblogs.microsoft.com/cosmosdb/change-feed-unsung-hero-of-azure-cosmos-db/).

### Community Blogs

### ASP.NET Core

- Christian Nagel [walks through local users with ASP.NET Core](https://csharp.christiannagel.com/2020/07/07/aspnetcoreroles/).
- Andrew Lock [continues talking about adding an endpoint graph to ASP.NET Core.](https://andrewlock.net/adding-an-endpoint-graph-to-your-aspnetcore-application/).
- Thomas Ardal [adds Razor runtime compilation for ASP.NET Core](https://blog.elmah.io/add-razor-runtime-compilation-when-developing-asp-net-core/).
- Anthony Giretti [exposes proto files in a lightweight gRPC service](https://anthonygiretti.com/2020/07/06/exposing-proto-files-in-a-grpc-service-over-a-frameworkless-and-lightweight-api/).
- Neel Bhatt [introduces event sourcing in .NET Core](https://neelbhatt.com/2020/07/05/event-sourcing-in-net-core-part-1-a-gentle-introduction/).
- Dominick Baier [talks about flexible access token validation in ASP.NET Core](https://leastprivilege.com/2020/07/06/flexible-access-token-validation-in-asp-net-core/).
- The .NET Rocks podcast [talks about ASP.NET Core Endpoints with Steve Smith](https://www.dotnetrocks.com/default.aspx?ShowNum=1695).
- The ON.NET show [discusses SignalR](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-SignalR).

### Blazor

- Wael Kdouh [secures a Blazor WebAssembly Application With Azure Active Directory](https://medium.com/@waelkdouh/securing-a-blazor-webassembly-application-with-azure-active-directory-7822148f332b).
- Jon Hilton [discusses Blazor validation logic on the client and the server](https://jonhilton.net/blazor-client-server-validation-with-fluentvalidation/).
- Marinko Spasojevic [consumes a web API with Blazor WebAssembly](https://code-maze.com/blazor-webassembly-httpclient/).
- Matthew Jones [continues writing Minesweeper for Blazor Web Assembly](https://exceptionnotfound.net/minesweeper-in-blazor-webassembly-part-2-the-blazor-component/).
  
### Entity Framework

- Khalid Abuhakmeh [talks about adding custom database functions for EF Core](https://khalidabuhakmeh.com/add-custom-database-functions-for-entity-framework-core).
- Jon P. Smith [discusses soft deleting data with Global Query Filters in EF Core](https://www.thereformedprogrammer.net/ef-core-in-depth-soft-deleting-data-with-global-query-filters/).

### Languages

- Dave Brock goes deep on C# 9, with [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), [pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching), and [top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs).
- Ian Griffiths [continues his series on C# 8 nullable references with conditional post-conditions.](https://endjin.com/blog/2020/07/dotnet-csharp-8-nullable-references-conditional-post-conditions.html).
- Khalid Abuhakmeh [walks through reading from a file in C#](https://khalidabuhakmeh.com/understand-reading-from-a-file-using-csharp).

#### Azure

- Joseph Guadagno [uses Azure Key Vault to secure Azure Functions](https://www.josephguadagno.net/2020/07/10/securing-azure-function-settings-with-azure-key-vault) (hi, Joe!).
- Visual Studio Magazine [walks through Azure Machine Learning Studio Web](https://visualstudiomagazine.com/articles/2020/07/09/azure-ml-studio-web.aspx).
- Damien Bowden [walks through using external inputs in Azure Durable Functions](https://damienbod.com/2020/07/06/using-external-inputs-in-azure-durable-functions/).
- Azure Tips and Tricks [has new content about Azure certifications for developers](https://microsoft.github.io/AzureTipsAndTricks/blog/tip269.html).
- Jason Gaylord [discusses adding Azure feature flags to client applications](https://www.jasongaylord.com/blog/2020/07/09/adding-azure-feature-flags-to-client-side-applications).

### Xamarin

- Simon Bisson [pontificates on .NET MAUI and the future of Xamarin](https://www.infoworld.com/article/3565550/understanding-net-maui-and-the-future-of-xamarin.html).
- Leomaris Reyes [uses biometric identification in Xamarin.Forms](https://askxammy.com/using-biometric-identification-in-xamarin-forms/).
- Kym Phillpotts [creates a pizza shop in Xamarin.Forms](https://kymphillpotts.com/pizza-shop.html).
- The Xamarin Podcast [discusses Xamarin.Forms 4.7 and other topics](https://www.xamarinpodcast.com/75).

### Tools

- JetBrains introduces [the .NET Guide](https://blog.jetbrains.com/dotnet/2020/07/09/introducing-the-net-guide-tutorials-and-tips-tricks-for-net-rider-and-resharper/).
- Jason Gaylord [shows us how to create and delete branches in Visual Studio Code](https://www.jasongaylord.com/blog/2020/07/08/create-delete-branches-using-visual-studio-code).
- Mike Larah [uses custom browser configurations with Visual Studio](https://endjin.com/blog/2020/07/debugging-web-apps-in-visual-studio-with-custom-browser-configurations.html).
- Muhammad Rehan Saeed [shows us how to publish NuGet packages quickly](https://rehansaeed.com/the-fastest-nuget-package-ever-published-probably/).
- Patrick Smacchia [talks about his top 10 Visual Studio refactoring tips](https://blog.ndepend.com/top-10-visual-studio-refactoring-tips/).
- Bruce Cottman asks [if GitHub Actions will kill off Jenkins](https://medium.com/swlh/will-github-actions-kill-off-jenkins-f85e614bb8d3).

### Projects

- Oren Eini [releases CosmosDB Profiler 1.0](https://ayende.com/blog/191363-C/ann-cosmos-db-profiler-1-0-release?Key=6bba589c-83a2-4b83-b864-9e46505cb1e3).
- Manuel Grundner [introduces his new Tasty project, an effort to bring the style of his favorite test frameworks to .NET](https://blog.delegate.at/2020/07/06/tasty-delicious-dotnet-testing.html).

### Community podcasts and videos

- Scott Hanselman [shows off his Raspberry Pi and shows off how you can run .NET Notebooks and .NET Interactive on it](https://www.youtube.com/watch?v=fFxYq0hAAHw), [talks about Git pull requests](https://www.youtube.com/watch?v=Mfz8NQncwiQ), [shows off Git 101 basics](https://www.youtube.com/watch?v=WBg9mlpzEYU), and [walks through setting up a prompt with Git, Windows Terminal, PowerShell, and Cascadia Code](https://www.youtube.com/watch?v=lu__oGZVT98&feature=youtu.be).
- The ASP.NET Monsters [talk about NodaTime and API controllers](https://www.youtube.com/watch?v=NnUoOdnsIko).
- The 6-Figure Developer podcast [talks about AI with Matthew Renze](https://6figuredev.com/podcast/episode-151-artificial-intelligence-with-matthew-renze/).
- The No Dogma podcast [talks with Bill Wagner about .NET 5 and unifying .NET](https://no-dogma-podcast-301aeb97.simplecast.com/episodes/144-bill-wagner-net-5-and-unifying-net-dOZsxRN4).
- The Coding Blocks podcast [studies The DevOps Handbook](https://www.codingblocks.net/podcast/the-devops-handbook-the-technical-practices-of-flow/).

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
