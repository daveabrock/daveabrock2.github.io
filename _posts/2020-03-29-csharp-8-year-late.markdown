---
date: "2020-03-29"
title: "C# 8, A Year Late"
tags: [csharp]
subtitle: I catch up on all that C# 8 has to offer.
share-img: /assets/img/year-late-card.png
readtime: true
---

After working in C# for the better part of a decade, last year I went on a hiatus from the .NET ecosystem and dove in head-first on Node.js, TypeScript, and React. Now, I'm back! Since I've been writing C# code again and shaking off the rust, Visual Studio 2019 (and more specifically, the Roslyn analyzers) keep reminding me: "**Fun fact: did you know you can do this new thing?**"

After repeating this exercise approximately 26 times, I decided to do some actual research and see what's changed with C# 8—I hope this helps you, too.

This article, while on the long side (sorry!), just mentions a few of my favorite improvements to the language. If you want to geek out on everything, the [What's New in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8) article is a great place to start.

**Worth mentioning**: According [to the article in question](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8), C# 8.0 is supported on .NET Core 3.*x* and .NET Standard 2.1 (see [the documentation](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version) on language versioning).

Also, a big thank you goes out to Microsoft's [David Pine](https://twitter.com/davidpine7) for his valuable feedback on this post.

This post contains the following content.

- [Simplified using declarations](#simplified-using-declarations)
- [Enhanced pattern matching](#enhanced-pattern-matching)
  - [Switch statements are now switch expressions](#switch-statements-are-now-switch-expressions)
  - [Property patterns](#property-patterns)
- [Default interface methods](#default-interface-methods)
- [Nullable reference types and null-coalescing](#nullable-reference-types-and-null-coalescing)
  - [Nullable reference types](#nullable-reference-types)
  - [Null-coalescing](#null-coalescing)
- [Async streams](#async-streams)
- [Indices and ranges](#indices-and-ranges)
  - [Indexes](#indexes)
  - [Ranges](#ranges)
- [Staying current with C# developments](#staying-current-with-c-developments)

# Simplified using declarations

When you use a **using** declaration, you are telling the C# compiler that the current variable should automatically be disposed at the end of the variable's scope. Remember how your grandparents did this, way back in C# 7?

```csharp
// using System.IO;

static void DoStuffWithAFile(string doingSomeStuff)
{
    using (var file = new StreamWriter("MyFile.txt"))
    {
        // three for-eaches
        // five logging statements
        // nine if statements
    }
}
```

Now, you can accomplish this with a simplified **using var** statement and avoid being caught up in bracket hell. It doesn't seem like a huge deal, but definitely adds to code quality as it makes the code more readable and maintainable.

```csharp
static void DoStuffWithAFile(string doingSomeStuff)
{
    using var file = new StreamWriter("MyFile.txt");
    // three for-eaches
    // nine if statements
}
```

# Enhanced pattern matching

C# 8 introduces a lot of new pattern matching functionality, which helps provide a line of demarcation between data and functionality. My favorite improvements include a refined switch expression and simplified property patterns.

## Switch statements are now switch expressions

With your regular switch expression, you have to go through a lot of motions with typing out each `case`, some `break`s (if needed), and a `default` value.

Let's take a look at an admittedly simple switch statement we've all written more than we'd like to admit.

```csharp
public static string FindAProgrammingLanguage(string languageInput)
{
    string languagePhrase;

    switch (languageInput)
    {
        case "C#":
            languagePhrase = "C# is fun!";
            break;
        case "JavaScript":
            languagePhrase = "JavaScript is mostly fun!";
            break;
        case "TypeScript":
            languagePhrase = "TypeScript makes JavaScript more fun!";
            break;
        case "C++":
            languagePhrase = "C++ has pointers!";
            break;
        default:
             throw new Exception("You code in something else I don't recognize.");
    };
    return languagePhrase;
}
```

With syntax improvements in C# 8, we can:

- Replace the `case` and `:` elements with a more concise `=>`
- Replace the `default` statement with `_`.
- Turn the concept of a switch statement into a switch expression

Look at us now!

```csharp
public static string FindAProgrammingLanguage(string languageInput)
{
    string languagePhrase = languageInput switch
    {
        "C#" => "C# is fun!",
        "JavaScript" => "JavaScript is mostly fun!",
        "TypeScript" => "TypeScript makes JavaScript more fun!",
        "C++" => "C++ has pointers!",
         _ => throw new Exception("You code in something else I don't recognize."),
    };
    return languagePhrase;
}
```

## Property patterns

Let's use this switch *expression* to look at property patterns, which allow you to work with properties dependent on constant, predictable values.

If we wanted to match if `programmer.PreferredLanguage` equals `C#`, we could do it like this:

```csharp
switch (programmer)
{
    case { PreferredLanguage: "C#" }:
        // do something
}
```

You can also use this on other pattern matching keywords introduced in earlier C# versions, like `is`.

# Default interface methods

So, this is exciting: you can now define an implementation when you declare a member of an interface. A common scenario, coming [straight from Microsoft](https://docs.microsoft.com/dotnet/csharp/tutorials/default-interface-methods-versions), is if you want to make enhancements to your interfaces without breaking consumers of the existing implementation. Before C# 8, you couldn't add members to an interface without breaking the classes that implement it.

Take a look at [this article](https://devblogs.microsoft.com/dotnet/default-implementations-in-interfaces/) for a brief example to get you started.

# Nullable reference types and null-coalescing

Who doesn't love working with null types? (Me, for one.) To express your intent to the compiler, you can do a few new things.

## Nullable reference types

```csharp
string? firstName;
string lastName;
```

The `firstName` variable is declared as a nullable reference type. We are used to doing this to nullable value types (which you've been doing since C# 2, by the way, when generics were the cat's meow). Because the `?` is not appended in the `lastName` variable, the compiler sees it as a non-nullable reference type.

According to [the Microsoft documentation](https://docs.microsoft.com/dotnet/csharp/nullable-references), the compiler uses static analysis to determine if a nullable reference is non-null. To provide some, well, flexibility, you can use a null-forgiving operator (`!`) following a variable name, like `lastName!.Length`.

The next time I introduce an unintended null reference to the codebase, I'll just tell my colleagues my app wasn't very null-forgiving. Life is all about marketing, after all.

## Null-coalescing

This is quick, but also a quick win. If you wanted to conditionally assign a value to a variable whether it is null, you would conditionally check for null or maybe use a ternary operator. Now, it's far easier. Just do this with the `??=` operator:

```csharp
List<string> favoriteSongs = null;
favoriteSongs ??= new List<string>();
```

The operator has two question marks for the double-take you will do when you first see the operator, but you'll learn to love it.

# Async streams

Async improvements are not new to the language, as the `async/await` pattern was introduced way back in C# 5. With C# 8, though, we now have async streams, which allow you to write an async method to return multiple values.

With async streams, they:

- Are declared with an `async` keyword modifier
- Return an `IAsyncEnumerable`
- Contain `yield` statements to return the stream elements

You can see the benefits in cycling through a traditional `for` loop. Check out the [Microsoft Docs article](https://docs.microsoft.com/dotnet/csharp/tutorials/generate-consume-asynchronous-stream) for a full demonstration.

# Indices and ranges

As a part of [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar)—a $500 word that only means expressing syntax more clearly—accessing indices and ranges is a lot easier and should require less brain cells to determine which index you are at, for example.

C# 8 introduces:

- Two new types: `System.Index` and `System.Range`, which signify an index and range of a sequence, respectively
- An index from end (`^`) operator, and a range operator (`...`)

As an example, let's hypothetically say the world is forced to endure a pandemic and a programmer decides to catch up on all James Bond movies. Again, this is hypothetical, but you get the point.

Here's my array of movies:

```csharp
var movies = new string[]
{
    "Dr. No",
    "From Russia with Love", // best one, obviously
    "Goldfinger",
    "Thunderball",
    "You Only Live Twice",
    //...
    "Die Another Day",
    "Casino Royale",
    "Quantum of Solace",
    "Skyfall",
    "Spectre" // eek
};
```

## Indexes

You should know how indexes work, hopefully—as arrays are zero-based, *Dr. No* would be 0, *From Russia with Love* would be 1, and all the way up to *Spectre* having an index of 9. What C# introduces now, though, is the `^` index from end operator, which works as the complete opposite.

For example, the first element would have an index from end value as 10, the next element 9, and so on.

```csharp
var movies = new string[]
{
    "Dr. No", // ^10
    "From Russia with Love", // ^9
    "Goldfinger", // ^8
    "Thunderball", // ^7
    "You Only Live Twice", // ^6
    //...
    "Die Another Day", // ^5
    "Casino Royale", // ^4
    "Quantum of Solace", // ^3
    "Skyfall", // ^2
    "Spectre" // ^1
    // words.length value is ^0
};
```

Now, you don't have to do counting from an index and makes things a lot easier. For example, to get *Skyfall* (please use string interpolation to prove you aren't a monster):

```csharp
Console.WriteLine($"The second to last Bond movie is {movies[^2]}");
```

See? You've already forgotten about *Spectre*.

## Ranges

I don't know about you, but 90% of my time getting a substring involves me saying to myself (or anyone who will listen): **"Is the end inclusive? Is it exclusive? Why do I always have to Google this?"**

With ranges in C# 8, remember this: the start of the range is included, and the end of the range isn't. With that said, let's grab Connery's first three Bond films.

```csharp
// Returns first three movies (index 0, 1, 2 => Dr. No, From Russia, Goldfinger)
var conneryMovies = movies[0..3];
Console.WriteLine($"The first three Connery movies are: {string.Join(", ", conneryMovies)}");
```

You can use the `^` index from end operator, as discussed previously, to grab my favorite Daniel Craig films (*Skyfall*, *Quantum of Solace* (work with me here), and *Casino Royale*).

```csharp
var craigMovies = movies[^4..^1];
Console.WriteLine($"My favorite Craig movies are: {string.Join(", ", craigMovies)}");
```

You can also use the `Range` struct, which can then be used inside `[` and `]` to give you that array-like kind of feeling.

Indices and ranges are not just for strings—according [to the documentation](https://docs.microsoft.com/dotnet/csharp/tutorials/ranges-indexes#type-support-for-indices-and-ranges), you can also use them with `Span<T>` and `ReadOnlySpan<T>`.

# Staying current with C# developments

Did you know C# is open source and the design of the language has its own [GitHub repo](https://github.com/dotnet/csharplang)? There, you can find active feature proposals, notes from design meetings, and the language version history. Community involvement is definitely encouraged.

The .NET team recently had an [**All Things C#** stream](https://www.youtube.com/embed/fIXujTOA6BE), where some key designers of the language discussed what's coming—I'll be writing about some of these things shortly, but until then the video is definitely worth your time.

<iframe width="560" height="315" src="https://www.youtube.com/embed/fIXujTOA6BE?start=550" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
