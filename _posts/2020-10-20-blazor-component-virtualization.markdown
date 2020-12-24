---
date: "2020-10-20"
title: "Improve rendering performance with Blazor component virtualization"
excerpt: "Use Blazor component virtualization to improve perceived performance of components that work with large data sets."
tags: [blazor, aspnet-core]
header:
    overlay_image: /assets/assets/img/virtualization-card.png
    overlay_filter: 0.8
---

When measuring web performance—especially on page load—it's not always about a consistent metric, such as how quickly the entire HTML tree loads from the server. It's helpful to think in terms of [perceived performance](https://developer.mozilla.org/en-US/docs/Learn/Performance/perceived_performance)—do you understand what needs to be rendered now, and what can be rendered later? End users should never have to wait for something that, well, can wait.

The ASP.NET Core team recently rolled out [Blazor component virtualization](https://docs.microsoft.com/aspnet/core/blazor/components/virtualization?view=aspnetcore-5.0), a technique for limiting UI rendering to the visible page elements only. You can easily leverage this through a built-in `Virtualize` component.

Here's a common scenario: let's say you have a requirement to list a bunch of items on a table, and you might be stuck with a lot of data. If you're listing several thousand items on a page, users often have to sit and wait for the entire site to load. With Blazor component virtualization, the app will load only the records in the user's window, then render more only when scrolling.

This post will discuss the following content.

- [Prerequisites](#prerequisites)
- [The "up and running in 30 seconds" solution](#the-up-and-running-in-30-seconds-solution)
- [Our sample app](#our-sample-app)
- [Additional parameters](#additional-parameters)
  - [OverscanCount](#overscancount)
  - [Lazy loading with `ItemsProvider` and `Placeholder`](#lazy-loading-with-itemsprovider-and-placeholder)
  - [ItemSize](#itemsize)
- [Reminder: this isn't a catch-all](#reminder-this-isnt-a-catch-all)
- [Wrap up](#wrap-up)
- [References](#references)

**Heads up!** This post assumes you have [some familiarity with Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor), like how to create and render a basic component.
{: .notice--warning}

# Prerequisites

To work with Blazor component virtualization, you need .NET 5 RC1 or greater. Head on over to the [.NET 5 SDK downloads](https://dotnet.microsoft.com/download/dotnet/5.0), or have Visual Studio 2019 Preview 3 (v16.8) or greater installed.

# The "up and running in 30 seconds" solution

Assume you have a collection called `people` that's a list of `Person` objects with properties like `FirstName`, `LastName`, and so on. In your component's `.razor` file, replace your traditional for-each loop...

```csharp
@foreach (var person in people)
{
    <p>
        @person.FirstName @person.LastName is only fun on Fridays.
    </p>
}
```

... with the `Virtualize` component, and pass the collection into the `Items` parameter and use `context` to access your object's properties:

```html
<Virtualize Items="@people">
    <p>
        @context.FirstName @context.LastName is only fun on Fridays.
    </p>
</Virtualize>
```

Additionally, you could explicitly specify a context in your component:

```html
<Virtualize Context="person" Items="@people">
    <p>
        @person.FirstName @person.LastName is only fun on Fridays.
    </p>
</Virtualize>
```

The component does the hard work of getting the height of your container and the size of the items to render. When we speak of the "container" we are talking about the rendered element: it can be one or more Razor components, a mix of HTML and Razor components, or plain old Razor code.

That's really how easy it is, and will cover a majority of your use cases. Keep reading to understand the `Virtualize` component in greater detail—and how you can customize and extend it for your specific needs.

# Our sample app

To further illustrate the need for Blazor component virtualization, let's kick things up a notch. In the sample Blazor app's `FetchData` component (or any component you'd like), let's make a bunch of objects in memory when the page loads (in `OnInitializedAsync`). In the `@code` block of the component, we'll populate a list of 10,000 cars to display on the page. (To state the obvious, we should never *actually* do this.)

```csharp
private List<Car> cars;

protected override async Task OnInitializedAsync()
{
  cars = await MakeTenThousandCars();
}

private async Task<List<Car>> MakeTenThousandCars()
{
  List<Car> myCarList = new List<Car>();

  for (int i = 0; i < 10000; i++)
  {
    var car = new Car()
    {
      Id = Guid.NewGuid(),
      Name = $"Car {i}",
      Cost = i * 100
    };

    myCarList.Add(car);
  }  
  return await Task.FromResult(myCarList);
}

public class Car
{
  public Guid Id { get; set; }
  public string Name { get; set; }
  public int Cost { get; set; }
}
```

Then, in the markup: when we get all our cars, we'll lay them out in a single table.

```html
@if (cars == null)
{
  <p><em>Loading so many cars...</em></p>
}
else
{
  <table class="table">
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Cost</th>
      </tr>
    </thead>
    <tbody>
      @foreach (var car in cars)
      {
        <tr>
            <td>@car.Id</td>
            <td>@car.Name</td>
            <td>@car.Cost</td>
        </tr>
       }
    </tbody>
  </table>
}
```

Launch your app, fire up your favorite dev tools, and head over to the **Fetch data** link at `http://localhost:xxxx/fetchdata`.

According to my dev tools with the cache disabled, load ranges anywhere from 2.5 to 4 seconds (I refreshed ten times) and quite a bit of lag.

As before, I can do is replace my for-loop with the following, then re-launch my application.

```html
<Virtualize Items="cars" Context="car">
  <tr>
    <td>@car.Id</td>
    <td>@car.Name</td>
    <td>@car.Cost</td>
  </tr>
</Virtualize>
```

Now, we're looking at 1.2 to 1.9 seconds uncached, about twice the speed.

In the following video, keep an eye on Chrome Developer tools. You'll see how the elements change as I scroll.

<video autoplay controls muted loop>
  <source src="{{ site.url }}{{ site.baseurl }}/videos/Twitter.mp4" type="video/mp4">
</video>

The `Items` and `Context` are the most common parameters to use here, but there's plenty more you can utilize.

# Additional parameters

We'll talk through four additional parameters: `OverscanCount`, `ItemsDelegate`, `Placeholder`, and `ItemSize`.

## OverscanCount

You can also specify an `OverscanCount` parameter, which specifies how many more items to render before and after the viewable container. The default is currently three items ([src](https://github.com/dotnet/aspnetcore/blob/686150953f7ccd3f56afd4d7b2f0a934c3557a10/src/Components/Web/src/Virtualization/Virtualize.cs#L100)). You may want to tweak this to prevent excessive rendering when you know there will be a lot of scrolling.

Here's how we would do it in our first example:

```html
<Virtualize Items="@cars" Context="car" OverscanCount="5">
  <tr>
    <td>@car.Id</td>
    <td>@car.Name</td>
    <td>@car.Cost</td>
  </tr>
</Virtualize>
```

Obviously, the higher the number the more elements you'll render—so use this cautiously.

## Lazy loading with `ItemsProvider` and `Placeholder`

To the casual observer, it might seem like the rendering fetches data periodically. In reality all data is queued up in memory by default. If you don't want to do this, you can work with an item provider delegate method—in C#, [a delegate](https://docs.microsoft.com/dotnet/csharp/programming-guide/delegates/) is a type that refers to methods with a parameter list and return type.

For example, you might be calling an external API (or any other service) and not always know how much data you're getting back. With the `ItemsProvider` parameter, you can ask for requested items on demand.

The provider asks for an `ItemsProviderRequest`, which contains a `StartIndex` (where to start) and  `Count` (how many items to provide), and a `CancellationToken`. After fetching the data, the data returns an `ItemsProviderResult<TItem>` along with a total item count.

Let's add this method to our `@code` block in our Razor file:

```csharp
private async ValueTask<ItemsProviderResult<Car>> LoadCars(ItemsProviderRequest request)
{
  var cars = await MakeTenThousandCars();
  return new ItemsProviderResult<Car>(cars.Skip(request.StartIndex).Take(request.Count), cars.Count());
}
```

If you aren't familiar with the [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/):

- The `Skip` operator [skips over](https://docs.microsoft.com/dotnet/api/system.linq.queryable.skip?view=netcore-3.1) elements until we get to the `StartIndex` and return the remainder
- The `Take` operator takes the next *x* elements from what `Skip` returned, where *x* is the `Count` to return

Before we update the Razor code, let's talk about `Placeholder`.

Typically with Blazor components that run on initialization, you'll see this pattern to display a message while the collection is populating.

```html
@if (cars == null)
{
  <p><em>Loading so many cars...</em></p>
}
```

In our case, you can remove that block and replace it with a `Placeholder`. Here's the updated code.

```html
<Virtualize Context="car" ItemsProvider="@LoadCars">
    <ItemContent>
        <tr>
            <td>@car.Id</td>
            <td>@car.Name</td>
            <td>@car.Cost</td>
        </tr>
    </ItemContent>
    <Placeholder>
        <p>Loading so many cars...</p>
    </Placeholder>
</Virtualize>
```

## ItemSize

You can also specify the size of each item, in pixels. The default is 50px ([src](https://github.com/dotnet/aspnetcore/blob/686150953f7ccd3f56afd4d7b2f0a934c3557a10/src/Components/Web/src/Virtualization/Virtualize.cs#L79)).

Here's how we'd do it in our example:

```html
<Virtualize Items="@cars" Context="car" ItemSize="15">
  <tr>
    <td>@car.Id</td>
    <td>@car.Name</td>
    <td>@car.Cost</td>
  </tr>
</Virtualize>
```

# Reminder: this isn't a catch-all

Considering how easy it is to use `Virtualize` it might be easy to use it as a catch-all: *I have to load a bunch of stuff, so I'll drop it here*. It's important to use this component thoughtfully. For example, all items must be a known height so that Blazor can calculate the total scroll range and, therefore, what to render. 

For example, you might have elements that wrap unexpectedly when using different window sizes. In these cases, the virtualization logic won't work as you expect ([there's some chatter about how to handle this](https://github.com/dotnet/aspnetcore/issues/26099)).

As with anything else in your codebase, it's a story of tradeoffs. Understand them before you implement.


# Wrap up

In this post, we talked about the `Virtualize` component and how it can help you. We worked through a quick and dirty example, then worked through a list with a lot of records. Then, we talked about other parameters available to you, such as `OverscanCount` and `ItemSize`. We then discussed how to perform lazy loading with the `ItemsDelegate` and `Placeholder` parameters.

# References

- [ASP.NET Core Blazor component virtualization](https://docs.microsoft.com/aspnet/core/blazor/components/virtualization?view=aspnetcore-5.0) (Microsoft Docs)
- [Virtualize component source code](https://github.com/dotnet/aspnetcore/blob/master/src/Components/Web/src/Virtualization/Virtualize.cs#L79) (GitHub)
