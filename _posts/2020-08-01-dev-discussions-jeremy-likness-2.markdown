---
date: "2020-08-01"
title: "Dev Discussions - Jeremy Likness (2 of 2)"
subtitle: We continue our discussion with Jeremy Likness, the senior PM of .NET Data at Microsoft.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/likness-2-card.png
---

This is the full interview from my discussion with Jeremy Likness in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away!
{: .box-note}

If you've worked on Azure or .NET for awhile—like Azure Functions and Entity Framework—you're probably familiar with Jeremy Likness, the senior PM for .NET data at Microsoft. He's spoken at several conferences (remember those?), [writes about various topics at his site](https://blog.jeremylikness.com/), is a familiar face on various .NET-related videos, and, as of late, can be seen in the Entity Framework community standups (and more!).

I caught up with Jeremy to talk about his path to software development and all that's going on with Entity Framework and .NET 5.

Last week, [we got to know Jeremy](https://daveabrock.com/2020/07/25/dev-discussions-jeremy-likness-1). Now, we'll get into his work and discuss Entity Framework and .NET 5.

We go through a lot of resources in this post. I've included them all in a list at the bottom of this article.
{: .notice--info}

![Jeremy Likness]({{ site.url }}{{ site.baseurl }}/assets/img/jeremy-likness.jpg)

**To be honest, when I think of your role as the "Sr. PM of .NET Data," I mostly think of "Sr. PM of Entity Framework." Does anything else fall under that umbrella?**

Entity Framework is certainly a large part of what I do, but the role is really about all of the ways that .NET developers interact with data. 
In addition to EF Core, I also deal with big data and manage .NET for Spark. I don’t just limit my scope to products I’m directly responsible for, but also focus on things like OData, gRPC and GraphQL for connecting to data over APIs.

I’m interested in improving the experience for the products we own at Microsoft as well as supporting community projects like Hot Chocolate, GraphQL .NET, and Dapper—just to name a few. I work closely with the team responsible for the SQL client. I work with Azure SQL and Cosmos DB.

I’m focused on an initiative to reorganize our documentation to have a landing page for .NET Data so that developers can find what they need with just a few clicks. It will focus on topics ranging from storage and NoSQL to relational databases, cache, APIs, and more.  I encourage anyone reading this to participate and share feedback using [the GitHub issue](https://github.com/dotnet/docs/issues/19029). Our goal is to provide the best possible experience for .NET developers working with data, so you can imagine there is a lot of surface area to consider.

That’s actually something that applies to the EF Core team as well. They are branded based on that product, but are really passionate about all things data-related. The team “owns” `System.Data`.

**I know that when EF Core first rolled out, many features from EF 6 weren't ported over (I'm thinking of 'Always On' encryption, for example). Coming into .NET 5, can we say that with progress over the last few years, EF Core can handle virtually any production need?**

Specific to your example, I’m fairly certain that “Always On” encryption was an issue with lack of support in SqlClient and had nothing to do with Entity Framework. More generally, “any production need” covers a lot of ground.

Both EF 6 and EF Core are very mature and widely used and customers are employing both versions in production. For the common case of a data access layer for a web app, I believe that EF Core 5.0 will be “feature complete” for the majority of scenarios developers are looking to address. We are building support and documentation for end-to-end scenarios across all supported platforms, from Xamarin and Blazor to WinForms, WPF, and of course Azure.

Looking at the cloud, we are growing the set of features available for our Cosmos DB provider. The SQL Provider works with Azure SQL with nothing more than a connection string change. We are investigating how features like encryption and sharding work with EF Core to provide guidance where needed and consider features if gaps exist.

>One caveat is that EF Core is *not* a micro-ORM and there is not a goal to support that use case. We don’t consider it the tool to solve all data access needs.

In fact, some customers use a lightweight solution like Dapper for queries and use EF Core for the create/update/delete side of things. This allows them to take advantage of the features that make EF Core shine, like advanced field-mapping with shadow properties, concurrency resolution, and the built-in change trackers and proxies, while having a go-to for lightweight data transfer objects and complex queries using the micro-ORM.

**EF has definitely had its fair share of demanding customers about when features would be developed. This is just me speculating here, but previously I think EF wasn't as community-focused as it could have been, and the community didn't have a lot of insights about the team and how decisions were made.**

**We're all seeing some nice community involvement with roadmaps, documentation, and community standups (in which someone commented about the tremendous productivity for a team so small!). Can you talk about how you are engaging with the community today, and the different ways we can learn, stay up to speed, offer feedback, and understand what's coming?**

I can’t speak to the full history of the EF team, but my impression is they have been very community-focused for quite some time. They’ve been posting weekly updates for several years now and you can see open discussions of issues dating back longer than that. One thing that resonated with me when I joined the team is their focus and desire to be completely open. Some members came over to Microsoft after being contributors, and most of the engineering team have worked on the same product for years—some over a decade!

We do everything in the open. Everything is tracked via our issues and/or discussions. We prioritize features based on community feedback, including upvotes and comments. We use issues and discussions for design decisions. The roadmap is publicly available and updated whenever priorities shift. 

There are a few ways to keep track with what’s new and to interface directly with the team. First, most of the team is active on Twitter. You can use [the alias to find profiles and ask questions directly](https://aka.ms/efteam-twitter). Second, we are active on GitHub. Perhaps the most useful link is [the issue that is pinned to the issues list and updated weekly with progress](https://github.com/dotnet/efcore/issues/19549). For major releases, [keep an eye on the .NET blog](https://devblogs.microsoft.com/dotnet/author/jeremy-likness/)  - it’s not just a list of what’s new, but also an opportunity to recognize our many community contributors. You might be amazed to see how many people are involved in code and documentation updates for EF Core 5.0!

The other way to keep in touch, and one I am very excited about, is our EF Core Community Standup. We host these every other Wednesday at 10am PT (17:00 UTC). During the standup, we highlight updates, community blog posts, projects, and contributions. It is a live stream and we encourage live Q&A so we can interact directly and answer the community questions that come up. We also take suggestions for shows and bring on guests when we can. You can find the standup schedules and watch previous recorded standups at *[live.dot.net](https://live.dot.net/)*.

**I've been noticing how, in preparation for .NET 5, the EF Core preview releases seem to be in lock-step with ASP.NET Core. Is this intentional? Are you working on a more predictable release cycle and coordination with other teams?**

Yes. We have intentionally aligned EF Core releases with .NET Core releases. ASP.NET Core follows the same cadence. Due to the way all three interoperate, it just makes sense to align them together.

That way we can build a release that potentially takes advantage of new runtime, framework, and SDK features that is released the same time, and likewise ASP.NET Core for example can pull in our releases for things like templates that provide identity management that relies on EF Core. Finally, it provides a consistent cadence for messaging. It makes it very clear when customers can acquire and test preview releases and when to expect the final release.

**Speaking of, while I can easily see what you've been delivering, what are some of the best upcoming feature improvements for EF Core you're most proud of?**

Many of the features are ready for preview. I’m excited that the team has tackled some of the most requested items for this release.

The many-to-many implementation that makes it possible to define relationships in a fluid way—for example, teams and individuals might have relationships, but down the road you want to add metadata about the relationship so you go from `Teams` having a `Person`, and `Person` belongs to `Teams` to a `TeamsPerson` domain object-that will be fully baked into EF Core 5.0.

Split includes provide flexibility over the way you handle sub-collections (like a subquery vs. separate SQL call to fetch the collection). There has been significant effort put into improving the experience for managing schemas via migrations, from additional command line options and support to enhanced documentation. A huge feature is table-per-type mapping, and another is filtered includes that provide far more control over the data sets you return.

Honestly I’m excited about [everything that’s coming out](https://aka.ms/efcore5). I’ve personally been working on end-to-end scenarios and will be very happy to see [clear guidance for using EF Core in Xamarin](https://docs.microsoft.com/ef/core/get-started/xamarin), Blazor, WinForms, WPF, UWP, and other platforms in addition to the documentation we already have.

**What have you discovered about things like EF Core that you weren't aware of, before you joined the team?**

I was surprised just how popular and useful the Cosmos DB provider is. I was skeptical because the SDK is great, so why put EF Core in front of it? After joining the team and talking to customers I realized there is a huge ecosystem that looks at EF Core as a common interface to data. They want to use it in mobile (Xamarin), and in serverless Azure Functions, and to talk to NoSQL databases in addition to relational databases.

>It was really eye-opening to discover just how much development time is saved when we’re able to support these scenarios and provide that common strategy for data access. Just something as simple as managing multiple types in the same collection with automatic discriminators makes a huge difference.

Some scenarios like Xamarin have been very customer-driven. Adoption in Blazor is huge, too, so we’re focused on providing appropriate guidance for all platforms and closing gaps that might exist.

I also am really impressed with the community collaborations the team has built. I sat on a call with a third-party database team that is building a .NET LINQ provider for their own SDK (not part of EF Core) and the EF Core team was happy to provide guidance, share gotchas and lessons learned and offer support from our experience building EF Core.

The team has great relationships across the board—we have conversations with the Dapper team, for example, and on the mobile side are happy to suggest SQLite-Net when it makes sense as a native ORM solution for mobile. It’s really a team focused on solving problems in the best way possible, rather than trying to make their own product the solution for everything.

## Resources

* [GitHub issue for new .NET Data landing page](https://github.com/dotnet/docs/issues/19029)
* [The EF Team on Twitter](https://twitter.com/i/lists/1253069921669410818)
* [EF weekly status updates](https://github.com/dotnet/efcore/issues/19549)
* [Jeremy's posts at the .NET Blog](https://devblogs.microsoft.com/dotnet/author/jeremy-likness/)
* [.NET community standup page](https://dotnet.microsoft.com/platform/community/standup)
* [What's New in EF Core 5.0](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/whatsnew)

*You can follow Jeremy Likness [at his site](https://blog.jeremylikness.com/) and [on Twitter](https://twitter.com/jeremylikness).*
