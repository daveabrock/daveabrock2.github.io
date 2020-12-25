---
date: "2020-09-04"
title: "NDepend: Boost Your Team's Code Quality"
tags: [tools]
share-img: /assets/img/ndepend-card.png
subtitle: I take a look at what's new with NDepend, the robust static code analysis tool.
---

Have you used NDepend before? If you've been working on .NET for some time, you may have either used it or heard of it—I believe it's been around since 2007 or so.

If you aren't familiar, NDepend is a robust static code analysis tool that allows you to gain a concrete understanding of your project's complexity, technical debt, associations, dependencies, and more. You can easily integrate NDepend into your daily workflow with a Visual Studio extension and it also has extensions for your build pipelines as well.

I gave NDepend 2020.1 a test spin and have been using it the last few weeks. I'll spend the rest of the post writing about my experiences.

I couldn't possibly go through **all** the features and functionality that NDepend offers, so this post will show off what caught my eye. You can [take a look at the docs for the full treatment](https://www.ndepend.com/docs/getting-started-with-ndepend).

*Disclaimer*: I am reviewing NDepend with a free review license.
{: .notice--info}

This post covers the following topics.

- [Attach NDepend to your .NET solution](#attach-ndepend-to-your-net-solution)
- [Review the NDepend dashboard](#review-the-ndepend-dashboard)
  - [Technical debt severity levels](#technical-debt-severity-levels)
- [Visualizing complexity](#visualizing-complexity)
- [Understand dependencies](#understand-dependencies)
- [Query code with the CQLinq syntax](#query-code-with-the-cqlinq-syntax)
- [Integrate NDepend with your continuous integration pipeline](#integrate-ndepend-with-your-continuous-integration-pipeline)
- [Wrap up](#wrap-up)

# Attach NDepend to your .NET solution

Once you install NDepend, the easiest way to get started is to attach NDepend to an existing .NET solution in Visual Studio.

In your version of Visual Studio, enter **ndepend** in your global search bar at the top, and click **Analyze a VS solution**. Browse to a solution file you want NDepend to analyze, then click **OK**. At the next screen, select the assemblies you want analyzed, and let NDepend loose.

The first time you analyze, a beginner-friendly screen will ask you how you want to proceed. Click **View NDepend Dashboard**.

![go to NDepend dashboard]({{ site.url }}{{ site.baseurl }}/assets/img/ndepend-what-to-do.png)

# Review the NDepend dashboard

The dashboard gives you a nice view of overall tech debt, quality gates, and issue and rule estimation. My sample app needs some work but ... in my career, I've definitely seen worse.

![go to NDepend dashboard]({{ site.url }}{{ site.baseurl }}/assets/img/dashboard.png)

You can drill into the dashboard to get details about any violations.

## Technical debt severity levels

If you're wondering about the NDepend severity levels, here's some guides:

* **Info** - a small improvement can be made
* **Minor** - a type of warning that won't have a significant impact if not addressed
* **Major** - should be fixed quickly but can wait until the next run
* **Critical** - these issues shouldn't move to production
* **Blocker** - it must be fixed

Once you improve your code, you can rerun the analysis again and NDepend will show you how you improved from the last baseline.

# Visualizing complexity

It's been a few years since I last worked with NDepend, and a welcome new addition I noticed is the visualization of cyclomatic complexity. While it doesn't tell the whole story, high complexity numbers are typically bad news for code quality and testability.

Here you'll see a nice visualization view where you can hover over "trouble spots" in your application. In the top-left of the image, you'll see details about the offending method (I hovered over the redness).

![visualize code complexity]({{ site.url }}{{ site.baseurl }}/assets/img/visualizer.png)

This capability isn't just for cyclomatic complexity—you can do it for documentation (comments), test coverage, and more. It's a great way to see the high-level state of your application.

I can personally see a lot of value in using the [code coverage visualization](https://www.ndepend.com/docs/monitor-test-coverage#identify-which-code-need-more-test), as it easily shows which components are in need of more coverage.

# Understand dependencies

NDepend ships with a dependencies view that allows you to visualize the relationships between your projects. It's an interactive view, where you can click and hover over dependencies, to see how tightly or loosely coupled your dependencies are.

![visualize code complexity]({{ site.url }}{{ site.baseurl }}/assets/img/graph.png)

If you tell your boss something is taking a while because it's a "bowl of spaghetti" and he or she wants evidence, here's where you can get it.

# Query code with the CQLinq syntax

NDepend offers Code Query LINQ (CQLinq) to query .NET code through the use of LINQ queries. If you want greater control of the metrics, or want a one-off result, it's a great capability. You can use any C# LINQ syntax.

Let's say I want to find any methods where the cyclomatic complexity is greater than 10.

The first thing I notice is the Intellisense-like previews as I type:

![cqlinq]({{ site.url }}{{ site.baseurl }}/assets/img/intellisense.png)

Now, I can execute the query like this:

```csharp
from m in Methods where m.CyclomaticComplexity > 10
select new { m, m.CyclomaticComplexity }
```

I don't need to click a Run button or anything—the results will populate automatically.

![cqlinq]({{ site.url }}{{ site.baseurl }}/assets/img/cqlinq-results.png)

I can run this just once, or even save my queries for later use. As a C# developer you're likely familiar with the LINQ syntax, so this offers a great and quick way for you to grab just the data you need.

# Integrate NDepend with your continuous integration pipeline

To make the greatest impact to your team, you can use NDepend with your CI tools—it supports any major CI tool like Azure DevOps/TFS, TeamCity, Jenkins, SonarQube, and more. You will need to purchase a [Build Machine Edition *or* Azure DevOps/TFS Edition license for this capability](https://www.ndepend.com/editions).

If you are using something like Azure DevOps, you can use an extension that contains a build task. This will produce the dashboard at a moment's notice, just like we saw in our local Visual Studio.

Much like how you can enforce quality gates in your CI tooling for code coverage and coding conventions, you can also do for NDepend. The extension ships with a dozen quality gates for things such as technical debt amount or issues with a particular severity—for example, you could enforce a gate where developers don't push any code with Major or Critical issues. The quality gates are also C# LINQ queries, so you [can easily tweak existing gates or create new ones](https://mailstat.us/tr/t/2elpw4ntkesbnhaa/9/https://www.ndepend.com/docs/quality-gates#Customization).

On top of enforcing code quality across the team, you have the advantage of the metrics in one place (and not scattered across dev machines) and also can report progress up the chain, if needed.

This is a powerful capability and if you're paying for NDepend already, you should give this serious consideration. This will morph your use of NDepend from "we should really fix those issues" to "we've set a standard to not introduce anything to our codebase with these issues"—a huge difference.

Take a look at the [NDepend CI documentation](https://www.ndepend.com/docs/azure-devops-tfs-vsts-integration-ndepend) for more details.

# Wrap up

In this article, I took a test drive through the latest version of NDepend. Once we attached NDepend to a sample solution, we discussed the NDepend dashboard, complexity visualization, the dependency graphs, querying code with the CQLinq syntax, and reviewed its CI capabilities.

I was happy to get acquainted with NDepend once again and take a look at the improvements it has made over the years, and see that it's still the best-in-class tool for .NET static analysis. If you have the budget for it, you can't go wrong.
