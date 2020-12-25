---
date: "2020-12-12"
title: "The .NET Stacks #29: More on route-to-code and some Kubernetes news"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-29-card.png
subtitle: This week, we dig deep on route-to-code and discuss some Kubernetes news.
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

*Note: This is the published version of my free, weekly newsletter, The .NET Stacks. It was originally sent to subscribers on December 7, 2020. Subscribe at the bottom of this post to get the content right away!*

Happy Monday! Here's what we're talking about this week:

- Digging deeper on "route-to-code"
- Kubernetes is deprecating Docker ... what?
- Last week in the .NET world

## 🔭 Digging deeper on "route-to-code"

Last week, I talked about [the future of writing APIs for ASP.NET Core MVC](https://daveabrock.com/2020/12/05/dotnet-stacks-28#the-future-of-aspnet-core-mvc-apis). The gist: there's a new initiative (Project Houdini) coming to move MVC productivity features to the core of the stack, and part of that is generating imperative APIs for you at compile time using source generation.

This leverages a way to write slim APIs in ASP.NET Core without the bloat of the MVC framework: it's called "route-to-code." We [talked about it in early October](https://daveabrock.com/2020/10/09/dotnet-stacks-20#use-route-to-code-with-aspnet-core). I thought it'd be fun to migrate a simple MVC CRUD API to this model, and [I wrote about it this week](https://daveabrock.com/2020/12/04/migrate-mvc-to-route-to-code).

As I wrote, this isn't meant to be an MVC replacement, but a solution for simple JSON APIs. It does *not* support model binding or validation, content negotiation, or dependency injection from constructors. Most times, though, you're wanting to separate business logic from your execution context—it's definitely worth a look.

Here's me using an in-memory Entity Framework Core database to get some bands:

```csharp
endpoints.MapGet("/bands", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();
    var bands = repository.Bands.ToListAsync();
    await context.Response.WriteAsJsonAsync(bands);
});
```

There's no framework here, so instead of using DI to access my EF context, I get a service through the `HttpContext`. Then, I can use helper methods that let me read from and write to my pipe. Pretty slick.

Here's me getting a record by ID:

```csharp
endpoints.MapGet("/bands/{id}", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();
    var id = context.Request.RouteValues["id"];
    var band = await repository.Bands.FindAsync(Convert.ToInt32(id));

    if (band is null)
    {
        context.Response.StatusCode = StatusCodes.Status404NotFound;
        return;
    }
    await context.Response.WriteAsJsonAsync(band);
});
```

The simplicity comes with a cost: it's all very manual. I even have to convert the ID to an integer myself (not a big deal, admittedly). 

How does a POST request work? I can check to see if the request is asking for JSON. With no framework or filters, my error checking is setting a status code and returning early. (I can abstract this out, obviously. It took awhile to get used to not having a framework to lean on.)

```csharp
endpoints.MapPost("/bands", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();

    if (!context.Request.HasJsonContentType())
    {
        context.Response.StatusCode = StatusCodes.Status415UnsupportedMediaType;
        return;
    }

    var band = await context.Request.ReadFromJsonAsync<Band>();
    await repository.SaveChangesAsync();
    await context.Response.WriteAsJsonAsync(band);
});
```

In the doc, Microsoft [will be the first to tell you](https://docs.microsoft.com/aspnet/core/web-api/route-to-code?view=aspnetcore-5.0#notable-missing-features-compared-to-web-api) it's for the simplest scenarios. It'll be interesting to see what improvements come: will mimicking DI become easier? I hope so. 

## 🤯 Kubernetes is deprecating Docker ... what?

I know this is a .NET development newsletter, but these days you probably need at least a *passing* knowledge of containerization. To that end: this week, you may have heard something along the lines of "Kubernetes is deprecating Docker." It sounds concerning, but [Kubernetes says you probably shouldn't worry](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/) and Docker [is saying the same](https://www.docker.com/blog/what-developers-need-to-know-about-docker-docker-engine-and-kubernetes-v1-20/).

Still, it's true: Kubernetes is deprecating Docker as a container runtime after v1.20—currently planned for late 2021.

From a high level, I think Google's [Kelsey Hightower summed it up best](https://twitter.com/kelseyhightower/status/1334920845999783937):

>Think of it like this -- Docker refactored its code base, broke up the monolith, and created a new service, containerd, which both Kubernetes and Docker now use for running containers.

Docker isn't a magical "make me a container" button—it's an entire tech stack. Inside of that is [a container runtime](https://www.docker.com/blog/what-is-containerd-runtime/), *containerd*. It contains a lot of bells and whistles for us when doing development work, but k8s doesn't need it because it isn't a person. (If it were, I'd like to have a chat.) 

For k8s to get through this abstraction layer, it needs to use the Dockershim tool to get to *containerd*—yet another maintenance headache. Kubelets [are removing Dockershims](https://kubernetes.io/blog/2020/12/02/dockershim-faq/) at the end of 2021, which removes Docker support. When this change comes, you just need to change your container runtime from Docker to another supported runtime. 

Because this addresses a different environment than most folks use with Docker, it shouldn't matter—the install you're using in dev is typically different than the runtime in your k8s cluster. This change would largely impact k8s administrators and not developers.

Hopefully this clears up some potential confusion. We don't talk about Docker and Kubernetes often, but this was too important not to discuss. (I could hardly *contain* myself.)

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Niels Swimberghe [makes phone calls from Blazor WebAssembly with Twilio Voice](https://swimburger.net/blog/dotnet/making-phone-calls-from-blazor-webassembly-with-twilio-voice). (And while you're there, hover over the burger. You're welcome.)
* Steve Smith [warns against wrapping DbContext in using, and other gotchas](https://ardalis.com/avoid-wrapping-dbcontext-in-using/).
* Khalid Abuhakmeh [writes about understanding the .NET 5 runtime environment](https://khalidabuhakmeh.com/understand-the-dotnet-five-runtime-environment).

### 📢 Announcements

* Claire Novotny [shines a light on debug-time productivity with Source Link](https://devblogs.microsoft.com/dotnet/improving-debug-time-productivity-with-source-link).
* .NET Core 2.1, 3.1, and 5.0 [updates are coming to Microsoft Update](https://devblogs.microsoft.com/dotnet/net-core-updates-coming-to-microsoft-update).
* Uno Platform 3.1 [is released](https://platform.uno/blog/uno-platform-3-1-released-linux-new-winui-controls-prism-8-0-and-more/).
* Scott Addie [recaps what's new in the ASP.NET Core docs for November 2020](https://docs.microsoft.com/aspnet/core/whats-new/2020-11).
* Bri Achtman [writes about ML.NET Model Builder November updates](https://devblogs.microsoft.com/dotnet/ml-net-model-builder-november-updates).
* Tara Overfield [releases the November 2020 cumulative update preview for .NET Framework](https://devblogs.microsoft.com/dotnet/net-framework-november-2020-cumulative-update-preview-for-windows-10-2004-and-windows-server-version-2004).

### 📅 Community and events

* Just one community standup this week: Xamarin [talks about .NET MAUI](https://www.youtube.com/watch?v=7Gvr0d1vTQ4).
* The .NET Docs Show [talks to Dave Brock](https://www.youtube.com/watch?v=sU9xwxY3Ikw) (yes, that one) about C# 9.

### 😎 ASP.NET Core / Blazor

* Dave Brock [writes about simple JSON APIs with ASP.NET Core route-to-code](https://daveabrock.com/2020/12/04/migrate-mvc-to-route-to-code).
* David Ramel [writes about reported performance degradation when moving from WinForms to Blazor](https://visualstudiomagazine.com/articles/2020/12/01/blazor-performance.aspx).
* Ricardo Peres [writes about the pitfalls when working with async file uploads in ASP.NET Core](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-async-file-uploads).
* Jon Hilton [passes arguments to onclick functions in Blazor](https://www.telerik.com/blogs/how-to-pass-arguments-to-your-onclick-functions-blazor).
* Marinko Spasojevic [writes about complex model validation in Blazor apps](https://code-maze.com/complex-model-validation-in-blazor/).
* Damien Bowden [secures an ASP.NET Core API that uses multiple access tokens](https://damienbod.com/2020/12/03/securing-an-asp-net-core-api-which-uses-multiple-access-tokens/).
* Michael Shpilt [writes about some must-know packages for ASP.NET Core](https://oz-code.com/blog/net-c-tips/8-must-know-nuget-packages-asp-net-core-application).

### 🚀 .NET 5

* Jonathan Allen [writes more about .NET 5 breaking changes](https://www.infoq.com/news/2020/12/net-5-breaking-changes-2).
* Eran Stiller [writes about .NET 5 runtime improvements](https://www.infoq.com/news/2020/12/net-5-runtime-improvements).
* Jonathan Allen [writes about .NET 5 breaking changes to the BCL](https://www.infoq.com/news/2020/12/net-5-breaking-changes).
* Antonio Laccardi [writes about ASP.NET Core improvements in .NET 5](https://www.infoq.com/news/2020/12/aspnet-core-improvement-dotnet-5).
* Norm Johnson [writes about .NET 5 AWS Lambda support with container images](https://aws.amazon.com/blogs/developer/net-5-aws-lambda-support-with-container-images).

### ⛅ The cloud

* Frank Boucher [configures a secured custom domain on an Azure Function or website](http://www.frankysnotes.com/2020/12/how-to-configure-secured-custom-domain.html).
* Paul Michaels [handles events inside an Azure Function](https://www.pmichaels.net/2020/11/28/handling-events-inside-an-azure-function).
* David Ramel [writes how Google Cloud Functions supports .NET Core 3.1 but not .NET 5](https://visualstudiomagazine.com/articles/2020/11/30/cloud-functions-net.aspx).
* Abel Wang and Isaac Levin [write about dev productivity with GitHub, VS Code, and Azure](https://devblogs.microsoft.com/devops/optimum-developer-productivity-github-visual-studio-code-azure).

### 📔 Languages

* Claudio Bernasconi [writes about top-level statements in C# 9](https://www.claudiobernasconi.ch/2020/12/03/csharp-9-top-level-statements/), and also [works through switch expressions in C# 8](https://www.claudiobernasconi.ch/2020/12/04/csharp-8-switch-expressions/).
* Munib Butt [uses the proxy pattern in C#](https://www.c-sharpcorner.com/article/using-the-proxy-pattern-in-c-sharp/).
* Ian Russell [introduces partial function application in F#](https://www.softwarepark.cc/blog/2020/11/30/introduction-to-partial-function-application-in-f).
* Ian Griffiths [shows the pitfalls of mechanism over intent with C# 9 patterns](https://endjin.com/blog/2020/12/dotnet-csharp-9-patterns-mechanism-over-intent.html).
* Matthew Crews [writes about object expressions in F#](https://matthewcrews.com/blog/2020-12-04-the-under-appreciated-power-of-object-expressions/).

### 🔧 Tools

* Sean Killeen [gets started with PowerShell Core in Windows Terminal](https://seankilleen.com/2020/11/getting-started-with-powershell-core-in-windows-terminal/), and also [writes about things he's learned about NUnit](https://seankilleen.com/2020/12/cool-things-i-learned-about-nunit-while-re-launching-the-docs/).
* Andrew Lock [uses Quartz.NET with ASP.NET Core and worker services](https://andrewlock.net/using-quartz-net-with-asp-net-core-and-worker-services/).


### 📱 Xamarin

* James Montemagno [warns against using Android in your namespaces](https://montemagno.com/dont-put-android-in-your-namespace-in-xamarin-apps/), and also [gets you writing your first app for iOS and Android with Xamarin and Visual Studio](https://montemagno.com/build-your-first-for-ios-android-app-with-xamarin-and-visual-studio/).
* Yogeshwaran Mohan [creates a marquee control](https://www.syncfusion.com/blogs/post/create-xamarin-marquee-control.aspx).
* Nick Randolph [explains the correlation between .NET 5, WinUI, and MAUI (Xamarin.Forms)](https://nicksnettravels.builttoroam.com/net5-crossplatform).
* Matheus Castello [writes about Linux + .NET 5 + VS Code XAML Preview + Hot Reload running on embedded Linux](https://microhobby.com.br/blog/2020/11/30/vs-code-xaml-preview-embedded-linux-dotnet-core/).

### 👍 Design, architecture and best practices

* Kamil Grzybek [continues his series on modular monoliths](http://www.kamilgrzybek.com/design/modular-monolith-domain-centric-design/).
* Scott Brady reminds us: [OAuth is not user authorization](https://www.scottbrady91.com/OAuth/OAuth-is-Not-User-Authorization).
* Peter Vogel [shows the advantages of end-to-end testing](https://www.telerik.com/blogs/the-only-testing-that-matters-testing-through-eyes-of-user).
* Nathan Bennett [compares GraphQL to REST](https://www.bignerdranch.com/blog/graphql-versus-rest/).
* Derek Comartin [talks about handling duplicate messages](https://codeopinion.com/handling-duplicate-messages-idempotent-consumers/).

### 🎤 Podcasts

* The Changelog [talks about growing as a software engineer](https://changelog.com/podcast/422).
* The Stack Overflow podcast [explains why devs are increasing demanding ethics in tech](https://the-stack-overflow-podcast.simplecast.com/episodes/ensuring-your-software-is-ethical-might-save-you-money-in-the-long-run-ft7H842f).
* The 6 Figure Developer [talks to Rob Richardson about .NET 5, pipelines, and testing](https://6figuredev.com/podcast/episode-172-rob-richardson-net-5-pipelines-testing/).


### 🎥 Videos

* Jeff Fritz [works with Entity Framework Core](https://www.youtube.com/watch?v=bqujgzOaoE8).
* The ON.NET Show [customizes the Graph SDKs](https://channel9.msdn.com/Shows/On-NET/Customizing-the-Graph-SDKs) and [discusses microcontrollers with the Meadow IoT platform](https://www.youtube.com/watch?v=QmbU1vwBEp8).
* Data Exposed [gets started with DevOps for Azure SQL](https://channel9.msdn.com/Shows/Data-Exposed/Getting-Started-with-DevOps-for-Azure-SQL).
* The Loosely Coupled Show [talks about the difficulties of caching](https://www.youtube.com/watch?v=1ps4QJpUfVc).
