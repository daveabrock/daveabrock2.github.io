---
date: "2020-07-05"
title: "The .NET Stacks #6: Blazor mobile bindings, EF update, ASP.NET Core A-Z, more!"
tags: [dotnet-stacks]
comments: false
subtitle: A rundown on mobile Blazor bindings, EF updates, ASP.NET Core A-Z, and more!
share-img: /assets/img/stacks-6-card.png
---

This is an archive of my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away! Subscribers don't have to wait a week to receive the content.
{: .box-note}

On tap this week:

- Mobile Blazor bindings
- More Entity Framework updates (get your questions in)!
- Dev Discussions: Shahed Chowdhuri
- Community roundup

## Mobile Blazor bindings

In this week's ASP.NET community standup, Jon Galloway [talked with Eilon Lipton](https://www.youtube.com/watch?v=ibIl3mgH0LQ) about his *experimental* Mobile Blazor Bindings project. The project has been out in experimental mode for a while and, as Eilon said in the standup, looks to be slated for general public consumption by the end of July or so.

So, what is Mobile Blazor Bindings? (We'll call it MBB from now on.) MBB allows you to write native, cross-platform mobile applications using the Blazor programming model. (If you don't know Blazor yet, it's a new ASP.NET Core technology which allows you to write C# across the stack, in most cases replacing JavaScript.) This allows you to use Blazor with Razor markup, as an alternative to the Xamarin.Forms XAML model, which might be foreign to many web developers. In February, [Dylan Berry wrote about](https://www.dylanberry.com/2020/02/05/mobile-blazor-bindings-getting-started-why-you-should-care/) how he was able to port over a Xamarin.Forms MVVM screen, which was 188 lines - and was only 68 lines with the MBB model.

Based on the success of this project so far, a natural question to ask is what this says for the fate of Xamarin. Based on [Microsoft's past communications on MBB](https://devblogs.microsoft.com/aspnet/mobile-blazor-bindings-experiment/), this is messaged as an option. In other words: *you do you*. If you like to write mobile apps using XAML, continue doing that! If you're a fan of Razor syntax and features (and the Blazor model), this is an option for you. As someone who enjoys the latter and is allergic to XAML (despite what my doctor says), it's a big selling point for me, as it makes the barrier to entry a lot easier if you want to dip your toes into mobile app development.

For more details, [check out the GitHub repo](https://github.com/xamarin/mobileblazorbindings).

## More Entity Framework updates (get your questions in!)

A few weeks ago, we [checked in on EF Core](https://daveabrock.com/2020/06/14/dotnet-stacks-4). As things evolve quickly, and with [this week's release of EF Core 5 Preview 6](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-efcore-5-0-preview-6/), there's still more to discuss! This release includes a lot of requested functionality, including split queries for related collections, an IndexAttribute annotation, improved query translation exceptions, IPAddress mapping, and more.

Split queries is a big one. Until this release, EF Core generates a single SQL query for any LINQ query, even when you use **.Include** or a projection type that returns you multiple related collections. While it ensures consistency, it can be slow-performing.

Now, you can leverage the **AsSplitQuery()** API. Let's say you have a Blogger entity, which has Posts and then Tags. Now, this would be split into three different queries: one to get the blogger (by ID, likely), one to get all Posts for the blogger using an inner join on Blogger, and a third to get Tags, with joins on the Blogger and the Post.

This new feature supports all operations, including **OrderBy**, **Skip**, **Take**, **Join**, and **FirstOrDefault**. Take a look at [the release announcement](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-efcore-5-0-preview-6/) for details.

Also, **I'm excited to say I have a .NET Stacks interview set up with Jeremy Likness, the Senior PM for .NET Data at Microsoft**! If you have any questions about Entity Framework, or any data-related .NET stuff, let me know this week and I will forward it to him.  

## Dev discussions: Shahed Chowdhuri

As a huge fan of the Marvel universe, Microsoft’s Shahed Chowdhuri must feel like Captain America after publishing the last post of his [ASP.NET Core A-Z blog series](https://wakeupandcode.com/aspnetcore/#aspnetcore2020) this week. In these posts, he wrote 26 ASP.NET Core posts in 26 weeks, from A-Z (Authentication & Authorization to Zero-Downtime Web Apps). This was all using a single sample project!

I talked with Shahed about his path to Microsoft, the evolution of the blog series, his side projects and interests and, of course, which Marvel character sums him up best. You can reach out to Shahed [on Twitter](https://twitter.com/shahedC) and follow his site, [Wake Up and Code](https://wakeupandcode.com/).

![Shahed Chowdhuri]({{ site.url }}{{ site.baseurl }}/assets/img/shahed-chowdhuri.jpg)

**What got you into software development, and Microsoft? What are you doing for Microsoft these days?**

Although I took two and a half years to complete a four-year degree in Civil and Environmental Engineering, I also taught myself how to build websites and software applications while in college. I worked on my personal website and also additional websites for the Civil Engineering department, the International Students Association, and my dorm.

I realized that I could build something, see results instantly, fix issues, and then see my changes right away. I got an internship as a web application developer during my second (and final) summer vacation, and then got a full-time job offer upon graduation.

I joined Microsoft as a Tech Evangelist 6+ years ago, as I was already using my spare time for public speaking on various topics. My role has changed in recent years, and I’m currently working with our enterprise customers to collaborate on software development projects and solve real-world business problems.

**What made you take on the original ASP.NET Core A-Z blog series in 2019?**

In the fall of 2018, I was helping a colleague with some .NET code for a customer project (file upload into Azure Blob Storage) during a company hackathon. Instead of emailing him the code sample with an explanation, I decided to write a blog post and publish the code in a GitHub repository. To lead into this blog post, I also worked on a “Hello World” article, that was appropriately titled “Hello, ASP.NET Core!” so that I could help new developers get started with ASP.NET Core.

As a result, this was the birth of a mini series to end 2018 with a bang. Over a span of 12 weeks from October to December 2018, I published these seemingly randomly-selected topics on ASP.NET Core. Eventually, I revealed the hidden message “HAPPY NEW YEAR” just before the new year. Knowing that I couldn’t pull off the same trick twice, I decided go with a new series in 2019.

There are 52 weeks in a year, 26 weeks in half a year. This was the perfect opportunity to cover 26 different topics from A-Z in 2019, since there are also 26 letters in the alphabet.

**As you kicked off a new version of the series with an approach to feature everything in a single project, did you come across anything unexpected or interesting?**

Before 2019, I wasn’t sure if I could pick a topic for each letter if I stuck to ASP.NET Core. I was debating whether I should cover HoloLens, Bot Framework, Machine Learning and various other topics throughout the series. But I narrowed it down to topics that would be relevant to an ASP.NET Core developer, and also reviewed my list with Jon Galloway.

The 2019 series included code snippets that were all over the place. For the 2020 series, I decided to start with a real-world open-source web app (NetLearner) and build upon it week after week. I made sure that the web app included similar functionality across multiple web projects (MVC, Razor Pages, Blazor) with a shared library for core/infrastructure code.

**What is your one piece of programming advice?**

One piece of advice I would give anyone is to always keep learning. That doesn’t necessarily mean that you should learn every new programming language or framework that pops up on your radar. It could mean that you may want to dig deeper into a language you’ve already been working with. It could be some business skills that you want to pick up, to run your own software business or work as a consultant to help others run theirs.

This is only an excerpt of my talk with Shahed. Read [the full interview over at my website](https://daveabrock.com/2020/06/28/dev-discussions-shahed-chowdhuri), especially if you like Baby Groot from the Guardians of the Galaxy movies.

## Community roundup

### From Microsoft

- .NET 5.0 [Preview 6 is out](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-6/), and you can also see the [ASP.NET Core updates for it](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-6/). Not to be outdone, [EF Core 5 Preview 6](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-efcore-5-0-preview-6/) is also out.
- Microsoft introduces [dotnet-monitor, an experimental tool for accessing diagnostic information from a dotnet process](https://devblogs.microsoft.com/dotnet/introducing-dotnet-monitor/).
- Some information from Microsoft on [Project Reunion, a set of libraries and tools for Windows platform functionality](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md).
- Microsoft [provides a roadmap for Windows Terminal 2.0](https://github.com/microsoft/terminal/blob/master/doc/terminal-v2-roadmap.md).
- Mads Kristensen announces [the essential extension pack for Visual Studio](https://devblogs.microsoft.com/visualstudio/delivering-on-a-promise-the-essential-extension-pack/).
- A nice behind-the-scenes on how Azure is built: [design](https://azure.microsoft.com/blog/azurecom-operates-on-azure-part-1-design-principles-and-best-practices/), then [architecture](https://azure.microsoft.com/blog/how-azurecom-operates-on-azure-part-2-technology-and-architecture).

### Blog posts

- Scott Hanselman [shows off the Blockly visual programming editor](https://www.hanselman.com/blog/UsingTheBlocklyVisualProgrammingEditorToCallANETCoreWebAPI.aspx).
- Wael Kdouh talks about [utilizing gRPC-Web from Blazor WebAssembly](https://medium.com/@waelkdouh/how-to-utilize-grpc-web-from-a-blazor-webassembly-application-e8313444f75b).
- Khalid Abuhakmeh talks about [contributing to the most impactful .NET OSS projects](https://khalidabuhakmeh.com/contribute-top-ten-impactful-dotnet-oss-2020) and [parsing Markdown front matter with C#](https://khalidabuhakmeh.com/parse-markdown-front-matter-with-csharp).
- David Grace [performs a partial update using PATCH](https://www.roundthecode.com/dotnet/asp-net-core-api-how-to-perform-partial-update-using-http-patch).
- Damien Bowden uses the [Azure CLI for Azure app registrations](https://damienbod.com/2020/06/22/using-azure-cli-to-create-azure-app-registrations/).
- Damian Meher provides [code snippets for Xamarin Forms](https://damian.fyi/2020/06/19/snippets-platform-specifics/).
- Delpin Susai Raj talks about [how to enable default zooming in Xamarin](https://xamarinmonkeys.blogspot.com/2020/06/xamarinforms-enable-default-zooming-in.html).
- Dave Brock talks about the [state of simplifying null validation in C# 9](https://daveabrock.com/2020/06/24/simplified-null-validation).
- Jason Gaylord runs through [his favorite Visual Studio Code extensions](https://www.jasongaylord.com/blog/2020/06/22/favorite-visual-studio-code-extensions).
- Johnny Reilly [warns about Task.WhenAll / Select](https://blog.johnnyreilly.com/2020/06/taskwhenall-select-is-footgun.html).
- Jon Hilton talks about [managing state in your Blazor application](https://jonhilton.net/blazor-state-management/).
- Chase Aucoin [deploys a .NET container with AWS Fargate](https://developer.okta.com/blog/2020/06/22/deploy-dotnet-container-aws-fargate).
- Andrew Lock talks about [how to get started with ASP.NET Core](https://andrewlock.net/aspnetcore-in-action-2e-getting-started-with-asp-net-core/).
- Shahed Chowdhuri [talks about XML + JSON output for Web APIs](https://wakeupandcode.com/xml-json-output-for-web-apis-in-asp-net-core-3-1/) and [YAML-defined CI/CD](https://wakeupandcode.com/yaml-defined-ci-cd-for-asp-net-core-3-1/).
- Anthony Giretti talks about [improved pattern matching in C# 9](https://wakeupandcode.com/xml-json-output-for-web-apis-in-asp-net-core-3-1/).
- Jonathan Allen discusses [C# 9 type inference with the new keyword](https://www.infoq.com/news/2020/06/csharp-9-new).
- Lee Richardson [talks about securing external APIs](http://www.leerichardson.com/2020/06/securing-external-web-apis-with-api.html).
- Patrick Smacchia has [10 Visual Studio productivity tips](https://blog.ndepend.com/10-visual-studio-solution-ninja-code-edition-tips/).
- Gerald Versluis [creates a Chromecast app with Xamarin.Forms in under 10 minutes](https://blog.verslu.is/xamarin/xamarin-forms-xamarin/google-chromecast-app/?utm_source=rss&utm_medium=rss&utm_campaign=google-chromecast-app).
- Leomaris Reyes [replicates the Schedule UI in Xamarin Forms](https://askxammy.com/replicating-schedule-ui-in-xamarin-forms/).
- Derek Comartin discusses [gotchas when multi-targeting NuGet packages](https://codeopinion.com/multi-targeted-nuget-package-gotchas).

### Podcasts/videos

- The 6-Figure Developer podcast [talks about Xamarin with Scott Ertz](https://6figuredev.com/podcast/episode-149-xamarin-with-scott-ertz/).
- The ASP.NET Monsters walk through [Noda Time and Entity Framework Core](https://www.youtube.com/watch?v=zl0h2J6a0w4).
- The Visual Studio Toolbox [continues their series on EF Core in-depth](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Entity-Framework-Core-In-Depth-Part-5?WT.mc_id=DX_MVP4025064).
- The ON.NET Show discusses [high-performance requests with gRPC](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-High-performance-requests-with-gRPC?WT.mc_id=DX_MVP4025064).
- As for the community standups this week, a busy week with [Entity Framework talking about EF Core in Blazor](https://www.youtube.com/watch?v=HNJYIqeBLQc), [new XAML features for Desktop](https://www.youtube.com/watch?v=q7ODj3RJnME), and the [ASP.NET standup on Mobile Blazor Bindings](https://www.youtube.com/watch?v=ibIl3mgH0LQ).
- Also, [we](https://www.youtube.com/watch?v=R5G4scTRRNQ) [have](https://www.youtube.com/watch?v=rx_098IdZU0) [three](https://www.youtube.com/watch?v=vEUndbRMOjQ) GitHub reviews for .NET APIs.

## New subscribers and feedback

Has this email been forwarded to you? Welcome! I'd love for you [to subscribe](https://www.dotnetstacks.com) and join the community. I promise to guard your email address with my life.

I would love to hear any feedback you have for The .NET Stacks! My goal is to make this the one-stop shop for weekly updates on developing in the .NET ecosystem, so I look forward to any feedback you can provide. You can directly reply to this email, or [talk to me on Twitter as well](https://www.dotnetstacks.com). See you next week!
