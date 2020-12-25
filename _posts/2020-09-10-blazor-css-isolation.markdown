---
date: "2020-09-10"
title: "Use CSS isolation in your Blazor projects"
tags: [aspnet-core, blazor]
share-img: /assets/img/css-isolation-card.png
subtitle: We talk about scoping your CSS to your Blazor components—all without a stylesheet reference.
---

I'm excited to see Blazor now supporting CSS isolation—also known as scoped CSS.

This post discusses how to use CSS isolation with the latest preview bits, a feature adored by those in the Angular and Vue space, and for good reason—once you have it, you'll soon wonder how you ever went without it.

This post covers the following topics.

- [Prerequisites](#prerequisites)
- [The problem](#the-problem)
- [Use CSS isolation](#use-css-isolation)
  - [How does this magic work?](#how-does-this-magic-work)
  - [Reminder: CSS isolation is a build-time step](#reminder-css-isolation-is-a-build-time-step)
- [How to work with child components](#how-to-work-with-child-components)
- [Integrate with your favorite preprocessors](#integrate-with-your-favorite-preprocessors)
- [Disable automatic bundling](#disable-automatic-bundling)
- [Wrap up](#wrap-up)

# Prerequisites

Before we get started, make sure you've installed the latest preview bits. To do this, [install the latest .NET 5 SDK](https://dotnet.microsoft.com/download/dotnet/5.0).

Also, you'll need to create a Blazor app (either Server or WebAssembly is fine) using the tooling of your choice. For me, the quickest way to get started is [with the .NET CLI](https://docs.microsoft.com/dotnet/core/tools/):

```bash
dotnet new blazorwasm -o CssIsolationApp
cd CssIsolationApp
dotnet run
```

Of course, you can use Visual Studio tooling as well—do whatever works for you!

Once you verify the app is up and running, you'll be ready to go.

# The problem

The beauty of Blazor is in its component model. With [components](https://docs.microsoft.com/aspnet/core/blazor/components/?view=aspnetcore-3.1), you get a self-contained "chunk" of your UI that allows you to share and reuse them across your projects (not to mention with [shared class libraries](https://docs.microsoft.com/aspnet/core/blazor/components/class-libraries?view=aspnetcore-3.1&tabs=visual-studio)). Until Blazor CSS isolation came along, using CSS with your components went against a lot of that, which can lead to a frustrating experience. Let's walk through an example to explain why.

In the generated sample Blazor app, we have three pages: **Home**, **Counter**, and **Fetch data**. With even a basic knowledge of CSS concepts, we know that if we do something like this in `wwwroot/css/app.css`, the site's global CSS...

```css
h1 {
    color: brown;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
```

...we see that this change applies to every `h1` on every page in our project. So, as a result, if we want a different heading style on every page—like a crazy person!—we need to differentiate them somehow:

```css
.hello-world-heading {
    color: brown;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.counter-heading {
    color: aquamarine;
    font-family: 'Times New Roman', Times, serif;
}

.fetch-data-heading {
    color: blueviolet;
    font: 'Comic Sans';
}
```

Not only that, I need to go into each of my pages and apply my CSS classes to it.

For `Pages/Index.razor`:

```html
<h1 class="hello-world-heading">Hello, world!</h1>
```

For `Pages/Counter.razor`:

```html
<h1 class="counter-heading">Counter</h1>
```

For `Pages/FetchData.razor`:

```html
<h1 class="fetch-data-heading">Weather forecast</h1>
```

After you do that, all the styles should be applied—and, as an added bonus, you've stopped taking me seriously since I told you to use Comic Sans.

Even using a basic example, you're already seeing the pain points. Because you're styling defensively to avoid collisions between components and other libraries, you're left with a bloated file with no way to track them to your components. You're essentially working without namespaces. Can you imagine? We would *never* do this in C#, but this is what we're doing with our CSS.

In addition to a terrible developer experience, you're also adding bloat to your application by loading styles when they aren't referenced.

In your browser's developer tools, you can verify this quite easily—I'm using [Show Coverage from Chrome Dev Tools](https://developers.google.com/web/tools/chrome-devtools/coverage). As you can see from the screenshot below, I'm not even using 40% of my styles. You can see the heading styles from the other components are being loaded, even though we know they aren't used.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/css-coverage.png)

There are ways to get around this by bringing in external libraries and tools from both inside and outside of the Blazor ecosystem. If it works for you, great—but I ask: isn't the promise of one toolchain a big reason why you're using Blazor?

With a knowledge of our pain points, let's add CSS isolation to our sample application.

# Use CSS isolation

It's quite easy to bind your CSS to your component. To do this, inside of your Pages directory (**and not with the global CSS file**), add new files with the format `MyComponent.razor.css`. So, add these three files to the project:

* `Index.razor.css`
* `Counter.razor.css`
* `FetchData.razor.css`

Once you do that, cut and paste the styles you created for the individual headings into the individual files. If you run your project again, you'll see that everything still works. The difference here is that everything is scoped to the single component—and **you don't even need to add a reference**!

If you happen to run the coverage test again, you'll see that the styles for your other components aren't being loaded with your existing component.

Without worrying about conflicting with other components or libraries, we can change our CSS styles to simple `h1`'s. For example, in our `Index` component, we can change this style:

```css
.hello-world-heading {
    color: brown;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
```

To a simple `h1`:

```css
h1 {
    color: brown;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
```

I'm incredibly happy with the simplicity of this solution. Many folks have asked about using `@css` blocks in components, but it involved integrating a CSS parser into the Razor compiler—which appears to be quite expensive.

## How does this magic work?

For this to work, Blazor appends a special attribute to your CSS classes, which binds your classes to the specific component.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/css-identifier.png)

If you're curious, you can head over to the Network panel of your favorite browser's developer tools. You'll see that Blazor loads in a `MyProject.styles.css` file, where `MyProject` is the name of your project. Here, you'll see the styles for all our components, each referenced by that unique ID—as a bonus, it's super helpful to have the component's name commented for us.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/scoped-css.png)

This `styles.css` file is the result of bundling all your isolated CSS files for your project into a single output. Don't take my word for it: if you view the `<head>` on your page, you'll see the reference that's generated for you.

```html
<link href="MyProject.styles.css" rel="stylesheet">
```

Because each project has a different `styles.css` file, they'll need to know about each other somehow. This is accomplished using CSS imports. In this example, you'll see my project references a shared Razor component.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/css-import.png)

Armed with this knowledge, if we take a larger view of the DOM it'll make a lot more sense. The new `h1` class refers to our `Index` component (`b-dew6pvofzw`), and the other styles are brought in from the `Shared/MainLayout` component (`b-vtqmmfsxlh`).

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/scoped-css-html.png)

This pattern has [worked well with Vue](https://vue-loader.vuejs.org/guide/scoped-css.html#scoped-css) and there was no sense in reinventing the wheel.

## Reminder: CSS isolation is a build-time step

To support isolation, Blazor rewrites all the CSS selectors during the build process. This makes prerendering a snap, since there's no reliance on existing .NET or JavaScript code. On the other side of the coin, this means you'll need to recompile to see any new changes—if you're used to saving a CSS change and seeing your changes immediately, it's a drag.

# How to work with child components

Call me a mind reader, but you're probably wondering how this works with child components. Thanks so much for asking. There's only one way to find out.

In your `Pages` directory, add a new component and call it `MyChild.razor` and add the following:

```html
@page "/child"

<h1>I'm a child component it's true</h1>

<p>No, seriously.</p>
```

Finally, drop the (child) component in our `Index.razor` (parent) component. Your `Index.razor` component will look like this now:

```html
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<MyChild />
```

Fire up your app and see what happens. We notice that, by default, **scoped styles do not apply to child components**. The styles in your `*.razor.css` files only get applied to the rendered output of that specific component.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/no-inherit.png)

Don't worry, though: we can cascade styles down to child components without the need for a new component-specific CSS file. We'll do this with a `::deep` combinator in our CSS. Change the contents of `Index.razor.css` to the following:

```css
::deep h1 {
    color: brown;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
```

Fire up your app and see that ... it doesn't work. Because of how the markup is structured, Blazor can't determine the relationship between the parent component and the child component. Surround the markup with a `<div>` tag, and it'll work:

```html
@page "/"

<div>
    <h1>Hello, world!</h1>

    Welcome to your new app.

    <MyChild />
</div>
```

Now, it works great. We're able to have our child components inherit styles from our parent component.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/it-works.png)

If we look at our attribute:

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/parent-child-attributes.png)

Blazor identifies the child style as "belonging" to the parent component in `scoped.styles.css`.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/scoped-styles-inheritance.png)

# Integrate with your favorite preprocessors

You may be leveraging your own CSS preprocessor. A popular preprocessor, [like SASS](https://sass-lang.com/guide), makes the writing of CSS more enjoyable with support for things that CSS doesn't provide out of the box—like variables, nesting, modules, mixins, and inheritance.

In an effort to make things more generalized and extensible (and, ahem, not to mention shipping this on time), the Blazor CSS isolation feature does not *directly* offer CSS preprocessor support—but it doesn't need to. For whatever tool you're using, you just need to ensure that the preprocessor compiles to CSS to your `MyComponent.razor.css` file before the Blazor build step occurs. This allows you to be flexible: you can continue using existing tools like Webpack or one of the several .NET tools available, [like Delegate.SassBuilder](https://www.nuget.org/packages/Delegate.SassBuilder). Let's do a quick demo using `Delegate.SassBuilder`.

First, go out and get `SassBuilder` in one of the many ways available to you (NuGet Package Manager, .NET CLI, Package Manager Console). For me, I'll just add it to my project file and let the restore process take over. Add the reference to your existing `<ItemGroup>` packages.

```xml
<ItemGroup>
    <PackageReference Include="Delegate.SassBuilder" Version="1.4.0" />
</ItemGroup>
```

In your `Pages` directory, add a new SASS file. Let's call it `Index.razor.scss`. It'll be placed alongside the CSS file. You don't need to touch the CSS file—our changes will be compiled to this file.

In `Index.razor.scss`, have fun with some variables (we changed our color from brown to red to validate things are working):

```sass
$color: red;
$font: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

h1 {
    color: $color;
    font-family: $font;
}
```

During the build, the `.scss` file is compiled to `Index.razor.css` and we see that our changes are in place.

![A lot of unused styles]({{ site.url }}{{ site.baseurl }}/assets/img/red.png)

# Disable automatic bundling

If you have a process that works for you, fantastic. If you want to opt-out of how Blazor publishes and loads scoped files at runtime, you can disable it by using an MSBuild property. As [mentioned in the GitHub issue](https://github.com/dotnet/aspnetcore/issues/10170#issuecomment-671342247), this means it's *your* responsibility to grab the scoped CSS files from the `obj` directory and do the required steps to publish and load them during runtime.

If you're good with that, add the `DisableScopedCssBundling` MSBuild property to your project file.

```xml
<PropertyGroup>
  <DisableScopedCssBundling>true</DisableScopedCssBundling>
</PropertyGroup>
```

# Wrap up

In this post, we reviewed the new CSS isolation feature for Blazor. We discussed its benefits, the problems it solves, how to use it, and how you can pass styles to child components. We also talked about how to use CSS isolation with preprocessors and how to disable automatic bundling.

Thanks to the popularity of this post, it got turned into [official Microsoft ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/blazor/components/css-isolation?view=aspnetcore-5.0)!

If you have any comments or feedback, please let me know by commenting or [connecting on Twitter](https://twitter.com/daveabrock).
