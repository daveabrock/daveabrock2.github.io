---
date: "2020-07-14"
title: "C# 9 Deep Dive: Target Typing and Covariant Returns"
subtitle: In a C# 9 deep dive, we talk about target typing and covariant returns.
tags: [csharp, csharp-9]
share-img: /assets/img/target-typing-card.png
---

We've been quite busy, my friends. In this C# 9 deep dive series, we've looked at [init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), [pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching), and then [top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs). To complete this series (before showing off everything in a single app), we'll discuss the last two items featured in [the Build 2020 announcement](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/#improved-target-typing): target typing and covariant returns. These are not related, but I've decided to bundle these in a single blog post.

This is the fifth post in a six-post series on C# 9 features in-depth:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 - [Records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records)
- Post 3 - [Pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching)
- Post 4 - [Top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)
- Post 5 (*this post*) - Target typing and covariant returns
- Post 6 - [Putting it all together with a scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt)

This post covers the following topics.

- [Improved target typing](#improved-target-typing)
  - [Target-typed `new` expressions](#target-typed-new-expressions)
  - [Target typing with conditional operators](#target-typing-with-conditional-operators)
- [Covariant returns](#covariant-returns)
- [Wrapping up](#wrapping-up)

# Improved target typing

C# 9 includes improved support for target typing. What is target typing, you say? It's what C# uses, normally in expressions, for getting a type from its context. A common example would be the use of the `var` keyword. The type can be inferred from its context, without you needing to explicitly declare it.

The improved target typing in C# 9 comes in two flavors: `new` expressions and target-typing `??` and `?:`.

## Target-typed `new` expressions

With target-typed `new` expressions, you can leave out the type you instantiate. At first glance, this appears to only work with direct instantiation and not coupled with `var` or constructs like ternary statements.

Let's take a condensed `Person` class from previous posts:

```csharp
public class Person
{
    private string _firstName;
    private string _lastName;

    public Person(string firstName, string lastName)
    {
        _firstName = firstName;
        _lastName = lastName;
    }
}
```

To instantiate a new `Person`, you can omit the type on the right-hand side of the equality statement.

```csharp
class Program
{
    static void Main(string[] args)
    {
        Person person = new ("Tony", "Stark");
    }
}
```

A big advantage to target-typed `new` expressions is when you are initializing new collections. If I wanted to create a list of multiple `Person` objects, I wouldn't need to worry about including the type every time I create a new object.

With the same `Person` class in place, you can change the `Main` function to do this:

```csharp
class Program
{
    static void Main(string[] args)
    {
        var personList = new List<Person>
        {
            new ("Tony", "Stark"),
            new ("Howard", "Stark"),
            new ("Clint", "Barton"),
            new ("Captain", "America")
            // ...
        };
    }
}
```

## Target typing with conditional operators

Speaking of ternary statements, we can now infer types by using the conditional operators. This works well with `??`, the [null-coalescing operator](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/null-coalescing-operator). The `??` operator returns the value of what's on the left if it is *not null*. Otherwise, the right-hand side is evaluated and returned.

So, imagine we have some objects that shared the same base class, like this:

```csharp
public class Person
{
    private string _firstName;
    private string _lastName;

    public Person(string firstName, string lastName)
    {
        _firstName = firstName;
        _lastName = lastName;
    }
}

public class Student : Person
{
    private string _favoriteSubject;

    public Student(string firstName, string lastName, string favoriteSubject) : base(firstName, lastName)
    {
        _favoriteSubject = favoriteSubject;
    }
}

public class Superhero : Person
{
    private string _maxSpeed;

    public Superhero(string firstName, string lastName, string maxSpeed) : base(firstName, lastName)
    {
        _maxSpeed = maxSpeed;
    }
}
```

While the code below does not get past the compiler in C# 8, it will in C# 9 because there's a target (base) type that is convert-able:

```csharp
static void Main(string[] args)
{
    Student student = new Student ("Dave", "Brock", "Programming");
    Superhero hero = new Superhero ("Tony", "Stark", "10000");

    Person anotherPerson = student ?? hero;
}
```

# Covariant returns

It has been a long time, coming—almost two decades of begging and pleading, actually. With C# 9, it looks like return type covariance is finally coming to the language. You can now say bye-bye to implementing some interface workarounds. OK, so just saying *return type covariance* makes me sound super smart, but what is it?

With return type covariance, you can override a base class method (that has a less-specific type) with one that returns a more specific type.

Before C# 9, you would have to return the same type in a situation like this:

```csharp
public virtual Person GetPerson()
{
    // this is the parent (or base) class
    return new Person();
}

public override Person GetPerson()
{
    // you can return the child class, but still return a Person
    return new Student();
}
```

Now, you can return the more specific type in C# 9.

```csharp
public virtual Person GetPerson()
{
    // this is the parent (or base) class
    return new Person();
}

public override Student GetPerson()
{
    // better!
    return new Student();
}
```

# Wrapping up

In this post, we discussed how C# 9 makes improvements with target types and covariant returns. We discussed target-typing `new` expressions and their benefits (especially when initializing collections). We also discussed target typing with conditional operators. Finally, we discussed the long-awaited return type covariance feature in C# 9.
