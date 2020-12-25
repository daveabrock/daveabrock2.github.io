---
date: "2020-11-13"
title: "The .NET Stacks #25: .NET 5 officially launches tomorrow"
tags: [dotnet-stacks]
comments: false
share-img: /assets/img/stacks-25-card.png
subtitle: This week, .NET 5 ships and are C# 9 records actually immutable by default?
---

![Newsletter image](https://daveabrock.com/THE .NET STACKS.png)

With .NET 5 shipping this week, it's going to be such a release.

On tap this week:

* .NET 5 officially launches tomorrow
* Are C# 9 records actually immutable by default?
* Last week in the .NET world

# .NET 5 officially launches tomorrow

After eight preview releases, two release candidates, and some tears—by me, working on the early preview bits—it's finally happening: .NET 5 ships tomorrow. Of course, the release candidates shipped with a go-live license but everything becomes official tomorrow. You'll be able to download official bits and we [might even see it working with Azure App Service](https://twitter.com/coolcsh/status/1325218584272936960) (no pressure). There's also .NET Conf, where you'll get to [geek out on .NET 5 for three straight days](https://www.dotnetconf.net/).

It's been a long time coming—and it feels a bit longer with all this COVID-19 mess, doesn't it? Even so, the "Introducing .NET 5" post [hit about 18 months ago](https://devblogs.microsoft.com/dotnet/introducing-net-5/)—and with it, the promise of a unified platform (with Xamarin hitting .NET 6 because of pandemic-related resourcing constraints). We're talking a single .NET runtime and framework however you're developing an app, and a consistent release schedule (major releases every November).

Of course, this leads to questions about what it means to say .NET Framework, .NET Standard, and .NET Core—not to mention how the support will work. I covered [those not-sexy-but-essential questions two weeks ago](https://daveabrock.com/2020/10/31/dotnet-stacks-23).

Since I started this newsletter in May (we're at Issue #25!), we've been dissecting what's new week by week. (You can [look at the archives on my site](https://daveabrock.com/tags/#dotnet-stacks).) For a condensed version, here's my favorite things about .NET 5—both big and small. (This is a little like talking about your favorite song or movie, so your opinions might differ.)

## Custom JSON console logger

ASP.NET Core now ships with a [built-in JSON formatter](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/#json-console-logger) that emits *structured* JSON logs to the console. Is this a huge change? No. Will it make my life easier on a daily basis. You know it.

## Enhanced `dotnet watch` support

In .NET 5, running `dotnet watch` on an ASP.NET Core project now launches the default browser and auto-refreshes on save. This is a great quality-of-life developer experience improvement, as we patiently await for this to hit Visual Studio.

## Open API spec on by default for ASP.NET Core projects

When you create a new API project using `dotnet new webapi`, [you'll see OpenAPI output enabled by default](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#open-api-specification-on-by-default)—meaning you won't have to manually configure the Swashbuckle library and the Swagger UI page is enabled in development mode. This also means that F5 now [takes you straight to the Swagger page instead of a lonely 404](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#better-f5-experience-for-web-api-projects).

## Performance improvements

If you have some time to kill and aren't scared off by low-level details, I'd highly recommend [Stephen Toub's July post on .NET 5 performance improvements](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-5/). In short, text operations are 3x-5x faster, regular expressions are 7x faster with multiline expressions, serializing arrays and complex types in JSON are faster by 2x-5x, and much more.

## EF Core 5 updates

EF Core 5 is also shipping with a ton of new features. [Where to begin](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/plan)? Well, there's many-to-many navigation properties (skip navigations), table-per-type inheritance mapping, filtered and split includes, general query enhancements, event counters, `SaveChanges` events, savepoints, split queries for related collections, database collations, and ... well, [check out the What's New doc for the full treatment](https://docs.microsoft.com/ef/core/what-is-new/ef-core-5.0/whatsnew).

## Single file apps

Single-file apps are now supported with .NET 5, meaning you can publish and distribute an app in a single executable. [Hooray](https://github.com/dotnet/runtime/issues/36590).

## Blazor updates

.NET 5 ships with a ton of Blazor improvements, including [CSS isolation](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/#css-isolation-for-blazor-components), [JavaScript isolation](https://docs.microsoft.com/aspnet/core/blazor/call-javascript-from-dotnet?view=aspnetcore-5.0#blazor-javascript-isolation-and-object-references), [component virtualization](https://docs.microsoft.com/aspnet/core/blazor/components/virtualization), [toggle event support](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-support-for-ontoggle-event), [switching WebAssembly apps from Mono to .NET 5](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-7/#blazor-webassembly-apps-now-target-net-5), [lazy loading](https://docs.microsoft.com/aspnet/core/blazor/webassembly-lazy-load-assemblies), [server pre-rendering](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-prerendering), and so on.

## C# 9

I'm super excited for the release of C# 9, especially the embracing of paradigms common in functional programming. With [init-only properties](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits) and [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), C# developers have the flexibility to easily use immutable constructs in a mutable-by-design language. It might take some getting used to, but I love the possibilities. I see a lot of promise in keep the real-world object modeling, but also introducing immutability where treating objects as data makes sense.

(There's also great improvements with [top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs), [pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching), and [target typing](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants).)

# Are C# 9 records actually immutable by default?

Speaking of records, there seems to be some confusion on if C# 9 records are immutable by default. The answer is: yes, and no. Records are immutable by default when using positional arguments. When you want to use them with object initializers, they aren't—which makes sense, since initializers are more flexible in how objects are constructed.

Feel free to [check out my post for the full details](https://daveabrock.com/2020/11/02/csharp-9-records-immutable-default).

# Happy birthday, Dad

Someone has turned a lucky ... um ... well, numbers don't matter, do they? Happy Birthday, Dad. You can thank him for ... *waves hands* ... all this. I'm not sure what I'd be doing without him encouraging my nerdy tendencies during my most awkward years, but I'm glad I'll never have to find out. Have a good day, mister. 

When I said I like to wear many hats, I didn't mean this one.

![Picture with mom]({{ site.url }}{{ site.baseurl }}/assets/img/dad-and-me.jpeg)

(Don't worry, this is the last birthday wish. Back to regularly scheduled programming.)

# 🌎 Last week in the .NET world

## 🔥 The Top 3

* Beth Massi [previews .NET Conf from November 10-12](https://devblogs.microsoft.com/dotnet/net-5-0-launches-at-net-conf-november-10-12).
* Damien Bowden [implements a Blazor full text search using Azure Cognitive Search](https://damienbod.com/2020/11/02/implement-a-blazor-full-text-search-using-azure-cognitive-search/).
* Irina Scurtu [works with .NET Core with nginx on Linux](https://irina.codes/net-core-with-nginx-on-linux/), and also [talks about using a .NET API as a Linux service](https://irina.codes/net-api-as-a-linux-service/).

## 📢 Announcements

* Eilon Lipton [runs through preview 5 of Blazor Mobile Bindings](https://devblogs.microsoft.com/aspnet/unified-ui-mobile-blazor-bindings-preview-5).
* AWS has [stopped supporting AWS SDK for .NET version 1](https://aws.amazon.com/blogs/developer/announcing-the-end-of-support-for-the-aws-sdk-for-net-version-1).

## 📅 Community and events

* Microsoft Learn has launched a [.NET Learn Challenge](https://docs.microsoft.com/learn/challenges?id=58bdb63e-96b6-4598-afd1-d350438476b5&WT.mc_id=dotnet-10219-masalnik&_lrsc=89013caa-3a23-4196-b218-d4c4e53fa883).
* The .NET Docs show [talks about navigating ML.NET with Bri Achtman and Luis Quintanilla](https://www.youtube.com/watch?v=JBvGX2k23Jc).
* Just two .NET standups this week: Machine Learning [talks TorchSharp & Tensor programming](https://www.youtube.com/watch?v=0DNWLorVIh4), and Xamarin [discusses Mobile Blazor Bindings](https://www.youtube.com/watch?v=Y0bEAy6yxMQ).

## 😎 ASP.NET Core / Blazor

* Dave Brock (ahem) [talks about updating the HTML head from a Blazor component](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head).
* Jon Hilton [talks about migrating from MVC to Blazor](https://www.telerik.com/blogs/migrating-mvc-to-blazor).
* Marinko Spasojevic [uses browser functionalities](https://code-maze.com/use-browser-functionalities-with-blazor-webassembly/) and also [calls C# methods from JavaScript in Blazor WebAssembly](https://code-maze.com/how-to-call-csharp-methods-from-javascript-in-blazor-webassembly/).
* Kristoffer Strube [sends push notifications to a browser in ASP.NET Core](https://blog.elmah.io/how-to-send-push-notifications-to-a-browser-in-asp-net-core/).
* Paul Michaels [documents some recent tag helper issues](https://www.pmichaels.net/2020/10/31/tag-helper-not-working/).
* Sam Xu [introduces ASP.NET Core OData 8.0 routing](https://devblogs.microsoft.com/odata/routing-in-asp-net-core-8-0-preview).
* Imar Spaanjaars [discusses implementing health checks in ASP.NET Framework apps](https://imar.spaanjaars.com/609/implementing-health-checks-in-aspnet-framework-applications).
* Jason Gaylord discusses building APIs with GraphQL and .NET Core [post 1](https://www.jasongaylord.com/blog/2020/11/03/build-api-graphql-dotnet-core-part-1), [post 2](https://www.jasongaylord.com/blog/2020/11/04/build-api-graphql-dotnet-core-part-2).
* Ricardo Peres [talks about some gotchas with ASP.NET Core areas](https://weblogs.asp.net/ricardoperes/asp-net-core-pitfalls-areas).

## ⛅ The cloud

* David Grace [deals with ASP.NET Core Web API access restrictions and errors in Azure](https://www.roundthecode.com/dotnet/asp-net-core-web-hosting/dealing-asp-net-core-web-api-access-restrictions-errors-azure).
* Justin Yoo [writes about GitHub Actions, DNS, and SSL certs on Azure Functions](https://techcommunity.microsoft.com/t5/apps-on-azure/github-actions-dns-amp-ssl-certificate-on-azure-functions).
* Tore Nestenius [stores the ASP.NET Core Data Protection Key Ring in Azure Key Vault](https://www.edument.se/en/blog/post/storing-the-asp-net-core-data-protection-key-ring-in-azure-key-vault).
* Laurent Bugnion [talks about the best practice when naming Durable Functions in C#](https://galasoft.ch/posts/2020/11/best-practice-when-naming-durable-functions-in-c).
* Joe "Hulk Hogan" Guadagno works with [NLog, dependency injection, and Azure Functions](https://www.josephguadagno.net/2020/11/03/nlog-dependency-injection-and-azure-functions-oh-my).

## 📔 Languages

* Dave Brock (ahem) asks: [are C# 9 records immutable by default](https://daveabrock.com/2020/11/02/csharp-9-records-immutable-default)?
* Anthony Giretti [works with GetEnumerator extension support for foreach in C# 9](https://anthonygiretti.com/2020/11/01/introducing-c-9-extension-getenumerator-support-for-foreach-loops/).
* Jiří Činčura [discusses the new Environment.ProcessId in .NET 5](https://www.tabsoverspaces.com/233842-new-environment-processid-in-net-5).
* Steve Gordon [discusses additional HTTP, sockets, DNS, and TLS telemetry in .NET 5](https://www.stevejgordon.co.uk/additional-http-sockets-dns-and-tls-telemetry-in-dotnet-5).
* Thomas Levensque [uses C# 9 records as strongly-typed IDs](https://thomaslevesque.com/2020/10/30/using-csharp-9-records-as-strongly-typed-ids/).
* Khalid Abuhakmeh [takes a look at System.IO.Path](https://khalidabuhakmeh.com/looking-at-system-io-path).

## 🔧 Tools

* Derek Comartin [talks about CQRS myths](https://codeopinion.com/cqrs-myths-3-most-common-misconceptions).
* Tim Heuer [generates a GitHub Actions workflow file from the dotnet CLI](https://timheuer.com/blog/generate-github-actions-workflow-from-cli/).
* Kunal Chowdhury [offers some advanced Git tips](https://www.kunal-chowdhury.com/2020/11/advanced-git-tips.html).

## 📱 Xamarin

* Vicente Gerardo Guzmán Lucio [protects sensitive data in the background](https://www.syncfusion.com/blogs/post/protecting-sensitive-data-in-the-background-in-xamarin-forms.aspx).
* Leomaris Reyes [walks through the best Xamarin Forms app examples](https://www.telerik.com/blogs/best-xamarin-forms-app-examples), and also [gets started with text-to-speech](https://www.telerik.com/blogs/getting-started-with-text-to-speech-in-xamarin-forms).

## 🎤 Podcasts

The .NET Rocks podcast [discusses Cake 1.0 with Mattias Karlsson](https://www.dotnetrocks.com/default.aspx?ShowNum=1712).

## 🎥 Videos

* The Loosely Coupled Show [asks: should you learn a functional programming language](https://www.youtube.com/watch?v=pZx0X2hDx7A)?
* The Visual Studio Toolbox [talks about GitHub Codespaces](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/GitHub-Codespaces).
* The folks at Compositional IT [talk quickly about SAFE Stack web apps pn Azure Static Web Apps](https://www.youtube.com/watch?v=kLoLl3kqpfk&feature=youtu.be&ab_channel=CompositionalIT).
* Over at ON.NET, they talk about [the inner loop with VS Code and GitHub](https://www.youtube.com/watch?v=GfTlbjuWU5Q), and also [real-time data fetching with GraphQL and Blazor](https://www.youtube.com/watch?v=iDAYdrFHqGU).
