---
date: "2020-12-13"
title: "Blast Off with Blazor: Integrate Cosmos DB with Blazor WebAssembly"
subtitle: "In this post, I integrate Cosmos DB with our project."
tags: [aspnet-core]
share-img: /assets/img/cosmos-wasm.png
---

So far in our series, we've [walked through the intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro), [wrote our first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page), [dynamically updated the HTML head from a component](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head), and [isolated our service dependencies](https://daveabrock.com/2020/11/22/blast-off-blazor-service-dependencies).

It's time to address the elephant in the room—why is the image loading so slow?

![Our slow site]({{ site.url }}{{ site.baseurl }}/assets/img/SetTitleBar.gif)

There's a few reasons for that. First, we have to wait for the app to load when we refresh the page–and with Blazor WebAssembly, we're waiting for the .NET runtime to load. On top of that, we're calling off to a REST API, getting the image source, and sending that to our view. That's not incredibly efficient.

In this post, we're going to correct both issues. We'll first move the `Image` component to its own page, then we're going to use a persistence layer to store and work with our images. This includes hosting our images on Azure Storage and accessing its details using the Azure Cosmos DB serverless offering. This will only help us as we'll create components to search on and filter our data.

This post contains the following content.

- [Move our Image component to its own page](#move-our-image-component-to-its-own-page)
  - [Create a new `Home` component](#create-a-new-home-component)
- [Integrate Cosmos DB with our application](#integrate-cosmos-db-with-our-application)
  - [Update the API](#update-the-api)
  - [Update the client app](#update-the-client-app)
- [Update our tests](#update-our-tests)
- [Wrap up](#wrap-up)

# Move our Image component to its own page

To move our `Image` component away from the default `Index` view, rename your `Index.razor` and `Index.razor.cs` files to `Image.razor` and `Image.razor.cs`.

In the `Image.razor` file, change the route from `@page "/"` to `@page "/image"`. That keeps it as a routable component, meaning it'll render whenever we browse to `/image`.

Then, in `Image.razor.cs`, make sure to rename the partial class to `Image`.

Here's how `Image.razor.cs` looks now:

```csharp
using Client.Services;
using Microsoft.AspNetCore.Components;
using System;
using System.Threading.Tasks;

namespace Client.Pages
{
    partial class Image : ComponentBase
    {
        Data.Image _image;

        [Inject]
        public IApiClientService ApiClientService { get; set; }

        private static string FormatDate(DateTime date) => date.ToLongDateString();

        protected override async Task OnInitializedAsync()
        {
            _image = await ApiClientService.GetImageOfDay();
        }
    }
}
```

## Create a new `Home` component

With that in place, let's create a new `Home` component. Right now, it'll welcome users to the site and point them to our images component. (If you need a refresher on how the `NavigationManager` works, check out [my previous post on the topic](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page#our-first-shared-component).) 

```html
@page "/"
@inject NavigationManager Navigator

<div class="flex justify-center">
    <div class="max-w-md rounded overflow-hidden shadow-lg m-12">
        <h1 class="text-4xl m-6">Welcome to Blast Off with Blazor</h1>
        <img class="w-full" src="images/armstrong.jpg" />

        <p class="m-4">
            This is a project to sample various Blazor features and functionality.

            We'll have more soon, but right now we are fetching random images.
        </p>
        <button class="text-center m-4 bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
                @onclick="ToImagePage">
            🚀 Image of the Day
        </button>
    </div>
</div>

@code {
    void ToImagePage() => Navigator.NavigateTo("/image");
} 
```

Here's how the `Home` component looks now. 

![Our new index component]({{ site.url }}{{ site.baseurl }}/assets/img/new-splash-page.png)

# Integrate Cosmos DB with our application

With our first fix out of the way, it's now time to speed up our image loading time. I'm going to do this in two ways:

- Store the images statically in Azure Storage
- Store image metadata, including the Azure Storage URLs in Cosmos DB

In the past, Cosmos has been incredibly expensive and wouldn't have been worth the cost for this project. With a new serverless offering (now in preview), it's a lot more manageable and can easily be run under my monthly Azure credits. While Cosmos excels with intensive, globally-distributed workloads, I'm after a fully-managed NoSQL offering that'll allow me flexibility if my schema needs change.

In this post, I won't show you how to create a Cosmos instance, upload our images to Azure Storage, then create a link between the two. This is all [documented in a recent post](https://daveabrock.com/2020/11/25/assets/img-azure-blobs-cosmos). After I have the data set up, I need to understand how to access it from my application. That's what we'll cover.

Now, I *could* [use the Azure Cosmos DB C# client](https://docs.microsoft.com/azure/cosmos-db/sql-api-get-started) to work with Cosmos. There's a lot of complexities here, and I don't need any of that business. I need it for basic CRUD operations. I'm a fan of [David Pine](https://twitter.com/davidpine7)'s [Azure Cosmos DB Repository .NET SDK](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-db-repository-net-sdk-v-1-0-4/), and will be using it here. This allows me to maintain the abstraction layer between the API and the client application, and is super easy to work with.

## Update the API

After [adding the NuGet package](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) to my `Api` and `Data` projects, I can start to configure it. There's a few different ways to wire up your Cosmos details—check out [the readme](https://github.com/IEvangelist/azure-cosmos-dotnet-repository) for details—I'll use the `Startup`.

Here's the `Configure` method for my Azure Function in the `Api` project:

```csharp
public override void Configure(IFunctionsHostBuilder builder)
{
    builder.Services.AddCosmosRepository(
        options =>
        {
            options.CosmosConnectionString = "my-connection-string";
            options.ContainerId = "image";
            options.DatabaseId = "APODImages";
        });
};
```

Next, I'll need to make some changes to the model. Take a look and I'll describe after:

```csharp
using Microsoft.Azure.CosmosRepository;
using Newtonsoft.Json;
using System;

namespace Data
{
    public class Image : Item
    {
        [JsonProperty("title")]
        public string Title { get; set; }

        [JsonProperty("copyright")]
        public string Copyright { get; set; }

        [JsonProperty("date")]
        public DateTime Date { get; set; }

        [JsonProperty("explanation")]
        public string Explanation { get; set; }

        [JsonProperty("url")]
        public string Url { get; set; }
    }
}
```

You'll see that the model now inherits from `Item`, which is required by the project. It contains an `Id`, a `Type` (for filtering implicitly on your behalf), and a partition key property. I'm also using `JsonProperty` attributes to match up with the Cosmos fields. Down the line, I might split models between my app and my API but this should work for now.

Now, in `ImageGet.cs`, I can call off to Cosmos quite easily. Here I'm calling off to my Cosmos instance. I can query by a random date.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.CosmosRepository;
using Data;

namespace Api
{
    public class ImageGet
    {
        readonly IRepository<Image> _imageRepository;

        public ImageGet(IRepository<Image> imageRepository) => _imageRepository = imageRepository;

        [FunctionName("ImageGet")]
        public async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "image")] HttpRequest req,
            ILogger log)
        {
            var imageResponse = _imageRepository.GetAsync
                (img => img.Date == GetRandomDate());
            return new OkObjectResult(imageResponse.Result);
        }

        private static DateTime GetRandomDate()
        {
            var random = new Random();
            var startDate = new DateTime(1995, 06, 16);
            var range = (DateTime.Today - startDate).Days;
            return startDate.AddDays(random.Next(range));
        }
    }
}
```

## Update the client app

In our `ApiClientService`, I'll need to slightly modify my `GetImageOfDay` method. The call returns an `IEnumerable<Image>`, so I'll just grab the result.

```csharp
public async Task<Image> GetImageOfDay()
{
    try
    {
        var client = _clientFactory.CreateClient("imageofday");
        var image = await client.GetFromJsonAsync<IEnumerable<Image>>("api/image");
        return image.First();
    }
    catch (Exception ex)
    {
        _logger.LogError(ex.Message, ex);
    }

    return null;
}
```

The images are now loading much faster!

# Update our tests

Thanks to successfully isolating our service last time, the tests for the `Image` component don't need much fixing. Simply changing the component to `Image` instead of `Index` does the trick:

```csharp
[Fact]
public void ImageOfDayComponentRendersCorrectly()
{
    var mockClient = new Mock<IApiClientService>();
    mockClient.Setup(i => i.GetImageOfDay()).ReturnsAsync(GetImage());

    using var ctx = new TestContext();
    ctx.Services.AddSingleton(mockClient.Object);

    IRenderedComponent<Client.Pages.Image> cut = ctx.RenderComponent<Client.Pages.Image>();
    var h1Element = cut.Find("h1").TextContent;
    var imgElement = cut.Find("img");
    var pElement = cut.Find("p");

    h1Element.MarkupMatches("My Sample Image");
    imgElement.MarkupMatches(@"<img src=""https://nasa.gov"" 
        class=""rounded-lg h-500 w-500 flex items-center justify-center"">");
    pElement.MarkupMatches(@"<p class=""text-2xl"">Wednesday, January 1, 2020</p>");
}
```

We can also add a quick test to our `Home` component in a new `HomeTest` file, which is similar to how we did our `NotFound` component:

```csharp
[Fact]
public void IndexComponentRendersCorrectly()
{
    using var ctx = new TestContext();
    var cut = ctx.RenderComponent<Home>();
    var h1Element = cut.Find("h1").TextContent;
    var buttonElement = cut.Find("button").TextContent;

    h1Element.MarkupMatches("Welcome to Blast Off with Blazor");
    buttonElement.MarkupMatches("🚀 Image of the Day");
}
```

# Wrap up

In this post, we worked on speeding up the loading of our images. We first moved our `Image` component off the home page, then integrated Cosmos DB into our application. Finally, we cleaned up our tests.
