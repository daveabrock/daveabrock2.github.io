---
date: "2020-05-30"
title: "Using Microsoft.FeatureManagement.AspNetCore to filter actions and HTML"
subtitle: Use Microsoft.FeatureManagement.AspNetCore to filter HTML and controller actions.
tags: [csharp, dotnet-core, feature-flags]
---

In our previous post, we introduced `Microsoft.FeatureManagement` as a way to [manage feature flag functionality in your .NET applications](https://daveabrock.com/2020/05/24/introducing-feature-management-copy). As mentioned in the post, this library is compatible with any .NET Standard application.

In this post, we'll kick things up a notch and show how you can pair this with an ASP.NET Core-only library, `Microsoft.FeatureManagement.AspNetCore`, to perform the following tasks with minimal required configuration:

- Filter out controller action methods and classes
- Conditionally filter out HTML in your views

This is part 2 in a four-part series on .NET native feature flags:

- Part 1: [Introducing the `Microsoft.FeatureManagement` library](https://daveabrock.com/2020/05/24/introducing-feature-management-copy)
- Part 2, this post: Use Microsoft.FeatureManagement.AspNetCore to filter actions and HTML
- Part 3: [Implement custom filters in your ASP.NET Core feature flags](https://daveabrock.com/2020/06/07/custom-filters-in-core-flags)
- Part 4: [Manage feature flags with Azure App Configuration](https://daveabrock.com/2020/06/17/use-feature-flags-azure-app-config)

This post contains the following content.

- [Set up the sample application](#set-up-the-sample-application)
- [Filter out controller action methods and classes](#filter-out-controller-action-methods-and-classes)
- [Conditionally render HTML in your views](#conditionally-render-html-in-your-views)

# Set up the sample application

If you wish to follow along, refer to the previous post for details on [how we set up our sample application](https://daveabrock.com/2020/05/24/introducing-feature-management-copy). In addition, you'll need to add the `Microsoft.FeatureManagement.AspNetCore` library by performing one of the following two steps:

- From the NuGet Package Manager (easiest way is to right-click your solution in Visual Studio, then clicking Manage NuGet Packages), search for and install `Microsoft.FeatureManagement.AspNetCore`
- From the `dotnet` CLI, you can execute the `dotnet add package Microsoft.FeatureManagement.AspNetCore` command

Here is what the project file looks like now:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.FeatureManagement" Version="2.0.0" />
    <PackageReference Include="Microsoft.FeatureManagement.AspNetCore" Version="2.0.0" />
  </ItemGroup>
</Project>
```

# Filter out controller action methods and classes

To better manage your feature flags, you can filter from the level of an action method, or even a class, using the `FeatureGate` attribute.

Before we get to the fun bits, let's set up another feature flag in our configuration. As we did in the previous post, add a new feature flag in `appsettings.json`. Let's call it `FlagController`. Here's what our configuration looks like now:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "FeatureManagement": {
    "WelcomeText": true,
    "FlagController": true 
  },
  "AllowedHosts": "*"
}
```

Note that, by default, we will set this one `true` for a cleaner demo. Then, let's go to our `FeatureFlags.cs` file and define what we created. A look at the updated file:

```csharp
public static class FeatureFlags
{
    public const string WelcomeText = "WelcomeText";
    public const string FlagController = "FlagController";
}
```

Finally, add a new controller called `FlagController` and make sure it implements the `Controller` interface. The file should look like this:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.FeatureManagement.Mvc;

namespace FeatureFlags.Controllers
{
    public class FlagController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }
    }
}
```

Finally, in your `Views` folder, create a `Flag` folder, and then a `Flag.cshtml` file with the following content. (You could have scaffolded this, but since we aren't using Entity Framework, this is probably faster.)

```html
<div class="text-center">
    <h1 class="display-4">The controller flag is enabled!</h1>
    <p>Learn about <a href="https://docs.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
</div>
```

To better manage your feature flags, you can filter from the level of an action method, or even a class, using the `FeatureGate` attribute. This attribute is super flexible: you can pass in multiple features and decide to gate actions if any or all of the features are enabled. Take a look at the FeatureGateAttribute [documentation for details](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement.mvc.featuregateattribute?view=azure-dotnet-preview).

For our example, we will annotate `[FeatureGate(FeatureFlags.FlagController)]` to our controller class. Any action methods in this class will only be accessible if this flag is set to true. Here's the latest `FlagController.cs`.

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.FeatureManagement.Mvc;

namespace FeatureFlags.Controllers
{
    [FeatureGate(FeatureFlags.FlagController)]
    public class FlagController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }
    }
}
```

If you run your app, and navigate to `http://localhost:<port>`, you will see the new view rendered appropriately.

Now, if you go to your `appsettings.json` and set the flag to `false`, then reload the page you'll get an ugly 404 page. It's ugly, but it means it works—great!

Of course, if you want to provide a better experience for your users, you can add something like `UseStatusCodePages` [middleware](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling?view=aspnetcore-3.1#usestatuscodepages) to render something friendlier.

# Conditionally render HTML in your views

Let's say you want to only render a component on your site if a certain condition applies: for example, that someone is viewing a beta environment. You could definitely mimic what we did in our controllers, right in your razor view, with something like this:

```html
@if (_featureManager.IsEnabled(FeatureFlags.Beta))
{
  <div class="beta">We are in beta!</div>
}
```

To make it easier on your life, you can use a [specialized tag helper for this](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement.mvc.taghelpers.featuretaghelper?view=azure-dotnet-preview), the `FeatureTagHelper`.

So, instead, you can try something like this:

```html
<feature name="@FeatureFlags.Beta">
    <div class="beta">We are in beta!</div>
</feature>
```

You can definitely take it further than this as well—the tag helpers allow you to have alternate content displayed in the feature is disabled (using the `Negate` property), and you can also require ANY or ALL from a feature set (using the `Requirement` property). Take a look [at the documentation](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement.mvc.taghelpers.featuretaghelper?view=azure-dotnet-preview) for details.
