---
date: "2020-07-06"
title: "C# 9 Deep Dive: Records"
subtitle: In a C# 9 deep dive, we go in depth on records.
tags: [csharp, csharp-9]
share-img: /assets/img/records.png
---

**Note**: Originally published five months before the official release of C# 9, I've updated this post after the release to capture the latest updates.
{: .box-note}

In the [previous post of this series](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), we discussed the init-only features of C# 9, which allowed you to make individual properties immutable. That works great on a case-by-case basis, but the real power in leveraging C# immutability is when you can do this for custom types. This is where records shine, and will be the focus of this post.

This is the second post in a six-post series on C# 9 features in-depth:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 (*this post*) - Records
- Post 3 - [Pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching)
- Post 4 - [Top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs)
- Post 5 - [Target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants)
- Post 6 - [Putting it all together with a scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt)

This post covers the following topics.

- [A quick primer on immutable types](#a-quick-primer-on-immutable-types)
- [OK, so what is a record?](#ok-so-what-is-a-record)
- [What's the difference between structs and records?](#whats-the-difference-between-structs-and-records)
- [Create your first record](#create-your-first-record)
- [Use `with` expressions with records](#use-with-expressions-with-records)
  - [Use inheritance with the `with` expression](#use-inheritance-with-the-with-expression)
- [Implementing positional records](#implementing-positional-records)
- [Evaluating record equality](#evaluating-record-equality)
- [Wrapping up](#wrapping-up)

# A quick primer on immutable types

Before we get started, let's briefly talk about the concept of immutable types. For sure, immutability is a stuffy word to some but the premise here is simple: once instantiated or initialized, immutable types never change.

What do you get with immutable types? First, you get simplicity. An immutable object only has one state, the state you specified when you created the object. You'll also see that they are secure and thread-safe with no required synchronization. Because you don't have threads fighting to change an object, they can be shared freely in your applications.

Put another way: **immutable types reduce risk, are safer, and help to prevent a lot of nasty bugs that occur when you update your objects.**

Even if you aren't familiar with this concept yet, or haven't been forced to think this way, you're already using it in the .NET world. For example, `System.DateTime` is immutable, as are strings. And now with records in C# 9, you can create your own immutable types.

# OK, so what is a record?

What is a record, exactly? A record is a construct that allows you to encapsulate property state. (I am avoiding the use of the word object, for clarity.) Or, put in less geeky terms, records allow you to perform value-like behaviors on properties. This is why I'm avoiding saying "objects" when I speak of records. We need to start thinking in terms of data, and not objects. Records aren't meant for mutable state—if you want to represent change, create a new record. That way, you define them by working with the data, and not passing around a single object that gets changed by multiple functions.

Records are incredibly flexible. Anthony Giretti [found that classes can have records as properties, and also that records can contain structs and objects](https://anthonygiretti.com/2020/06/19/introducing-c-9-questions-answers-about-records/).

# What's the difference between structs and records?

Allow me to read your mind—you might be asking: how is this different than structs? If you haven't used it before, we can define a value type, called a `struct`, in C# today. So, why don't we just build on the `struct` functionality instead of introducing a new member (ha!) to C#? After all, you can declare an immutable value type by saying `readonly struct`.

Records are offering the following advantages, from what I can see:

- An easy, simplified construct whose intent is to use as an immutable data structure with easy syntax, like `with` expressions to copy objects (keep reading for details!)
- Robust equality support with `Equals(object)`, `IEquatable<T>`, and `GetHashCode()`
- Constructor/deconstructor support with simplified positional (constructor-based) records

Of course you can do this with structs, and even classes, but this requires tedious boilerplate. The idea here is to have a construct that is simple and straightforward to implement.

~~**UPDATE**: One of the biggest draws of records over structs is the reduced memory allocation that is required. Since C# records are compiled to reference types behind the scenes, they are accessed by a reference and not as a copy. As a result, no additional memory allocation is required other than the original record allocation. **Thanks to commenter Tecfield for mentioning this!**~~

Thanks to Isaac Abraham for the correction concerning memory allocation (and [confirmed by C# lead designer Mads Torgersen](https://twitter.com/MadsTorgersen/status/1327033065168748545)):

<blockquote class="twitter-tweet" data-conversation="none" data-theme="dark"><p lang="en" dir="ltr">Hmm. Not sure about the memory stuff - as with all reference vs struct, the former creates GC pressure whereas the latter is passed by value. There are good reasons for wanting both types.</p>&mdash; Isaac Abraham (@isaac_abraham) <a href="https://twitter.com/isaac_abraham/status/1327021232001323008?ref_src=twsrc%5Etfw">November 12, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


Hopefully by now, if I did my job, you know what records are and the rationale for them. Let's see some code.

# Create your first record

To declare a record, you use the new `record` keyword. Brilliant, right?

```csharp
public record Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Address { get; set; }
    public string City { get; set; }
    public string FavoriteColor { get; set; }
    // and so on...
}
```

When you mark a type as `record` like this, it won't give you immutability on its own—you'll need to use `init` properties, like in the following example: 

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string Address { get; init; }
    public string City { get; init; }
    public string FavoriteColor { get; init; }
    // and so on...
}
```

To achieve default immutability, you can create objects by using positional arguments (constructor-like syntax). When you do this, you can declare records with one line:

```csharp
public record Person(string FirstName, string LastName, string Address, string City, string FavoriteColor);
```

For more details, check out my post: *[Are C# 9 records immutable by default](https://daveabrock.com/2020/11/02/csharp-9-records-immutable-default)*?

# Use `with` expressions with records

Before C# 9, you would likely represent new state by creating new values from existing ones.

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "red"
};

var newPerson = person;
newPerson.FirstName = "Howard";
newPerson.City = "Pasadena";
```

This pattern is referred to as non-destructive mutation (now *that* will make you seem smart at your next dinner party!). C# 9 has a new type of expression, a `with` expression, to assist you.

This functionality is only available in records, and not structs or classes.
{: .notice--info}

```csharp
var person = new Person
{
    FirstName = "Tony",
    LastName = "Stark",
    Address = "10880 Malibu Point",
    City = "Malibu",
    FavoriteColor = "red"
};

var newPerson = person with { FirstName = "Howard", City = "Pasadena" };
```

You can easily use your familiar object initializer syntax to differentiate what has changed between objects, including multiple properties. Under the covers, the record has a `protected` copy constructor. If you wish, you can change the default behavior of the copy constructor but the default behavior should be suitable in most of your cases (to create a copy, change properties to what you passed in, and copy the properties that you don't).

## Use inheritance with the `with` expression

Remember when I said records support inheritance, while structs do not? I wasn't lying. We can use inheritance with our `with` expression. Let's say we have a `Superhero` class that inherits from `Person` and has a new `MaxSpeed` property.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Superhero
        {
            FirstName = "Tony",
            LastName = "Stark",
            Address = "10880 Malibu Point",
            City = "Malibu",
            FavoriteColor = "red",
            MaxSpeed = 1000
        };

        var newPerson = person with { FirstName = "Howard", City = "Pasadena" };

        Console.WriteLine(newPerson.FirstName); // Howard
        Console.WriteLine(newPerson.MaxSpeed); // 1000
    }
}

public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string Address { get; init; }
    public string City { get; init; }
    public string FavoriteColor { get; init; }
}

public record Superhero : Person
{
    public int MaxSpeed { get; init; }
}
```

Hey, this works! How? Records actually have a hidden virtual method that clones the *entire* object. An inherited record overrides this method to call the copy constructor of that type, chaining to the copy constructor of the base record.

# Implementing positional records

Sometimes, you'll need to take a positional approach—meaning data is supplied from constructor arguments. You can definitely do this with records.

Here's what you can do to enable your own constructor and deconstructor.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Person("Tony", "Stark", "10880 Malibu Point", "Malibu", "red");
        Console.WriteLine(person.FirstName); // Tony
        Console.WriteLine(person.LastName); // Stark
    }
}


public record Person
{
    public string FirstName;
    public string LastName;
    public string Address;
    public string City;
    public string FavoriteColor;

    public Person(string firstName, string lastName, string address, string city, string favoriteColor)
      => (FirstName, LastName, Address, City, FavoriteColor) = (firstName, lastName, address, city, favoriteColor);
    public void Deconstruct(out string firstName, out string lastName, out string address, 
                            out string city, out string favoriteColor) 
      => (firstName, lastName, address, city, favoriteColor) = (FirstName, LastName, Address, City, FavoriteColor);
}
```

You can clean this up with new simplified syntax, as that one-liner will give you construction and deconstruction out-of-the-box.

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Person("Tony", "Stark", "10880 Malibu Point", "Malibu", "red");
        var (first, last, address, city, color) = person;

        Console.WriteLine(person.FirstName); // Tony
        Console.WriteLine(person.LastName); // Stark
        Console.WriteLine(first); // Tony
        Console.WriteLine(last); // Stark
    }
}

public record Person(string FirstName, string LastName, string Address, string City, string FavoriteColor);

```

# Evaluating record equality

If we're being honest, *technically* records are a kind of class, which also means they are *technically* reference types. But that's OK—like structs, records override the `Equals(object)` method that every class has, to achieve the value-ness we are after. This means we can work with value-based equality as well.

For example, if we create two new objects we know that they have different references in memory, so a `ReferenceEquals` call (or even a `==` call) will return `false` even if they have the same values. This is different than structs—because structs are value types, this will not occur.

With records, we'll compare values.

Watch what happens as we:

- Create a new `Person` called `person`, Tony Stark
- Create another `Person`, called `newPerson`, Howard Stark, with two different properties (`FirstName` and `City`)
- Create a third `Person` called `anotherPerson`, and set `anotherPerson` to the same values as the original `person`

```csharp
class Program
{
    static void Main(string[] args)
    {
        var person = new Person
        {
            FirstName = "Tony",
            LastName = "Stark",
            Address = "10880 Malibu Point",
            City = "Malibu",
            FavoriteColor = "red"
        };

        var newPerson = person with { FirstName = "Howard", City = "Pasadena" };

        Console.WriteLine(Object.ReferenceEquals(person, newPerson)); // false
        Console.WriteLine(Object.Equals(person, newPerson)); // false

        var anotherPerson = newPerson with { FirstName = "Tony", City = "Malibu" };
        Console.WriteLine(Object.Equals(person, anotherPerson)); // true
    }
}

public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public string Address { get; init; }
    public string City { get; init; }
    public string FavoriteColor { get; init; }
}
```

Nice! Also, along with the `Equals` override records ship with a `GetHashCode()` override if you're so inclined.

# Wrapping up

In this post, we discussed a lot about records in C# 9. We discussed what a record is, how it compares to a struct, `with` expressions, inheritance, positional records, and how to evaluate equality.

As I discussed, this is continually changing—let me know how this works for you, and if things have changed since this was published. These are exciting new features and I hope you are able to see their benefit.
