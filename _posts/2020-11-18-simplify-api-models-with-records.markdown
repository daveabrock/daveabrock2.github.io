---
date: "2020-11-18"
title: "Simplify your ASP.NET Core API models with C# 9 records"
subtitle: "In this post, a quick tip about how to use records to simplify your API models."
tags: [aspnet-core]
share-img: /assets/img/record-models.png
---

Out of all the new capabilities C# 9 brings, [records are my favorite](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records). With positional syntax, they are [immutable by default](https://daveabrock.com/2020/11/02/csharp-9-records-immutable-default), which makes working with data classes a snap. I love the possibility of maintaining mutable state in C# where appropriate, like for business logic, and maintaining immutability (and data equality!) with records.

And did you know that [with ASP.NET Core 5, model binding and validation supports record types](https://docs.microsoft.com/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-5.0#model-binding-and-validation-with-c-9-record-types)?

In the last post about [OpenAPI support in ASP.NET Core 5](https://daveabrock.com/2020/11/16/httprepl-openapi-swagger-netcoreapis), I used a sample project that worked with three *very simple* endpoints (or controllers): `Bands`, `Movies`, and `People`. Each model was in its own class, like this:

`Band.cs`:

```csharp
using System.ComponentModel.DataAnnotations;

namespace HttpReplApi.Models
{
    public class Band
    {
        public int Id { get; set; }

        [Required]
        public string Name { get; set; }
    }
}
```

`Movie.cs`:

```csharp
using System.ComponentModel.DataAnnotations;

namespace HttpReplApi.Models
{
    public class Movie
    {
        public int Id { get; set; }

        [Required]
        public string Name { get; set; }
        public int ReleaseYear { get; set; }

    }
}
```

`Person.cs`:

```csharp
using System.ComponentModel.DataAnnotations;

namespace HttpReplApi.Models
{
    public class Person
    {
        public int Id { get; set; }

        [Required]
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

I can simplify these with using records instead. In the root, I'll just create an `ApiModels.cs` file (I could have them in the controllers themselves, but that feels ... messy):

```csharp
using System.ComponentModel.DataAnnotations;

namespace HttpReplApi
{
    public record Band(int Id, [Required] string Name);
    public record Movie(int Id, [Required] string Name, int ReleaseYear);
    public record Person(int Id, [Required] string FirstName, string LastName);
}
```

After I [change my `SeedData` class to use positional parameters](https://github.com/daveabrock/HttpReplApi/blob/master/HttpReplApi/Data/SeedData.cs), I am good to go—this took me about 90 seconds.

For some fun, if I grab a movie by ID, I can use the deconstruction support to get out my properties. (I'm using a discard since I'm not doing anything with the first argument, the `Id`.)

```csharp
[HttpGet("{id}")]
public async Task<ActionResult<Movie>> GetById(int id)
{
    var movie = await _context.Movies.FindAsync(id);

    if (movie is null)
    {
        return NotFound();
    }

    var (_, name, year) = movie;
    _logger.LogInformation($"We have {name} from {year}");

    return movie;
}
```




