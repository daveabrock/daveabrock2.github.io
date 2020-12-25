---
date: "2020-12-04"
title: "Use ASP.NET Core route-to-code for simple JSON APIs"
subtitle: "In this post, we explore how you can use route-to-code instead of controllers, and the benefits and drawbacks."
tags: [aspnet-core, apis]
share-img: /assets/img/route-to-code-card.png
---

When you need to write an API in ASP.NET Core, you've traditionally been forced to use [ASP.NET Core MVC](https://docs.microsoft.com/aspnet/core/web-api/?view=aspnetcore-5.0). While it's very mature and full-featured, it also runs against the core principles of ASP.NET Core—it's not lightweight or as efficient as other ASP.NET Core offerings. As a result you're saddled with using all of a framework, even if you aren't using a lot of its features. In most cases, you're doing your logic somewhere else and the execution context (the CRUD actions over HTTP) deserves better.

This doesn't even include the pitfalls of using controllers and action methods. The MVC pattern invites you to abuse controllers with dependencies and bloat, as much as we preach to other developers about the importance of thin controllers. Microsoft is very aware. This week, in the ASP.NET Standup, architect David Fowler mentioned Project Houdini—an effort to help make APIs over MVC more lightweight and performant (more on this at the end of this post).

We now have [an alternative](https://docs.microsoft.com/aspnet/core/web-api/route-to-code?view=aspnetcore-5.0) called "route-to-code." You can now write ASP.NET Core APIs by connecting routing with your middleware. As a result, your code reads the request, writes the response, and you aren't reliant on a heavy framework and its advanced configuration. It's a wonderful pipe-to-pipe experience for simple JSON APIs. 

To be clear—and [Microsoft will be the first to tell you this](https://docs.microsoft.com/aspnet/core/web-api/route-to-code?view=aspnetcore-5.0#notable-missing-features-compared-to-web-api)—it is for basic JSON APIs that don't need things like model binding, content negotiation, and advanced validation. For those scenarios, ASP.NET Core MVC will always be available to you.  

Luckily for me, I just [wrote a sample API](https://github.com/daveabrock/HttpReplApi) in MVC to [look at the HttpRepl tool](https://daveabrock.com/2020/11/16/httprepl-openapi-swagger-netcoreapis). This is a great project to use to convert to route-to-code and show my experiences.

This post contains the following content.

- [How does route-to-code work?](#how-does-route-to-code-work)
- [A quick tour of the route-to-code MVC project](#a-quick-tour-of-the-route-to-code-mvc-project)
- [Migrate our APIs from MVC to route-to-code](#migrate-our-apis-from-mvc-to-route-to-code)
  - [The "get all items" endpoint](#the-get-all-items-endpoint)
  - [The "get by id" endpoint](#the-get-by-id-endpoint)
  - [The post endpoint](#the-post-endpoint)
  - [The delete endpoint](#the-delete-endpoint)
- [The path forward](#the-path-forward)
- [Wrap up](#wrap-up)

# How does route-to-code work?

In route-to-code, you specify the APIs in your project's `Startup.cs` file. In this file ([or even in another one](https://docs.microsoft.com/aspnet/core/web-api/route-to-code?view=aspnetcore-5.0#api-project-structure)), you define the routing and API logic in an application request's pipeline in [UseEndpoints](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints?view=aspnetcore-5.0).

To use this, ASP.NET Core gives you three helper methods to use:

- [ReadFromJsonAsync](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.httprequestjsonextensions.readfromjsonasync) - reads JSON from the request and deserializes it to a given type
- [WriteAsJsonAsync](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.httprequestjsonextensions.readfromjsonasync) - writes a value as JSON to the response body (and also sets the response type to `application/json`).
- [HasJsonContentType](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.httprequestjsonextensions.hasjsoncontenttype) - a boolean method that checks the `Content-Type` header for JSON

Because route-to-code is for basic APIs only, it does not currently support:

- Model binding or validation
- OpenAPI (Swagger UI)
- Constructor dependency injection
- Content negotiation

There are ways to work around this, as we'll see, but if you find yourself writing a lot of repetitive code, it's a good sign that you might be better off leveraging ASP.NET Core MVC.

# A quick tour of the route-to-code MVC project

I've got the route-to-code project [out on GitHub](https://github.com/daveabrock/RouteToCodePost). I won't go through all the setup code but will just link to the key parts if you care to explore further.

In the sample app, I've wired up an in-memory Entity Framework database. I'm using [a `SampleContext` class](https://github.com/daveabrock/RouteToCodePost/blob/master/Data/SampleContext.cs) that uses Entity Framework Core's `DbContext`. I've got [a `SeedData` class](https://github.com/daveabrock/RouteToCodePost/blob/master/Data/SeedData.cs) that populates the database with [a model](https://github.com/daveabrock/RouteToCodePost/blob/master/Data/ApiModels.cs) using C# 9 records.

In the [`ConfigureServices` middleware in `Startup.cs`](https://github.com/daveabrock/RouteToCodePost/blob/master/Startup.cs), I add the in-memory database to the container, then [flip on the switch in `Program.cs`](https://github.com/daveabrock/RouteToCodePost/blob/master/Program.cs).

# Migrate our APIs from MVC to route-to-code

In this post, I'll look at the GET, GET (by id), POST, and DELETE methods in my MVC controller and see what it takes to migrate them to route-to-code.

Before we do that, a quick note. In MVC, I'm using constructor injection to access my data from my `SampleContext`. Because `context` is a common term in route-to-code, I'll be naming it `repository` in route-to-code.

Anyway, here's how the constructor injection looks in the ASP.NET Core MVC project:

```csharp
using HttpReplApi.Data;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using HttpReplApi.ApiModels;

namespace HttpReplApi.Controllers
{
    [Produces("application/json")]
    [Route("[controller]")]
    public class BandsController : ControllerBase
    {
        private readonly SampleContext _context;

        public BandsController(SampleContext context)
        {
            _context = context;
        }

        // action methods

    }
}
```

In the following sections, I'll go through all the endpoints and see what it takes to move to route-to-code.

## The "get all items" endpoint

For a "get all" endpoint, the MVC action method is pretty self-explanatory. Since I've injected the context and the data is ready for me to query, I just need to get it.

```csharp
[HttpGet]
public ActionResult<IEnumerable<Band>> Get() =>
    _context.Bands.ToList();
```

As mentioned earlier, all the endpoint routing and logic in route-to-code will be inside of `app.UseEndpoints`:

```csharp
app.UseEndpoints(endpoints =>
{
    // endpoints here
};
```

Now, we can write a `MapGet` call to define and configure our endpoint. Take a look at this code and we'll discuss after.

```csharp
endpoints.MapGet("/bands", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();
    var bands = await repository.Bands.ToListAsync();
    await context.Response.WriteAsJsonAsync(bands);
});
```

I'm requesting an HTTP GET endpoint with the `/bands` route template. In this case, the `context` is `HttpContext`, where I can request services, get query strings, and read and write JSON. 

I can't use constructor-based dependency injection (DI) using route-to-code. Because there's no framework to inject services into, we need to manually resolve these services. So, from `HttpContext.RequestServices`, I can call `GetService` to resolve my `SampleContext`. For any services with a transient or scoped lifetime, you need to use the `RequestServices`. For services with singleton scopes, like loggers, you can declare it from the service provider. In those cases, you can use those across different requests. But in our case, we'll need to repeat the `repository` service discovery for every endpoint, which is a drag.

Finally, I'll send back the `Bands` collection as JSON using the `Response.WriteAsJsonAsync` method.

## The "get by id" endpoint

What about the most common use case—getting an item by `id`? Here's how we do it in an MVC controller:

```csharp
[HttpGet("{id}")]
public async Task<ActionResult<Band>> GetById(int id)
{
    var band = await _context.Bands.FindAsync(id);

    if (band is null)
    {
        return NotFound();
    }

    return band;
}
```

In route-to-code, here's how it looks:

```csharp
endpoints.MapGet("/bands/{id}", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();
    var id = context.Request.RouteValues["id"];
    var band = await repository.Bands.FindAsync(Convert.ToInt32(id));

    if (band is null)
    {
        context.Response.StatusCode = StatusCodes.Status404NotFound;
        return;
    }
    await context.Response.WriteAsJsonAsync(band);
});
```

The route matches a passed in `id` from the route. I need to fetch it by accessing it in `Request.RouteValues`. One note that isn't in the documentation: because there's no model binding, I need to manually convert the `id` string to an integer. After I figured that out, I was able to call `FindAsync` from my context, validate it, then write it in the `WriteAsJsonAsync` method.

## The post endpoint

Here's where things can get interesting—how can we post without model binding or validation?

Here's how the existing controller works in ASP.NET Core MVC.

```csharp
[HttpPost]
public async Task<ActionResult<Band>> Create(Band band)
{
    _context.Bands.Add(band);
    await _context.SaveChangesAsync();

    return CreatedAtAction(nameof(GetById), new { id = band.Id }, band);
}
```

In route-to-code, here's what I wrote:

```csharp
endpoints.MapPost("/bands", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();

    if (!context.Request.HasJsonContentType())
    {
        context.Response.StatusCode = StatusCodes.Status415UnsupportedMediaType;
        return;
    }

    var band = await context.Request.ReadFromJsonAsync<Band>();
    await repository.SaveChangesAsync();
    await context.Response.WriteAsJsonAsync(band);
});
```

As we saw earlier, we have a `HasJsonContentType` method at our disposal to see if I'm getting JSON. If not, I can set the appropriate status code and return.

Since I have JSON (no validation or binding, just verification that it *is* JSON), I can read it in and save it to the database. Once I'm done, I can either `WriteAsJsonAsync` to give the caller back the new record, or do something like this:

```csharp
context.Response.StatusCode = StatusCodes.Status201Created;
return;
```

The `WriteAsJsonAsync` will return a 200, and only a 200. Remember, this needs to be done by hand because we don't have a framework.

## The delete endpoint

For our DELETE API, we'll see it's quite similar to POST (we're just doing the opposite action).

Here's what we do in the traditional MVC controller:

```csharp
[HttpDelete("{id}")]
public async Task<IActionResult> Delete(int id)
{
    var band = await _context.Bands.FindAsync(id);

    if (band is null)
    {
        return NotFound();
    }

    _context.Bands.Remove(band);
    await _context.SaveChangesAsync();

    return NoContent();
}
```

Here's what I wrote in the route-to-code endpoint.

```csharp
endpoints.MapDelete("/bands/{id}", async context =>
{
    var repository = context.RequestServices.GetService<SampleContext>();

    var id = context.Request.RouteValues["id"];
    var band = await repository.Bands.FindAsync(Convert.ToInt32(id));

    if (band is null)
    {
        context.Response.StatusCode = StatusCodes.Status404NotFound;
        return;
    }

    repository.Bands.Remove(band);
    await repository.SaveChangesAsync();
    context.Response.StatusCode = StatusCodes.Status204NoContent;
    return;
});
```

As you can see, there's a lot of considerations to make even for ridiculously simple APIs like this one. It's tough to work around dependency injection if you rely on it in your application.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I wish the <a href="https://twitter.com/aspnet?ref_src=twsrc%5Etfw">@aspnet</a> core Map methods supported DI enabled requestDelegates 🥺<br><br>Instead I have to program like a caveman <a href="https://t.co/JtjSrkF6yI">pic.twitter.com/JtjSrkF6yI</a></p>&mdash; Khalid 🎅🎄 (@buhakmeh) <a href="https://twitter.com/buhakmeh/status/1334576089755213828?ref_src=twsrc%5Etfw">December 3, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

That's the price you pay for using a no-framework solution like this one. In return you get simplicity and performance. It's up to you to decide if you need all the bells and whistles or if you only need route-to-code.

# The path forward

The use case for this is admittedly small, so here's a thought: what if I could enjoy these benefits in MVC? That's the impetus behind "Project Houdini", which David Fowler discussed in this week's ASP.NET Standup. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/d9Bjg31VuHw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


This effort focuses on pushing MVC productivity features to the core of the stack with an eye on performance. Fowler showed off a way to generate these route-to-code APIs for you at compile time using source generation—allowing you to keep writing the traditional MVC code. Of course, when AOT comes with .NET 6, it'll benefit from performance and treeshaking capabilities.

# Wrap up

In this post, we discussed the route-to-code API style, and how it can help you write simpler APIs. We looked at how to migrate controller code to route-to-code, and also looked at the path forward.
