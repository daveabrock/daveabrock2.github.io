---
date: "2020-12-16"
title: "Blast Off with Blazor: Build a responsive image gallery"
subtitle: "In this post, we build a responsive image gallery using Blazor and Tailwind CSS."
tags: [aspnet-core]
share-img: /assets/img/image-gallery.png
readtime: true
---

So far in our series, we've [walked through the intro](https://daveabrock.com/2020/10/26/blast-off-blazor-intro), [wrote our first component](https://daveabrock.com/2020/10/28/blast-off-blazor-404-page), [dynamically updated the HTML head from a component](https://daveabrock.com/2020/11/08/blast-off-blazor-update-head), [isolated our service dependencies](https://daveabrock.com/2020/11/22/blast-off-blazor-service-dependencies), and worked on [hosting our images over Azure Blob Storage and Cosmos DB](https://daveabrock.com/2020/12/13/blast-off-blazor-cosmos).

Now, we're going to query Cosmos DB, fetch our images, and display them in a responsive image gallery. We'll learn how to reuse components and pass parameters to them.

After we work on this, we'll enhance the gallery in future posts, with:

- Enabling the "infinite scrolling" feature with Blazor virtualization
- Filtering and querying images
- Creating a dialog to see a larger image and other details

This post contains the following content.

- [A quick primer](#a-quick-primer)
- [Customize the service layer](#customize-the-service-layer)
- [Update `Images` component to list our image collection](#update-images-component-to-list-our-image-collection)
- [Create a reusable `ImageCard` component](#create-a-reusable-imagecard-component)
- [Wrap up](#wrap-up)

# A quick primer

If you haven't been with me for the whole series, we're building a Blazor Web Assembly app [hosted with Azure Static Web Apps](https://daveabrock.com/2020/10/26/blast-off-blazor-intro) at *[blastoffwithblazor.com](https://blastoffwithblazor.com)*. I've copied images from the NASA APOD API (all 25 years!) [to Azure Blob Storage](https://daveabrock.com/2020/11/25/assets/img-azure-blobs-cosmos), and are storing the image metadata in [a serverless Cosmos DB instance](https://daveabrock.com/2020/12/13/blast-off-blazor-cosmos). Feel free to read those links to learn more.

With the images in place, we're going to build the image gallery. It's responsive and will look good on devices of any size.

![Our slow site]({{ site.url }}{{ site.baseurl }}/assets/img/image-gallery.png)

All code [is on GitHub](https://github.com/daveabrock/NASAImageOfDay).

# Customize the service layer

In previous posts, to get up and running we fetched a random image. To make things more interesting, we're going to fetch images from the last 90 days. (In future posts, we'll work on infinite scrolling and searching and filtering.) This requires updates to our Azure Function. We'll ask for a `days` query string parameter that allows the caller to request up to 90 days of images. For example, if we call `api/assets/img?days=90`, we get images from the last 90 days.

I've added logic to verify and grab the `days`, make sure it's in the appropriate range, then query Cosmos for the data itself.

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
using System.Transactions;
using System.Collections.Generic;

namespace Api
{
    public class ImageGet
    {
        readonly IRepository<Image> _imageRepository;

        public ImageGet(IRepository<Image> imageRepository) => _imageRepository = imageRepository;

        [FunctionName("ImageGet")]
        public IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "image")] HttpRequest req,
            ILogger log)
        {

            bool hasDays = int.TryParse(req.Query["days"], out int days);
            log.LogInformation($"Requested images from last {days} days.");

            if (!hasDays && (days <= 1 || days > 90))
                return new BadRequestResult();

            ValueTask<IEnumerable<Image>> imageResponse;
            imageResponse = _imageRepository.GetAsync
                 (img => img.Date > DateTime.Now.AddDays(-days));

            return new OkObjectResult(imageResponse.Result);
        }
    }
}
```

In the `ApiClientService` class in the `Client` project, update the call to take in the `days`. We'll also order the images descending (newest to oldest):

```csharp
public async Task<IEnumerable<Image>> GetImageOfDay(int days)
{
    try
    {
        var client = _clientFactory.CreateClient("imageofday");
        var images = await client.GetFromJsonAsync
                <IEnumerable<Image>>($"api/image?days={days}");
                return images.OrderByDescending(img => img.Date);
    }
    catch (Exception ex)
    {
        _logger.LogError(ex.Message, ex);
    }

     return null;
}
```

Now, in the code for the `Images` component, at `Images.razor.cs`, change the call to pass in the days:

```csharp
protected override async Task OnInitializedAsync()
{
    _images = await ApiClientService.GetImageOfDay(days: 90);
}
```

# Update `Images` component to list our image collection

So, how should we lay out our images? I'd like to list them left-to-right, top-to-bottom. Luckily, I can use [CSS grid layouts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout). We can define how we want to lay out our rows and columns, and grid can handle how they render when the user's window size is at different dimensions.

Using Tailwind CSS, I'm going to add a little bit of padding. Then, I'll have one column on small devices, two columns on medium devices, and three columns on large and extra-large devices.

```html
<div class="p-2 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-3">
</div>
```

In between the `<div>`'s, we'll iterate through our images and display them. We could handle the rendering here, but that's asking for maintenance headaches and won't give you any reusability options. The advantage of Blazor is in its component model. Let's build a reusable component.

We can pass parameters to our components, and here we'll want to pass down our `Image` model. Here's how we'll use the new component from the `Images` page, then:

```html
<div class="p-2 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-3">
    @foreach (var image in _images)
    {
        <ImageCard ImageDetails="image" />
    }
 </div>
```

# Create a reusable `ImageCard` component

Now, in `Pages`, create two files: `ImageCard.razor`, and `ImageCard.razor.cs`. In the `.cs` file, use the `[Parameter]` attribute to pass down the `Image`. (We'll likely add much more to this component, so are writing a partial class.)

```csharp
using Microsoft.AspNetCore.Components;

namespace Client.Pages
{
    partial class ImageCard : ComponentBase
    {
        [Parameter]
        public Data.Image ImageDetails { get; set; }
    }
}
```

How will our new component look? As you can imagine, a lot of the work is in designing the layout. There's a lot of different sized images, and I spent a lot of time getting it to work. Even though we aren't going deep on Tailwind CSS in these posts, it's worth mentioning here. (Also, much thanks to [Khalid Abuhakmeh](https://twitter.com/buhakmeh) for lending a hand.)

In the outer `<div>`, we're giving the card a large shadow and some margin:

```html
<div class="m-6 rounded overflow-hidden shadow-lg">
</div>
```

Then we'll render our image, and assign the alternate tag to the title. It's all in our model. We're saying here it'll fill the entire width of the card, only reach a specified height, and use `object-cover`, which [uses the `cover` value from the `object-fit` CSS property](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit). It maintains an image's aspect ratio as an image is resized.

```html
<div class="m-6 rounded overflow-hidden shadow-lg">
    <img class="w-full h-48 object-cover" src="@ImageDetails.Url" alt="@ImageDetails.Title" />
</div>
```

In the rest of the markup, we do the following:

- We have an `IsNew` property on our model that we use to apply a `New` badge if an image is newer than three days old. 
- If so, we give it a teal background and darker teal text, and apply some effects to it. We use flexbox to align it appropriately. If it isn't new, we make sure things are aligned appropriately without the badge.
- Finally, we display the `Title`. We use the `truncate` property, which gives long titles `...` at the end. This way, the cards don't align differently depending on how many lines of text the title consumes.

```html
<div class="m-6 rounded overflow-hidden shadow-lg">
        <img class="w-full h-48 object-cover" src="@ImageDetails.Url" alt="@ImageDetails.Title" />
        <div class="p-6">
            <div class="flex items-baseline">
                @if (ImageDetails.IsNew)
                {
                    <div class="flex items-baseline">
                        <span class="inline-block bg-teal-200 text-teal-800 text-xs px-2 rounded-full
                      uppercase font-semibold tracking-wide">New</span>
                        <div class="ml-2 text-gray-600 text-md-left uppercase font-semibold tracking-wide">
                            @ImageDetails.Date.ToLongDateString()
                        </div>
                    </div>
                }
                else
                {
                    <div class="text-gray-600 text-md-left uppercase font-semibold tracking-wide">
                        @ImageDetails.Date.ToLongDateString()
                    </div>
                }

            </div>
            <h3 class="mt-1 font-semibold text-2xl leading-tight truncate">@ImageDetails.Title</h3>
        </div>
</div>
```

Here's how a card looks with the `New` badge:

![The card with a new badge]({{ site.url }}{{ site.baseurl }}/assets/img/new-badge.png)

And again, here's how the page looks. Check it out [live at *blastoffwithblazor.com* as well](https://www.blastoffwithblazor.com/assets/img)!

![Our slow site]({{ site.url }}{{ site.baseurl }}/assets/img/image-gallery.png)

For comparison, here's how it looks on an iPhone X.

![Our slow site]({{ site.url }}{{ site.baseurl }}/assets/img/iphone-cards.png)

# Wrap up

In this post, we showed off how to query images from our Cosmos DB and display our images using a responsive layout. Along the way, we learned how to pass parameters to reusable components.

In future posts, we'll work on infinite scrolling, clicking an image for more details, and querying and searching the data.
