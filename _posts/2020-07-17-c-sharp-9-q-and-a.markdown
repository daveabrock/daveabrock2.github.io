---
date: "2020-07-17"
title: "C# 9: Answering your questions"
subtitle: In this post, I discuss the questions you had after I wrote my posts.
tags: [csharp, csharp-9]
share-img: /assets/img/q-and-a-card.png
---

**Note**: Originally published five months before the official release of C# 9, I've updated this post after the release to capture the latest updates.
{: .box-note}

In the last month or so, I've written almost 8,000 words about C# 9. That seems like a lot (and it is!) but there is so much to cover! I've talked about how it [reduces mental energy](https://daveabrock.com/2020/06/18/reduce-mental-energy-with-c-sharp), [simplifies null validation](https://daveabrock.com/2020/06/24/simplified-null-validation), and took on a deep dive series featuring [init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), [records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), [pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching), [top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs), and [target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants).

After publishing all these posts, I received a lot of great questions in my Disqus comment section. Instead of burying the conversations there, I'd like to discuss these questions in case you missed them. I learned a lot from the questions, so thank you all!

# Init-only features

From the post on [init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits), we had two questions:

**Fernando Margueirat asks: What's the difference between `init` and `readonly`?**

The big difference is that with C# 9 init-only properties you are allowed to use object initialization. With `readonly` properties, you cannot.

The Microsoft announcement says: *"The one big limitation today is that the properties have to be mutable for object initializers to work ... first call the object’s constructor and then assigning to the property setters."* Because `readonly` value types are immutable, you can't use them with object initializers.

**saint4eva asks: Can a get-only property provide the same level of immutability as an init-only property?**

Similar to the last question, using init-only properties allow for initialization, while get-only properties are read-only and do not.

# Records

From the post on [record types](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records), we also had one and a half questions:

**WOBFIE says: So we should use so called "records" just because... some monkey encoded "struct" as "class"?!**

OK, this is less of a question and more of something that made me laugh (and why I say this is half of a question, as much as I love all my readers). But: let's read between the lines of what WOBFIE might be thinking—something along the lines of this being a hacked together `struct`?

In the post itself, I explained the rationale for adding a new construct over building on top of `struct`.

- An easy, simplified construct whose intent is to use as an immutable data structure with easy syntax, like `with` expressions to copy objects
- Robust equality support with `Equals(object)`, `IEquatable<T>`, and `GetHashCode()`
- Constructor/deconstructor support with simplified positional records

The endgame is not to complicate workarounds—it is to devote a construct for immutability that doesn't require a lot of wiring up from your end.

**Daniel DF says: I would imagine that Equals performance decreases with the size of the record particularly when comparing two objects that are actually equal. Is that true?**

That is a wonderful question. Since I was unsure, I reached out to the language team [on their Gitter channel](https://gitter.im/dotnet/csharplang). I got an answer within minutes, so thanks to them!

Here is what Cyrus Najmabadi says:

> Equals is spec'ed at the language to do pairwise equality of the record members.

> they have value-semantics

> in general, the basic approach of implementing this would likely mean you pay more CPU for equality checks. Though the language doesn't concern itself with that. It would be an implementation detail of the compiler.

# Target typing and covariant returns

From my post on [target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants), we had one question from two different readers.

**Pavel Voronin and saint4eva ask: Are covariant return types a runtime feature or is it just a language sugar?**

This was another question I sent to the team. Short answer: covariant return types are a runtime feature.

Long answer: it could have been implemented as syntactic sugar only using stubs—but the team was concerned about it leading to worse performance and increased code bloat when working with a lot of nested hierarchies. Therefore, they went with the runtime approach.

Also, while in the Gitter channel I learned that *covariant returns are only supported for classes as of now.* The team will look to address interfaces at a later time.
