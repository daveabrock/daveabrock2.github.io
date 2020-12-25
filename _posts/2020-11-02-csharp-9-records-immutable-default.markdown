---
date: "2020-11-02"
title: "Are C# 9 records immutable by default?"
subtitle: "Kinda?"
tags: [csharp, csharp-9,dotnet]
share-img: /assets/img/records-immutable.png
---

I'm seeing a lot of people excited about [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records) in C# 9, myself included. I wrote about records in excruciating detail this summer: but the gist is that records are types that allow you to perform value-like behaviors on properties with the promise of *immutability*. They [support `with` expressions](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records#use-with-expressions-with-records), [inheritance](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records#use-inheritance-with-the-with-expression), and [positional records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records#implementing-positional-records), too. We can finally enforce immutability in C# without a bunch of hacky boilerplate.

This begs the question: can you just declare a `record` type and get immutability right away? I'm hearing some people saying records are immutable by default, and some are saying they aren't. Both statements are correct. Let's explain why.

# Records using positional syntax are immutable by default

If you typically create objects using the positional style—by using constructors to create an object by passing an argument list—then, yes, records are immutable by default.

For example, using the positional style you can declare a `Person` record in just one line of code.

```csharp
record Person(string FirstName, string LastName);
```

Then, in the `Main` method I can do something like this:

```csharp
static void Main(string[] args)
{
    var person = new Person("Dave", "Brock");
    Console.WriteLine($"I'm {person.FirstName} {person.LastName}");
}
```

Most importantly, if I try to mutate my `FirstName` property after initialization...

```csharp
person.FirstName = "Bob";
```

...my compiler gets upset, with the error: `Init-only property or indexer 'Person.FirstName' can only be assigned in an object initializer, or on 'this' or 'base' in an instance constructor or an 'init' accessor.`

So, yes, if you are using positional records, *they are immutable by default*. From here, you are free to argue with your team about whether you should declare a bunch of one-line types in your *.cs* files.

# Records using nominal creation aren't immutable by default

There's another way to work with records in C# 9. You can declare a record with traditional getters and setters, that you've likely been doing forever.

```csharp
public record Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

Here you're using nominal creation by using object initializer syntax. When you do this in C# 9, there's nothing stopping you from doing something like this, where you can mutate properties after instantiation:

```csharp
static void Main(string[] args)
{
    var person = new Person
    {
        FirstName = "Dave",
        LastName = "Brock"
    };
    Console.WriteLine($"I'm {person.FirstName} {person.LastName}"); // I'm Dave Brock
    person.FirstName = "Bob";
    Console.WriteLine($"I'm {person.FirstName} {person.LastName}"); // I'm Bob Brock
}
```

Using positional creation, then, *we can say that records are not immutable by default*.

Of course, making this immutable is simple work by changing `set` to `init`:

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

When I do this, the compiler will complain again about mutating a property after instantiation: `Init-only property or indexer 'Person.FirstName' can only be assigned in an object initializer, or on 'this' or 'base' in an instance constructor or an 'init' accessor.`

I can see myself using both positional and nominal methods. I'd use nominal methods when I want the entire record to be immutable, and nominal creation when I want to more particular about which properties are immutable and which are not. For example, in this silly and simple example, I could keep the `FirstName` immutable but not the `LastName` (making room for marriages and divorces):

```csharp
record Person
{
    public string FirstName { get; init; }
    public string LastName { get; set; }
}
```

There are plenty of developers who will argue that records should be completely immutable—but in a natural mutable language like C#, the option for both exists by design.

# Whatever you use, records are better than a bunch of buggy ceremony

Whatever method you choose, it's much better than trying to enforce immutability in C# 8 with a `readonly struct` or classes that implement the `IEquatable` interface.

For example, in C# 8 we'd write the following ceremony to implement `IEquatable`:

```csharp
class Person : IEquatable<Person>
{
    public Person(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }

    public string FirstName { get; set; }
    public string LastName { get; set; }

    public bool Equals(Person other)
    {
        // write some Equals override to compare 
        //   from data in the object and not reference itself
        throw new NotImplementedException();
    }
}
```

To enforce value-like equality, we'll need to write our own `Equals` override and probably will end up overriding `GetHashCode` and `ToString`, too. With records, we can use either the nominal or positional convention to remove a lot of ceremony—not to mention the inevitable bugs when we forget to update the code with any new properties we add to our object. 

# Wrap up

In this post, we talked about when C# 9 records are immutable by default, and when they aren't. We also showed how either approach beats the buggy ceremony we were forced to create in C# 8 or earlier. 












