---
date: "2020-07-06"
title: "C# 9 Deep Dive: Pattern Matching"
subtitle: In a C# 9 deep dive, we explore improved pattern matching.
tags: [csharp, csharp-9]
share-img: /assets/img/pattern-matching.png
---

In the [previous post of this series](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), we discussed the power of records. That was a heavy topic.

For something completely different, we'll discuss improved pattern matching in C# 9. This is not a completely new feature, but something that has evolved since it was first released way back in C# 7, albeit in basic form. This [Microsoft article](https://docs.microsoft.com/dotnet/csharp/pattern-matching) runs down the basics of pattern matching, which improved greatly in C# 8 as well. The pattern matching works with the `is` operator and with `switch` expressions, much of which I showed off in my article [*C# 8, A Year Late.*](https://daveabrock.com/2020/03/29/csharp-8-year-late.html)

This is the third post in a six-post series on C# 9 features in-depth:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 - [Records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records)
- Post 3 (*this post*) - Pattern matching
- Post 4 - [Top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)
- Post 5 - [Target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants)
- Post 6 - [Putting it all together with a scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt)

This post covers the following topics.

- [First, get to know the C# 8 `switch` expression syntax](#first-get-to-know-the-c-8-switch-expression-syntax)
- [How pattern matching helps you](#how-pattern-matching-helps-you)
- [Our C# 8 baseline example](#our-c-8-baseline-example)
  - [Relational patterns](#relational-patterns)
  - [Logical patterns](#logical-patterns)
- [Wrapping up](#wrapping-up)

# First, get to know the C# 8 `switch` expression syntax

Before we get started with pattern matching enhancements in C# 9, much of it is based off the improved `switch` syntax from C# 8. (If you are already familiar, you can scroll to the next section.)

To be clear, they are now called `switch` *expressions,* and not switch *statements.* Before C# 8, you would typically have this (stolen from my C# 8 article):

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
        default:
             throw new Exception("You code in something else I don't recognize.");
    };
    return languagePhrase;
}
```

With switch expressions, we can replace `case` and `:` with `=>` and replace the default statement with `_`. That "underscore operator" is *technically* [called a discard](https://docs.microsoft.com/dotnet/csharp/discards)—a temporary, dummy variable that you want intentionally unused. This gives us a much cleaner, expression-like syntax.

Be honest: switch statements enable `goto`-like control flow (so we are clear on [how I feel about this](https://stackoverflow.com/questions/4756084/use-a-goto-in-a-switch)) and just executes code. I find the expressive style, which forces you to return a value, much better. You know that empty "well, better than a million `if`'s, I guess?" feeling you get with `switch` statements? This should make you feel better.

```csharp
public static string FindAProgrammingLanguage(string languageInput)
{
    string languagePhrase = languageInput switch
    {
        "C#" => "C# is fun!",
        "JavaScript" => "JavaScript is mostly fun!",
         _ => throw new Exception("You code in something else I don't recognize."),
    };
    return languagePhrase;
}
```

Now that we see how this improved C# 8 `switch` behavior helps you, let's move on to pattern matching.

# How pattern matching helps you

Pattern matching allows you to simplify scenarios where you need to cohesively manage data from different sources. An obvious example is when you call an external API where you don't have any control over the types of data you are getting back. Of course, typically you would create types in your application for all the different types you could get back from this API. Then, you would build an object model off those types. This is *a lot* of work. What's that old joke about object-oriented programming about [the gorilla and the banana](https://www.johndcook.com/blog/2011/07/19/you-wanted-banana/)?

Imagine if you are working with multiple APIs! What if you provide shipping services, and are working with all the necessary APIs (FedEx, USPS, and more). You think they all got together to form one shared data model?

To make our lives easier, let's sprinkle some functional, C# 9 magic on top of our OO language and make your life simpler.

(In-depth pattern matching techniques are beyond the scope of this post, but [do check out Bill Wagner's excellent work](https://docs.microsoft.com/dotnet/csharp/tutorials/pattern-matching).)

# Our C# 8 baseline example

To build off our previous posts, let's stick with the Iron Man theme. Here's some C# 8 code we use to calculate a superhero's fuel cost based on a maximum speed.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var superhero = new Superhero
        {
            FirstName = "Tony",
            LastName = "Stark",
            MaxSpeed = 10000
        };
        static decimal GetFuelCost(object hero) => 
        hero switch
        {
            Superhero s when s.MaxSpeed < 1000 => 10.00m,
            Superhero s when s.MaxSpeed <= 10000 => 7.00m,
            Superhero _ => 12.00m,
            _ => throw new ArgumentException("I do not know this one", nameof(hero))
        };
        Console.WriteLine(GetFuelCost(superhero)); // 7.00
    }
}

public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string FavoriteColor { get; set; }
}

public class Superhero : Person
{
    public int MaxSpeed { get; set; }
}
```

## Relational patterns

With C# 9, we can simplify our switch expression using relational patterns. This allows us to use the relational operators such as `<`, `<=`, `>`, and `>=`. We can simplify our program—take a look at our new `GetFuelCost` method:

```csharp
static decimal GetFuelCost(Superhero hero) => hero.MaxSpeed switch
{
    < 1000 => 10.00m,
    <= 10000 => 7.00m,
    _ => 12.00m
};
```

## Logical patterns

Similarly, you can use logical operators, like `and`, `or`, and `not`, as a complement to using relational patterns. This might be a more readable option for you if relational operators are not your jam. 

Let's try a slightly modified example with words instead of symbols:

```csharp
static decimal GetFuelCost (Superhero hero) => hero.MaxSpeed switch
{
    1 or 2 => 1.00m,
    < 1000 and < 5000 => 10.00m,
    <= 10000 => 7.00m,
    _ => 12.00m
};
```

You can also use the `not` operator, as I've highlighted in previous posts on C# 9 improvements.

As described in the [*Welcome to C# 9*](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) post by Microsoft, it's convenient if you use the `null` constant pattern:

```csharp
not null => throw new ArgumentException($"Not a known person: {hero}", nameof(hero)),
null => throw new ArgumentNullException(nameof(hero))
```

It also helps you think more clearly about negation logic. If you are used to something like this:

```csharp
if (!(hero is Person)) { ...}
```

Your co-workers will help you, if you change it to this:

```csharp
if (hero is not Person) { ... }
```

# Wrapping up

In this post, we discussed the advantages of pattern matching, especially with coupled with powerful switch expressions, which were introduced in C# 8. We then discussed how C# 9 can help clean up your syntax with its relational and logical patterns.

Stay tuned for the next post, which discusses target typing and covariant returns in C# 9.
