---
date: "2020-09-05"
title: "The .NET Stacks #15: The final preview and ML.NET with Luis Quintanilla"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-15-card.png
subtitle: We discuss the final .NET preview, talk about ML.NET with Luis Quintanilla, and more!
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Welcome to the start of another week—I hope you are well and safe. We had an insanely busy week in the .NET world, so let's get to it.

## It's the final ... preview

Are you ready for .NET 5? This week, Microsoft [announced the release](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-8/) of .NET 5 Preview 8 (and also [ASP.NET Core](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/) and [EF 5](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-ef-core-5-0-preview-8/) updates). This means that .NET 5 is more-or-less feature complete with the exception of bug fixes. Up next, we have two go-live release candidates, and then the official release in November.

You'll want to hit up the links ([and GitHub](https://github.com/dotnet/runtime/issues/37269)) for all the gory details, but here's a quick recap of what's coming in .NET 5 from a high level:

* [Single file applications](https://github.com/dotnet/runtime/issues/36590)
* [Increased performance](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5/)
* [Nullable reference type annotations](https://twitter.com/terrajobst/status/1296566363880742917)
* [Windows ARM64 support](https://github.com/dotnet/runtime/issues/36699)
* [Web Assembly support](https://webassembly.org/)
* [JsonSerializer API improvements](https://github.com/dotnet/runtime/issues/41313)
* [C# 9](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) and [F# 5](https://devblogs.microsoft.com/dotnet/f-5-and-f-tools-update-for-june/)

We [discussed this a few weeks back](https://daveabrock.com/2020/08/22/dotnet-stacks-13), but for the preview itself, the ASP.NET Core side is jam-packed with Blazor improvements and capabilities. There's CSS isolation, lazy loading, UI focus, and more. From the EF 5 side, there's table-per-type mapping, table-valued functions, and more—and, [as mentioned last week](https://daveabrock.com/2020/08/29/dotnet-stacks-14), many-to-many is now in the daily builds.

## Speed up runtime with C# source generators

While not directly linked to C# 9, I'm getting excited about C# source generators (I [wrote a "first look" post back in May](https://daveabrock.com/2020/05/08/first-look-c-sharp-generators)).

These generators are basically code that runs during compilation and can produce additional files that are compiled together with the rest of your code. It's a complication step that generates code *for you* based on your code.

Here's a big benefit that I wrote about:

>If you’ve ever leaned on reflection in your projects, you might begin to see many use cases for these solutions—C# source generators provide a lot of advantages that reflection currently offers and few, if any, drawbacks. Reflection is extremely powerful when you want to query properties and attributes you don’t know about when you typically compile. Of course, getting type information at runtime can incur a large performance cost, so offloading this to compilation is definitely a game-changer in C#.

This week, Microsoft [announced some new C# source generator samples](https://devblogs.microsoft.com/dotnet/new-c-source-generator-samples/). You can also check out the [design document](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.md) and a [source generators cookbook](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.cookbook.md). If you've tried it, let me know your thoughts!

## Dev Discussions: Luis Quintanilla

Machine learning is a fascinating world and, to many, a complicated one. As .NET developers, we definitely see the benefit in training our data but between the learning curve and using other languages like Python for machine learning—a language .NET devs might not be familiar with—ML is often sent to a developer's "I should look into that sometime" queue.

That changed in 2018, when Microsoft launched ML.NET—a free, open source, x-plat machine learning framework for .NET. With ML.NET, you can use your favorite languages like C# or F# to work with your custom machine learning models. The idea is to meet you where you are and make ML more accessible.

There's no one better to talk to about this than Luis Quintanilla. Luis has been with ML.NET since the beginning and was eventually scooped up by Microsoft to work on the docs for ML.NET.

![Luis Quintanilla]({{ site.url }}{{ site.baseurl }}/assets/img/luisquintanilla-picture.jpg)

### What made you focus on ML.NET over other development tech?

I could write an entire essay on why ML.NET but all of the reasons can be summarized in a single word, .NET. Now, to expand on that, here are a few reasons why I enjoy ML.NET so much!

#### Languages

Though not unique to .NET, I like statically-typed languages. I'm sure many of the readers are able to build their applications and successfully run them without errors on the first try 😉. That, however, is usually not my experience. Therefore, I prefer catching as many errors as possible at compile time. Another reason I like types is they provide a way of documenting your code.

This is of extreme importance when working in data science and machine learning scenarios. Although ultimately the data used by machine learning algorithms to train models is encoded as numbers, knowing your data schema and checking it at compile time may help reduce the number of errors in your code as you transform your data.

Lately I've been doing more F# development and the more I use it, the more I like it. F# for me provides a nice balance between Python and C#. F# gives you the productivity and succinctness of a language like Python, while still having the compiler and many other neat features at your disposal.

#### Runtime

The .NET runtime is fast and performant. This is important in two scenarios, training machine learning models and deploying them. A good part of training machine learning models involves performing operations on vectors and matrices. .NET provides Single Instruction Multiple Data (SIMD) enabled types via the [System.Numerics](https://docs.microsoft.com//dotnet/standard/simd)  namespace. ML.NET leverages these types where possible to increase the throughput of the training operations making training fast and efficient.

#### Tooling

.NET has world class tooling across the board and you can't go wrong with any of your choices. [Visual Studio](https://visualstudio.microsoft.com/) is an excellent IDE packed with tons of functionality to help developers be more productive. Alternatively, another great IDE for .NET is [Jetbrains Rider](https://www.jetbrains.com/rider/). If you're looking for a more lightweight development environment, you can also use [Visual Studio Code](https://code.visualstudio.com/). When working with F#, you can use the [Ionide](http://ionide.io/) extension which makes F# development a pleasant experience.

Data science and machine learning workflows are very experimental. This means that you sometimes may want to have an interactive environment where you get feedback in near real-time of the outputs generated by your code. You’d also like a way to visualize your data inline. Within the data science community, a popular interactive computing environment is Jupyter Notebooks. You can leverage this interactive environment in .NET through .NET Interactive, which provides among many things, a kernel for you to run .NET code interactively.

#### Extensible

Although .NET is great, a large portion of the data science and machine learning space is predominantly made up of libraries and frameworks built in Python. That however does not limit 
ML.NET because it is extensible. ML.NET supports working with TensorFlow and Open Neural Network Exchange (ONNX) models. [TensorFlow](https://www.tensorflow.org/) is a popular platform for building machine learning models. Using [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), a set of C# bindings for TensorFlow, users can train and deploy TensorFlow models with ML.NET. [ONNX](https://onnx.ai/) is an open format built to represent machine learning models. This means that you can train a model in other popular tools and frameworks like Azure Custom Vision, PyTorch, Scikit Learn. Then, you can export or convert that model to ONNX and consume it using ML.NET.

#### Open Source & Community

ML.NET, like .NET, is open source. This allows for the community to collaborate and contribute to it. Users have various ways of contributing to ML.NET, whether it's raising issues, updating documentation or submitting pull requests, they're all valuable contributions that only help make the framework that much better for everyone to use.

### Correct me if I'm wrong, but I believe a big mission of ML.NET is making machine learning accessible—that is, I shouldn't have to be an expert in machine learning to do it in .NET. Even still: how much should I know before I get started?

That's right! ML.NET provides many ways of interacting with it depending on what you're most comfortable with. The easiest way to get started is by using the tooling. The tooling provides a low-code way of training and consuming ML.NET models. If you prefer a graphical user interface, you can try [Model Builder](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet/model-builder), a Visual Studio extension that guides you through the steps involved in training a machine learning model. As long as you have a general sense of the problem you're trying to solve (classify text, predict a number, categorize images) and you have a dataset, Model Builder takes care of the rest.

Alternatively, if you prefer working on the command line you can use the [ML.NET CLI](https://www.nuget.org/packages/MLNet/), a .NET command line tool for training ML.NET model models and generating consumption code. The idea is very much the same as Model Builder, except now you interact with the tooling via the command line. The CLI is also a great choice for Machine Learning Operations (MLOps) scenarios where model training and deployment is done as part of a continuous integration (CI) or continuous deployment (CD) pipeline.

For folks who want more control, prefer a code-first approach, or are more familiar with machine learning concepts, there's other ways of using ML.NET. One is with the ML.NET Automated ML (Auto ML) API. The AutoML API is leveraged by the tooling to try to find the "best" model. The best model for your problem depends on many factors such as the quantity and distribution of your data and time to train. Therefore, it helps to try different algorithms with different parameters.

If you want full control over your machine learning pipeline, you can use the ML.NET API. The API provides you with direct access to data loaders, transformations, trainers, and prediction components that you can configure as needed to solve your problem.

One of the nice things is, none of the ways of using ML.NET is mutually exclusive. You can start off with the tooling to bootstrap the model training process and from there use the ML.NET API to make further refinements. In the end, it's all about choice and depending on your experience with machine learning and preferred workflow, there's an option for you.

*[Check out the full interview with Luis at my site](https://daveabrock.com/2020/08/29/dev-discussions-luis-quintanilla-1).*

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* .NET 5 Preview 8 all the things: Richard Lander [makes the announcement](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-8/), Daniel Roth [provides an update on ASP.NET Core updates in the new preview](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/), and Jeremy Likness [announces the new preview for EF Core 5](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-ef-core-5-0-preview-8/).
* Jonathon Marolf [talks about how to automatically find latent bugs in .NET 5](https://devblogs.microsoft.com/dotnet/automatically-find-latent-bugs-in-your-code-with-net-5/).
* Andrew Lock asks: [should you unit-test API.MVC controllers in ASP.NET Core](https://andrewlock.net/should-you-unit-test-controllers-in-aspnetcore/)?

### 📢 Announcements

* Phillip Carter [discusses new F# 5 updates](https://devblogs.microsoft.com/dotnet/f-5-update-for-august/).
* There's a new [release for Windows Terminal, 1.3](https://devblogs.microsoft.com/commandline/windows-terminal-preview-1-3-release/).
* Luca Bolognese [introduces some new C# source generator samples](https://devblogs.microsoft.com/dotnet/new-c-source-generator-samples/).
* Microsoft is [overhauling the Visual Studio feedback system](https://devblogs.microsoft.com/visualstudio/overhauling-the-visual-studio-feedback-system/), and Visual Studio 2019 v16.8 Preview 2 [has released some new features](https://devblogs.microsoft.com/visualstudio/visual-studio-2019-v16-8-preview-2/).
* Tara Overfield [provides the August cumulative update for .NET Framework](https://devblogs.microsoft.com/dotnet/august-2020-cumulative-update-preview/).
* Bri Achtman [provides August updates for ML.NET API and tooling](https://devblogs.microsoft.com/dotnet/august-ml-net-api-and-tooling-updates/).

### 📅 Community and events

Three community standups this week: ML.NET [joins the club](https://www.youtube.com/watch?v=2gjMrZ9XbRQ), Desktop [runs through WinForms innovations and XAML tooling](https://www.youtube.com/watch?v=m-NdyJVBf2Y), and ASP.NET [discusses Razor tooling](https://www.youtube.com/watch?v=nci0ZMoGcLM)).

### 😎 Blazor

* David Ramel [writes about Blazor WebAssembly getting lazy loading](https://visualstudiomagazine.com/articles/2020/08/27/blazor-updates.aspx).
* David Grace [writes OnClick events in C# using Blazor](https://www.roundthecode.com/dotnet/blazor/write-onclick-events-c-sharp-blazor-eliminate-javascript).

### 🚀 .NET Core

* Gunasekaran Thirumoorthy [uses .NET Core and C# to create and update a table of contents in Word](https://www.syncfusion.com/blogs/post/create-and-update-table-of-contents-in-word-documents-in-net-core-c.aspx).
* Dave Brock (ahem) [uses Project Tye to deploy a distributed application to k8s](https://daveabrock.com/2020/08/27/microservices-with-tye-2).
* Michał Białecki [executes a stored procedure with EF Core 5](http://www.michalbialecki.com/2020/08/27/execute-a-stored-procedure-with-entity-framework-core-5).
* David Ramel [discusses the feature-completeness of the new .NET 5 preview](https://visualstudiomagazine.com/articles/2020/08/26/net-5-preview-8.aspx).
* Jimmy Bogard [talks about his change to support constrained open generics in the .NET Core DI container](https://jimmybogard.com/constrained-open-generics-support-merged-in-net-core-di-container).
* Camilo Reyes [builds a REST API in .NET Core](https://www.red-gate.com/simple-talk/dotnet/c-programming/build-a-rest-api-in-net-core/).
* Thomas Ardal [has some lessons learned after migrating 25+ projects to .NET Core](https://blog.elmah.io/lessons-learned-after-migrating-25-projects-to-net-core/).
* The Code Maze blog [discusses FluentValidation in ASP.NET Core](https://code-maze.com/fluentvalidation-in-aspnet/).
* Damien Bowden [encrypts texts for an identity in ASP.NET Core Razor Pages using AES and RSA](https://damienbod.com/2020/08/22/encrypting-texts-for-an-identity-in-asp-net-core-razor-pages-using-aes-and-rsa/).

### ⛅ The cloud

* JoeG [secures Azure containers and blobs with managed identities](https://www.josephguadagno.net/2020/08/22/securing-azure-containers-and-blobs-with-managed-identities).
* Julie Lerman [talks about her experience using AWS for .NET](http://thedatafarm.com/dotnet/aws-for-net-developers/).
* Anupam Maiti [deletes blobs using an Azure Function with timer triggers](https://www.c-sharpcorner.com/article/delete-blobs-using-timer-trigger-azure-function-in-net-core/).
* Jiří Činčura [penny-pinches his ASP.NET Core app on Azure VMs](https://www.tabsoverspaces.com/233836-running-aspnet-core-app-on-azure-b1ls-vm-penny-pinching).
* Christos Matskas [builds an Azure Function using the latest Azure SDKs securely using Azure.Identity libraries](https://devblogs.microsoft.com/azure-sdk/secretless-azure-functions-dev-with-the-new-azure-identity-libraries/).
* Sruti Guhathakurta introduces [paginators in AWS SDK for .NET v3.5](https://aws.amazon.com/blogs/developer/introducing-paginators-in-aws-sdk-for-net-v3-5).

### 📔 Languages

* Khalid Abuhakmeh [uses the Range and Index classes in C# 8](https://khalidabuhakmeh.com/use-range-and-index-in-csharp-8), and also [disposes of resources asynchronously with IAsyncDisposable](https://khalidabuhakmeh.com/iasyncdisposable-dispose-resources-asynchronously).
* Giorgi Dalakishvili [writes a simple GraphQL tutorial using C#](https://developer.okta.com/blog/2020/08/24/simple-graphql-csharp).
* Thomas Claudius Huber [writes about C# 9 init-only properties](https://www.thomasclaudiushuber.com/2020/08/25/c-9-0-init-only-properties/).
* Maarten Balliauw [discusses Producer/consumer pipelines with System.Threading.Channels](https://blog.maartenballiauw.be/post/2020/08/26/producer-consumer-pipelines-with-system-threading-channels.html).
* Matthew Jones [creates C# enums from a SQL database using T4 text templates](https://exceptionnotfound.net/creating-csharp-enums-from-a-sql-database-using-t4-text-templates).
* Josef Ottosson [writes a custom Dictionary<string, object> JsonConverter for System.Text.Json](https://josef.codes/custom-dictionary-string-object-jsonconverter-for-system-text-json/).

### 🔧 Tools

* The Code Maze blog [dives into different validators with FluentValidation](https://code-maze.com/deep-dive-validators-fluentvalidation/).
* Mike Shepard [talks about how he uses Git (the 5 percent)](https://powershellstation.com/2020/08/25/git-the-5-percent-that-i-always-use/).
* ErikEK discusses [reverse engineering advanced options for EF Core Power Tools](https://erikej.github.io/efcore/2020/08/24/ef-core-power-tools-advanced-options.html).
* Paul Michaels [manually creates a test harness in .NET](https://www.pmichaels.net/2020/08/22/manually-creating-a-test-harness-in-net/).
* Josef Ottosson [uses CSVHelper to read a column value into a List property](https://josef.codes/csvhelper-read-column-value-into-a-list-property/).
* Jason Roberts [simplifies and reduces test code with AutoFixture](http://dontcodetired.com/blog/post/Simplify-and-Reduce-Test-Code-with-AutoFixture).
* John Juback [builds cross-platform desktop apps with Electron.NET](https://www.grapecity.com/blogs/building-cross-platform-desktop-apps-with-electron-dot-net).
* Jonathan Pereira [uses Fiddler Everywhere to inspect web traffic](https://www.telerik.com/blogs/use-fiddler-everywhere-to-inspect-your-web-traffic).
* Michał Białecki [creates a NuGet package targeting multiple frameworks](http://www.michalbialecki.com/2020/08/21/how-to-create-a-nuget-package-targeting-multiple-frameworks).

### 📱 Xamarin

* Leomaris Reyes [replicates a drink and food menu UI](https://www.syncfusion.com/blogs/post/replicating-a-drink-and-food-menu-ui-in-xamarin-forms.aspx).
* Bohdan Benetskyi [creates a native month and year picker](https://medium.com/@benetskyybogdan/xamarin-forms-month-and-year-picker-105550772222).

### 🎤 Podcasts

* The .NET Rocks podcast [talks about building serverless .NET apps on AWS](https://www.dotnetrocks.com/default.aspx?ShowNum=1702).
* The Xamarin Podcast [talks about drawing, Visual Studio 2019, and more](https://devblogs.microsoft.com/xamarin/drawing-xamarin-podcast/).
* The 6-Figure Developer podcast [talks about Project Tye with Glenn Condron](https://6figuredev.com/podcast/episode-158-project-tye-with-glenn-condron/).
* The .NET Core Show [talks with Michael Shpilt about practical debugging](https://dotnetcore.show/episode-58-practical-debugging-for-net-developers-with-michael-shplit/).
* The Merge Conflict podcast [geeks out on GUIDs and UUIDs](https://www.mergeconflict.fm/216).

### 🎥 Videos

* Data Exposed [explores Azure SQL Edge](https://channel9.msdn.com/Shows/Data-Exposed/What-is-Azure-SQL-Edge).
* The Xamarin Show [continues talking about the new Drawing API](https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-New-Drawing-API-Part-2).
* The ON.NET Show introduces [PeachPie](https://channel9.msdn.com/Shows/On-NET/Getting-started-PeachPie).
* Scott Hanselman [explains Docker](https://www.youtube.com/watch?v=0oEsMwSxBsk) and also [talks about ports and processes](https://www.youtube.com/watch?v=4P0KXWC3V0U).
* Jeff Fritz [discusses C# methods, events, and delegates for beginners](https://www.youtube.com/watch?v=-E2xB4kOSV0).
* The ASP.NET Monsters [work with AsyncLocal](https://www.youtube.com/watch?v=QFarimuMujg).
* Azure Friday [deploys .NET Core apps to Azure with GitHub Actions](https://channel9.msdn.com/Shows/Azure-Friday/Learn-how-to-deploy-NET-Core-apps-to-Azure-with-GitHub-Actions).
