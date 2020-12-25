---
date: "2020-10-16"
title: "The .NET Stacks #21: Azure Static Web Apps, .NET 6 feedback, and more!"
tags: [dotnet-stacks]
share-img: /assets/img/stacks-21-card.png
subtitle: This week, we take a look at Azure Static Web Apps, dotnet-monitor, and more.
comments: false
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

Happy Monday! It looks like my pun from last week received some attention (and some new subscribers). I'm glad you all joined the newsletter before it gets code outside.

This week:

* Catch up on Azure Static Web Apps
* Provide input on .NET 6
* Last week in the .NET world

## ⛅ Catch up on Azure Static Web Apps

This week, Anthony Chu joined the ASP.NET community standup to [talk about Azure Static Web Apps](https://www.youtube.com/watch?v=8xsp6Z_HjIg). Azure Static Web Apps has been at the top of my "I really need to take a look at this" list, so the timing was right. 😎

Static sites are definitely the new hotness, and have been for awhile. If content on your site doesn't change that often, you'll just need to serve up some static HTML files. For example, on my site I utilize a static site generator called Jekyll and host the content on GitHub Pages—this helps my site load super fast and with little overhead. Why introduce database overhead and the like if you don't need it? (If it wasn't clear I'm talking about you, WordPress.)

Many times, though, you'll also need to call off to an API eventually—this is quite common with SPAs like Vue, Angular, React, and now Blazor: you have a super-lightweight front-end that calls off to some APIs. A common architecture is serving up files statically with a serverless backend, such as Azure Functions.

Enter Azure Static Web Apps. Introduced at Ignite this year, Azure Static Web Apps allow you to leverage this architecture with one easy solution. If you're good with GitHub lock-in and are looking for a static hosting solution, it's worth a look.

You can [check out the docs for the full treatment](https://docs.microsoft.com/azure/static-web-apps/overview), but Azure Static Web Apps offers web hosting (duh), native Azure Functions support, GitHub triggers over GitHub Actions, free renewable SSL certificates, custom domains, and a bunch of auth integrations, and fallback routes.

My favorite feature is GitHub PR testing sites. Once a PR kicks off, a GitHub Action executes and creates a temporary test staging site to view changes (it goes away once the PR is merged or discarded). This is a wonderful tool for you and others to test any changes to your app. Are you working on a complicated PR and want to have testers put your app through its paces? Send them the staging link.

It's nice to have all this integrated in Azure, especially if that's where you do a lot of business. But why not just use GitHub Pages if you don't have a lot of complexity? That's a fair question. With the [just announced Blazor Web Assembly support](https://devblogs.microsoft.com/aspnet/azure-static-web-apps-with-blazor/), Azure Static Web Apps is the clear winner. With Azure Static Web Apps, the GitHub Actions step becomes aware of a Blazor WebAssembly app and can do Blazor-specific precompression steps. Otherwise, you'd have to integrate an additional workflow to your app. With Azure Static Web Apps, it's available out of the box.

## 👨‍💻 Provide input on .NET 6

With the general release of .NET 5 not even a month away, Microsoft is setting its sights on .NET 6.

Check out [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/26625) to provide feedback on what features you want to see. Unsurprisingly, [Blazor AoT compilation is leading the charge](https://github.com/dotnet/aspnetcore/issues/5466). As a whole, the ASP.NET folks have noted that speeding up the developer feedback loop is a big priority for .NET 6.

## 🎂 Happy birthday, Mom

Before we get into the links: I quickly wanted to wish my mom a very Happy Birthday, as she turns ... 30? I love you, Mom. And to think I would never get you back for all the embarrassing moments.

Here's a picture of us when I was young. I think she's trying to convince me to use Kubernetes.

![Picture with mom]({{ site.url }}{{ site.baseurl }}/assets/img/mom.jpg)

## 🌎 Last week in the .NET world

### 🔥 The Top 3

* Michael Shpilt [writes about best practices to keep a .NET app's memory healthy](https://michaelscodingspot.com/application-memory-health/).
* Patrick Smacchia [talks about the new 'and' or 'not' C# 9 keywords](https://blog.ndepend.com/csharp9-new-keywords-for-pattern-matching-and-or-not/).
* Andrew Lock [continues his series on k8s by adding health checks with liveness, readiness, and startup probes](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-6-adding-health-checks-with-liveness-readiness-and-startup-probes/).

### 📢 Announcements

* David Pine [shares an *amazing* Azure Cosmos DB Repository .NET SDK](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-db-repository-net-sdk-v-1-0-4).
* Tara Overfield [runs through the October .NET Framework cumulative update](https://devblogs.microsoft.com/dotnet/net-framework-october-1-2020-cumulative-update-preview-update-for-windows-10-version-2004-and-windows-server-version-2004).

### 📅 Community and events

* The .NET Conf [has announced their speakers](https://www.dotnetconf.net/).
* The GitHub Docs [are now open source](https://github.blog/2020-10-07-github-docs-are-now-open-source/).
* ESLint offers [an interesting perspective on paying contributors](https://eslint.org/blog/2020/10/year-paying-contributors-review).
* The .NET Docs Show [talks to Julie Lerman](https://www.youtube.com/watch?v=4wCPzfd2wIY).
* In the .NET community standups: Languages & Runtime [talks about source generators](https://www.youtube.com/watch?v=A4479Etdx4I), Machine Learning [talks about contributing to ML.NET](https://www.youtube.com/watch?v=IpW0tan7Ts4), and ASP.NET [talks about Blazor support for Azure Static Web Apps](https://www.youtube.com/watch?v=8xsp6Z_HjIg&t=1s).

### 😎 ASP.NET Core / Blazor

* Marinko Spasojevic [writes about refresh tokens with Blazor WASM and ASP.NET Core Web API](https://code-maze.com/refresh-token-with-blazor-webassembly-and-asp-net-core-web-api/).
* Jon Hilton [prerenders Blazor WASM with .NET 5](https://jonhilton.net/blazor-wasm-prerendering/), [works with Blazor CSS isolation](https://jonhilton.net/blazor-css-isolation/), and [updates the HTML head from Blazor components](https://jonhilton.net/blazor-update-html-head/).
* Chris Sainty [builds a simple tooltip component in Blazor](https://chrissainty.com/building-a-simple-tooltip-component-for-blazor-in-under-10-lines-of-code/).
* Peter Vogel [works with the Telerik UI for Blazor DataGrid](https://www.telerik.com/blogs/retrieving-data-as-you-need-it-telerik-ui-for-blazor-datagrid).
* Bogdan Chorniy [talks about Blazor form validation internals](https://medium.com/swlh/blazor-form-validation-mechanics-overview-f5ed02abd9d1).
* Khalid Abuhakmeh discusses [ASP.NET Razor TagHelpers](https://khalidabuhakmeh.com/enrich-html-aspnet-razor-taghelpers).
* At the CodeMaze blog, [how to publish Angular with ASP.NET Core](https://code-maze.com/how-to-publish-angular-with-aspnet/).

### ⛅ The cloud

* Derek Legenzoff [talks about how you can add search to an app with the new Azure Cognitive Search SDK](https://devblogs.microsoft.com/azure-sdk/search-app-with-cognitive-search).
* Damien Bowden [uses Key Vault certs with Microsoft.Identity.Web and ASP.NET Core apps](https://damienbod.com/2020/10/09/using-key-vault-certificates-with-microsoft-identity-web-and-asp-net-core-applications/).
* Gunnar Peipman [runs ASP.NET Core 5 RC apps on Azure App Service](https://gunnarpeipman.com/aspnet-core-5-rc-azure-app-service/).
* Chris Noring [deploys a Blazor app with Azure Static Web Apps](https://techcommunity.microsoft.com/t5/apps-on-azure/deploy-your-net-blazor-app-in-minutes-with-azure-static-web-apps/ba-p/1739102).

### 📔 C#

* Matthew Jones [works on code blocks, basic statements, and loops in C#](https://exceptionnotfound.net/csharp-in-simple-terms-5-basic-statements-and-loops), [talks about operators in C#](https://exceptionnotfound.net/csharp-in-simple-terms-4-operators/), and also [works with casting, conversion, and parsing in C#](https://exceptionnotfound.net/csharp-in-simple-terms-3-casting-conversion-parsing-is-as-and-typeof/).
* Jason Gaylord [exports Bitly links using C#](https://www.jasongaylord.com/blog/2020/10/09/export-bitly-links-to-json-using-dotnet).
* Nick Randolph [debugs C# 9 source generators](https://nicksnettravels.builttoroam.com/debug-code-gen).
* Jennifer Marsh [does web scraping with C#](https://www.scrapingbee.com/blog/web-scraping-csharp/).
* Anthony Giretti [explores the System.Net.Http.Json namespace in .NET 5](https://anthonygiretti.com/2020/10/03/net-5-exploring-system-net-http-json-namespace/).
* Ian Griffiths [warns against misusing 'as' in C# 8](https://endjin.com/blog/2020/10/dotnet-csharp-8-nullable-references-prepare-do-not-misuse-as-keyword.html).
* Steve Gordon writes about `System.Threading.Channels.UnboundedChannel<T>` ([part 1](https://www.stevejgordon.co.uk/dotnet-internals-system-threading-channels-unboundedchannel-part-1), [part 2](https://www.stevejgordon.co.uk/dotnet-internals-system-threading-channels-unboundedchannel-part-2)).
* Kirtesh Shah [discusses init-only properties in C# 9](https://www.c-sharpcorner.com/article/c-9-0-introductions-to-init-only-properties/).
* Asma Khalid [builds web URL query parameters in C#](https://www.asmak9.com/2020/10/cnet-how-to-build-web-url-query.html).
* Josef Ottosson [tidies up HttpClient usage](https://josef.codes/tidy-up-your-httpclient-usage/), and [throttles outgoing HTTP requests in C#](https://josef.codes/c-sharp-throttle-http-requests-concurrent/).
* Bruno Sonnino [mocks non-virtual methods of a class](https://blogs.msmvps.com/bsonnino/2020/10/04/mocking-non-virtual-methods-of-a-class/).

### 📗 F#

* Jeremie Chassaing works with applicative computation expressions in F# in [two](https://thinkbeforecoding.com/post/2020/10/07/applicative-computation-expressions) [posts](https://thinkbeforecoding-uk.azurewebsites.net/post/2020/10/08/applicative-computation-expressions-2).
* Alican Demirtas [works with React components in F#](https://www.compositional-it.com/news-blog/working-with-react-components-in-fsharp/).
* James Randall [talks about creating FableTrek](https://www.azurefromthetrenches.com/creating-fabletrek-part-1/).
* Callum Linington [gets a Node.js experience in F#](https://www.linkedin.com/pulse/f-get-nodejs-experience-callum-linington/).

### 🔧 Tools

* Dave Brock (ahem) [starts a series on Docker for .NET developers](https://daveabrock.com/2020/10/10/docker-aspnet-core-intro).
* Mark Downie [discusses cross-platform managed memory dump debugging in Visual Studio](https://devblogs.microsoft.com/visualstudio/linux-managed-memory-dump-debugging/), [collects dumps with dotnet-monitor](https://www.poppastring.com/blog/collecting-dumps-anywhere-with-dotnetmonitor), and [finds the address of an object in Visual Studio](https://www.poppastring.com/blog/find-the-address-of-an-object-in-visual-studio).
* John Petersen [discusses interactive unit testing with .NET Core and VS Code](https://codemag.com/Article/2009101/Interactive-Unit-Testing-with-.NET-Core-and-VS-Code).
* Franco Tiveron [deploys a .NET container with Azure DevOps](https://developer.okta.com/blog/2020/10/07/dotnet-container-azure-devops).
* Jason Gaylord [uses SpecFlow, nUnit and .NET Core in VSCode](https://www.jasongaylord.com/blog/2020/10/07/setup-specflow-nunit-vscode).
* Scott Hanselman [uses autocomplete at the command line for dotnet, git, winget, and more](https://www.hanselman.com/blog/HowToUseAutocompleteAtTheCommandLineForDotnetGitWingetAndMore.aspx), and also [works with panes in Windows Terminal](https://www.hanselman.com/blog/HowToUseOpenResizeAndSplitPanesInTheWindowsTerminal.aspx).
* Thomas Ardal [compares .NET mocking libraries](https://blog.elmah.io/moq-vs-nsubstitute-vs-fakeiteasy-which-one-to-choose/).
* Mark Seemann [doesn't squash his commits](https://blog.ploeh.dk/2020/10/05/fortunately-i-dont-squash-my-commits/).
* Jon Smith [creates a .NET Core global tool](https://solrevdev.com/2020/10/05/creating-a.net-core-global-tool.html).
* Ron Powell asks: [do I really need Kubernetes](https://thenewstack.io/do-i-really-need-kubernetes/)?
* Rick Strahl [creates custom .NET project types with dotnet new project templates](https://weblog.west-wind.com/posts/2020/Oct/05/Creating-a-dotnet-new-Project-Template).
* Michał Białecki [configures relationships in Entity Framework Core 5](https://www.michalbialecki.com/2020/10/02/how-to-configure-relationships-in-entity-framework-core-5/).

### 📱 Xamarin

* James Montemagno [previews Xamarin.Essentials 1.6](https://devblogs.microsoft.com/xamarin/xamarin-essentials-1-6-preview).
* Jean-Marie Alfonsi [works with shadows and MaterialFrame blur](https://www.sharpnado.com/shadows-and-materialframe-performance-updates/).
* Steven Thewissen [provides negative numeric input on Xamarin.Forms iOS](https://www.thewissen.io/how-to-provide-negative-numeric-input-on-xamarin-forms-ios/).
* Nigel Ferrissey [writes about when to Use Xamarin.Forms vs Xamarin Native](https://www.telerik.com/blogs/when-to-use-xamarin-forms-vs-xamarin-native).

### 🎤 Podcasts

* Scott Hanselman [talks about normalizing failure with Susana Benavidez](https://hanselminutes.simplecast.com/episodes/normalizing-failure-with-susana-benavidez-SzXBgV6Y).
* .NET Rocks [talks about GitHub Codespaces with Anthony van der Hoorn](https://www.dotnetrocks.com/default.aspx?ShowNum=1708).
* The 6-Figure Developer podcast [talks about .NET MAUI with Auri Rahimzadeh](https://6figuredev.com/podcast/episode-164-net-maui-with-auri-rahimzadeh/).
* The .NET Core Show [talks Azure and live conferences with Andy Morrell](https://dotnetcore.show/episode-61-azure-and-live-conferences-with-andy-morrell/).

### 🎥 Videos

* The ON.NET show discusses [.NET microservices with DAPR](https://www.youtube.com/watch?v=TeHVd3UlfY8), [default literal expressions in C#](https://www.youtube.com/watch?v=xgxudnuWkPw), and [C# 9 language features](https://www.youtube.com/watch?v=qiuzCWwYe0Y).
* The Xamarin Show [talks about binding tools for Swift](https://channel9.msdn.com/Shows/XamarinShow/Binding-Tools-for-Swift--The-Xamarin-Show).
* Data Exposed [discusses Azure SQL capacity planning](https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Capacity-Planning-Scenarios).
