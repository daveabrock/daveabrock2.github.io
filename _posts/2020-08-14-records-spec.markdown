---
date: "2020-08-14"
title: "C# 9: Records Revisited"
tags: [csharp, csharp-9]
share-img: /assets/img/records-revisited.png
subtitle: We learn a few more things about records using the released C# 9 spec proposal.
---

We had a lot of fun [playing with](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits) the [new C# 9 features](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) over the last few months. As with learning anything brand new, we had to decipher the info the best we could: we went off the [announcement in May during Build](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/), meeting minutes and GitHub issues from the team, and good old trial and error.

With .NET 5 [fast approaching](https://devblogs.microsoft.com/dotnet/announcing-net-5-0-preview-7/) this fall, things have taken shape and Microsoft has released the set of [C# 9 specification proposals](https://docs.microsoft.com/dotnet/csharp/language-reference/proposals/csharp-9.0/records). These proposals offer clarity on the new features and help to reinforce things I've heard or saw, and also allowed me to learn a thing or two.

I wanted to quickly share with you some new things I came across while scanning the records proposal.

# What are records?

What are records?

Short-ish answer: a record is a construct that allows you to encapsulate property state by providing immutable behavior without the need for extensive boilerplate (like for readonly structs). Record types are reference types, like class declarations, but allow you to implement value-like behaviors.

Long answer: [check out last month's post for a deep dive on the topic](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records).

# Random facts that I assumed but am glad are in writing

* Record parameters can't use `ref`, `out`, or `this`, but can use `in` and `params`
* Records can inherit from other records, but *cannot* inherit from classes (unless it's `object`) and classes can't inherit from records

# Print a record's members

It looks like, as long as the record derives from `object`, records do include a synthesized method that allows you to inspect all the members for that record:

```csharp
bool PrintMembers(System.StringBuilder builder);
```

From the spec, they note that it'll iterate through a records `public` field and property members and take the format of `this.firstName = "Dave", this.lastName = "Brock"` and so on. You can declare this method explicitly. 

I think using `PrintMembers` will definitely come in handy when you are working with complex records/inheritance and don't want to override `ToString`. Quick and easy—I like it.

(It looks like the synthesized `ToString` method that ships with records invokes the `PrintMembers` method.)

# Wrap up

This was a quick post to show a few things I came across while reading the specification. Take [a look yourself](https://docs.microsoft.com/dotnet/csharp/language-reference/proposals/csharp-9.0/records) to check out details on how it works.
