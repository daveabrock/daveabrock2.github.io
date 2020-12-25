---
date: "2020-06-29"
title: "C# 9 Deep Dive: Init-only features"
subtitle: In a C# 9 deep dive, we will first walk through init-only features.
tags: [csharp, csharp-9]
share-img: /assets/img/init-only.png
---

**Note**: Originally published five months before the official release of C# 9, I've updated this post after the release to capture the latest updates.
{: .box-note}

A few weeks ago, we took a quick tour of some upcoming C# 9 features that will [make your development life easier](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp). We dipped our toes in the water. But now it's time to dig a little deeper.

I'm starting a new series over the next several weeks, that will showcase [all of the announced features](https://devblogs.microsoft.com/dotnet/c-9-0-on-the-record/) incrementally. Then, we will tie it all together with an all-in-one app. As for the features we are showing off, we could always dig deeper by what we see in the [Language Feature Status in GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md), but the publicly-announced-at-Build features will most likely make it when .NET 5.0 launches in November 2020.

Here's what we'll be walking through:

- Post 1 (*this post*) - Init-only features
- Post 2 - [Records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records)
- Post 3 - [Pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching)
- Post 4 - [Top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)
- Post 5 - [Target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants)
- Post 6 - [Putting it all together with a scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt)

# A focus on immutability

A big focus on C# 9 is enabling features that empower you to make immutability easy. The rise of functional programming has showed us how error-prone mutating objects can often be. While we can have immutability in previous versions of C#, it was a little hacky.

With C# 9, the team is shipping a bunch of features that help with immutability. 

# Doing immutability before C# 9

So how would I do immutability before C# 9? For virtually all of your C# life, you've done something like the following:

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string FavoriteColor { get; set; }
    // and so on...
}
```

This gets and sets properties of my `Person` with no restrictions. To achieve immutability I'd modify my class to only include a `get` accessor:

```csharp
public class Person
{
    public string FirstName { get; }
    public string LastName { get; }
    public string Address { get; }
    public string City { get; }
    public string FavoriteColor { get; }
    // and so on...
}
```

With that in place, I would use constructors to enforce this behavior:

```csharp
public class Person
{
    public Person(string firstName, string lastName, string address, string city, string favoriteColor)
    {
        FirstName = firstName;
        LastName = lastName;
        Address = address;
        City = city;
        FavoriteColor = favoriteColor;
    }

    public string FirstName { get; }
    public string LastName { get; }
    public string Address { get; }
    public string City { get; }
    public string FavoriteColor { get; }
}
```

That's great, but I can't do this with object initializers. If I wanted to initialize an object like this...

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "Red"
};
```

...there's nothing that prevents me from mutating after the fact:

```csharp
Console.WriteLine(person.FirstName); // Tony
person.FirstName = "Howard";
Console.WriteLine(person.FirstName); // Howard
```

Previously, for object initialization to work, the properties must be mutable. To set values in a new `Person` in an immutable way, you'll need to call the object's constructor (in this case, as in most cases, a parameterless constructor) and then perform assignment from the setters.

## Introducing the `init` accessor

With C# 9, we can change this with an `init` accessor. This means you can only create and set a property when you initialize the object. If we modify our `Person` model like this, we can prevent the `FirstName` from being changed:

```csharp
public class Person
{
    public string FirstName { get; init; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string FavoriteColor { get; set; }
    // and so on...
}
```

With this, that means this code will work:

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "Red"
};
```

However, when you try to execute this code:

```csharp
person.FirstName = "Howard";
```

The compiler will not be happy.

## Warning: init-only properties aren't mandatory

The beauty of object initializers is the ability to set whatever you want, wherever you want, like so:

```csharp
var person = new Person
{
    FirstName = "Tony",
    City = "Malibu",
};
```

If you want to come in and set an `Address` that has an `init` property on it, thinking you can do so because you haven't set its value yet, you're wrong. For example, I can't come in and do this...

```csharp
person.FavoriteColor = "Red";
```

...because the object has already been initialized.

## Init accessors and read-only fields

As we just saw, `init` accessors can only be called when you initialize. If you wish to work with `readonly` fields, the mutability only applies during initialization, just as with non-read-only ones.

With this in mind, using private fields will set a value during initialization - otherwise, we will have an `ArgumentNullException` thrown:

```csharp
public class Person
{
    private readonly string _firstName;
    private readonly string _lastName;
    private readonly string _address;
    private readonly string _city;
    private readonly string _favoriteColor;

    public string FirstName
    {
        get => _firstName;
        init => _firstName = (value ?? throw new ArgumentNullException(nameof(FirstName)));
    }
    public string LastName
    {
        get => _lastName;
        init => _lastName = (value ?? throw new ArgumentNullException(nameof(LastName)));
    }
    public string Address
    {
        get => _address;
        init => _address = (value ?? throw new ArgumentNullException(nameof(Address)));
    }
    public string City
    {
        get => _city;
        init => _city = (value ?? throw new ArgumentNullException(nameof(City)));
    }
    public string FavoriteColor
    {
        get => _favoriteColor;
        init => _favoriteColor = (value ?? throw new ArgumentNullException(nameof(FavoriteColor)));
    }
}
```

# What's next

In this post, we learned how to make individual properties become immutable. If you want this behavior for your entire object, you'll want to work with records - one of the best new features in C# 9, in my opinion. Stay tuned for my next post to discuss this.
