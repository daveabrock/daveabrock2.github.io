---
date: "2020-09-12"
title: "The .NET Stacks #16: App trimming and more on ML.NET"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-16-card.png
subtitle: We discuss app trimming in .NET 5, more on ML.NET with Luis Quintanilla, and more!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Welcome to 2020, where [Thomas Running is finally getting the credit he deserves](https://twitter.com/codinghorror/status/1301970691135160321).

With the .NET 5 last preview hitting last week, you'd think we wouldn't have much to talk about! Oh, no—there is *always* something to talk about.

This week:

* App trimming in .NET 5
* More with Luis Quintanilla on ML.NET
* Community roundup

## App trimming in .NET 5

With .NET 5 preview 8 shipped last week, Microsoft has been pushing a lot of articles about the performance improvements. This week was no exception as Sam Spencer [discussed how app trimming will work in .NET 5](https://devblogs.microsoft.com/dotnet/app-trimming-in-net-5/). It's not as sexy as Blazor, but crucially important.

A huge driver for .NET Core over .NET Framework, among several other things, is self-contained deployments. There's no dependency on a framework installation, so setup is easier but the size of the apps is much larger. With .NET Core 3, Microsoft introduced [app trimming, or assembly linking, that optimizes your deployment size](https://docs.microsoft.com/dotnet/core/deploying/trim-self-contained). Long story short, it only packages assemblies that are used.

That's great if you forget to remove dependencies you're no longer using, but the real value comes in opening those assemblies. That's what's coming with .NET 5: they've expanded app trimming to remove types and members unused by your application as well. This seems both exciting and scary: it's quite risky with extensive testing required and as a result, Spencer notes it's an experimental feature not ready for large adoption yet. With that in mind, the default trimming in .NET is assembly-level, but you can use a `<TrimMode>Link</TrimMode>` setting in your project file to enable member-level trimming.

This is not setting and forgetting, though: it only does a static analysis of your code and, as you likely know, .NET Core depends heavily on dynamic code patterns like reflection—especially for dependency injection and object serialization. Because the trimmer can't discover types at runtime that leverage these patterns, it could lead to disastrous results. What if you dynamically discover an interface at runtime and the analyzer doesn't find it, then the types are trimmed out? Instead of trying to resolve all your problems for you, which will never be a perfect process, the approach in .NET 5 is to both provide feedback to you if the trimmer isn't sure about something, and also annotations for you to use that will help the trimmer—especially when dealing with these dynamic behaviors.

Speaking of reflection, do you remember [when we talked about source generators last week](https://daveabrock.com/2020/09/05/dotnet-stacks-15)? The .NET team is looking at implementing source generators to move functionality from reflection at runtime to build-time. This speeds up performance but also allows the trimmer to better analyze your code—and with a higher level of accuracy.

A big winner here is Blazor—with the release in May, Blazor now utilizes .NET 5 instead of Mono (and with it, it's increased size). Blazor WebAssembly is still a little heavy and this will go a long way to making the size more manageable.

Until native AOT comes to .NET—[and boy, we're impatiently waiting](https://visualstudiomagazine.com/articles/2020/08/31/aot-survey.aspx)—this will hopefully clear the path for its success. I'm cautiously optimistic. I know app trimming has been a bumpy road this far, but the annotations might provide for a better experience—and allow us to confidently trim our apps without sacrificing reliability.

Take a look [at the introductory article](https://devblogs.microsoft.com/dotnet/app-trimming-in-net-5/) as well as [a deep dive on customization](https://devblogs.microsoft.com/dotnet/customizing-trimming-in-net-core-5/).

## Dev Discussions: More with Luis Quintanilla

Last week, we began a conversation with Luis Quintanilla about ML.NET. This week, we discussing when to use ML.NET over something like Azure Cognitive Services, practical uses, and more.

![Luis Quintanilla]({{ site.url }}{{ site.baseurl }}/assets/img/luisquintanilla-picture.jpg)

### Where is the dividing line for when I should use machine learning, or use Azure Cognitive Services?

This is a really tough one to answer because there's so many ways you can make the comparison. Both are great products and in some areas, the lines can blur. Here are a few of them that I think might be helpful.

#### Custom Training vs Consumption

If you're looking to add machine learning into your application to solve a fairly generic problem, such as language translation or identifying popular landmarks, Azure Cognitive Services is an excellent option. The only knowledge you need to have is how to call an API over HTTP.

Azure Cognitive Services provides a set of robust, state-of-the-art, pretrained models for a wide variety of scenarios. However, there’s always edge cases. Suppose you have a set of documents that you want to classify and the terminology in your industry is rare or niche. In that scenario, the performance of Azure Cognitive Services may vary because the pretrained models most likely have never encountered some of your industry terms. At that point, training your own model may be a better option, which for this particular scenario, Azure Cognitive Services does not allow you to do.

That's not to say Azure Cognitive Services does not allow you to train custom models. Some scenarios like language understanding and image classification have training capabilities. The difference is, training custom models is not the default for Azure Cognitive Services. Instead, you are encouraged to consume the pretrained models. Conversely, with ML.NET, for training purposes, you're always training custom models using your data.

#### Online vs Offline

By default, Azure Cognitive Services requires some form of internet connectivity. In scenarios where there is strong network connectivity, this is not a problem. However, for offline or air-gapped environments, this is not an option. Although in some scenarios, you can deploy your Azure Cognitive Services model as a container, therefore reducing the number of requests made over the network, you still need some form of internet connectivity for billing purposes. Additionally, not all scenarios support container deployments therefore the types of models you can deploy is limited.

While Azure in general makes sure to responsibly protect the privacy and security of users data in the cloud, in some cases whether it's a regulatory or organizational decision, putting your data in the cloud may not be an option. In those cases, you can leverage Azure Cognitive Services container deployments. Like with the offline scenario, the types of models you can deploy is limited. Additionally, you most likely would not be training custom models since you wouldn't want to send your data to the cloud.

ML.NET is offline first, which means you can train and deploy your models locally without ever interacting with a cloud environment. That being said, you always have the option to scale your training and consumption by provisioning cloud resources. Another benefit of being offline first is, you don't have to containerize your application in order to run it locally. You can take your model and deploy it as part of a WPF application without the additional overhead of a container or connecting over to the internet.

### Do you have some practical use cases for using ML.NET in business apps today?

Definitely! If you have a machine learning problem, the framework you use is fairly agnostic as long as the scenario is supported. Since ML.NET supports classical machine learning as well as deep learning scenarios, it practically can help solve a wide variety of problems. Some examples include:

* Sentiment analysis
* Sales forecasting
* Image classification
* Spam detection
* Predictive maintenance
* Fraud detection
* Web ranking
* Product recommendations
* Document classification
* Demand prediction
* And many others...
  
I would suggest for users interested in seeing how companies are using ML.NET in production today, visit the [ML.NET customer showcase](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet/customers).

### What kind of stuff do you work on over at your Twitch stream? When is it?

Currently I stream on Monday and Wednesday mornings at 10 AM EST. My stream is a live learning session where folks sometimes drop in to learn together. On stream, I focus on building data and machine learning solutions using .NET and other technologies. The language of choice on stream is F# though I don’t strictly abide by that and will use what works best for getting the solution working.

Most recently, I built a deep learning model for malware detection using ML.NET. I've been trying to build .NET pipelines with Kubeflow, a machine learning framework for Kubernetes on stream as well. I've had some trouble with that, but that's what the stream is about, learning from mistakes.

Inspired by a reddit post which asked how to get started with data analytics in F#, I’ve started working on using F# and various .NET tools like .NET Interactive to analyze the arXiv dataset.

If any of that sounds remotely interesting, feel free to check out and follow the channel on [Twitch](https://www.twitch.tv/lqdev1). You can also catch the recordings from previous streams on [YouTube](https://www.youtube.com/channel/UCkA5fHdQ4cf3D1J19UNgV7A).

### What is your one piece of programming advice?

Just do it! Everyone has different learning styles, but I strongly believe no amount of videos, books or blog posts compare to actually getting your hands dirty. It can definitely be daunting at first, but no matter how small or basic the application is, building things is always a good learning experience. Often there is a lot of work that goes into producing content, so end users typically get the polished product and the happy path. When you're not sure of where to start or would like to go more in depth, these resources are excellent. However, once you stray from that guided environment, you start making mistakes. Embrace these mistakes because they’re a learning experience.

*[Check out the full interview with Luis at my site](https://daveabrock.com/2020/09/05/dev-discussions-luis-quintanilla-2).*

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Sam Spencer introduces [app trimming in .NET 5](https://devblogs.microsoft.com/dotnet/app-trimming-in-net-5/), and then [writes about customizing it](https://devblogs.microsoft.com/dotnet/customizing-trimming-in-net-core-5).
* Andrew Lock [starts a series about deploying ASP.NET Core apps to Kubernetes](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-1-an-introduction-to-kubernetes/).
* Thomas Claudius Huber [explores records in C# 9](https://www.thomasclaudiushuber.com/2020/09/01/c-9-0-records-work-with-immutable-data-classes/).

### 📢 Announcements

* GitHub has [launched a new container registry](https://github.blog/2020-09-01-introducing-github-container-registry/), and Chad Metcalf [talks about Docker support for it](https://www.docker.com/blog/docker-support-for-the-new-github-container-registry/).
* Kunal Pathak [talks about ARM64 performance in .NET 5](https://devblogs.microsoft.com/dotnet/arm64-performance-in-net-5/).
* Elinor Fung [shows off improvements in native code interop in .NET 5](https://devblogs.microsoft.com/dotnet/improvements-in-native-code-interop-in-net-5-0/).
* Tim Heuer [announces how you can use the .NET CLI templates in Visual Studio now](https://devblogs.microsoft.com/dotnet/net-cli-templates-in-visual-studio/).
* The Azure Cache for Redis [Visual Studio Code extension is now in preview](https://azure.microsoft.com/updates/azure-cache-for-redis-visual-studio-code-extension-now-in-preview-2/).
* Jeremy Likness [wants your feedback on .NET for Apache Spark](https://devblogs.microsoft.com/dotnet/big-plans-for-big-data-and-net-for-spark/).

### 📅 Community and events

* The .NET Docs Show [talks with Matt Millican about front-ends with ASP.NET Core and Vue.js](https://www.youtube.com/watch?v=1nC0YXuu_yI).
* We had three community standups this week: Xamarin [talks XAML tooling](https://www.youtube.com/watch?v=L41j_HY65vE), Entity Framework talks about [syncing your database with DotMim.Sync](https://www.youtube.com/watch?v=_SHryJiblRo), and ASP.NET discusses the [YARP reverse proxy toolkit](https://www.youtube.com/watch?v=WpslfYHKwDc).

### 😎 Blazor

* Patrick Smacchia [writes about the Blazor internals you need to know](https://blog.ndepend.com/blazor-internals-you-need-to-know/).
* Peter Vogel [dynamically selects and filters reports with Blazor and the Telerik ReportViewer](https://www.telerik.com/blogs/dynamically-selecting-filtering-reports-blazor-telerik-reportviewer).
* Marco Dalla Libera [walks through 10 steps to replace REST services with gRPC-Web in Blazor WebAssembly](https://www.syncfusion.com/blogs/post/10-steps-to-replace-rest-services-with-grpc-web-in-blazor-webassembly.aspx).
* David Grace [discusses why Blazor WASM is the best choice for API integration](https://www.roundthecode.com/dotnet/blazor/why-blazor-wasm-is-the-best-choice-for-api-integration).
* Matthew Jones [kicks off a series playing Yahtzee in Blazor WebAssembly](https://exceptionnotfound.net/yahtzee-in-blazor-webassembly-part-1-the-csharp-model/), and then [writes his Blazor component for it](https://exceptionnotfound.net/yahtzee-in-blazor-webassembly-part-2-the-blazor-component).
* David Jones [wraps up his Blazor series](https://davidjones.sportronics.com.au/blazor/Blazor_How_To-And_now_for_a_Rap_Up-blazor.html).

### 🚀 .NET Core

* The Windows Developer team [shows you how to call Windows APIs from .NET 5](https://blogs.windows.com/windowsdeveloper/2020/09/03/calling-windows-apis-in-net5).
* Damir Arh [gives an overview of what you need to know about .NET 5](https://www.dotnetcurry.com/dotnetcore/dotnet-5-new-features).
* Michał Białecki [selects data with a stored procedure with EF Core 5](http://www.michalbialecki.com/2020/09/03/select-data-with-a-stored-procedure-with-entity-framework-core-5).
* Vladimir Pecanac [goes over ASP.NET Core configuration concepts](https://code-maze.com/aspnet-configuration-basic-concepts/), [works through dependency injection in ASP.NET Core](https://code-maze.com/dependency-injection-aspnet/), and [writes about the Options pattern in ASP.NET Core](https://code-maze.com/aspnet-configuration-options/).
* Jason Robert [compares ASP.NET Core Razor Pages with traditional MVC](https://espressocoder.com/2020/09/01/comparing-asp-net-core-razor-pages-with-mvc/).
* David Ramel [responds to a survey noting that lack of ahead-of-time compilation is a sore spot for .NET devs](https://visualstudiomagazine.com/articles/2020/08/31/aot-survey.aspx).
* Andrew Robilliard [deploys .NET Core to Heroku](https://dev.to/alrobilliard/deploying-net-core-to-heroku-1lfe).
* Damien Bowden [uses digital signatures to check the integrity of cipher texts in ASP.NET Core Razor Pages](https://damienbod.com/2020/09/01/using-digital-signatures-to-check-integrity-of-cipher-texts-in-asp-net-core-razor-pages/).
* Anup Hosur [works with Serilog in ASP.NET Core 3.1](https://www.c-sharpcorner.com/article/serilog-in-asp-net-core-3-1/).
* Thomas Levesque [injects a service into a System.Text.Json converter](https://thomaslevesque.com/2020/08/31/inject-service-into-system-text-json-converter/).
* Paul Michaels [changes the default ASP.NET Core layout to use feature folders](https://www.pmichaels.net/2020/08/30/change-the-default-asp-net-core-layout-to-use-feature-folders).

### ⛅ The cloud

* Gunnar Peipman [accesses restricted blob storage from virtual networks and the Azure CDN](https://gunnarpeipman.com/access-restricted-blob-storage-from-azure-cdn/).
* Damien Bowden [secures Azure Functions using certificate authentication](https://damienbod.com/2020/09/04/securing-azure-functions-using-certificate-authentication/).
* Franco Tiveron [discusses the Azure CLI](https://developer.okta.com/blog/2020/09/02/10x-development-azure-cli-dotnet).
* JoeG has been busy with Microsoft Identity: [he assigns a role](https://www.josephguadagno.net/2020/08/29/working-with-microsoft-identity-assigning-a-role), [configures local development](https://www.josephguadagno.net/2020/08/29/working-with-microsoft-identity-configure-local-development), and  [registers an app](https://www.josephguadagno.net/2020/08/29/working-with-microsoft-identity-registering-an-application).
* Tim Mitchell [walks through creating your first Azure Data Factory](https://www.timmitchell.net/post/2020/08/28/creating-your-first-azure-data-factory/).

### 📔 Languages

* There's a new type coming, and [you don't know the Half of it](https://devblogs.microsoft.com/dotnet/introducing-the-half-type/).
* Khalid Abuhakmeh [removes rows from Razor HTML tables](https://khalidabuhakmeh.com/remove-rows-from-razor-html-tables) and also [cleans some things up with TempDataAttribute](https://khalidabuhakmeh.com/use-tempdataattribute-for-clean-code).
* Dave Brock (ahem) [talks about how not to hate JavaScript](https://daveabrock.com/2020/09/02/how-to-not-hate-javascript).
* Gunnar Peipman [discusses top-level programs in C# 9](https://gunnarpeipman.com/csharp-top-level-programs/) and also [translates NHibernate LINQ queries to SQL](https://gunnarpeipman.com/nhibernate-linq-tosql/).
* Martin Cerruti [introduces the new features in C# 9](https://medium.com/swlh/an-introduction-to-the-new-features-in-c-9-305dc8fb74d2).

### 🔧 Tools

* Jon Hilton [summarizes the Edit and Replay feature on the new Edge](https://jonhilton.net/debuggable-network-requests/).
* Medi Madelen Gwosdz asks: [if everyone hates OOP, why is it so widely spread](https://stackoverflow.blog/2020/09/02/if-everyone-hates-it-why-is-oop-still-so-widely-spread/)?
* David Ramel [discusses Mobilize.Net, which automates converting Visual Basic apps to .NET Core](https://visualstudiomagazine.com/articles/2020/08/28/vb-to-core.aspx).
* Scott Hanselman [writes about the Coravel library](https://www.hanselman.com/blog/ExploringTheNETCoreLibraryCoravelForTaskSchedulingCachingMailingAndMore.aspx).

### 📱 Xamarin

* Vicente Gerardo Guzmán Lucio [writes about new features in Xamarin.Forms 4.8](https://www.syncfusion.com/blogs/post/new-features-in-xamarin-forms-4-8-gradients-brushes-and-flyout-backdrop-color.aspx).
* Joe Meyer [runs through the multi-column UIPickerView](https://iwritecodesometimes.net/2020/09/03/multi-column-uipickerview-in-xamarin-forms/).
* Jean-Marie Alfonsi [works through shadows for Xamarin.Forms components creators](https://www.sharpnado.com/shadows-for-creators/).
* John Thiriet [uses Huawei mobile services ](https://johnthiriet.com/using-huawei-mobile-services-with-xamarin/).
* Nick Randolph [discusses MVVM navigation for XAML frameworks, like Xamarin and Uno](https://nicksnettravels.builttoroam.com/viewmodel-navigation).

### 🎤 Podcasts

* At Yet Another Podcast, [a discussion on .NET MAUI](http://jesseliberty.com/2020/09/03/net-maui).
* The Azure DevOps Podcast [talks with Derek Comartin on migrating to .NET Core](http://azuredevopspodcast.clear-measure.com/derek-comartin-on-migrating-to-net-core-episode-104).
* At the No Dogma podcast, [Mads Torgersen takes some C# 9 questions](https://no-dogma-podcast-301aeb97.simplecast.com/episodes/146-mads-torgersen-c-9-part-2-listener-questions-lCGKSL5_).
* The .NET Rocks podcast [talks about Microsoft 365 APIs with Glenn Block](https://www.dotnetrocks.com/default.aspx?ShowNum=1703).
* The Merge Conflict podcast [stands up a custom identity service on ASP.NET Core](https://www.mergeconflict.fm/217).
* The Coding Blocks podcast [discusses enabling safe deployments](https://www.codingblocks.net/podcast/the-devops-handbook-enabling-safe-deployments/).

### 🎥 Videos

* Azure Friday [discusses serverless containers with Azure Container Instances (ACI)](https://channel9.msdn.com/Shows/Azure-Friday/Serverless-containers-with-Azure-Container-Instances-ACI).
* Jeff Fritz [works on C# conditionals and loops for beginners](https://www.youtube.com/watch?v=3PSgjUX3maE).
* Data Exposed [deploys SQL Server 2019 to Kubernetes](https://channel9.msdn.com/Shows/Data-Exposed/Deploying-SQL-Server-2019-in-Kubernetes).
* The Maintainers [talks to Nicholas Blumhardt about Autofac and Serilog](http://wildermuth.com/2020/08/31/The-Maintainers-Nicholas-Blumhardt-Autofac-and-Serilog).
* The ASP.NET Monsters [do SQL profiling with Azure Data Studio](https://www.youtube.com/watch?v=pM6NHhDNJw0).
* The ON.NET Show [discusses how to use Wordpress on .NET Core](https://channel9.msdn.com/Shows/On-NET/Wordpress-on-NET-Core).
