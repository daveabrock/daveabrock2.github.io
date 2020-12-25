---
date: "2020-06-18"
title: 'Reduce mental energy with C# 9'
subtitle: Learn how C# 9 makes things cleaner, more maintainable, and minimizes mental energy.
tags: [csharp, csharp-9]
share-img: /assets/img/mental-energy-card.png
---

{: .box-note}
**Note**: Originally published five months before the official release of C# 9, I've updated this post after the release to capture the latest updates.

This is a humbling yet completely accurate fact: you spend much more time reading code than writing it. Any experienced programmer will tell you the reading-to-writing ratio is easily 5-to-1 or even 10-to-1. You're understanding how things work. You're hunting for bugs. You're scrolling past code with thoughts like, "*Nope, doesn't apply ... doesn't matter, doesn't matter ...*" until you have to pause and think, and spend a silly amount of time trying to understand how something works.

It could be a developer trying to be clever, or an unfortunate function with an [arrow-shaped pattern](http://wiki.c2.com/?ArrowAntiPattern) ... you know, a variety of things. Whatever the case, it interrupts your flow. When you think how much time you spend reviewing code, it adds up and can turn into a big annoyance.

For example, let's say you're trying to figure out a bug and you come across this C# 8 code.

```csharp
if (!(dave is Developer))  
```

This is getting a little ridiculous. Nobody has time for this negation logic and double parentheses. Best case, it interrupts your flow and mental model. Worst case, you scan it and misunderstand it. I might sound crazy, I get it - this may have only taken an extra few seconds. But for a large application, hundreds of times a day? You see what I mean? Why couldn't I do something like this?

```csharp
if (dave is not Developer)
```

See? I completely understand this: I can keep scrolling or stop and know I've found my bug. *If only I could do this*, you think.

If you aren't aware, *you can*. This syntax, and other improvements, are available in the [C# 9 release](https://devblogs.microsoft.com/dotnet/c-9-0-on-the-record/), released with .NET 5 in November 2020. C# 9 has a lot, but this post is going to focus on improvements that help restore valuable mental energy that is required in a mentally exhausting profession. And before you ask: no, C# 9 isn't full of FDA-approved health benefits, but I've found some great stuff that helps make code cleaner, more maintainable, and easier to understand, and prevents a lot of "wait, what?" moments.

Let's take a look at what's coming. This is just scratching the surface, and I'll write about more features in-depth as I come across them. I think you'll find the more you dive into C# 9, the more you appreciate its adoption of the [functional programming](https://en.wikipedia.org/wiki/Functional_programming), "no side effects" model.

This post covers the following topics.

- [Records](#records)
  - [Data member simplification](#data-member-simplification)
  - [With-expressions](#with-expressions)
- [Top-level programs](#top-level-programs)
- [Logical patterns](#logical-patterns)
- [New expressions for target types](#new-expressions-for-target-types)
- [Playing with the C# 9 preview bits](#playing-with-the-c-9-preview-bits)

# Records

One of the biggest features coming out of C# 9 is the concept of records. Records allow an entire object to be immutable, meaning you can do value-like things on them. Think data, not objects.

Let's take a `Developer` record:

```csharp
public record Developer
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string PreferredLanguage { get; init; }
}
```

Wait, what is `init` doing there? That is an init-only property, also new to C# 9. Before this, your properties needed to be mutable for them to be initialized. With `init` accessors, it's like `set` except it can only be called during object initialization.

Anyway, our record now gives us access to some other cool stuff that makes for some clean code.

## Data member simplification

If we initialize our objects using constructors like this:

```csharp
var dev = new Developer("Dave", "Brock", "C#");
```

...we can declare a record this way instead:

```csharp
public record Developer(string FirstName, string LastName, string PreferredLanguage);
```

## With-expressions

Much of your data is immutable, so if you wanted to create a new object with much, but not all, of the same data, you ~~would do something like this (your use cases would be much more complicated, hopefully)~~ are probably used to doing something like this in regular C# 8 with classes.

```csharp
using System;

var developer1 = new Developer
{
    FirstName = "David",
    LastName = "Brock",
    PreferredLanguage = "C#"
};

// ...
var developer2 = developer1;
developer2.LastName = "Pine";
Console.WriteLine(developer2.FirstName); // David
Console.WriteLine(developer2.LastName); // Pine
Console.WriteLine(developer2.PreferredLanguage); // C#

public class Developer(string FirstName, string LastName, string PreferredLanguage);
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string PreferredLanguage { get; set; }
}
```

In C# 9, try a `with` expression instead, with your records:

```csharp
using System;

var developer1 = new Developer
{
    FirstName = "David",
    LastName = "Brock",
    PreferredLanguage = "C#"
};
  
var developer2 = developer1 with { LastName = "Pine" };
Console.WriteLine(developer2.FirstName); // David
Console.WriteLine(developer2.LastName); // Pine
Console.WriteLine(developer2.PreferredLanguage); // C#

public record Developer
{
    public string FirstName;
    public string LastName;
    public string PreferredLanguage;
}
```

You can even specify multiple properties to just include what you need changed.

This C# 9 example above is actually an example of a top-level program! Speaking of which...

# Top-level programs

This is my favorite, even if I don't write a lot of console applications. Inside your `Main` method you would typically see:

```csharp
using System;

public class MyProgram
{
    public static void Main()
    {
        Console.WriteLine("Hello, Wisconsin!");
    }
}
```

No more of this silly boilerplate code! After your `using` statements, do this:

```csharp
using System;

Console.WriteLine("Hello, Wisconsin!");
```

This will need to follow the [Highlander rule](https://highlander.fandom.com/wiki/There_can_be_only_one#:~:text=There%20can%20be%20only%20one,one%22%20shall%20receive%20The%20Prize.) - there can only be one - but the same argument applies to the `Main()` entry method in your console applications today.

# Logical patterns

OK, moving on from records (for now). With the `is not` pattern we used to kick off this post, we showcased some logical pattern improvements. You can officially combine any operators with `and`, `or`, and `not`.

A great use case would be for every developer's battle: null checking. For example, you can easier code against `null`, or in this case, `not null`:

```csharp
not null => throw new ArgumentException($"Not sure what this is: {yourArgument}", nameof(yourArgument))
```

# New expressions for target types

Let's say I had a `Developer` type that takes in a first and last name from a constructor. To create the object, I'd do something like this:

```csharp
Developer dave = new Developer("Dave", "Brock", "C#");
var dave = new Developer("Dave", "Brock", "C#");
```

With C# 9, you can leave out the type.

```csharp
Developer dave = new ("Dave", "Brock", "C#");
```

# Playing with the C# 9 preview bits

Are you reading this before the C# 9 release in November 2020? If you want to play with the C# 9 bits, some good news: you can use [the LinqPad tool](https://www.linqpad.net/) to do so with a click of a checkbox - no install required!

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Play with the latest experimental C# 9 features in LINQPad, including records and init-only properties. Just click a checkbox; no install required! <a href="https://t.co/VbXTMdhfgl">https://t.co/VbXTMdhfgl</a> <a href="https://t.co/Jw3UEgwXyB">pic.twitter.com/Jw3UEgwXyB</a></p>&mdash; LINQPad·Joe Albahari (@linqpad) <a href="https://twitter.com/linqpad/status/1273191238087225345?ref_src=twsrc%5Etfw">June 17, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
