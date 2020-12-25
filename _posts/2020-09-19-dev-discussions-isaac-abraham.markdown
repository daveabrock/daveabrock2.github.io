---
date: "2020-09-19"
title: "Dev Discussions - Isaac Abraham"
subtitle: In the latest interview, I talk with Isaac Abraham about his journey to F#.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/abraham-card.png
---

This is the full interview from my discussion with Isaac Abraham in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com)!
{: .box-note}

As much as we all love C#, there's something that needs reminding from time to time: C# is *not* .NET. It is a large and important part of .NET, for sure, but .NET also supports two other languages: Visual Basic and F#. As for F#, it's been gaining quite a bit of popularity over the last several years, and for good reason: it's approachable, concise, and allows you to embrace a functional-first language while leveraging the power of the .NET ecosystem.

I caught up with Isaac Abraham to learn more about F#. After spending a decade as a C# developer, Isaac embraced the power of F# and founded [Compositional IT](https://www.compositional-it.com/), a functional-first consultancy. He's also the author of [*Get Programming with F#: A Guide for .NET Developers*](https://www.manning.com/books/get-programming-with-f-sharp).

![Isaac Abraham]({{ site.url }}{{ site.baseurl }}/assets/img/isaac-abraham.png)

## Can you walk us through your career, what you're doing now, and how you landed on F#?

I scraped into university having nearly flunked my senior school exams and took a degree in Computer Systems—it was a very practical software development degree, but there was basically minimal computer science. To this day, I don’t know how things like Big O notation works or how to implement a red-black tree—and I’m pretty woeful at maths.

I spent over 10 years as a C#/SQL enterprise-y developer after graduating, which was just when .NET 1.0 was launching. I went through the whole C#/OO developer journey—reading up on things like SOLID, doing the whole TDD thing religiously, and using IoC containers. It’s funny: until I learned about design patterns I had no real idea of how to “apply” OO in a coherent sense, which looking back is remarkable when I think about it now. I worked easily for over five years writing what I now know to be more or less procedural code in OO languages, but no one ever flagged it.

I actually think this is quite common in the industry. After working at some enterprise organisations and .NET consultancies, I worked as contractor in finance and ended up as a partner at a small Azure big data consultancy (both Azure and big data were in their infancy back then) before founding [Compositional IT](https://www.compositional-it.com/) (CIT).

I started getting into F# when I started building a rules engine for an investment bank. We wrote it in C# but I realised afterwards that we had unknowingly been writing a composable functional pipeline in an OO language, creating one-method interfaces and chaining calls together using decorators and stuff. This was around 2010, so F# had just been included “in the box” in Visual Studio 2010—so I started playing around with it as a hobby, partly because it looked interesting but also because I had grown frustrated with the amount of rigour and code needed in C# in order to ensure the quality of our software. I also started going to the London F# meetup which was a real eye opener: a different community with new ways of doing things in .NET. I also learned about open source—until then, I had no clue what GitHub was.

After I moved to the Azure consultancy, I started using F# more. Primarily, using it as a “glue” language to do things like scripts and data analysis, but I realised after a while that it was just a really flexible general purpose language that let me get stuff done more quickly—so I started writing all sorts in it. 

>I was surprised. I had thought it was simply for maths or data scraping but found it equally adept at web applications or cloud services in Azure. Once I’d gotten over the initial hurdle of resisting the OO muscle memory and learned techniques to achieve the same goals that I knew in C#, it was completely fun and empowering—as if I had started programming again and learning something new and exciting.

Just before I moved to Germany, I founded CIT—the goal was simple: functional programming for everyday software. Not maths and science or big data, but line of business apps, database systems, ETL processes, and so on. We’ve been going for over 4 years now. We provide training and coaching for teams looking to use F#, for consultancy and development services. We do all of our software development on .NET in F#, from full-stack web apps using [the SAFE Stack](https://safe-stack.github.io/) to data transformation engines to cloud services.

## I know it's more nuanced than this: but if you could sell F# to C# developers in a sentence or two, how would you do it?

F# really does bring the fun back into software development. You’ll feel more productive, more confident and more empowered to deliver high-quality software for your customers.

## Functional programming is getting a lot of attention in the C# world, as the language is adopting much of its concepts (especially with C# 9). It's a weird balance: trying to have functional concepts in an OO language. How do you feel the balance is going?

I have mixed opinions on this. On the one hand, for the C# dev it’s great—they have a more powerful toolkit at their disposal. But I would hate to be a new developer starting in C# for the first time. There are so many ways to do things now, and the feature (and custom operator!) count is going through the roof. More than that, I worry that we’ll end up with a kind of bifurcated C# ecosystem—those that adopt the new features and those that won’t, and worse still: the risk of losing the identity of what C# really is.

I’m interested to see how it works out. Introducing things [like records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records) into C# is going to lead to some new and different design patterns being used that will have to naturally evolve over time.

## I won't ask if C# will replace F#—you've eloquently written about why the answer is no. I will ask you this, though: is there a dividing line of when you should use C# (OO with functional concepts) or straight to F#?

I’m not really sure the idea of “OO with functional concepts” really gels, to be honest. Some of the core ideas of FP—immutability and expressions—are kind of the opposite of OO, which is all centered around mutable data structures, statements and side effects. By all means: use the features C# provides that come from the FP world and use them where it helps—LINQ, higher order functions, pattern matching, immutable data structures—but the more you try out those features to try what they can do without using OO constructs, the more you’ll find C# pulls you “back." It’s a little like driving an Audi on the German motorway but never getting out of third gear.

>My view is that 80% of the C# population today—maybe more—would be more productive and happier in F#. If you’re using LINQ, you favour composition over inheritance, and you’re excited by some of the new features in C# like records, switch expressions, tuples, and so on, F# will probably be a natural fit for you. All of those features are optimised as first-class citizens of the language, whilst things like mutability and classes are possible, but are somewhat atypical.

This also feeds back to your other question—I do fear that people will try these features out within the context of OO patterns, find them somehow limited, and leave thinking that FP isn’t worthwhile.

## Let's say I'm a C# programmer and want to get into F#. Is there any C# knowledge that will help me understand the concepts, or is it best to clear my mind of any preconceived notions before learning?

Probably the closest concept would be to imagine your whole program was a single LINQ query. Or, from a web app—imagine every controller method was a LINQ query. In reality it’s not like that, but that’s the closest I can think of. The fact that you’ll know .NET inside and out is also a massive help. The things to forget are basically the OO and imperative parts of the language: classes, inheritance, mutable variables, while loops, and statements. You don’t really use any of those in everyday F# (and believe me, you don’t need any of them to write standard line of business apps).

## With your experience bringing people and organizations on to F#, what have teams done to make it successful?

I’m [currently writing a series](https://www.compositional-it.com/news-blog/tag/adopting-f-series) right now on this very subject. In some ways, it’s no different to adopting any technology or tool. For example, “How can I introduce unit testing in my team?” The blockers are very rarely technical—it’s nearly always organisational or process-related. The main tips I can give is to plan your adoption thoroughly and thoughtfully, and to show that you believe in F#. Don’t do it as a one person endeavour—have at least two or three developers work on it. And ideally get a coach to help mentor your team as they start using F# to avoid common pitfalls.

>Most people at the management level will support a team adopting any new technology as long as you can show the benefits of it and (importantly) address any concerns that individuals may have, and F# is no different in that regard. Ironically, I often see F# suggested to teams from the management level and not the other way around!

Lastly, there are so many great learning resources available nowadays that simply weren’t available even 5 years ago, whether that’s paper, online courses, YouTube, Slack, and so on—whilst .NET Core has F# built-in, so you don’t even need to install anything to start trying it out.

## As an OO programmer, it's so painful always having to worry about "the billion dollar mistake": nulls. We can't assume anything since we're mutating objects all over the place and often throw up our hands and do null checks everywhere (although the language has improved in the last few versions). How does F# handle nulls? Is it less painful?

For F# types that you create, the language simply says: null isn’t allowed, and there’s no such thing as null. So in a sense, the problem goes away by simply removing it from the type system. Of course, you still have to handle business cases of “absence of a value,” so you create optional values—basically a value that can either have something or nothing. The compiler won’t let you access the “something” unless you first “check” that the value isn’t nothing. 

So, you spend more time upfront thinking about how you model your domain rather than simply saying that everything and anything is nullable. The good thing is, you totally lose that fear of “can this value be null when I dot into it” because it’s simply not part of the type system. It’s kind of like the flow analysis that C# 8 introduced for nullability checks—but instead of flow analysis, it’s much simpler. It’s just a built-in type in the language. There’s nothing magical about it.

However, when it comes to interoperating with C# (and therefore the whole BCL), F# doesn’t have any special compiler support for null checks, so developers will often create a kind of “anti-corruption” layer between the “unsafe outside world” and the safe F# layer, which simply doesn’t have nulls. There’s also work going on to bring in support for the nullability surface in the BCL but I suspect that this will be in F# 6.

## F#, and functional programming in general, emphasizes purity: no side effects. Does F# enforce this, or is it just designed with it in mind?

No, it doesn’t enforce it. There’s some parts of the language which make it obvious when you’re doing a side effect, but it’s nothing like what Haskell does. For starters, the CLR and BCL don’t have any notion of a side effect, so I think that this would difficult to introduce. It’s a good example of some of the design decisions that F# took when running on .NET—you get all the goodness of .NET and the ecosystem, but some things like this would be challenging to do. In fact, F# has a lot of escape hatches like this. It strongly guides you down a certain path, but it usually has ways that you can do your own thing if you really need to.

You still can (and people do) write entire systems that are functionally pure, and the benefits of pure functions are certainly something that most F# folks are aware of (it's much easier to reason about and test, for example). It just means that the language won’t force you to do it.

## Do you feel in the overall .NET community, that F# gets enough attention from Microsoft?

I’d always love for there to be more F# coverage! If I think back to when I first started looking into F# though, it’s come a long way—there’s more awareness of this language out there, especially from some of the bigger names in the .NET team. People are starting to realise that F# isn’t some crazy maths-and-science language, but it’s a general purpose language—it’s just that it takes a different approach towards how you organise your code. It still frustrates me to occasionally see people refer to use C# and .NET interchangeably, of course, but it’s getting better.

## What is your one piece of programming advice?

Great question. I think one thing I try to keep in mind is to avoid premature optimisation and design. Design systems for what you know is going to be needed, with extension points for what will most likely be required. You can never design for every eventuality, and you’ll sometimes get it wrong, that’s life—optimise for what is the most likely outcome.