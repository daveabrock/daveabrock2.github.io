---
date: "2020-08-08"
title: "The .NET Stacks #11: Newtonsoft, async void, more with Jeremy Likness, more!"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-11-card.png
subtitle: We discuss Newtonsoft.Json, using async void, more with Jeremy Likness, and more!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

This week, we:

* Talk a little about `Newtonsoft.Json`
* Discuss why `async void` is bad
* Give you a quick reminder to vote for the .NET Foundation
* Continue our conversation with Jeremy Likness, Microsoft's Sr. PM of .NET Data
* Take a trip around the community

## Newtonsoft.Json is still #1

If you [scan NuGet statistics](https://www.nuget.org/stats/packages) from time to time (what, this isn't how you spend your Sunday evenings?), you'll see `Newtonsoft.Json`, the popular JSON serializer/deserializer, still takes the #1 spot. It's been downloaded almost 52.2 million times in the last six weeks. That's almost *five times* more than Serilog, the #2 finisher.

While Microsoft scrapped this library in favor of `System.Text.Json` with .NET Core 3.0, this can't be too surprising—after all, I'm about 90% sure Web Forms and jQuery will outlive me.

In [this week's Visual Studio Magazine this week](https://visualstudiomagazine.com/articles/2020/07/28/json-serializers.aspx), they also take a look. Why the new-ish library? Last year, [Immo Landwerth gave three reasons](https://devblogs.microsoft.com/dotnet/try-the-new-system-text-json-apis/):

* Better performance for higher throughput in .NET Core
* Remove the library dependency and potential versioning headaches
* Provide an ASP.NET Core integration package

If I want to switch over, I'm really only concerned about the first bullet. The last two bullets appear to be Microsoft-speak for wanting complete ownership and control. If you're like me, when I heard this I was thinking of something like: "OK, great, better performance, so I'll switch over if that's something that's important to me."

It's a little more complicated than that. If you look at the migration guide, there's a table of differences that illustrates how it [isn't a simple package download](https://docs.microsoft.com/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to) and that in many ways, developers will cling to Newtonsoft's features even when there's a newer and more performant option.

This isn't to say Newtonsoft.Json is slow - it's not, it's very fast! According to [Spencer Schneidenbach's post on the topic](https://schneids.net/comparing-newtonsoft-json-with-system-text-json), the .NET team could not refactor it without making breaking changes. I'd expect Newtonsoft to continue to be the choice for existing applications, unless extremely high performance is a concern to you.
 
## Why `async void` is bad

C# is full of "just because you *can* do it, doesn't mean you *should"* pitfalls. Across Twitter these days, a lot of folks in the C# community are talking about why you shouldn't use `async void` methods. 

This isn't an original, new, or groundbreaking idea: Stephen Cleary asked you to not do this in 2013 and Phil Haack talked about these methods being a "[scourge upon your code](https://haacked.com/archive/2014/11/11/async-void-methods/)" in 2014.

So why even have this in the language, then? These methods exist to make async event handlers possible. Event handlers typically return void so `async` methods need to return void so you can have an async event handler. If you aren't using it for this exact purpose, here's how you will feel the pain:

* When you use `async void`, thrown exceptions will crash the process. Any uncaught exceptions kill your application and you won't have a decent call stack to work with, unless you do some silly workarounds
* There's no good way to notify the calling code that you've completed, since you aren't returning a `Task<T>`
* As a result, they are a nightmare to test

In short, don't do this and do `async Task<T>` instead. Thank you for coming to my TED talk.

## Last chance to vote for the .NET Foundation board

The voting for the .NET Foundation Board of Directors ends at 11:59pm on August 3 (PST, in the US). If you're a member, you should have received an email. If you aren't a member, [it isn't too late](https://dotnetfoundation.org/member/become-a-member)! (The donation level is suggested but should not be a barrier to joining.)

A lot of people (including myself) have been asking the Foundation for increased communication and transparency. They're listening: this week, they released their [first public budget](https://t.co/FcFdhh6dhg?amp=1) and [also wrote a "State of the Union" post](https://dotnetfoundation.org/blog/2020/07/30/state-of-the-net-foundation-summer-2020).

## More with Jeremy Likness, Sr. PM of .NET Data at Microsoft

Last week, we got to know Jeremy. This week, we’ll get into his work and discuss Entity Framework and .NET 5.

![Jeremy Likness]({{ site.url }}{{ site.baseurl }}/assets/img/jeremy-likness.jpg)

**To be honest, when I think of your role as the "Sr. PM of .NET Data," I mostly think of "Sr. PM of Entity Framework." Does anything else fall under that umbrella?**

Entity Framework is certainly a large part of what I do, but the role is really about all of the ways that .NET developers interact with data. 
In addition to EF Core, I also deal with big data and manage .NET for Spark. I don’t just limit my scope to products I’m directly responsible for, but also focus on things like OData, gRPC and GraphQL for connecting to data over APIs.

I’m interested in improving the experience for the products we own at Microsoft as well as supporting community projects like Hot Chocolate, GraphQL .NET, and Dapper—just to name a few. I work closely with the team responsible for the SQL client. I work with Azure SQL and Cosmos DB.

I’m focused on an initiative to reorganize our documentation to have a landing page for .NET Data so that developers can find what they need with just a few clicks. It will focus on topics ranging from storage and NoSQL to relational databases, cache, APIs, and more.  I encourage anyone reading this to participate and share feedback using [the GitHub issue](https://github.com/dotnet/docs/issues/19029). Our goal is to provide the best possible experience for .NET developers working with data, so you can imagine there is a lot of surface area to consider.

**I know that when EF Core first rolled out, many features from EF 6 weren't ported over (I'm thinking of 'Always On' encryption, for example). Coming into .NET 5, can we say that with progress over the last few years, EF Core can handle virtually any production need?**

Specific to your example, I’m fairly certain that “Always On” encryption was an issue with lack of support in SqlClient and had nothing to do with Entity Framework. More generally, “any production need” covers a lot of ground.

Both EF 6 and EF Core are very mature and widely used and customers are employing both versions in production. For the common case of a data access layer for a web app, I believe that EF Core 5.0 will be “feature complete” for the majority of scenarios developers are looking to address. We are building support and documentation for end-to-end scenarios across all supported platforms, from Xamarin and Blazor to WinForms, WPF, and of course Azure.

Looking at the cloud, we are growing the set of features available for our Cosmos DB provider. The SQL Provider works with Azure SQL with nothing more than a connection string change. We are investigating how features like encryption and sharding work with EF Core to provide guidance where needed and consider features if gaps exist.

One caveat is that EF Core is *not* a micro-ORM and there is not a goal to support that use case. We don’t consider it the tool to solve all data access needs.

In fact, some customers use a lightweight solution like Dapper for queries and use EF Core for the create/update/delete side of things. This allows them to take advantage of the features that make EF Core shine, like advanced field-mapping with shadow properties, concurrency resolution, and the built-in change trackers and proxies, while having a go-to for lightweight data transfer objects and complex queries using the micro-ORM.

**EF has definitely had its fair share of demanding customers about when features would be developed. This is just me speculating here, but previously I think EF wasn't as community-focused as it could have been, and the community didn't have a lot of insights about the team and how decisions were made.**

**We're all seeing some nice community involvement with roadmaps, documentation, and community standups (in which someone commented about the tremendous productivity for a team so small!). Can you talk about how you are engaging with the community today, and the different ways we can learn, stay up to speed, offer feedback, and understand what's coming?**

I can’t speak to the full history of the EF team, but my impression is they have been very community-focused for quite some time. They’ve been posting weekly updates for several years now and you can see open discussions of issues dating back longer than that. One thing that resonated with me when I joined the team is their focus and desire to be completely open. Some members came over to Microsoft after being contributors, and most of the engineering team have worked on the same product for years—some over a decade!

We do everything in the open. Everything is tracked via our issues and/or discussions. We prioritize features based on community feedback, including upvotes and comments. We use issues and discussions for design decisions. The roadmap is publicly available and updated whenever priorities shift. 

There are a few ways to keep track with what’s new and to interface directly with the team. First, most of the team is active on Twitter. You can use [the alias to find profiles and ask questions directly](https://aka.ms/efteam-twitter). Second, we are active on GitHub. Perhaps the most useful link is [the issue that is pinned to the issues list and updated weekly with progress](https://github.com/dotnet/efcore/issues/19549). For major releases, [keep an eye on the .NET blog](https://devblogs.microsoft.com/dotnet/author/jeremy-likness/)  - it’s not just a list of what’s new, but also an opportunity to recognize our many community contributors. You might be amazed to see how many people are involved in code and documentation updates for EF Core 5.0!

The other way to keep in touch, and one I am very excited about, is our EF Core Community Standup. We host these every other Wednesday at 10am PT (17:00 UTC). During the standup, we highlight updates, community blog posts, projects, and contributions. It is a live stream and we encourage live Q&A so we can interact directly and answer the community questions that come up. We also take suggestions for shows and bring on guests when we can. You can find the standup schedules and watch previous recorded standups at *[live.dot.net](https://live.dot.net/)*.

**Speaking of, while I can easily see what you've been delivering, what are some of the best upcoming feature improvements for EF Core you're most proud of?**

Many of the features are ready for preview. I’m excited that the team has tackled some of the most requested items for this release.

The many-to-many implementation that makes it possible to define relationships in a fluid way—for example, teams and individuals might have relationships, but down the road you want to add metadata about the relationship so you go from `Teams` having a `Person`, and `Person` belongs to `Teams` to a `TeamsPerson` domain object-that will be fully baked into EF Core 5.0.

Split includes provide flexibility over the way you handle sub-collections (like a subquery vs. separate SQL call to fetch the collection). There has been significant effort put into improving the experience for managing schemas via migrations, from additional command line options and support to enhanced documentation. A huge feature is table-per-type mapping, and another is filtered includes that provide far more control over the data sets you return.

Honestly I’m excited about [everything that’s coming out](https://aka.ms/efcore5). I’ve personally been working on end-to-end scenarios and will be very happy to see [clear guidance for using EF Core in Xamarin](https://docs.microsoft.com/ef/core/get-started/xamarin), Blazor, WinForms, WPF, UWP, and other platforms in addition to the documentation we already have.

**What have you discovered about things like EF Core that you weren't aware of, before you joined the team?**

I was surprised just how popular and useful the Cosmos DB provider is. I was skeptical because the SDK is great, so why put EF Core in front of it? After joining the team and talking to customers I realized there is a huge ecosystem that looks at EF Core as a common interface to data. They want to use it in mobile (Xamarin), and in serverless Azure Functions, and to talk to NoSQL databases in addition to relational databases.

It was really eye-opening to discover just how much development time is saved when we’re able to support these scenarios and provide that common strategy for data access. Just something as simple as managing multiple types in the same collection with automatic discriminators makes a huge difference.

Some scenarios like Xamarin have been very customer-driven. Adoption in Blazor is huge, too, so we’re focused on providing appropriate guidance for all platforms and closing gaps that might exist.

I also am really impressed with the community collaborations the team has built. I sat on a call with a third-party database team that is building a .NET LINQ provider for their own SDK (not part of EF Core) and the EF Core team was happy to provide guidance, share gotchas and lessons learned and offer support from our experience building EF Core.

The team has great relationships across the board—we have conversations with the Dapper team, for example, and on the mobile side are happy to suggest SQLite-Net when it makes sense as a native ORM solution for mobile. It’s really a team focused on solving problems in the best way possible, rather than trying to make their own product the solution for everything.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* At Visual Studio Magazine, David Ramel [talks about how Newtonsoft.JSON is still the top dog](https://visualstudiomagazine.com/articles/2020/07/28/json-serializers.aspx).
* Derek Comartin [talks about how to manage NuGet versions across your solution](https://codeopinion.com/nuget-packagereference-versions-solution-wide/).
* Bertrand Le Roy [introduces LunrCore, a lightweight search library for .NET](https://weblogs.asp.net/bleroy/lunrcore).

### 📢 Announcements

* Mika Dumont [walks through some new productivity improvements in Visual Studio](https://devblogs.microsoft.com/dotnet/learn-about-the-latest-net-productivity-features/).
* GitHub [announced a public roadmap](https://github.blog/2020-07-28-announcing-the-github-public-roadmap/).
* For Windows devs, Rich Turner [announces a new developer landing page and issues repo](https://blogs.windows.com/windowsdeveloper/2020/07/28/new-developer-landing-page-and-issues-repo).
* Jamie Maguire [introduces a public preview of his Social Opinion library](https://www.jamiemaguire.net/index.php/2020/08/02/introduction-to-social-opinion-public-preview/), a way to consume Twitter Labs API endpoints and work with dashboards.

### 📅 Community and events

* The .NET Conf - Focus on Microservices is a wrap and you can [check out the recorded stream](https://www.youtube.com/watch?v=J9oJTKwASjA).
* Just one community syandup this week because of the microservices conference: [ASP.NET talks migrating from Web Forms to Blazor](https://www.youtube.com/watch?v=l4Xpx9Zx-4Q).
* The .NET Docs Show talks about [reverse proxies and DAPR with Cecil Phillip](https://www.youtube.com/watch?v=5u1YDR4W7Ic).


### 😎 Blazor

* Marinko Spasojevic [talks about PUT and DELETE actions and calling JS in Blazor WASM](https://code-maze.com/blazor-webassembly-put-delete-calling-javascript-functions/).
* Marinko Spasojevic [uploads a file with Blazor WASM and ASP.NET Core Web API](https://code-maze.com/blazor-webassembly-file-upload/).
* Jon Hilton [takes Blazor's EditForms up a notch with Tailwind CSS](https://jonhilton.net/pimp-up-blazor-edit-forms-with-tailwind-css/).

### 🚀 .NET Core

* Scott Hanselman [finds which directory his .NET Core console app was started in or running from](https://www.hanselman.com/blog/HowDoIFindWhichDirectoryMyNETCoreConsoleApplicationWasStartedInOrIsRunningFrom.aspx).
* Carl Rippon [handles concurrency in an ASP.NET Core Web API with Dapper](https://www.carlrippon.com/handling-concurrency-in-an-asp-net-core-web-api-with-dapper/).
* Jason Robert [looks in the ASP.NET Core MVC template](https://espressocoder.com/2020/07/27/whats-in-the-asp-net-core-mvc-template/).

### ⛅ The cloud

* Dave Brock (ahem) [plays with the Microsoft Bot Framework and Azure sentiment analysis](https://daveabrock.com/2020/07/28/azure-bot-service-cognitive-services).
* At the AWS Developer Blog, [how to run Blazor-based .NET apps on AWS Serverless](https://aws.amazon.com/blogs/developer/run-blazor-based-net-web-applications-on-aws-serverless/).
* David Grace [lists and download the contents of a Google Drive shared folder in C#](https://markheath.net/post/list-and-download-google-drive-cs).
* Azure Tips and Tricks [has a new article on Azure security best practices](https://microsoft.github.io/AzureTipsAndTricks/blog/tip272.html).

### 📔 Languages

* Khalid Abuhakmeh [works on sentiment analysis With C#, ML.NET, and Oakton commands](https://khalidabuhakmeh.com/sentiment-analysis-with-csharp-mldotnet-and-oakton-commands).
* Jeremy Miller [calls generic methods from non-generic code in .NET](https://jeremydmiller.com/2020/07/27/calling-generic-methods-from-non-generic-code-in-net/).
* Adam Storr [returns different values from a method call in C#](https://adamstorr.azurewebsites.net/blog/returning-different-values-from-a-method-call).
* Mark Seemann [looks at task async programming as an IO surrogate](https://blog.ploeh.dk/2020/07/27/task-asynchronous-programming-as-an-io-surrogate/).
* Honey the Codewitch [works through the .NET SynchronizationContext in .NET with C#](https://www.codeproject.com/Articles/5274751/Understanding-the-SynchronizationContext-in-NET-wi).
* Eirik Tsarpalis [talks about effect programming in C#](https://eiriktsarpalis.wordpress.com/2020/07/20/effect-programming-in-csharp/).
* Isaac Abraham [discusses how to write more succinct C#, in F#](https://www.compositional-it.com/news-blog/writing-more-succinct-c-in-f-part-1/).

### 🔧 Tools

* Joseph Woodward [integration tests his Polly policies with HttpClient Interception](http://josephwoodward.co.uk/2020/07/integration-testing-polly-policies-httpclient-interception).
* Patrick Smacchia is back [with 14 Visual Studio web development productivity tips](https://blog.ndepend.com/14-visual-studio-web-development-productivity-tips/).
* Scott Hanselman [talks about how you can remote debug a .NET Core Linux app in WSL2 from Visual Studio on Windows](https://www.hanselman.com/blog/OfficialSupportForRemoteDebuggingANETCoreLinuxAppInWSL2FromVisualStudioOnWindows.aspx).
* The full data set [for the Stack Overflow 2020 Developer Survey is available](https://stackoverflow.blog/2020/07/27/public-data-release-of-stack-overflows-2020-developer-survey/).
* Sean Killeen [asks when it makes sense to use containers in a development workflow](https://seankilleen.com/2020/07/when-does-it-make-sense-to-use-containers/).
* Rick Strahl [excludes files and folders in a Visual Studio Web Site project](https://weblog.west-wind.com/posts/2020/Jul/25/Excluding-Files-and-Folders-in-Visual-Studio-Web-Site-Project).

### 📱 Xamarin

* Lance McCarthy [builds a CRM with Xamarin.Forms and Azure](https://www.telerik.com/blogs/building-a-crm-xamarin-forms-azure-part-1).
* Rendy Del Rosario [continues talking about creating a binding library in Xamarin](https://www.xamboy.com/2020/07/28/creating-a-binding-library-in-xamarin-part-2/).

### 🎤 Podcasts

* The Azure DevOps podcast [discusses Infrastructure as Code](http://azuredevopspodcast.clear-measure.com/joe-duffy-on-infrastructure-as-code-episode-99).
* The Azure Podcast [talks about Durable Functions](http://azpodcast.azurewebsites.net/post/Episode-339-Durable-Functions) and also [messaging patterns](http://azpodcast.azurewebsites.net/default.aspx).
* The Merge Conflict podcast [talks about Model-View-Update](https://www.mergeconflict.fm/212).
* The Microsoft 365 Developer Podcast [discusses the Bot Framework Virtual Assistant](https://www.m365devpodcast.com/e/bot-framework-virtual-assistant-with-dewain-robinson-and-gary-pretty/).

### 🎥 Videos

* The ON.NET Show [packages and deploys .NET Core for Linux](https://channel9.msdn.com/Shows/On-NET/Packaging-and-deploying-NET-Core-for-Linux-Part-1).
* The Visual Studio Toolbox [continues their series on performance profiling](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Performance-with-Profiling-Part-3-Profiling-and-Production).
* Data Exposed [talks about .NET Interactive notebooks](https://channel9.msdn.com/Shows/Data-Exposed/Jupyter-Launch-NET-Interactive-Notebooks--Data-Exposed-MVP-Edition).
* Kayla Cinnamon [shares how the Windows Terminal logo came to be](https://www.youtube.com/watch?v=eAnbO02ZD1E).
* The ASP.NET Monsters [talk about YARP, the new .NET reverse proxy](https://www.youtube.com/watch?v=3hOzV_-k-LM).
* At The Maintainers, [Shawn Wildermuth talks to Brad Wilson about xUnit](http://wildermuth.com/2020/07/27/The-Maintainers-Brad-Wilson-and-xUnit).
* The Loosely Coupled Show [discusses when to use CQRS](https://www.youtube.com/watch?v=bX5KRmzkokE).
