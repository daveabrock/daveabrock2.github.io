---
date: "2020-11-22"
title: "Blast Off with Blazor: Isolate and test your service dependencies"
subtitle: "In this post, we refactor our component to inject an API service wrapper, to abstract away a direct HttpClient dependency."
tags: [aspnet-core]
share-img: /assets/img/service-dependency-card.png
---

So far in our series, we've [walked through the intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro), [wrote our first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page), and [dynamically updated the HTML head from a component](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head). 

I've made testing a crucial part of this project and not an afterthought—as discussed previously, we're [using the bUnit project](https://bunit.egilhansen.com/docs/getting-started/index.html) to unit test our components. As I [discussed last time](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head#what-about-the-tests), though, testing our `Index` component was a little cumbersome because of the `HttpClient` dependency. There are ways to [mock and test it](https://bunit.egilhansen.com/docs/test-doubles/mocking-httpclient.html), but we should ask ... why are we injecting it directly?

It was great to inject it easily to get up and running but what happens as we build more components and more APIs to call, each with different endpoints and request headers? How will we manage that? And if we want to unit test our components, will I have to mock an `HttpClient` every time? What a nightmare.

Instead, we'll create an API wrapper and inject that in our components. Any service-level implementation details can be abstracted away from the component. Along the way, we'll learn about working with separate C# classes, using a named `IHttpClientFactory`, and how to quickly mock and test a service in bUnit. Let's get started.

This post contains the following content.

- [Does my code always have to reside in @code blocks?](#does-my-code-always-have-to-reside-in-code-blocks)
- [Add an API service wrapper to our project](#add-an-api-service-wrapper-to-our-project)
  - [How to call HttpClient from our app](#how-to-call-httpclient-from-our-app)
  - [Inject our new service from our component](#inject-our-new-service-from-our-component)
- [Test our component](#test-our-component)
- [Wrap up](#wrap-up)

# Does my code always have to reside in @code blocks?

To recap, here's how our main component currently looks.

```csharp
@page "/"
@inject HttpClient http

@if (image != null)
{
    <div class="p-4">
        <h1 class="text-6xl">@image.Title</h1>
        <p class="text-2xl">@FormatDate(image.Date)</p>
        @if (image.Copyright != null)
        {
            <p>Copyright: @image.Copyright</p>
        }
    </div>

     <div class="flex justify-center p-4">
        <img src="@image.Url" class="rounded-lg h-500 w-500 flex items-center justify-center"><br />
    </div>
}

@code {
    private Data.Image image;

    private string FormatDate(DateTime date) => date.ToLongDateString();

    protected override async Task OnInitializedAsync()
    {
        image = await http.GetFromJsonAsync<Data.Image>("api/image");
    }
}
```

While we don't have a lot of lines of code in the `@code` block, there's still a lot going on in this component. We're directly injecting `HttpClient` to directly call our Azure Function. In the `@code` section I've written a helper method as well as `OnInitializedAsync` behavior. As we add more features and functionality, that `@code` block is only going to grow. We can definitely keep the C# coupled with our Razor syntax, as it makes it easy to see all that's going on in one file—but we also can move all of this to a [separate C# file](https://docs.microsoft.com/aspnet/core/blazor/components/?view=aspnetcore-5.0#partial-class-support) for reuse and maintainability purposes.

This is a "code-behind" approach, as the code will sit behind the view logic in a partial class. To do this, we'll create an `Index.razor.cs` file. If you're using Visual Studio, you'll see it's nested "inside" the Blazor component.

Cut and paste everything inside the `@code` block to the new file. You'll see some build errors and will need to resolve some dependencies. To resolve these:

- Make the new file a `partial class`
- Add a using statement for `Microsoft.AspNetCore.Components`
- With the using added, inherit `ComponentBase`

What about injecting `HttpClient`, though? We can't carry over that Razor syntax to our C# file. Instead, we'll add it as a property with an `Inject` annotation above it.

Here's how the class looks:

```csharp
using Client.Services;
using Data;
using Microsoft.AspNetCore.Components;
using System;
using System.Threading.Tasks;

namespace Client.Pages
{
    partial class Index : ComponentBase
    {
        Image _image;

        [Inject]
        public HttpClient http { get; set; }

        private static string FormatDate(DateTime date) => date.ToLongDateString();

        protected override async Task OnInitializedAsync()
        {
            image = await http.GetFromJsonAsync<Data.Image>("api/image");
        }
    }
}
```

Now, when we remove the `@code` block and `HttpClient` injection, our component looks cleaner:

```html

@page "/"

@if (_image is null)
{
    <p>Loading...</p>
}
else
{
    <div class="p-4">
        <h1 class="text-6xl">@_image.Title</h1>
        <p class="text-2xl">@FormatDate(_image.Date)</p>
        @if (_image.Copyright != null)
        {
            <p>Copyright: @_image.Copyright</p>
        }
    </div>

    <div class="flex justify-center p-4">
        <img src="@_image.Url" class="rounded-lg h-500 w-500 flex items-center justify-center"><br />
    </div>
}
```

If we run the project, it'll work as it always has. Now, let's build out an API wrapper.

# Add an API service wrapper to our project

We're now ready to build our service. In our `Client` project, create an `ApiClientService.cs` file inside a `Services` folder. We'll stub it out for now with an interface to boot:

```csharp
public interface IApiClientService
{
    public Task<Image> GetImageOfDay();
}

public class ApiClientService : IApiClientService
{
    public async Task<Image> GetImageOfDay()
    {
        throw new NotImplementedException();
    }
}
```

We'll also want to add the new folder to bottom of our `_Imports.razor` file:

```html
@using Services
```

## How to call HttpClient from our app

We could still call `HttpClient` directly, but over the course of this project we'll be connecting to various APIs with different endpoints, different headers, and so on. As we look forward, [we should create an `IHttpClientFactory`](https://docs.microsoft.com/aspnet/core/fundamentals/http-requests?view=aspnetcore-5.0#named-clients). This allows us to work with named instances, allows us to delegate middleware handlers, and manages the lifetime of handler instances for us.

To add a factory to our project, we'll add a named client to our `Program.cs` file. While we're here, we'll inject our new `IApiClientService` as well.

```csharp
public static async Task Main(string[] args)
{
    // important stuff removed for brevity
    builder.Services.AddHttpClient("imageofday", iod =>
    {
        iod.BaseAddress = new Uri(builder.Configuration["API_Prefix"] ?? builder.HostEnvironment.BaseAddress);
    });
    builder.Services.AddScoped<IApiClientService, ApiClientService>();
}
```

In `AddHttpClient`, I'm specifying a URI, and referencing my API as `imageofday`. With that in place, I can scoot over to `ApiClientService` and make it work.

First, let's inject our `ILogger` and `IHttpClientFactory` in the constructor.

```csharp
public class ApiClientService : IApiClientService
{
    readonly IHttpClientFactory _clientFactory;
    readonly ILogger<ApiClientService> _logger;

    public ApiClientService(ILogger<ApiClientService> logger, IHttpClientFactory clientFactory)
    {
        _clientFactory = clientFactory;
        _logger = logger;
    }
}
```

In our `GetImageOfDay` logic, we'll create our named client and use it to call to our Azure Function at the `api/image` endpoint. Of course, we'll catch any exceptions and log them appropriately.

```csharp
public async Task<Image> GetImageOfDay()
{
    try
    {
        var client = _clientFactory.CreateClient("imageofday");
        var image = await client.GetFromJsonAsync<Image>("api/image");
        return image;
    }
    catch (Exception ex)
    {
        _logger.LogError(ex.Message, ex);
    }

    return null;
}
```

## Inject our new service from our component

With the service wrapper now complete, we can inject our new service instead of direct dependency on `HttpClient`. Change the `HttpClient` injection in `Index.razor.cs` to our new service instead:

```csharp
[Inject]
public IApiClientService ApiClientService { get; set; }

// stuff

protected override async Task OnInitializedAsync()
{
    _image = await ApiClientService.GetImageOfDay();
}
```

If you run the app, you should see no changes—we didn't have to modify the markup at all—but we've made our lives a lot easier and now testing should be a snap as we add to our project).

# Test our component

With our API wrapper in place, testing is a whole lot easier. I've created an `ImageOfDayTest` class in our `Test` library.

I'll be adding a reference to `Moq`, a popular mocking library, to mimic the response back from our service. You can [download the package](https://www.nuget.org/packages/moq/) from NuGet Package Manager or just drop this in `Test.csproj`:

```xml
<ItemGroup>
    <PackageReference Include="Moq" Version="4.15.1" />
</ItemGroup>
```

I'll build out a sample `Image` to return from the service. I'll create a private helper method for that:

```csharp
private static Image GetImage()
{
    return new Image
    {
        Date = new DateTime(2020, 01, 01),
        Title = "My Sample Image",
        Url = "https://nasa.gov"
    };
}
```

In my test case, I'll mock my client, return an image, and inject it into my bUnit's `TestContext`:

```csharp
var mockClient = new Mock<IApiClientService>();
mockClient.Setup(i => i.GetImageOfDay()).ReturnsAsync(GetImage());

using var ctx = new TestContext();
ctx.Services.AddSingleton(mockClient.Object);
```

*Note*: The integration test vs. mocking in a unit test [is a hot topic](https://tyrrrz.me/blog/unit-testing-is-overrated), especially when testing dependencies. My intent here is to unit test rendering behavior and not my services, but calling the endpoint from the test is also an option if you're up for it.
{: .notice--info}

With that in place, I can render my component, and assert against expected output with the following code:

```csharp
var cut = ctx.RenderComponent<Client.Pages.Index>();
var h1Element = cut.Find("h1").TextContent;
var imgElement = cut.Find("img");
var pElement = cut.Find("p");

h1Element.MarkupMatches("My Sample Image");
imgElement.MarkupMatches(@"<img src=""https://nasa.gov"" 
    class=""rounded-lg h-500 w-500 flex items-center justify-center"">");
pElement.MarkupMatches(@"<p class=""text-2xl"">Wednesday, January 1, 2020</p>");
```

My tests pass—ship it!

# Wrap up

In this post, we learned how to isolate `HttpClient` dependencies in our Blazor code. To do this, we moved our component's C# code to a partial "code-behind class" and built a service that uses the `IHttpClientFactory`. Then, we were able to use bUnit to test our component quite easily.

Are refactorings sexy? No. Are they fun? Also no. Are they important? Yes. Is this the last question I'll ask myself in this post? Also yes. In the next post, we'll get back to updating the UI.
