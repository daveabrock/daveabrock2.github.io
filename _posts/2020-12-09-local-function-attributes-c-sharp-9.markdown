---
date: "2020-12-09"
title: "Use local function attributes with C# 9"
subtitle: "In this quick post, I introduce how to use attributes on local functions in C# 9."
tags: [csharp, csharp-9]
share-img: /assets/img/local-attributes.png
---

If you look at what's new in C# 9, you'll see records, init-only properties, and top-level statements [get all the glory](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-9). And that's fine, because they're great. At the end of the bulleted list, I noticed [support for local attributes on local functions](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-9).

When it comes to local functions, with C# 9 you can [apply attributes to function declarations, parameters, and type parameters](https://docs.microsoft.com/dotnet/csharp/language-reference/proposals/csharp-9.0/local-function-attributes).

The most common use case I've seen is using the `ConditionalAttribute`. This attribute is used for checking conditional compilation symbols—instead of using `#define DEBUG` regions, for example, you can do it here. You can declare multiple attributes, just like for a "normal" method. For example:

```csharp
static void Main(string[] args)
{
    [Conditional("DEBUG")]
    [Conditional("BETA")]
    static void DoSuperSecretStuff()
    {
        Console.WriteLine("This only is executed in debug or beta mode.");
    }

    DoSuperSecretStuff();
}
```

If you want to work with the `ConditionalAttribute` the methods must be both `static` and `void`.

Of course, you can work with any kind of attributes. In the GruutBot project ([which I showcased here](https://daveabrock.com/2020/07/28/azure-bot-service-cognitive-services)), there's a local function that has a switch expression to determine Gruut's emotion:

```csharp
static string GetReplyText(TextSentiment sentiment) => sentiment switch
{
    TextSentiment.Positive => "I am Gruut.",
    TextSentiment.Negative => "I AM GRUUUUUTTT!!",
    TextSentiment.Neutral => "I am Gruut?",
    _ => "I. AM. GRUUUUUT"
};
```

Here I can go wild with a few more attributes:

```csharp
[Obsolete]
[MethodImpl(MethodImplOptions.AggressiveOptimization)]
static string GetReplyText(TextSentiment sentiment) => sentiment switch
{
    TextSentiment.Positive => "I am Gruut.",
    TextSentiment.Negative => "I AM GRUUUUUTTT!!",
    TextSentiment.Neutral => "I am Gruut?",
    _ => "I. AM. GRUUUUUT"
};
```

The `ObsoleteAttribute` [signifies to callers](https://docs.microsoft.com/dotnet/api/system.obsoleteattribute?view=net-5.0) that the function is no longer use (and will trigger a warning when invoked).

As for the `MethodImplAttribute`, it allows me to specify `AggressiveOptimization`. The details are a topic for another post, but from a high level: this method is JIT'ed when first called, and the JIT'ed code is always optimized—making it ineligible for tiered compilation.

This new local function support brings attributes to this scope, which is a welcome addition. These aren't "new" attributes, but just the ability to do it in local functions now.

# Wrap up

In this post, I quickly showed you how to use local function attributes in C# 9. If you have any suggestions, please let me know!



