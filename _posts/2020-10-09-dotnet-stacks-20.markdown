---
date: "2020-10-09"
title: "The .NET Stacks #20: Route to Code, IdentityServer, community links"
tags: [dotnet-stacks]
share-img: /assets/img/stacks-20-card.png
subtitle: This week, we look at Route to Code and check in on big IdentityServer news.
comments: false
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Based on my eating habits this week, I should probably rename this newsletter to *The .NET Snacks*.

This week:

* Use "route to code" with ASP.NET Core
* The IdentityServer project goes commercial
* Understand Blazor Web Assembly performance best practices
* Last week in the .NET world

## Use "route to code" with ASP.NET Core

I've been thinking a lot about "route to code" in ASP.NET Core lately. There isn't much out there but have found *some* good content: the [ON.NET Show brought up the concept recently](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Route-to-Code), and Anthony Giretti [has been writing some nice posts about it lately, too](https://anthonygiretti.com/2020/06/29/nano-services-with-asp-net-core-or-how%20-to-build-a-light-api).

As an ASP.NET developer, you've likely leveraged controllers in MVC or Web API apps. That is, the controller will intercept your HTTP request and handle routing for you—you can do some configuration, but it's largely managed for you. This is great if you don't need much control, but it sure adds a lot of ceremony (if you've ever wait for ASP.NET to scaffold a new MVC app, you know what I mean). You may have scenarios where you need fine-grained control, simplicity, or high performance without an explicit framework.

This "route to code" concept offers a solution somewhere between ASP.NET Core middleware and controllers—where you can handle routing right in the `Startup.cs` file of your ASP.NET Core project. You can get started by creating an "Empty" ASP.NET Core web app in Visual Studio.

If we look at Anthony's example, he creates a list of countries and instantiates it in the `Startup` constructor. The fun stuff, though, is in the `app.UseEndpoints` middleware.

![Creating endpoints]({{ site.url }}{{ site.baseurl }}/assets/img/use-endpoints.png)

Here, we're [using the `MapGet` extension method](https://docs.microsoft.com/aspnet/core/fundamentals/routing?view=aspnetcore-3.1)—you use it to match the HTTP/URL method, and then execute by running a delegate (and yes, there are other methods for the other HTTP verbs). You can definitely use this in more complex ways—like using string interpolation to create routing templates, adding authentication, and dependency injection. It takes some getting used to after years of depending on controllers—but it's a great way to cut straight to what matters.

## The IdentityServer project goes commercial

This week, it was announced that IdentityServer—an open-source OpenID Connect (OIDC) and OAuth 2.0 framework for ASP.NET and ASP.NET Core—has gone commercial. With [12 million NuGet downloads to date for the IdentityServer4 package](https://www.nuget.org/packages/IdentityServer4/), this is a big deal. For most of us, we've used the free (for us) library for the last decade. While congratulations are in order for Brock Allen and Dominick Baier—and they should be praised for finding a path for sustaining the project over the long term—a logical next question is what this means for the greater .NET ecosystem.

From [Dominick Baier's post on the news](https://leastprivilege.com/2020/10/01/the-future-of-identityserver/):

>The current version (IdentityServer4 v4.x) will be the last version we work on as free open source. We will keep supporting IdentityServer4 until the end of life of .NET Core 3.1 in November 2022.
>
>To continue our work, we have formed a new company Duende Software, and IdentityServer4 will be rebranded as Duende IdentityServer. Duende IdentityServer will contain all new feature work and will target .NET Core 3.1 and .NET 5 (and all versions beyond).

What does this mean for use in your projects? IdentityServer4 is still free, and appears to always will be. However, in two years it won't be supported and you won't get any critical security updates for it. IdentityServer5 will have this new pricing model. (And short term, .NET will ship with IdentityServer 4 templates.)

As for that pricing model: this new Duende IdentityServer company will offer two versions of IdentityServer. You'll have a free reciprocal public license (RPL) for folks using open-source work, and a commercial license that is paid (the exact charges based on company size and usage). I'm seeing the $1500 price point being passed around, but [others have noted it isn't so simple](https://www.reddit.com/r/dotnet/comments/j4pt67/something_to_be_aware_of_with_duende/). You'll also want to check out the lively GitHub issue that [discusses where to go from here](https://github.com/dotnet/aspnetcore/issues/26489).

If you don't want to pay for it, fine—you've got two years on IdentityServer4 and you can evaluate if less complex solutions like the out-of-the-box `Microsoft.AspNetCore.Identity` work better for you. If not, you can roll your own solution. To that I say: unless you're a security expert you'll probably find the cost in development time and headaches far exceeds licensing for IdentityServer (for most cases). If your company needs the complex use cases and can pay for it, my opinion is to do just that.

## Understand Blazor Web Assembly performance best practices

This week, Steve Sanderson—the architect at Microsoft behind Blazor—announced he's working on [documenting Blazor Web Assembly performance best practices](https://github.com/dotnet/AspNetCore.Docs/blob/1e199f340780f407a685695e6c4d953f173fa891/aspnetcore/blazor/webassembly-performance-best-practices.md#aspnet-core-blazor-webassembly-performance-best-practices). While performance is always important, it appears doubly so when you're loading a complete .NET runtime into the browser. I definitely learned a lot, and it's worth a read.

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Heather Downing [talks about building securely with Blazor Web Assembly](https://developer.okta.com/blog/2020/09/30/blazor-webassembly-wasm-dotnetcore).
* Patrick Smacchia [discusses app trimming in .NET 5](https://blog.ndepend.com/net-5-0-app-trimming-and-potential-for-future-progress/).
* Phil Haack [finds a subtle gotcha with Azure deployment slots and ASP.NET Core](https://haacked.com/archive/2020/09/28/azure-swap-with-warmup-aspnetcore).

### 📢 Announcements

* Microsoft released [its survey results on repo experience](https://devblogs.microsoft.com/dotnet/repo-experience-survey-results), and folks in the WPF community [have voiced their displeasure](https://www.zdnet.com/article/microsoft-told-were-not-happy-by-github-contributors-to-open-source-net-core-wpf/#ftag=RSSbaffb68).
* Maria Naggaga [announces .NET Interactive Preview 3](https://devblogs.microsoft.com/dotnet/net-interactive-preview-3-vs-code-insiders-and-polyglot-notebooks).
* Bri Achtman [gives us September updates on ML.NET](https://devblogs.microsoft.com/dotnet/ml-net-september-updates).
* David Ortinau [previews Xamarin.Forms 5's advanced UI controls](https://devblogs.microsoft.com/xamarin/xamarin-forms-5-preview).

### 📅 Community and events

* The .NET Docs Show [double-clicks on accessibility and autonomous systems with John Alexander](https://www.youtube.com/watch?v=TqiMDHhADi8).
* JetBrains [has some nice .NET OSS ideas for Hacktoberfest](https://blog.jetbrains.com/dotnet/2020/09/30/hacktoberfest-2020-and-net-oss/).
* Three .NET community standups this week: Xamarin [talks with Theodora Tataru](https://www.youtube.com/watch?v=zyFxAJjlPys), Entity Framework [runs through geographic data with NetTopologySuite](https://www.youtube.com/watch?v=IHslY5rrxD0), and ASP.NET [runs through .NET 5 with Scott Hunter](https://www.youtube.com/watch?v=l5nnYDd7gpk).

### 😎 ASP.NET Core / Blazor

* Shaun Curtis [adds new record types to his Blazor database app](https://www.codeproject.com/Articles/5281000/Building-a-Database-Application-in-Blazor-Part-6-A).
* Jon Hilton [renders diagrams on the fly in Blazor](https://jonhilton.net/blazor-diagrams/), and also [discusses when Blazor decides to render your UI](https://jonhilton.net/when-does-blazor-render-your-ui).
* Michael Shpilt [uses attributes and middleware in ASP.NET Core](https://michaelscodingspot.com/attributes-and-middleware-in-asp-net-core/).
* Michał Białecki [reads request headers as an object in ASP.NET Core](https://www.michalbialecki.com/2020/09/28/read-request-headers-as-an-object-in-asp-net-core-api).
* Anthony Giretti [uses Microsoft.AspNetCore.Http JSON extensions to assist with "route to code."](https://anthonygiretti.com/2020/09/29/asp-net-core-5-route-to-code-taking-advantage-of-microsoft-aspnetcore-http-json-extensions/)
* Andrew Lock [sets environment variables for ASP.NET Core apps in a Helm chart](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-5-setting-environment-variables-in-a-helm-chart/).

### ⛅ The cloud

* Pavel Krymets [discusses connection pool limits for .NET Framework when using the new .NET Azure SDK](https://devblogs.microsoft.com/azure-sdk/net-framework-connection-pool-limits).
* Muhammed Saleem [publishes an ASP.NET Core app to Azure App Service from Visual Studio](https://code-maze.com/publishing-an-asp-net-core-app-to-azure-app-service-using-visual-studio/).

### 📔 C#

* Khalid Abuhakmeh [reads and converts QueryCollection values in ASP.NET](https://khalidabuhakmeh.com/read-and-convert-querycollection-values-in-aspnet).
* Franco Tiveron [shows how to use SameSite with .NET](https://developer.okta.com/blog/2020/09/28/adapt-dotnet-app-for-samesite-fix).
* Matthew Jones [discusses casting, conversion, and parsing in C#](https://exceptionnotfound.net/csharp-in-simple-terms-3-casting-conversion-parsing-is-as-and-typeof).
* Khalid Abuhakmeh [serializes interface instances with System.Text.Json](https://khalidabuhakmeh.com/serialize-interface-instances-system-text-json).
* David Grace [shows off four new C# 9 features](https://www.roundthecode.com/dotnet/four-features-c-sharp-9-that-arent-in-c-sharp-8).
* Matthew Jones [runs through primitive types, literals, and nullables in C#](https://exceptionnotfound.net/csharp-in-simple-terms-2-primitive-types-literals-and-nullables).
* Konrad Kokosa [explores getting rid of array bound checks, ref-returns, and .NET 5](https://tooslowexception.com/getting-rid-of-array-bound-checks-ref-returns-and-net-5/).
* Mark Seemann [uses EnsureSuccessStatusCode as an assertion](https://blog.ploeh.dk/2020/09/28/ensuresuccessstatuscode-as-an-assertion/).

### 📗 F#

* A nice piece on [how to use JWT with RSA in F#](https://secanablog.wordpress.com/2020/09/28/f-jwt-with-rsa/).
* Bartosz Sypytkowski [works on CRDTs with arrays in F#](https://bartoszsypytkowski.com/operation-based-crdts-arrays-1/).
* Wojciech Gawroński [tries out F# with AWS](https://awsmaniac.com/functional-programming-with-aws-cdk/).
* Ryan Palmer [works with the Fabulous project with Xamarin and F#](https://www.compositional-it.com/news-blog/fabulous-mobile-apps-with-xamarin-and-f/).
* Jeremie Chassaing [works with applicatives](https://thinkbeforecoding.com/post/2020/10/03/applicatives-irl).

### 🔧 Tools

* Dominick Baier [writes about the future of IdentityServer](https://leastprivilege.com/2020/10/01/the-future-of-identityserver/).
* Michał Białecki [talks about how not to pass parameters in Entity Framework 5](https://www.michalbialecki.com/2020/09/26/how-not-to-pass-parameters-in-entity-framework-core-5).
* Kevin Logan [writes about transitioning from TFVC to Git](https://www.aligneddev.net/blog/2020/transitioning-from-tfvc-to-git/).
* Nicholas Blumhardt [works with user-defined functions in Serilog expressions](https://nblumhardt.com/2020/10/serilog-expressions-user-defined-functions/), and also [discusses programmable text and JSON formatting for Serilog](https://nblumhardt.com/2020/10/programmable-serilog-formatting/).
* Scott Hanselman [works with dotnet-monitor](https://www.hanselman.com/blog/ExploringYourNETApplicationsWithDotnetmonitor.aspx).
* The Edge team [talks about bringing the browser dev tools experience to Visual Studio Code](https://blogs.windows.com/msedgedev/2020/10/01/microsoft-edge-tools-vscode).
* Sophia Prafina [writes about avoiding Kubernetes anti-patterns](https://www.pulumi.com/blog/kubernetes-anti-patterns/).
* Dmitrij Kovaliov [writes about resiliency in your .NET apps with Polly](https://hackernoon.com/dont-let-your-net-applications-fail-resiliency-with-polly-uz1z3t8t?source=rss).
* Ian Griffiths [minimizes allocations in AIS.NET](https://endjin.com/blog/2020/09/arraypool-vs-memorypool-minimizing-allocations-ais-dotnet.html).
* Dan Clarke [shows how to deal with unrelated changes on feature branches with Git](https://www.danclarke.com/git-unrelated-changes-in-feature-branch).
* James Dawson [streamlines .NET dependency management with NuGet meta packages](https://endjin.com/blog/2020/09/streamline-dependency-management-with-nuget-meta-packages.html).
* David Walsh [shows how to detect the default branch in a Git repository](https://davidwalsh.name/get-default-branch-name).
* Neils Swimberghe [writes a PowerShell script to scan documentation for broken links](https://swimburger.net/blog/powershell/powershell-script-scan-documentation-for-broken-links).

### 📱 Xamarin

* Jean-Marie Alfonsi [hunts down Xamarin shadows leaks on Android](https://www.sharpnado.com/shadows-leaks/).
* Denys Fiediaiev [creates a custom Xamarin pop-up using MvvmCross](https://medium.com/@prin53/pop-up-xamarin-e2d815441a54).
* Leomaris Reyes [replicates a Sneaker UI app](https://askxammy.com/replicating-sneakers-ui-in-xamarin-forms/).
* Alex Shirshov [shows you to use C# 9 in Xamarin today](https://omnitalented.com/how-to-use-csharp9-with-xamarin-forms-today/).

### 🎤 Podcasts

* Scott Hanselman [talks to Dr. Michaela Greiler about enjoyable code reviews](https://hanselminutes.simplecast.com/episodes/enjoyable-code-reviews-with-dr-michaela-greiler-8NZo1UeJ).
* The Microsoft Cloud Show [recaps Ignite 2020](https://www.microsoftcloudshow.com/podcast/Episodes/378-microsoft-ignite-2020-review).
* The Coding Blocks podcast [continues looking at the DevOps Handbook](https://www.codingblocks.net/podcast/the-devops-handbook-the-value-of-a-b-testing/).
* The 6-Figure Developer podcast [talks about MLOps and ML.NET with Alexander Slotte](https://6figuredev.com/podcast/episode-163-mlops-and-ml-net-with-alexander-slotte/).

### 🎥 Videos

* The IoT Show [talks about connecting sensors to Azure](https://channel9.msdn.com/Shows/Internet-of-Things-Show/Connect-any-IoT-sensor-to-Azure).
* The Visual Studio Toolbox talks about [the Bridge to Kubernetes feature](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Bridge-to-Kubernetes).
* The AI Show [automates machine learning on Azure](https://channel9.msdn.com/Shows/AI-Show/Automated-Machine-Learning-on-Azure).
* Azure Friday [goes under the hood with Azure Container Instances (ACI)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Container-Instances-ACI-under-the-hood).
* The ASP.NET Monsters [discusses static site generators with Khalid Abuhakmeh](https://www.youtube.com/watch?v=MrpNY1c7qiI).
* Data Exposed [begins a series on Infrastructure as Code and Azure](https://channel9.msdn.com/Shows/Data-Exposed/Infrastructure-as-Code-and-Azure--A-Match-Made-in-the-Cloud-Part-1).
