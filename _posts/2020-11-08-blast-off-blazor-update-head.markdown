---
date: "2020-11-08"
title: "Blast Off with Blazor: Use .NET 5 to update the HTML head from a Blazor component"
subtitle: "In the latest post, we'll learn how to update the HTML head dynamically using .NET 5."
tags: [blazor, aspnet-core]
share-img: /assets/img/update-head.png
---

So far in this series, we've [walked through a project intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro) and also [got our feet wet with our first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page).

Today, we're going to take a look at a welcome .NET 5 feature in Blazor: the ability to [update your HTML head on the fly](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/#influencing-the-html-head-in-blazor-apps) without the need for JavaScript interoperability.

When I speak of updating the HTML head, I'm referring to what's inside the `<head>` tag in your `index.html` file. You can now use the following native Blazor components to dynamically update the HTML head:

- `<Title>` - the title of the document. You'd use this if you want to update your user on any updates, especially if they are busy with other tabs. You've probably seen this with updates for new email messages or new chat notifications.
- `<Link>` - gives you the ability to link to other resources. For example, you might want to dynamically update this to change stylesheets when the user clicks a button to specify light or dark mode.
- `<Meta>` - allows you to specify a variety of information for things like SEO, social cards, and RSS information.

Since these are individual components, you can definitely use them together in your code. Following the email example, you could update the title bar with an unread message count and also change the "you have a message" favicon appropriately.

In this post, we're going to dynamically update the HTML `<title>` tag from our Blazor component. And since we're already working with the browser's title bar, I'll walk you through adding favicons to our site.

This post contains the following content.

- [Dynamically set page title](#dynamically-set-page-title)
  - [Add package reference](#add-package-reference)
  - [Add script reference](#add-script-reference)
  - [Reference the project](#reference-the-project)
  - [Set the page title](#set-the-page-title)
  - [How it all works](#how-it-all-works)
- [Add favicons to our app](#add-favicons-to-our-app)
- [What about the tests?](#what-about-the-tests)
- [Wrap up](#wrap-up)
- [Resources](#resources)

# Dynamically set page title

As I mentioned, we're going to dynamically update the `<title>` tag of our component. Before we do that, we'll first need to add a package reference, add a script reference, and reference the new package in our project.

## Add package reference

From the `Client` directory of our project, open your terminal and add a package reference using the `dotnet` CLI.

```bash
dotnet add package Microsoft.AspNetCore.Components.Web.Extensions --prerelease
```

**Heads up!** At this time, the `--prerelease` flag is required.
{: .notice--warning}

## Add script reference

In the `index.html` file in `wwwroot`, add the following reference:

```html
<script src="_content/Microsoft.AspNetCore.Components.Web.Extensions/headManager.js"></script>
```

## Reference the project

We can use one of two options to reference the new package in our app. Our first option is a direct `@using` in the `Index.razor` component:

```html
@using Microsoft.AspNetCore.Components.Web.Extensions.Head
```

Or, we can include it in our `_Imports.razor` file. The `_Imports.razor` file, which sits at the root of our `Client` project, allows us to reference our app's imports globally. That way, we won't have to manually add shared references to all our components that need it. With my new addition, here's what my `_Imports.razor` file looks like now:

```html
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.AspNetCore.Components.WebAssembly.Http
@using Microsoft.JSInterop
@using Client
@using Client.Shared
@using Data
@using Microsoft.AspNetCore.Components.Web.Extensions.Head
```

Either method works, but I'm going with the latter option for reusability. We're now ready to set the page title.

## Set the page title

Here's what we'll do: while I'm waiting for the NASA Astronomy Picture of the Day (APOD) API call to return, our title bar will say *Blasting off...*. Then, when we get a response back, it'll say *New! From YEAR!*, where *YEAR* is the year of the image. This value comes from the `Year` property returned by the API.

In the `@code` section of our `Index.razor` component, add the following code to parse the year from our `Date` property:

```csharp
private int GetYear(DateTime date)
{
    return date.Year;
}
```

Then, we can use a one-line ternary statement to return a message based on us whether we get a response from the API.

```csharp
private string GetPageTitle()
{
    return image is null ? "Blasting off..." : $"New! From {GetYear(image.Date)}!";
}
```

Now, all that's left is to drop the `<Title>` component at the top of our markup, and call our logic to change the title text.

```html
<Title Value="@GetPageTitle()"></Title>
```

Here's a GIF of our update in action. (I set a `Thread.Sleep(1000)` for demonstration purposes, but I don't recommend it in practice obviously.)

![Our title bar in action]({{ site.url }}{{ site.baseurl }}/assets/img/SetTitleBar.gif)

## How it all works

Before .NET 5, updating the HTML head dynamically required the use of JavaScript interoperability.

You'd likely drop the following JavaScript at the bottom of your `index.html`:

```javascript
<script>
    function SetTitle(title) {
        document.title = title;
    }
</script>
```

Then, you'd have to inject JavaScript interop and probably create a special component for it:

```csharp
@inject IJSRuntime JSRuntime

// markup here...

@code {
    [Parameter]
    public string Value { get; set; }

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        await JSRuntime.InvokeVoidAsync("SetTitle", Value);
    }
}
```

Then, you'd add the component on your page to update the title bar dynamically, where your `GetPageTitle` method would do what it needs to populate the title.

```html
<PageTitle Value="@GetPageTitle()" />
```

In .NET 5, this still happens—it's just abstracted away from you. If you [take a look at the source code](https://github.com/aspnet/AspLabs/blob/master/src/Components.Web.Extensions/HeadManagement/Title.cs) for the `<Title>` component, it injects an `IJSRuntime` and takes a `Value`—just as you've done before. Then, in the `OnAfterRenderAsync` [component lifecycle method](https://docs.microsoft.com/aspnet/core/blazor/components/lifecycle?view=aspnetcore-3.1#after-component-render), it sets the title by calling the `headManager.js` file, where you'll see:

```javascript
export function setTitle(title) {
  document.title = title;
}
```

**Note**: The [`OnAfterRenderAsync` method is called](https://docs.microsoft.com/aspnet/core/blazor/components/lifecycle?view=aspnetcore-3.1#after-component-render) after a component has finished rendering.

So, this isn't doing anything too magical. It's doing it out-of-the-box and giving you one less component to maintain.

With that done, let's add favicons to our app.

# Add favicons to our app

When you create a Blazor app (or any ASP.NET Core app, for that matter), a bland `favicon.ico` file is dropped in your `wwwroot` directory. (For the uninitiated, [a favicon](https://en.wikipedia.org/wiki/Favicon) is the small icon in the top-left corner of your browser tab, right next to the page title.) This might lead you to believe that if you want to use favicons, you can either use that file or overwrite it with one of your own—then move on with your life.

While you *can* do that, you probably shouldn't. These days you're dealing with different browser requirements and multiple platforms (Windows, iOS, Mac, Android) where just one *favicon.ico* will not get the job done. For example, iOS users can pin a site to their homescreen—and iOS will grab your favicon icon as the "app" image.

Thanks to Andrew Lock's [wonderful post on the topic](https://andrewlock.net/adding-favicons-to-your-asp-net-core-website-with-realfavicongenerator/), I went over to the [RealFaviconGenerator](https://realfavicongenerator.net/) for assistance. (If you want more details on the process, like how to create your own, check out his post.)

Before we do that, we'll need to pick a new image.

Did you know the .NET team has a [branding GitHub repository](https://github.com/dotnet/brand) where you can look at logos, presentation templates, wallpapers, and a bunch of illustrations of the purple .NET bot? For our example, we'll use the [bot using a jetpack](https://github.com/dotnet/brand/blob/master/dotnet-bot-illustrations/dotnet-bot/dotnet-bot_jetpack-faceing-right.png) because—obviously. We're blasting off, after all.

Let's also update our header icon to this image, too. After dropping the file in our `wwwroot/assets/img` [directory](https://github.com/daveabrock/NASAImageOfDay/tree/main/Client/wwwroot/assets/img), we can edit our `NavBar` component in our `Shared` directory to include our new file.

```html
<nav class="flex items-center justify-between flex-wrap bg-black p-6 my-bar">
    <div class="flex items-center flex-shrink-0 text-white mr-6">
        <img src="images/dotnet-bot-jetpack.png">
        <span class="font-semibold text-xl tracking-tight">Blast Off with Blazor</span>
    </div>
    <! -- more html left out for brevity -->
</nav>
```

I feel better and, as a bonus, the fine folks at NASA don't have to worry about asking me to take their official logo down.

![Our title bar in action]({{ site.url }}{{ site.baseurl }}/assets/img/new-site-icon.png)

Now, we can head over the [RealFaviconGenerator](https://realfavicongenerator.net/) site to upload our new icon. After adjusting any settings to our liking, it'll give us a bunch of icons, which we'll unzip to the root of our `wwwroot` directory.

In `index.html` in the `wwwroot` directory, add the following inside the `<head>` tag:

```html
<link rel="apple-touch-icon" sizes="76x76" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="manifest" href="/site.webmanifest">
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5">
<meta name="msapplication-TileColor" content="#da532c">
<meta name="theme-color" content="#ffffff">
```

While it's not the worst idea to put this markup in a component or partial view, I'm fine with putting it right into my static `index.html` file—it's a one-time step and this is the only file that needs it.

# What about the tests?

I've placed an emphasis on [testing our components](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page#test-our-component-with-the-bunit-library) with the bUnit library. We'll work on testing this, and the rest of our `Index` component, in the next post. We need to mock a few things, like `HttpClient`, and I'm currently researching the best way to do so. Our next post will focus on testing Blazor components with `HttpClient` dependencies.

# Wrap up

In this post, we dynamically updated the HTML head by setting the page title based on the image. Then, we updated our header icon and also showed how to include favicons in a Blazor WebAssembly project.

Stay tuned for the next post where we work on writing tests for our `Index` component.

# Resources

- [Update the HTML head from your Blazor components](https://jonhilton.net/blazor-update-html-head/) (Jon Hilton)
- [Dynamically setting the page title in a Blazor application](https://www.meziantou.net/dynamically-setting-the-page-title-in-a-blazor-application.htm) (Gérald Barré)
- [Influencing the HTML head in Blazor apps](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-preview-8/#influencing-the-html-head-in-blazor-apps) (Daniel Roth)
- [Influencing HTML Head from a Blazor component](https://github.com/dotnet/aspnetcore/issues/10450) (GitHub issue)
- [Adding favicons to your ASP.NET Core website with Real Favicon Generator](https://andrewlock.net/adding-favicons-to-your-asp-net-core-website-with-realfavicongenerator/) (Andrew Lock)
