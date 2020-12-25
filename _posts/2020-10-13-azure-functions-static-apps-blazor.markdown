---
date: "2020-10-13"
title: "Blast Off with Blazor, Azure Functions, and Azure Static Web Apps"
subtitle: "Let's talk about deploying an Azure Static Web App running Blazor and Azure Functions."
tags: [blazor, azure, aspnet-core]
share-img: /assets/img/static-web-apps-card.png
---

Static sites are so great. After all, you're reading these words on a static site. Why bother with the overhead of dynamically generated files if you don't need them? It's not that static sites are boring—just that its served files, like HTML, aren't generated dynamically. With less to do, these sites perform better and are cheaper to run.

This works great for blogs like this one, but in many cases you'll still need a server-side aspect. You'll want to talk to some APIs and handle logic and whatnot. Traditionally, this would involve some storage providers like Azure Storage or Amazon S3. With the advent of serverless technologies like Azure Functions, we have even more to think about.

Now, with Azure Static Web Apps, we've got the best of both worlds: a light front-end paired with a serverless API in one pretty package. (The API is optional, if you're wondering.) With the recently announced Blazor support, I had to give it a shot.

In this post, we'll cover the following topics:

- [Prerequisites](#prerequisites)
- [Our application](#our-application)
  - [Fallback route support](#fallback-route-support)
  - [CORS support](#cors-support)
- [Create an Azure Static Web App](#create-an-azure-static-web-app)
- [Inspect the workflow file](#inspect-the-workflow-file)
  - [A gotcha about `package.json`](#a-gotcha-about-packagejson)
- [Pull request sites](#pull-request-sites)
- [A note on custom domains](#a-note-on-custom-domains)
- [Wrap up](#wrap-up)
- [References](#references)

# Prerequisites

To work with this post, you'll need:

* An Azure account
* A [non-preview .NET Core SDK](https://dotnet.microsoft.com/download) (I am using *3.1.403*)

While .NET 5 is weeks from going live at the time of this post, you aren't able to deploy .NET 5 code to Azure Static Web Apps yet. 

# Our application

Before you create an Azure Static Web App instance, you'll need an application hosted in GitHub—when you create the instance in Azure, it'll ask for a repo and its details. 

I wrote a quick app called *Blast Off with Blazor* (and you can see it live at [blastoffwithblazor.com](https://www.blastoffwithblazor.com/)). It's an admittedly simple app at this time, and I'll be adding to it over the next few months as I dig deep on Blazor concepts and best practices. Feel free to [clone the GitHub repo](https://github.com/daveabrock/NASAImageOfDay) (and read the instructions in the [README.md file](https://github.com/daveabrock/NASAImageOfDay/blob/main/README.md) for details on how to run it locally).

The app is served over Blazor Web Assembly. When the app loads, it calls an Azure Function. The Function, in turn, calls NASA's Astronomy Picture of the Day (APOD) API to get a picture that is out of this world—literally. The APOD site [has been serving up](https://apod.nasa.gov/apod/) amazing images daily since June 16, 1995. Every time you reload the app, I'll fetch a random image anytime between that start date and today.

In the `Index.razor` file, I'm doing that in the `@code` block in the `OnInitializedAsync()` function ([full code](https://github.com/daveabrock/NASAImageOfDay/blob/main/Client/Pages/Index.razor)):

```html
@code {
    private Data.Image image;

    protected override async Task OnInitializedAsync()
    {
        image = await http.GetFromJsonAsync<Data.Image>("api/image");
    }
}
```

In my Azure Function, in the `api/image` path, I call off to the APOD API ([full code](https://github.com/daveabrock/NASAImageOfDay/blob/main/Api/ImageGet.cs)):

```csharp
[FunctionName("ImageGet")]
public static async Task<IActionResult> Run(
      [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "image")] HttpRequest req,
      ILogger log)
{
      var apiKey = Environment.GetEnvironmentVariable("ApiKey");
      var response = await httpClient.GetAsync($"https://api.nasa.gov/planetary/apod?api_key={apiKey}&hd=true&date={GetRandomDate()}");
      var result = await response.Content.ReadAsStringAsync();
      return new OkObjectResult(JsonConvert.DeserializeObject(result));
}
```

Once I get a response back, I access the properties of the returned object to display the image, title, date, and copyright on the page. It's not rocket science (well, it *kinda* is), but it'll do.

I won't go to the moon and back on Blazor and Azure Functions in this post, as I'm focused on Azure Static Web Apps. In future posts, I'll discuss each in greater detail.
{: .notice--info}

Here's the application in action. Puts stars in your eyes, doesn't it?

![The Blast Off with Blazor app]({{ site.url }}{{ site.baseurl }}/assets/img/blast-off-demo.png)

Before we blast off, let's explore some app configuration.

## Fallback route support

In single-page applications, fallback routes are important. Let's assume I have a special path called `/nasa`. When I access the page, it'll display the path, including `/nasa` in my browser, but if I refresh the page manually, by default my app won't reload with `/nasa`. Instead, a 404 error can display because there isn't a `/nasa` page on the "server" (or, in our case, there isn't a server at all).

Luckily, Azure Static Web Apps supports fallback routing. Drop a `routes.json` file in the root of your static assets folder (typically `wwwroot`) like so:

```json
{
  "routes": [
    {
      "route": "/*",
      "serve": "/index.html",
      "statusCode": 200
    }
  ]
}
```

The `route` key makes sure to match all routes.

## CORS support

Because Azure Static Web Apps is configured with Azure Functions, you don't have to deal with Cross-Origin Resource Sharing (CORS) issues—in short, this is when your browser blocks your request unless the API server allows it. 

However, you'll need to do this in your local environment. In the `Properties` folder of your Azure Functions project, create a `launchSettings.json` file with this:

```json
{
     "profiles": {
         "Api": {
             "commandName": "Project",
             "commandLineArgs": "start --cors *"
         }
     }
 }
```

I'm using Visual Studio tooling. If you instead want to use Azure Functions from the command line, you'll need to do this inside a [*local.settings.json* file](https://docs.microsoft.com/azure/azure-functions/functions-run-local?tabs=windows,csharp,bash#local-settings-file).

With the application ready to go, let's head on over to the Azure Portal at *[portal.azure.com](https://portal.azure.com)* to create our Azure Static Web App.

# Create an Azure Static Web App

Once you're at the Azure Portal, search for **static** until you see **Static Web Apps (Preview)** show up—click that, and create a new instance.

From there, do the following:

1. Select an existing subscription, and resource group (or create a new one).
2. Enter a suitable name and region for your app. You'll see the only current SKU is the free one. 
3. Click the *Sign in with GitHub* to, well, sign in with your GitHub credentials and give Azure Static Web Apps rights to act on your behalf.
4. Select your account, the repository where your code resides, and the branch.
5. After you select those, a **Build Details** section displays. Select the type of app (like Blazor). When workflows trigger, they can use these presets to run steps based on app type—all out of the box.
6. **Important**: select the **App location**, **Api location**, and **App artifact location**, relative to your repo in GitHub.
   * **App location** - in my case, the folder that contains my Blazor project file. The default location is *Client*. 
   * **Api location** - select where your Azure Function project resides. The default location is *Api*.
   * **App artifact location** - the location of your deployed static files. The default is *wwwroot*.
7. Finally, click **Review + Create**, then **Create**. It should take just a few seconds to deploy your instance.

Your completed form should look similar to this:

![The Blast Off with Blazor app]({{ site.url }}{{ site.baseurl }}/assets/img/static-gh-details.png)

After the deployment completes, click **Go to resource**. You'll see a bunch of links to your new URL, deployment history, and a workflow file. Before your site is ready, the workflow file executes right away (it resides in the `.github/workflows` directory of your repo). 

# Inspect the workflow file

The workflow file is a YAML file that instructs Azure Static Web Apps how to build your site. Here's mine:

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_JOLLY_GROUND_0BDB93D10 }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for Github integrations (i.e. PR comments)
          action: "upload"
          ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
          # For more information regarding Static Web App workflow configurations, please visit: https://aka.ms/swaworkflowconfig
          app_location: "Client" # App source code path
          api_location: "Api" # Api source code path - optional
          app_artifact_location: "wwwroot" # Built app content directory - optional
          ###### End of Repository/Build Configurations ######

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_JOLLY_GROUND_0BDB93D10 }}
          action: "close"
```

This file looks daunting but as you look through it, it's actually pretty simple. If there's a direct push, or a pull request is created or is changed in **my main branch**, their Oryx build system—[which automatically compiles source code into runnable artifacts](https://github.com/microsoft/Oryx)—detects and builds our app, and uploads its content (including Azure Functions) to Azure.

Of course, you can modify this file to your liking.

You can head over to the **Actions** section of your GitHub repository to monitor the status of your workflow and view any logging.

## A gotcha about `package.json`

I'd love to tell you the build succeeded on the first try, but it kept spacing out:

![The Blast Off with Blazor app]({{ site.url }}{{ site.baseurl }}/assets/img/oryx-error.png)

In my project, I use Gulp to build and deploy Tailwind CSS styles ([thanks, Chris Sainty](https://chrissainty.com/integrating-tailwind-css-with-blazor-using-gulp-part-1/)). By accident, I deployed my `package.json` file to my repo. Because of that, Oryx detected a Node.js install, and therefore was looking for a `build` step in `package.json`. As a result, my build failed. 

This was a silly mistake on my end, but I can definitely see other scenarios where this could be an issue. Azure Static Web Apps is in preview, so they are ironing out some things, so this might get addressed—but now you know. If you want to push your `package.json` you would need to edit your workflow file to change build order, or a new `app_build_command` parameter.

# Pull request sites

This is my favorite thing about Azure Static Web Apps: when you create a pull request against your main branch, **GitHub Actions creates a temporary URL for you to view changes**. (When the PR is merged and closed, the workflow runs and deletes your temporary environment.)

For example, I created a PR to fix a minor bug. Once I click **Create pull request** in GitHub, a workflow will run and the PR will update with a link to the temporary environment:

![The Blast Off with Blazor app]({{ site.url }}{{ site.baseurl }}/assets/img/temp-site.png)

Additionally, if you hit up the **Environments** link in the Azure Portal, it'll display everything nicely:

![The Blast Off with Blazor app]({{ site.url }}{{ site.baseurl }}/assets/img/environments.png)

# A note on custom domains

Azure Static Web Apps supports custom domains. Sort of. I can have `https://www.blastoffwithblazor.com` but not `https://blastoffwithblazor.com`. That's right: root domain support is not included.

Luckily, [Burke Holland has you covered](https://burkeholland.github.io/posts/static-app-root-domain/). I understand this impact of this change reaches greater than Azure Static Web Apps, so it might take some time to support.

# Wrap up

This was ... a blast. I hope it eclipsed your expectations. In this post, we discussed the value of Azure Static Web Apps. We walked through our out-of-this-world sample app, created an Azure Static Web app solution, explored the workflow file, explained some gotchas, and explored how pull request sites work.

If you're big on Blazor stay tuned—the app will be getting some enhancements as we learn more about Blazor together!

Of course, if you have any questions [comet me on Twitter](https://twitter.com/daveabrock).

# References

* [Azure Static Web Apps with .NET and Blazor](https://devblogs.microsoft.com/aspnet/azure-static-web-apps-with-blazor/) (Aaron Powell)
* [Blazor WebAssembly on Azure Static Web Apps](https://www.hanselman.com/blog/blazor-webassembly-on-azure-static-web-apps) (Scott Hanselman)
* [I Love Azure Static Web Apps](https://consultwithgriff.com/i-love-azure-static-web-apps/) (Kevin W. Griffin)
* [How to deploy Blazor WASM & Azure Functions to Azure Static Web Apps](https://swimburger.net/blog/dotnet/how-to-deploy-aspnet-blazor-webassembly-to-azure-static-web-apps) (Niels Swimberghe)
* [How to configure a root domain in an Azure Static Web App](https://burkeholland.github.io/posts/static-app-root-domain/) (Burke Holland)
* [Azure Static Web Apps docs](https://docs.microsoft.com/azure/static-web-apps/) (Microsoft Docs)
* [Quickstart: Building your first static web app](https://docs.microsoft.com/azure/static-web-apps/getting-started?tabs=vanilla-javascript) (Microsoft Docs)
* [Publish a Blazor WebAssembly app and .NET API with Azure Static Web Apps](https://docs.microsoft.com/learn/modules/publish-app-service-static-web-app-api-dotnet/) (Microsoft Learn)