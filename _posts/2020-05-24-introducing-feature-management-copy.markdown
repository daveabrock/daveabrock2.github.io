---
date: "2020-05-24"
title: "Introducing the Microsoft.FeatureManagement library"
subtitle: Use Microsoft.FeatureManagement to add native feature flags!
tags: [csharp, dotnet-core, feature-flags]
---

A couple of weeks ago, I needed to find a way to better manage new functionality for a feature that was not ready yet (it broke stuff). We didn't know exactly when the feature would be shipped, but we also didn't want to deal with branch and merging headaches when the moment came. 

My first thought: this was a perfect candidate for [feature flags](https://www.martinfowler.com/articles/feature-toggles.html) (or feature toggles). This allows you to merge code into a production branch, but only enable when ready. Instead of waiting for developers to be ready, you're waiting for the end user to be ready.

Instead of writing this logic and having to deal with the corner case headaches, I knew Microsoft [shipped](https://github.com/microsoft/FeatureManagement-Dotnet) a `Microsoft.FeatureManagement` library. 

Unfortunately, that was all I knew and took this as a great opportunity to dig deeper. I learned a ton and will be dividing my new-found knowledge into four blog posts over the next several weeks.

I'll be writing a few posts on this topic:

- Part 1, this post: Introducing the `Microsoft.FeatureManagement` library
- Part 2: [Use Microsoft.FeatureManagement.AspNetCore to filter actions and HTML](https://daveabrock.com/2020/05/30/introducing-feature-management-aspnetcore)
- Part 3: [Implement custom filters in your ASP.NET Core feature flags](https://daveabrock.com/2020/06/07/custom-filters-in-core-flags)
- Part 4: [Manage feature flags with Azure App Configuration](https://daveabrock.com/2020/06/17/use-feature-flags-azure-app-config)

# Create sample application and add NuGet packages

For this post, I'll be using an ASP.NET Core Web Application with the traditional `Model-View-Controller` scaffolding. If you want to code along with me, make sure to create a new project. You can do this from the Visual Studio UI, or if VS Code is your style, the `dotnet` CLI by executing `dotnet new mvc`.
Once you create your project, you'll need to install `Microsoft.FeatureManagement`. You can do this in one of two ways:

- From the NuGet Package Manager (easiest way is to right-click your solution in Visual Studio, then clicking **Manage NuGet Packages**)
- From the `dotnet` CLI, you can execute the `dotnet add package Microsoft.FeatureManagement` command

(While we are using an ASP.NET Core application, the library depends on .NET Standard 2.0—so even non-.NET Core applications can benefit.)

Once the package is installed, update the dependency injection in `Startup.cs` to the following:

```csharp
using Microsoft.FeatureManagement;
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddFeatureManagement();
}
```

You're now ready to get this party started!

# A simple example

To get the gist of feature flags, we can just update the default message for the app that ships with the default ASP.NET Core scaffolding. Out of the box, the message says, `Welcome`. Let's change it so that the message says `Welcome to Feature Flags` when the functionality is enabled.
The `Microsoft.FeatureManagement` library will check your `appsettings.json` file for its configuration. By default, it will look for a `FeatureManagement` section. Let's add an entry now.

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
    "WelcomeText": false
  },
  "AllowedHosts": "*"
}
```

In your `Models` folder, create an `IndexViewModel.cs` file and include the `Message` property.

```csharp
public class IndexViewModel
{
    public string Message { get; set; }
}
```

Then, in `Views/Home/Index.cshtml`, update the file to this:

```html
@model IndexViewModel
@{
    ViewData["Title"] = "Home Page";
}
<div class="text-center">
    <h1 class="display-4">@Model.Message</h1>
    <p>Learn about <a href="https://docs.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
</div>
```

Here, we are including the model we created, and rendering its `Message` property. We will soon introduce logic to populate the message with a certain value, whether feature flags are enabled in our application.
Next, let's open up `HomeController.cs` and implement the `IFeatureManager` service by injecting it into the constructor.

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;
    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
    // stuff we'll include soon
}
```

The `IFeatureManager` interface looks like this:

```csharp
public interface IFeatureManager
{
    IAsyncEnumerable<string> GetFeatureNamesAsync();
    Task<bool> IsEnabledAsync(string feature);
    Task<bool> IsEnabledAsync<TContext>(string feature, TContext context);
}
```

We'll use `IsEnabledAsync` to check for the feature we specified in `appsettings.json`. We could pass the string directly, but a better way would be to add a class to store these strings. To do that, create a file in the root of the project called `FeatureFlags.cs`.

```csharp
public static class FeatureFlags
{
    public const string WelcomeMessage = "Welcome Message";
}
```

There, I feel better. Back to `HomeController.cs`, change the `Index` action method to the following:

```csharp
public async Task<IActionResult> Index()
{
    var indexViewModel = new IndexViewModel()
    {
        Message = await _featureManager.IsEnabledAsync("WelcomeMessage")
                  ? "Welcome to the feature flag"
                  : "Welcome"
    };
    return View(indexViewModel);
}
```

In the `Index` view action, we are calling `IsEnabledAsync` and passing in our flag's name from the `appsettings.json` file. If that value is `true` the `Message` will display `Welcome to the feature flag` and if not, `Welcome`. The complexity you deal with every day, right?
If you run your app, you just see the standard welcome message, because your feature flag is set to `false` in your configuration.
![Before turning on flag]({{ site.url }}{{ site.baseurl }}/assets/img/feature-flag-welcome-before.PNG)

Now, if you go to your `appsettings.json` file and set `WelcomeText` to true and restart the *page*, you'll see the feature flag in action! (You only have to restart the page, and not the app, as the configuration provider handles the change without an app restart!)

![After turning on flag]({{ site.url }}{{ site.baseurl }}/assets/img/feature-flag-welcome-after.PNG)

When thinking in terms of deployment, you can set release variables and whatnot to better manage when to turn these on. We will discuss these when we talk about integrating with Azure.
