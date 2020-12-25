---
date: "2020-06-24"
title: "On simplifying null validation with C# 9"
subtitle: An update on simplified null checking in C# 9.
tags: [csharp]
share-img: /assets/img/simplify-null-card.png
---

{: .box-note}
**UPDATE:** *Since the initial publishing of the post, the approach has changed. This post has been updated to reflect the latest news*.

In my last post, I took a [test drive through some C# 9 features](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp) that might make your developer life easier. In it, I mentioned using logical patterns, such as the `not` keyword to throw an `ArgumentException`, if wanted:

```csharp
not null => throw new ArgumentNullException($"Not sure what this is: {yourArgument}", nameof(yourArgument))
```

# Championed proposal: simplified null-parameter checking

As it turns out, there is a new championed proposal which appears to gain some traction called [simplified null-parameter checking](https://github.com/dotnet/csharplang/issues/2145). Some folks have written about this, making it seem like it's a sure thing in C# 9, so I'd like to clarify some things I learned after doing some research (and, of course, hit up the comments if I'm incorrect as things are changing frequently).

As for the proposal itself: let's say you do what you've done quite a few times in regular C# code:

```csharp
public void DoSomethingCool(string coolString)
{
    if (coolString is null)
    {
        throw new ArgumentNullException($"Ooh, can't do anything with {coolString}", nameof(coolString));
    }

    // proceed to do some cool things
}
```

## Initial approach: add `!` to your parameter name

In this C# 9 proposal, you can add `!` to your parameter name to simplify things. Try this one instead:

```csharp
public void DoSomethingCool(string coolString!)
{
    // proceed to do some cool things
}
```

I have mixed feelings about this proposal.

Not only are you *super excited* about your parameter, you're also asking the C# parameter to trigger standard null checks for it. It is important to mention this is for runtime checks only and does not impact the type system. Therefore, the check is on the value and not the type. I love the clarity.

But, there's a lot here:

- Adding `!` in a C-based language might confuse developers who have a mental model of the negation operator
- It's very single-use and not very extensible
- There seems to be other approaches that make more sense like a `[NullCheck]` attribute, as was suggested, or using asserts, or even a project file directive

## New approach: use `!!` instead

Late last week, around June 25-*ish* of 2020, the C# team will be changing to use `!!` instead ([meeting notes here](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-06-24.md#parameter-null-checking)).

The general rules are:

- If this is used on a parameter, throw `ArgumentNullException(nameof(paramName))`
- If used elsewhere, throw `InvalidOperationException`. A `NullOperationException` might be clearer, and others have noticed this as well

This behavior could also be used as an operator. If you review the [GitHub issue on the topic](https://github.com/dotnet/csharplang/issues/2145), Jared Parsons notes:
>The decision was limited to using !! as parameter null checking. While LDM recognizes that !! could be useful as an operator, and in previous meetings we had sketched out how that would work, in this meeting we only decided on the parameter form.

I do think this is better, but not *that much better*. The confusion surrounding how `!` is used has been eliminated, but it still isn't an extensible solution. Adding a keyword might help here, but I personally would like to see more flexibility with more than just parameters, such as a `{ get; set!; }` construct.

Anyway, if you look at the [Language Feature Status](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md) and even the [spirited issue itself](https://github.com/dotnet/csharplang/issues/2145), it is very much in progress. If you're looking to simplify null checking in C# 9, for now, I would depend on using logical patterns until this is ironed out some more.
