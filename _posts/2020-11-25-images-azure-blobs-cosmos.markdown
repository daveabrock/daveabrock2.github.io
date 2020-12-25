---
date: "2020-11-25"
title: "Use Azure Functions, Azure Storage blobs, and Cosmos DB to copy images from public URLs"
subtitle: "In this post, we work with Azure Storage blobs and Cosmos DB to copy images that are available over the public Internet."
tags: [aspnet-core, azure]
share-img: /assets/img/public-url-images.png
---

There are several reasons why you'll want to host publicly accessible images yourself. For example, you may want to compress them or apply metadata for querying purposes on your site. For my *Blast Off with Blazor* project, I had that exact need.

In my case, I want to host images cheaply and also work with the image's metadata over a NoSQL database. Azure Storage blobs and Azure Cosmos DB were obvious choices, since I'm already deploying the site as an Azure Static Web App.

To accomplish this, I wrote a quick Azure Function that accomplishes both tasks. Let's take a look at how this works.

(Take a [look at the repository](https://github.com/daveabrock/ImageUploader) for the full treatment.)

# Before you begin: create Azure resources

Before you begin, you need to set up the following:

- An [Azure storage account](https://portal.azure.com/) (I set up StorageV2, with LRS, and Hot access tier)
- A [blob container](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) to store your images (I set it to public access as anonymous read access is not a concern for publicly available photos)
- A [Cosmos DB resource](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal) (I largely accepted the defaults except using the Serverless option, now in preview, to reduce costs)
- A [Cosmos DB collection, database, and container](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal)

Once I created all these resources, I added the configuration values to my `local.settings.json` file. These values are available in the portal when you browse to your various resources. (When you deploy your resources, they'll need to be added to your configuration.)

```json
{
  "Values": {
    "ApiKey": "my-nasa-api-key",
    "CosmosEndpoint": "url-to-my-cosmos-endpoint",
    "CosmosKey": "my-cosmos-primary-key",
    "CosmosDatabase": "name-of-my-cosmos-db",
    "CosmosContainer": "name-of-my-cosmos-container",
    "StorageAccount": "storage-account-name",
    "StorageKey": "storage-key",
    "BlobContainerUrl": "url-to-my-container"
  }
}
```

# The Azure function

In my Azure function I'm doing three things:

* Call the NASA Image of the Day API to get a response with image details—including URL, title, description, and so on
* From the URL in the response payload, copy the image to Azure Storage
* Then, update Cosmos DB with the URL of the new resource, and the other properties in the object

If we look at the [Astronomy Picture of the Day site](https://apod.nasa.gov/apod/astropix.html), it hosts an image and its metadata for the current day. I want to put the image in Storage Blobs and the details in Cosmos DB.

![The Astronomy Picture of the Day site]({{ site.url }}{{ site.baseurl }}/assets/img/apod-site.png)

Here's how the function itself looks:

```csharp
//using System;
//using System.Threading.Tasks;
//using Microsoft.AspNetCore.Mvc;
//using Microsoft.Azure.WebJobs;
//using Microsoft.Azure.WebJobs.Extensions.Http;
//using Microsoft.AspNetCore.Http;
//using Microsoft.Extensions.Logging;
//using Newtonsoft.Json;
//using System.Net.Http;
//using Azure.Storage;
//using Azure.Storage.Blobs;
//using System.Linq;
//using Microsoft.Azure.Cosmos;

public class ImageUploader
{
    private readonly HttpClient httpClient;
    private CosmosClient cosmosClient;

    public ImageUploader()
    {
        httpClient = new HttpClient();
        cosmosClient = new CosmosClient(Environment.GetEnvironmentVariable("CosmosEndpoint"),
                                        Environment.GetEnvironmentVariable("CosmosKey"));
    }

    [FunctionName("Uploader")]
    public async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = "upload")] HttpRequest req, ILogger log)
    {
        var apiKey = Environment.GetEnvironmentVariable("ApiKey");
        var response = await httpClient.GetAsync($"https://api.nasa.gov/planetary/apod?api_key={apiKey}");
        var result = await response.Content.ReadAsStringAsync();

        var imageDetails = JsonConvert.DeserializeObject<Image>(result);

        await UploadImageToAzureStorage(imageDetails.Url);
        await AddImageToContainer(imageDetails);
        return new OkObjectResult("Processing complete.");
    }
}
```

After I get the response back from the API, I deserialize it to an `Image` model and then do the work in Azure (keep reading for those details).

Here is the model:

```csharp
using Newtonsoft.Json;
using System;

namespace MassImageUploader
{
    public class Image
    {
        [JsonProperty("id")]
        public Guid Id { get; set; }

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

## Upload image to Azure Storage blob container

In my `UploadImageToAzureStorage` method, I pass along the URI of the publicly accessible image. From that, I get the `fileName` by extracting the last part of the URI (for example, *myfile.jpg*). With the `fileName` I build the path of my new resource in the `blobUri`—that path will include the Azure Storage container URL and the `fileName`.

After that, I pass my credentials to a new `StorageSharedKeyCredential`, and instantiate a new `BlobClient`. Then, the copy occurs in the `StartCopyFromUriAsync`, which passes in a URI.

```csharp
private async Task<bool> UploadImageToAzureStorage(string imageUri)
{
    var fileName = GetFileNameFromUrl(imageUri);
    var blobUri = new Uri($"{Environment.GetEnvironmentVariable("BlobContainerUrl")}/{fileName}");
    var storageCredentials = new StorageSharedKeyCredential(
        Environment.GetEnvironmentVariable("StorageAccount"),
        Environment.GetEnvironmentVariable("StorageKey"));
    var blobClient = new BlobClient(blobUri, storageCredentials);
    await blobClient.StartCopyFromUriAsync(new Uri(imageUri));
    return await Task.FromResult(true);
}

private string GetFileNameFromUrl(string urlString)
{
    var url = new Uri(urlString);
    return url.Segments.Last();
}
```

If I browse to my Azure Storage container, I'll see my new image.

![The result]({{ site.url }}{{ site.baseurl }}/assets/img/blob-result.png)

# Push metadata to Cosmos DB

With my image successfully stored, I can push the metadata to an Azure Cosmos DB container. In my `AddImageToContainer` method, I pass in my populated `Image` model. Then I'll get the Azure Cosmos DB container, get the path, then call `CreateItemAsync` while passing in the `image`. Easy.

```csharp
private async Task<bool> AddImageToContainer(Image image)
{
    var container = cosmosClient.GetContainer(
        Environment.GetEnvironmentVariable("CosmosDatabase"),
        Environment.GetEnvironmentVariable("CosmosContainer"));

    var fileName = GetFileNameFromUrl(image.Url);

    image.Id = Guid.NewGuid();
    image.Url = $"{Environment.GetEnvironmentVariable("BlobContainerUrl")}/{fileName}";

    await container.CreateItemAsync(image);
    return await Task.FromResult(true);
}
```

In the Data Explorer for my Cosmos DB resource in the Azure portal, I can see my new record.

![The result]({{ site.url }}{{ site.baseurl }}/assets/img/cosmos-result.png)


# Wrap up

In this post, we learned how to store a publicly accessible image in Azure Storage, then post that URL and other metadata in Azure Cosmos DB.

As always, feel free to post any feedback or experiences in the comments.





