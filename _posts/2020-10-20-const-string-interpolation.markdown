---
date: "2020-10-20"
title: "C# 10 First Look: Constant string interpolation"
subtitle: "Constant string interpolation looks to be set for C# 10—let's take a look."
tags: [csharp]
share-img: /assets/img/constant-string-interpolation.png
---

C# 9 is finally here, everybody.

This summer, we took a look at most of its core features. We looked at [init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), [pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching), [top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs), and [target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants).

Now, we can look forward to C# 10 (what is also referred to as *C# Next*). You probably think I'm crazy to be writing about C# 10 so early—and I am!—but there's a feature slated for C# 10 that looks all set to go, [constant string interpolation](https://github.com/dotnet/csharplang/issues/2951)—a request that's been [hanging out on GitHub since 2015](https://github.com/dotnet/roslyn/issues/4678). Fred Silberberg from the Roslyn team says that [it's implemented in a feature branch and will likely be part of C# 10](https://github.com/dotnet/csharplang/issues/2951#issuecomment-696784012). As a matter of fact, if you [head over to SharpLab](https://sharplab.io/), there's a **C# Next: Constant Interpolated Strings** branch you can play with that's been out there since ... October 18, 2020.

While it's madness to write about a new language feature so early, I don't think it's too crazy to talk about *this specific feature*. It's super easy to implement so I don't see that part changing too much—only when it gets shipped.


We'll discuss the following content in this post.

- [An absurdly quick example](#an-absurdly-quick-example)
- [Why can't I do that now?](#why-cant-i-do-that-now)
- [What about culture impacts?](#what-about-culture-impacts)
- [Wrap up](#wrap-up)

**Heads up!** This should go without saying, but things may change between now and the C# 10 release date. In other words: 🤷‍♂️.
{: .notice--warning}

# An absurdly quick example

These days—in C# 9 or earlier, that is—if you want to merge constant strings together, you will have to use concatenation, and not interpolation (or remove them as `const` variables altogether).

A common use is with paths. In current times, you *have* to do this if you are working with constant strings:

```csharp
const string myRootPath = "/src/to/my/root";
const string myFilePath = myRootPath + "README.md";
```

With constant string interpolation, you can do this:

```csharp
const string myRootPath = "/src/to/my/root";
const string myWholeFilePath = $"{myRootPath}/README.md";
```

Constant interpolated strings would be super convenient when working with attribute arguments. Let's say you want to flag a method with the [ObsoleteAttribute](https://docs.microsoft.com/dotnet/api/system.obsoleteattribute?view=netcore-3.1). Try this:

```csharp
[Obsolete($"Ooh, don't use me. Instead, use {nameof(MyBetterType)}.")]
```

# Why can't I do that now?

When I learned about string interpolation back in C# 6, I was excited to think I'd never have to do concatenation again—so this is a bummer. Why can't I do it, anyway?

If you try to use constant string interpolation in C# 9, you'll get this error back:

```bash
error CS0133: The expression being assigned to 'myFilePath' must be constant
```

When you use string interpolation, the interpolated strings end up getting converted to `string.Format` calls. Once you understand that point, you'll see it as:

```csharp
const string myFilePath = string.Format("{0}/README.md", myRootPath);
```

While it may look like a constant at compile time, it isn't because of the `string.Format` invocation. Of course, concatenation works fine since as there's hardly any magic gluing two string literals together.

# What about culture impacts?

While you may think that your string in question is just constants, you might be wrong and it might not be completely obvious. Even something as simple as outputting a float has impacts across cultures.

For example, here in the US I can do this:

```csharp
Console.WriteLine($"{1200.12}") // output: 1200.12;
```

Try a different culture, and the results are different (and, to our point, not constant):

```csharp
CultureInfo.CurrentCulture = new CultureInfo("es-ES", false);
Console.WriteLine($"{1212.12}"); // output: 1200,12
```

When it comes to constant string interpolation, will there be unintentional side effects when working with culture dependencies? The team insists [that won't be an issue](https://github.com/dotnet/csharplang/issues/2951#issuecomment-559196393):

>This feature won't work in this scenarios. It'll only work for interpolating other constant strings, so locale is not a concern. It's really just a slightly different syntax for string concatenation.

# Wrap up

In this post, we looked at constant string interpolation—a promising addition to C# 10. We discussed how to use it, the reasons why you can't use it now, and addressed culture impacts.

If you're interested in what is currently slated for the next release of C#, head over to the [Language Feature Status page](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md) in the Roslyn GitHub repo.
