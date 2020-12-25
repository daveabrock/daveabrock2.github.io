---
date: "2020-09-26"
title: "The .NET Stacks #18: RC1 is here, the fate of .NET Standard, and F# with Isaac Abraham"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-18-card.png
subtitle: This week, RC1 is here, we talk about .NET Standard, and discuss F# with Isaac Abraham.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

## .NET 5 RC1 is here

This week, Microsoft pushed out the [RC1 release for .NET 5](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-rc-1), which is scheduled to officially "go live" in early November. RC1 comes with a "go live" license, which means you get production support for it. With that, RC1 versions were released for [ASP.NET Core](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/) and [EF Core](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-efcore-5-0-rc1/) as well. 

I've dug deep on a variety of new features in the last few months or so—I won't  rehash them here. However, the links are worth checking out. For example, Richard Lander [goes in-depth](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-rc-1/) on C# 9 records and `System.Text.Json`.

## The fate of .NET Standard

While there are many great updates to the upcoming .NET 5 release, a big selling point is at a higher level: the promise of a [unified SDK experience for all of .NET](https://devblogs.microsoft.com/dotnet/introducing-net-5/). The idea is that you'll be able to use *one* platform regardless of your needs—whether it's Windows, Linux, macOS, Android, WebAssembly, and more. (Because of internal resourcing constraints, [Xamarin will join the party](https://devblogs.microsoft.com/dotnet/introducing-net-multi-platform-app-ui/) in 2021, with .NET 6.)

Microsoft has definitely struggled in communicating a clear direction for .NET the last several years, so when you pair a unified experience with predictable releases and roadmaps, it's music to our ears.

You've probably wondered: *what does this mean for .NET Standard*? The unified experience is great, but what about when you have .NET Framework apps to support? (If you're new to .NET Standard, it's [more-or-less a specification](https://docs.microsoft.com/dotnet/standard/net-standard) where you can target a version of Standard, and all .NET implementations that target it are guaranteed to support all its .NET APIs.)

Immo Landwerth [shed some light on the subject this week](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard/). .NET Standard is being thrown to the .NET purgatory with .NET Framework: it'll still technically be around, and .NET 5 will support it—but the current version, 2.1, will be its last.

As a result, we have some new target framework names: `net5.0`, for apps that run anywhere, combines and replaces `netcoreapp` and `netstandard`. There's also `net5.0-windows` (with Android and iOS flavors to come) for Windows-specific use cases, like UWP.

OK, so .NET Standard is still around but we have new target framework names. What should you do? With .NET Standard 2.0 being the last version to support .NET Framework, use `netstandard2.0` for code sharing between .NET Framework and other platforms. You can use `netstandard2.1` to share between Mono, Xamarin, and .NET Core 3.*x*, and then `net5.0` for anything else (and especially when you want to use .NET 5 improvements and new language features). You'll definitely want to [check out the post for all the details](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard).

What a mess: .NET Standard promised API uniformity and now we're even having to choose between that and a new way of doing things. The post lays out why .NET Standard is problematic, and it makes sense. But when you're trying to innovate at a feverish pace but still support customers on .NET Framework, the cost is complexity—and the irony is that with uniformity with .NET 5, that won't apply when you have legacy apps to support.

## Dev Discussions: Isaac Abraham

As much as we all love C#, there's something that needs reminding from time to time: C# is *not* .NET. It is a large and important part of .NET, for sure, but .NET also supports two other languages: Visual Basic and F#. As for F#, it's been gaining quite a bit of popularity over the last several years, and for good reason: it's approachable, concise, and allows you to embrace a functional-first language while leveraging the power of the .NET ecosystem.

I caught up with Isaac Abraham to learn more about F#. After spending a decade as a C# developer, Isaac embraced the power of F# and founded [Compositional IT](https://www.compositional-it.com/), a functional-first consultancy. He's also the author of [*Get Programming with F#: A Guide for .NET Developers*](https://www.manning.com/books/get-programming-with-f-sharp).

![Isaac Abraham]({{ site.url }}{{ site.baseurl }}/assets/img/isaac-abraham.png)

### I know it's more nuanced than this: but if you could sell F# to C# developers in a sentence or two, how would you do it?

F# really does bring the fun back into software development. You’ll feel more productive, more confident and more empowered to deliver high-quality software for your customers.

### Functional programming is getting a lot of attention in the C# world, as the language is adopting much of its concepts (especially with C# 9). It's a weird balance: trying to have functional concepts in an OO language. How do you feel the balance is going?

I have mixed opinions on this. On the one hand, for the C# dev it’s great—they have a more powerful toolkit at their disposal. But I would hate to be a new developer starting in C# for the first time. There are so many ways to do things now, and the feature (and custom operator!) count is going through the roof. More than that, I worry that we’ll end up with a kind of bifurcated C# ecosystem—those that adopt the new features and those that won’t, and worse still: the risk of losing the identity of what C# really is.

I’m interested to see how it works out. Introducing things [like records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records) into C# is going to lead to some new and different design patterns being used that will have to naturally evolve over time.

### I won't ask if C# will replace F#—you've eloquently written about why the answer is no. I will ask you this, though: is there a dividing line of when you should use C# (OO with functional concepts) or straight to F#?

I’m not really sure the idea of “OO with functional concepts” really gels, to be honest. Some of the core ideas of FP—immutability and expressions—are kind of the opposite of OO, which is all centered around mutable data structures, statements and side effects. By all means: use the features C# provides that come from the FP world and use them where it helps—LINQ, higher order functions, pattern matching, immutable data structures—but the more you try out those features to try what they can do without using OO constructs, the more you’ll find C# pulls you “back." It’s a little like driving an Audi on the German motorway but never getting out of third gear.

My view is that 80% of the C# population today—maybe more—would be more productive and happier in F#. If you’re using LINQ, you favour composition over inheritance, and you’re excited by some of the new features in C# like records, switch expressions, tuples, and so on, F# will probably be a natural fit for you. All of those features are optimised as first-class citizens of the language, whilst things like mutability and classes are possible, but are somewhat atypical.

This also feeds back to your other question—I do fear that people will try these features out within the context of OO patterns, find them somehow limited, and leave thinking that FP isn’t worthwhile.

### Let's say I'm a C# programmer and want to get into F#. Is there any C# knowledge that will help me understand the concepts, or is it best to clear my mind of any preconceived notions before learning?

Probably the closest concept would be to imagine your whole program was a single LINQ query. Or, from a web app—imagine every controller method was a LINQ query. In reality it’s not like that, but that’s the closest I can think of. The fact that you’ll know .NET inside and out is also a massive help. The things to forget are basically the OO and imperative parts of the language: classes, inheritance, mutable variables, while loops, and statements. You don’t really use any of those in everyday F# (and believe me, you don’t need any of them to write standard line of business apps).

### As an OO programmer, it's so painful always having to worry about "the billion dollar mistake": nulls. We can't assume anything since we're mutating objects all over the place and often throw up our hands and do null checks everywhere (although the language has improved in the last few versions). How does F# handle nulls? Is it less painful?

For F# types that you create, the language simply says: null isn’t allowed, and there’s no such thing as null. So in a sense, the problem goes away by simply removing it from the type system. Of course, you still have to handle business cases of “absence of a value,” so you create optional values—basically a value that can either have something or nothing. The compiler won’t let you access the “something” unless you first “check” that the value isn’t nothing. 

So, you spend more time upfront thinking about how you model your domain rather than simply saying that everything and anything is nullable. The good thing is, you totally lose that fear of “can this value be null when I dot into it” because it’s simply not part of the type system. It’s kind of like the flow analysis that C# 8 introduced for nullability checks—but instead of flow analysis, it’s much simpler. It’s just a built-in type in the language. There’s nothing magical about it.

However, when it comes to interoperating with C# (and therefore the whole BCL), F# doesn’t have any special compiler support for null checks, so developers will often create a kind of “anti-corruption” layer between the “unsafe outside world” and the safe F# layer, which simply doesn’t have nulls. There’s also work going on to bring in support for the nullability surface in the BCL but I suspect that this will be in F# 6.

### F#, and functional programming in general, emphasizes purity: no side effects. Does F# enforce this, or is it just designed with it in mind?

No, it doesn’t enforce it. There’s some parts of the language which make it obvious when you’re doing a side effect, but it’s nothing like what Haskell does. For starters, the CLR and BCL don’t have any notion of a side effect, so I think that this would difficult to introduce. It’s a good example of some of the design decisions that F# took when running on .NET—you get all the goodness of .NET and the ecosystem, but some things like this would be challenging to do. In fact, F# has a lot of escape hatches like this. It strongly guides you down a certain path, but it usually has ways that you can do your own thing if you really need to.

You still can (and people do) write entire systems that are functionally pure, and the benefits of pure functions are certainly something that most F# folks are aware of (it's much easier to reason about and test, for example). It just means that the language won’t force you to do it.

### What is your one piece of programming advice?

Great question. I think one thing I try to keep in mind is to avoid premature optimisation and design. Design systems for what you know is going to be needed, with extension points for what will most likely be required. You can never design for every eventuality, and you’ll sometimes get it wrong, that’s life—optimise for what is the most likely outcome.

*To read the entire interview, [head on over to my site](https://daveabrock.com/2020/09/19/dev-discussions-isaac-abraham).*

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* .NET 5 RC 1 is out: Richard Lander [has the announcement](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-rc-1), Jeremy Likness [talks about EF updates](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-efcore-5-0-rc1/), and Daniel Roth [discusses what's new for ASP.NET](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/).
* Immo Landwerth [speaks about the future of .NET Standard](https://devblogs.microsoft.com/dotnet/the-future-of-net-standard).
* Steve Gordon [walks through performance optimizations](https://www.stevejgordon.co.uk/dotnet-performance-optimisations-dont-have-to-be-complex).

### 📢 Announcements

* There's a new Learn module [for deploying a cloud-native ASP.NET microservice with GitHub Actions](https://docs.microsoft.com/learn/modules/microservices-devops-aspnet-core/).
* Mark Downie [talks about disassembly improvements for optimized managed debugging](https://devblogs.microsoft.com/visualstudio/disassembly-improvements-for-optimized-debugging/).
* Microsoft Edge [announces source order viewer in their DevTools](https://blogs.windows.com/msedgedev/2020/09/15/source-order-viewer-edge-devtools).
* Tara Overfield [provides September cumulative updates for the .NET Framework](https://devblogs.microsoft.com/dotnet/net-framework-september-2020-cumulative-update-preview-update).

### 📅 Community and events

* Microsoft Ignite [occurs this Tuesday through Thursday](https://myignite.microsoft.com/home).
* The .NET Docs Show [talks about the *dot.net* site with Maíra Wenzel](https://www.youtube.com/watch?v=nFROvRwDCmk).
* Three .NET community standups this week: .NET Tooling [finds latent bugs in .NET 5](https://www.youtube.com/watch?v=oTavmpy78Q0), Entity Framework [talks EF Core 5 migrations](https://www.youtube.com/watch?v=mSsGERmrhnE), and ASP.NET [discusses new features for .NET API developers](https://www.youtube.com/watch?v=-5B2lx1pelE).

### 😎 ASP .NET / Blazor

* Shaun Curtis [launches a series on building a database application in Blazor](https://www.codeproject.com/Articles/5279560/Building-a-Database-Application-in-Blazor-Part-1-P).
* Patrick Smacchia [walks through the architecture of a C# game rendered with Blazor, Xamarin, UWP, WPF, and Winforms](https://blog.ndepend.com/architecture-of-a-c-game-rendered-with-blazor-xamarin-uwp-wpf-and-winforms/).
* David Ramel [writes about increased Blazor performance in .NET 5 RC1](https://visualstudiomagazine.com/articles/2020/09/14/aspnet-5-rc1.aspx).
* Rick Strahl [warns about missing await calls for async code in ASP.NET Code middleware](https://weblog.west-wind.com/posts/2020/Sep/14/Dont-get-burned-by-missing-await-Calls-for-Async-Code-in-ASPNET-Core-Middleware).
* Dominique St-Amand [secures an ASP.NET Core Web API with an API key](https://www.domstamand.com/securing-asp-net-core-webapi-with-an-api-key/).
* Vladimir Pecanac [discusses how to secure sensitive data locally with ASP.NET Core](https://code-maze.com/aspnet-configuration-securing-sensitive-data/).
* David Grace [explores why you app might not be working in IIS](https://www.roundthecode.com/dotnet/four-reasons-why-your-asp-net-core-application-is-not-working-in-iis).

### 🚀 .NET Core

* Kay Ewbank [discusses the latent bug discovery feature coming with .NET 5](https://www.i-programmer.info/news/89-net/13990-net-adds-latent-bug-discovery-feature.html).
* Michał Białecki [executes raw SQL with EF 5](http://www.michalbialecki.com/2020/09/14/executing-raw-sql-with-entity-framework-core-5/).
* Fredrik Rudberg [serves images stored in a database through static URLs using .NET Core 3.1](https://www.codeproject.com/Articles/5278879/Serving-images-stored-in-a-Database-through-a-stat).
* Shawn Wildermuth [talks about hosting Vue in .NET Core](http://wildermuth.com/2020/09/13/Hosting-Vue-in-ASP-NET-Core-A-Different-Take).

### ⛅ The cloud

* Vladimir Pecanac [configures the Azure Key Vault in ASP.NET Core](https://code-maze.com/aspnet-configuration-azure-key-vault/).
* Richard Seroter [compares the CLI experience between Azure, AWS, and GCP](https://seroter.com/2020/09/15/lets-compare-the-cli-experiences-offered-by-aws-microsoft-azure-and-google-cloud-platform/).
* Jon Gallant [walks though the September updates to the Azure SDKs](https://devblogs.microsoft.com/azure-sdk/azure-sdk-release-september-2020).
* Christopher Scott [introduces the new Azure Tables client libraries](https://devblogs.microsoft.com/azure-sdk/azure-tables-client-libraries).
* Daniel Krzyczkowski [extracts Excel file content with Azure Logic Apps and Azure Functions](https://daniel-krzyczkowski.github.io/Extract-Excel-file-content-with-Azure-Logic-Apps-and-Azure-Functions/).
* Kevin Griffin [touts the great performance for Azure Static Web Apps and Azure Functions](https://consultwithgriff.com/crazy-web-performance-azure-static-web-apps-and-functions/).
* Matt Small [finds a gotcha: you can't use an Azure Key Vault firewall if you're in a situation where you're using App Gateway along with a Key Vault certificate for SSL termination](https://azidentity.azurewebsites.net/post/2020/09/14/key-vault-app-gateway-and-the-kv-firewall).
* Gunnar Peipman [hosts applications on Azure B-series virtual machines](https://gunnarpeipman.com/azure-b-series-virtual-machines/).

### 📔 C#

* Jeremy Clark [shows how to see all the exceptions when calling "await Task.WhenAll."](https://jeremybytes.blogspot.com/2020/09/await-taskwhenall-shows-one-exception.html).
* Jerome Laban [uses MSBuild items and properties in C# 9 source generators](https://jaylee.org/archive/2020/09/13/msbuild-items-and-properties-in-csharp9-sourcegenerators.html).

### 📗 F#

* A nice rundown of [10 ways to try F# in the browser](https://fsharp.org/use/browser/).
* Daniel Bykat talks about [the PORK framework and its use with F#](https://medium.com/rocket-mortgage-technology-blog/pork-a-technology-resilience-framework-745207bd28d5).
* Alican Demirtas [discusses string interpolation in F#](https://www.compositional-it.com/news-blog/string-interpolation-in-f/).
* Paul Biggar [talks about his async adventures](https://blog.darklang.com/adventures-in-async/).

### 🔧 Tools

* Derek Comartin [does a review of MediatR](https://codeopinion.com/why-use-mediatr-3-reasons-why-and-1-reason-not/).
* Tom Deseyn [uses OpenAPI with .NET Core](https://developers.redhat.com/blog/2020/09/16/using-openapi-with-net-core/).
* John Juback [builds cross-platform desktop apps with Electron.NET](https://www.codeproject.com/Articles/5278697/Building-Cross-Platform-Desktop-Apps-with-Electron).
* Andrew Lock [continues his k8s series by deploying applications with Helm](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-3-deploying-applications-with-helm/).
* You can now [debug Linux core dumps on the Windows Subsystem for Linux (WSL) or a remote Linux system directly from Visual Studio](https://devblogs.microsoft.com/cppblog/debug-linux-core-dumps-in-visual-studio).
* Adam Storr [uses Project Tye to run .NET worker services](https://adamstorr.azurewebsites.net/blog/using-project-tye-to-run-dotnet-worker-services).

### 📱 Xamarin

* Joe Meyer [wires up a fullscreen video background](https://iwritecodesometimes.net/2020/09/16/fullscreen-video-background-in-xamarin-forms/).
* Khalid Abuhakmeh [animates a mic drop](https://khalidabuhakmeh.com/animate-a-mic-drop-with-xamarin-dot-forms).
* Denys Fiediaiev [uses MvvmCross to log with Xamarin](https://medium.com/@prin53/logging-in-xamarin-application-logging-infrastructure-with-mvvmcross-2c9bef960c60).

### 🎤 Podcasts

* The .NET Rocks podcast [talks about ML with Zoiner Tejada](https://www.dotnetrocks.com/default.aspx?ShowNum=1705).
* Software Engineering Radio talks with [Philip Kiely about writing for software developers](https://www.se-radio.net/2020/09/episode-426-philip-kiely-on-writing-for-software-developers/).
* The Merge Conflict podcast [discusses the new Half type](https://www.mergeconflict.fm/219).
* The Coding Blocks podcast asks: [is Kubernetes programming?](https://www.codingblocks.net/podcast/is-kubernetes-programming/)
* The Azure DevOps Podcast [talks with Steve Sanderson about Blazor](http://azuredevopspodcast.clear-measure.com/steve-sanderson-on-blazor-episode-106).

### 🎥 Videos

* The ON .NET Show [talks about Steeltoe configuration](https://channel9.msdn.com/Shows/On-NET/Steeltoe-External-Configuration-with-Spring).
* Azure Friday [talks about Azure landing zones](https://channel9.msdn.com/Shows/Azure-Friday/Prepare-your-cloud-environments-using-Azure-landing-zones).
* Scott Hanselman [gives us a primer on the cloud](https://www.youtube.com/watch?v=BO6jvQ88ICQ).
* The ASP.NET Monsters [send dates from JavaScript to .NET](https://aspnetmonsters.com/2020/09/monsters-weekly/ep182/).

