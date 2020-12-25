---
date: "2020-11-16"
title: "Use OpenAPI, Swagger UI, and HttpRepl in ASP.NET Core 5 to supercharge your API development"
subtitle: "In the latest post, we look at how easy it is to work with Swagger and HttpRepl in ASP.NET Core 5."
tags: [aspnet-core]
share-img: /assets/img/swagger-repl-card.png
---

When developing APIs in ASP.NET Core, you've got many tools at your disposal. Long gone are the days when you run your app from Visual Studio and call your `localhost` endpoint in your browser. 

Over the last several years, a lot of tools have emerged that use the OpenAPI specification—most commonly, the [Swagger project](https://github.com/swagger-api). Many ASP.NET Core API developers are familiar with [Swagger UI](https://swagger.io/tools/swagger-ui/), a REST documentation tool that allows developers—either those developing it or developers consuming it—to interact with an API from a nice interface built from a project's `swagger.json` file. In the .NET world, the functionality comes from [the Swashbuckle library](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/), at 80 million NuGet downloads and counting.

Additionally, I've been impressed by [the HttpRepl project](https://github.com/dotnet/HttpRepl), which allows you explore your APIs from the command line. Different from utilities [like curl](https://curl.se/), it allows you to explore APIs from the command-line similar to how you explore directories and files—all from a simple and lightweight interface. You can `cd` into your endpoints and call them quickly to achieve lightning-fast feedback. Even better, it has OpenAPI support (including [the ability to perform OpenAPI validation on connect](https://github.com/dotnet/HttpRepl/pull/432)).

While these utilities are not new with ASP.NET Core 5, it's now much easier to get started. This post will discuss these tools.

Before you get started, you should have [an ASP.NET Core API](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api?view=aspnetcore-5.0&tabs=visual-studio) ready—likely one with create-read-update-delete (CRUD) operations. Feel free [to clone and use mine](https://github.com/daveabrock/HttpReplApi), if you'd prefer.

This post contains the following content.

- [What's the difference between OpenAPI and Swagger?](#whats-the-difference-between-openapi-and-swagger)
- [Use Swagger UI with ASP.NET Core projects by default](#use-swagger-ui-with-aspnet-core-projects-by-default)
  - [How it's enabled, and how you can opt out](#how-its-enabled-and-how-you-can-opt-out)
  - [Verify the middleware and launchSettings.json](#verify-the-middleware-and-launchsettingsjson)
- [Use HttpRepl for a great command-line experience](#use-httprepl-for-a-great-command-line-experience)
  - [Install](#install)
  - [Basic operations](#basic-operations)
  - [Modify data with a default editor](#modify-data-with-a-default-editor)
  - [Remove repetition with a script](#remove-repetition-with-a-script)
  - [Learn more about configuring HttpRepl](#learn-more-about-configuring-httprepl)
- [Wrap up](#wrap-up)

# What's the difference between OpenAPI and Swagger?

If you've heard OpenAPI and Swagger used interchangeably, you might be wondering what the difference is.

In short, OpenAPI is a specification used for documenting the capabilities of your API. Swagger is a set of tools from SmartBear (both open-source and commercial) that use the OpenAPI specification (like Swagger UI).

# Use Swagger UI with ASP.NET Core projects by default

For the uninitiated, the Swashbuckle project allows you to use Swagger UI—a tool that gives you the ability to render dynamic pages that allow to describe, document, and execute your API endpoints. Here's how mine looks.

![A high-level look at my Swagger page]({{ site.url }}{{ site.baseurl }}/assets/img/swagger-ui-high-level.png)

And if you look at a sample POST (or any action), you'll see a sample schema and response codes.

![A sample post in Swagger]({{ site.url }}{{ site.baseurl }}/assets/img/movies-post.png)

In previous versions of ASP.NET Core, you had to manually download the Swashbuckle package, configure the middleware, and optionally change your `launchSettings.json` file. Now, with ASP.NET Core 5, that's baked in automatically.

A big driver for the default OpenAPI support is [the integration with Azure API Management](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#azure-api-management-import). Now with OpenAPI support, the Visual Studio publishing experience offers an additional step—to automatically import APIs into Azure API Management. To the cloud!

## How it's enabled, and how you can opt out

If you create an ASP.NET Core 5 Web API project, you'll see an **Enable OpenAPI support** checkbox that's enabled by default. Just deselect it if you don't want it.

![Visual Studio OpenAPI checkbox]({{ site.url }}{{ site.baseurl }}/assets/img/enable-open-api-support.png)

If you create API projects using `dotnet new webapi` it'll be baked in, too. (To opt out from OpenAPI support here, execute `dotnet new webapi --no-openapi true`.)
{: .notice--info}

## Verify the middleware and launchSettings.json

After you create a project, we can see it's all done for us. If you open your *.csproj* file, you'll see the reference:

```xml
<ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="5.6.3" />
</ItemGroup>
```

Then, in the `ConfigureServices` method in your `Startup.cs`, you'll see your Swagger doc attributes defined and injected (this information will display at the top of your Swagger UI page). You can [definitely add more to this](https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-5.0&tabs=visual-studio#api-info-and-description).

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "HttpReplApi", Version = "v1" });
    });
}
```

And, in the `Configure` method in that same file you see the configuration of static file middleware:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseSwagger();
        app.UseSwaggerUI(c =>
        {
            c.SwaggerEndpoint("/swagger/v1/swagger.json", "HttpReplApi v1");
        });
    }

    // other services removed for brevity
}
```

Then, in `launchSettings.json`, you'll see your project launches the Swagger UI page with the `/swagger` path, instead of a lonely blank page.

```json
{
  "profiles": {
    "HttpReplApi": {
      "commandName": "Project",
      "dotnetRunMessages": "true",
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

Now that you've got this set up for you, you can go to town on documenting your API. You can [add XML documentation](https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-5.0&tabs=visual-studio#xml-comments) and [data annotations](https://docs.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-5.0&tabs=visual-studio#data-annotations)—two easy ways to boost your Swagger docs. For example, the `Produces` annotations help define the status codes to expect.

From [my `BandsController.cs`](https://github.com/daveabrock/HttpReplApi/blob/master/HttpReplApi/Controllers/BandsController.cs), I use a lot of [`Produces` annotations](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute?view=aspnetcore-5.0):

```csharp
[HttpDelete("{id}")]
[ProducesResponseType(StatusCodes.Status204NoContent)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ProducesDefaultResponseType]
public async Task<IActionResult> Delete(int id)
{
  // delete by id logic
}
```

I love that this is available by default, but would love to see more OpenAPI scaffolding in the templates. It might be a little opinionated, but adding some `Produces` annotations in the default `WeatherForecast` controller would allow for a better developer experience with Swagger out of the box (since it is now out of the box).

# Use HttpRepl for a great command-line experience

You can use HttpRepl to test your ASP.NET Core Web APIs from the command-line quickly and easily.

## Install

You install HttpRepl as a .NET Core Global Tool, by executing the following from the .NET Core CLI:

```bash
dotnet tool install -g Microsoft.dotnet-httprepl
```

As a global tool, you can run it from anywhere.

## Basic operations

To get started, I can launch my app and connect to it from `httprepl`:

```bash
httprepl https://localhost:5001
```

If I do an `ls` or `dir`, I can look at my endpoints:

![Doing a dir from the root]({{ site.url }}{{ site.baseurl }}/assets/img/localhost-dir.png)

To execute a get on my `/bands` endpoint, I get a response payload, with headers:

![Doing a GET on bands]({{ site.url }}{{ site.baseurl }}/assets/img/get-bands.png)

Then, I could say `get {id}` to get a `Band` by id:

![Doing a get by id]({{ site.url }}{{ site.baseurl }}/assets/img/get-eagles.png)

## Modify data with a default editor

What happens when you want to update some data? You could do something like this...

```bash
post --content "{"id":2,"name":"Tool"}"
```

...but that's only manageable for the simplest of scenarios. You'll want to set up a default text editor to work with request bodies. You can construct a request, then HttpRepl will wait for you to save and close the tab or window.

In HttpRepl, you can set various preferences, including a default editor. I'm using Visual Studio Code on Windows, so would execute something like this...

```bash
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

If using Visual Studio Code, you'll also want to pass a `--wait flag so VS Code waits for a closed file before returning.

```bash
pref set editor.command.default.arguments "--wait"
```

You can also [set various other preferences](https://docs.microsoft.com/aspnet/core/web-api/http-repl/?view=aspnetcore-5.0&tabs=windows#view-the-settings), like color and indentation.

Now, when you say `post` from your endpoint, VS Code opens a `.tmp` file with a default request body, then waits for you to save and close it. The POST should process successfully.

![Performing a post]({{ site.url }}{{ site.baseurl }}/assets/img/post-brothers.png)

With the ID in hand, I can do the same steps to modify using `put 5`, then run `get 5` to confirm my changes:

![Performing a put]({{ site.url }}{{ site.baseurl }}/assets/img/put-get-brothers.png)

If I want to delete my resource, I can execute `delete 5` and it'll be gone. I tweeted a video of me cycling through the CRUD operations.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I love HttpRepl. Check it out: going through the big four HTTP verbs in just 34 seconds! <br><br>This isn&#39;t a race, and your models will be much more complex obviously, but it&#39;s a good demo of how fast your API development feedback can be.<br><br>cc/ <a href="https://twitter.com/Scott_Addie?ref_src=twsrc%5Etfw">@Scott_Addie</a> <a href="https://twitter.com/bradygaster?ref_src=twsrc%5Etfw">@bradygaster</a> <a href="https://twitter.com/hashtag/dotnet?src=hash&amp;ref_src=twsrc%5Etfw">#dotnet</a> <a href="https://t.co/6w41j5esOk">pic.twitter.com/6w41j5esOk</a></p>&mdash; Dave (@daveabrock) <a href="https://twitter.com/daveabrock/status/1328413854859202561?ref_src=twsrc%5Etfw">November 16, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## Remove repetition with a script

If I find myself using the same test scenarios, I can throw these commands in a *.txt* file instead. For example, I created an *api-script.txt* file that I execute from the root of my `httprepl` session:

```txt
cd Bands
ls
get
post --content "{"id": 20, "name": "The Blue Medium Chili Peppers"}"
put 20 --content "{"id": 20, "name": "The Red Mild Chili Peppers"}"
get 20
delete 20
```

Then I can run it using `run {path_to_script}`.

## Learn more about configuring HttpRepl

This is a straightforward walkthrough, but you can do a whole lot more. I was impressed with the robust configuration options. This includes manually pointing to the OpenAPI description, verbose logging, setting preferences, configuring formatting, customizing headers and streaming, logging to a file, using additional HTTP verbs, testing secured endpoints, accessing Azure-hosted endpoints, and even configuring your tools to launch HttpRepl.

All this information can be found at the [official doc,](https://docs.microsoft.com/en-us/aspnet/core/web-api/http-repl/?view=aspnetcore-5.0&tabs=windows#manually-point-to-the-openapi-description-for-the-web-api) wonderfully done by [our friend](https://daveabrock.com/2020/08/15/dev-discussions-scott-addie) [Scott Addie](https://twitter.com/Scott_Addie).

# Wrap up

In this post, we talked about the difference between OpenAPI and Swagger, using Swagger UI by default in your ASP.NET Core Web API projects, and how to use the HttpRepl tool.

As always, let me know your experience with these tools.
