---
date: "2020-09-26"
title: "Dev Discussions - Phillip Carter"
subtitle: In the latest interview, we talk with Phillip Carter.
tags: [dotnet-stacks, dev-discussions]
subtitle: In the latest interview, I talk with Phillip Carter to get the Microsoft perspective on F#.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/carter-card.png
---

This is the full interview from my discussion with Phillip Carter in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com)!
{: .box-note}

Last week, we [talked to Isaac Abraham about F# from a C# developer's perspective](https://daveabrock.com/2020/09/19/dev-discussions-isaac-abraham). This week, I'm excited to get more F# perspectives from Phillip Carter. Phillip is a busy guy at Microsoft but a big part of his role as a Program Manager is overseeing F# and its tooling.

In this interview, we talk to Phillip about Microsoft F# support, F# tooling (and how it might compare to C#), good advice for learning F#, and more.

![Phillip Carter]({{ site.url }}{{ site.baseurl }}/assets/img/phillip-carter.jpg)

## Can you talk about what you've done at Microsoft, and how you landed on F#?

I spent some time bouncing around on various projects related to shipping .NET Core 1.0 for my first year-ish at Microsoft. A lot of it was me doing very little in the early days, since there was little for an entry-level PM to do. But I did find that that the Docs team needed help and so I ended up writing a good portion of the .NET docs that exist on *docs.microsoft.com* today. Some of the information architecture I contributed to is still present there today.

I got the F# gig because I had an interest in F# and the current PM was leaving for a different team. Rather than let it sit without a PM for an indeterminate amount of time, everyone agreed that I should take the position. Always better to have someone who’s interested in the space assume some responsibility than have nobody do it, right? I’ve been working on F# at Microsoft ever since.

## Do you feel F# gets the recognition and attention at Microsoft it deserves?

This is always a fun question. 

Many F# programmers would emphatically proclaim, “No!” and it’s of course a meme that Microsoft Doesn’t Care About F# or whatever. But the reality is that like all other functional programming languages, F# is a niche in terms of adoption, and it is likely to stay a niche if you compare it to the likes of C#, Java, C++, Python, and JavaScript.

>As a fan of programming languages in general, I feel like tech companies (like Microsoft) emphasize platforms far more than any given language. I find it kind of funny, because I’ve already seen multiple platforms come and go in our industry—but all the languages I’ve been involved with have only become more permanent and grown during that same timespan. This is kind of sidestepping the question, but it really is how I feel about the topic.­­­­­­

Being a niche language doesn’t mean something isn’t valuable. To the contrary, there are many large, paying customers who use F# today, and that is only expected to grow as more organizations incorporate and grow software systems and hiring people who like functional programming.  For example, F# powered Azure’s first billion-dollar startup (Jet.com) back in 2016. Could they have used C#? Sure. But they didn’t. Did F# cause them to use Azure? Maybe. They evaluated Azure and Google Cloud and decided on Azure for a variety of reasons, technological compatibility perhaps being one of them. But these questions don’t really matter. 

From Microsoft’s perspective, we want customers to use the tech they prefer, not the tech we prefer, and be successful with it. If that’s F#, then we want to make sure that they can use our developer tools and platforms and have a good time doing it. If they can’t, then we want to do work to fix it. If you look at official statements on things like our languages, we’re fairly unopinionated and encourage people to try the language and environment that interests them the most and works the best for their scenario.

All this context matters when answering this question. Yes, I think Microsoft gives F# roughly the attention and love it deserves, certainly from an engineering standpoint. I don’t think any other major company would do something like pay an employee to fly out to a customer’s office, collect problems they are having with tooling for a niche programming language, and then have a team refocus their priorities to fix a good set of those issues all in time for a major release (in case it wasn’t clear, I am speaking from experience). From the customer standpoint, this is the kind of support that they would expect.

From a “marketing” standpoint, I think more attention should be paid to programming languages in general, and that F# should be emphasized more proportionally. But the reality is that there are a lot more platforms than there are languages, so I don’t see much of a change in our industry. I do think that things have improved a lot for F# on this front in the past few years, and I’ve found that for non-engineering tasks I’ve had to work less to get F# included in something over time. 

This year alone, we’ve had four blog posts about F# updates for F# 5, with a few more coming along. And of course F# also has dedicated pages on our own .NET website, dedicated tutorials for newcomers, and a vast library of documentation that’s a part of the .NET docs site. But if people are waiting for Microsoft’s CEO to get on stage, proclaim that OOP is dead and we all need to do FP with F#, they shouldn’t be holding their breath.

This also speaks to a broader point about F# and some Microsoft tech. One of the reasons why we pushed everything into the open and worked to ensure that cross-platform worked well was because we wanted to shift the perception of our languages and tech stacks. 

>People shouldn’t feel like they need Microsoft to officially tell them to use something. They should feel empowered to investigate something like F# in the context of their own work, determine its feasibility for themselves, and present it to their peers. 

I think it’s Microsoft’s responsibility to ensure that potential adopters are armed with the correct information and have a strong understanding that F# and .NET are supported products. And it’s also Microsoft’s responsibility to communicate updates and ensure that F# gets to “ride the wave” of various marketing events for .NET. But I really, truly want people to feel like they don’t *need* Microsoft for them to be successful with using and evangelizing F#. I think it’s critical that the power dynamic when concerning F# and .NET usage in any context balances out more between Microsoft and our user base. This isn’t something that can come for free, and does require active participation of people like me in communities rather than taking some lame ivory tower approach.

## From the build system to the tooling, is F# a first-class citizen in Visual Studio and other tools like C# is? If I'm a C# dev coming over, would I be surprised about things I am used to?

This is a good question, and the answer to this is: it depends on what you do in Visual Studio. All developers are different, but I have noticed a stark contrast among the C# crowd: those who use visual designer tooling and those who do not.

For those who use visual designer tooling heavily, F# may not be to your liking. C# and VB are the only two Visual Studio languages that have extensive visual designer tooling support, and if you rely on or prefer these tools, then you’ll find F# to be lacking. F# has an abundance of IDE tooling for editing code and managing your codebase, but it does not plug into things like the EF6 designer, Code Map, WinForms designer, and so on.

For anyone who uses Visual Studio to primarily edit code, then F# may take some getting used to, but most of the things you’re used to using are present. In that sense, it is first class. Project integration, semantic colors, IntelliSense, tooltips (more advanced than those in C#), some refactorings and analyzers, and so on.

C# has objectively more IDE features than F#, and the number of refactorings available in F# tooling absolutely pales in comparison to C# tooling. Some of these come down to how each language works, though, so it’s not quite so simple as “F# could just have XYZ features that C# has.” But overall, I think people tend to feel mostly satisfied by the kinds of tooling available to them.

It’s often claimed that F# needs less refactoring tools because the language tends to guide programmers into one way to do things, and the combination of the F# type system and language design lends itself towards the idiom, “if it compiles, it works right.” This is mostly true, though I do feel like there are entire classes of refactoring tools that F# developers would love to use, and they’d be unique to F# and functional programming.

## What's the biggest hurdle you see for people trying to learn F#, especially from OO languages like C# or Java?

I think that OO programming in mainstream OO languages tends to over-emphasize ceremony and lead to overcomplicated programs. A lot of people normalize this and then struggle to work with something that is significantly simpler and has less moving parts.

>When you *expect* something to be complicated and it’s simple, this simplicity almost feels like it’s mocking you, like the language and environment is somehow smarter than you. That’s certainly what I felt when I learned F# and Haskell for the first time.

Beyond this, I think that the biggest remaining hurdles simply are the lack of curly braces and immutability. It’s important to recall that for many people, programming languages are strongly associated with curly braces and they can struggle to accept that a general-purpose programming language can work well without them. 

C, C++, Java, C#, and JavaScript are the most widely used languages and they all come from the same family of programming languages. Diverging greatly from that syntax is a big deal and shouldn’t be underestimated. This syntax hurdle is less of a concern for Python programmers, and I’ve found that folks with Python experience are usually warmer to the idea of F# being whitespace sensitive. Go figure!

The immutability hurdle is a big one for everyone, though. Most people are trained to do “place-oriented programming”—put a value in a variable, put that variable in a list of variables, change the value in the variable, change the list of variables, and so on. Shifting the way you think about program flow in terms of immutability is a challenge that some people never overcome, or they prefer not to overcome because they hate the idea of it. It really does fundamentally alter how you write a program, and if you have a decade or more of training with one way to write programs, immutability can be a big challenge.

## As a C# developer, in your opinion: what's the best way to learn F#?

I think the best way is to start with a console app and work through a problem that requires the use of F# types—namely records and unions—and processing data that is modeled by them. 

A parser for a Domain-Specific Language (DSL) [is a good choice](https://www.codeproject.com/Articles/39031/Making-DSLs-in-F), but a text-based game could also work well. From there, graduating to a web API or web app is a good idea. The [SAFE stack](https://www.compositional-it.com/news-blog/introducing-the-safe-stack/) combines F# on the backend with F# on the frontend via [Fable](https://fable.io/)—a wonderful F# to JavaScript compiler—to let you build an app in F# in multiple contexts. [WebSharper](https://websharper.com/) also allows you to accomplish this in a more opinionated way (it’s a supported product, too). Finally, [Bolero](https://fsbolero.io/) is a newer tech that lets you build WebAssembly apps using some of the more infrastructural Blazor components. Some rough edges, but since WebAssembly has the hype train going for it, it’s not a bad idea to check it out.

Although this wasn’t your question, I think a wonderful way to learn F# is to do some basic data analysis work with F# in Jupyter Notebooks or just an F# script [with F# Interactive](https://docs.microsoft.com/dotnet/fsharp/tutorials/fsharp-interactive/). This is especially true for Python folks who work in more analytical spaces, but I think it can apply to any C# programmer looking to develop a better understanding of how to do data science—the caveat being that most C# programmers don’t use Jupyter, so there would really be two new things to learn.

## What are you most excited about with F# 5?

Firstly, I’m most excited about shipping F# 5. It’s got a lot of features that people have been wanting for a long time, and we’ve been chipping away at it for nearly a year now. Getting this release out the door is something I’m eagerly awaiting.

If I had to pick one feature I like the most, it’s the ability to reference packages in F# scripts. I do a lot of F# scripting, and I use the mechanism in Jupyter Notebooks all the time, too. It just kind of feels like magic that I can simply state a package I want to use, and just use it. No caveats, no strings attached. In Python, you need to acquire packages in an unintuitive way due to weirdness with how notebooks and your shell process and your machine’s python installation work. It’s complexity that simply doesn’t exist in the F# world and I absolutely love it.

## What's on the roadmap past F# 5? Any cool features in the next few releases?

Currently we don’t have much of a roadmap set up for what comes next. There are still a few in-progress features, two of them being rather large: a `task { }` computation expression in FSharp.Core that rewrites into an optimized state machine, and a revamp of the F# constraint system to allow specifying static constraints in type extensions.

The first one is a big deal for anything doing high-performance work on .NET. The second one is a big deal for lots of general F# programming scenarios, especially if you’re in the numerical space and you want to define a specialized arithmetic for types that you’re importing from somewhere else. The second one will also likely fix several annoying bugs related to the F# constraint system and generally make library authors who use that system heavily much happier.

Beyond this, we’ll have to see what comes up during planning. We’re currently also revamping the F# testing system in our repository to make it easier for contributors to add tests and generally modernize the system that tens of thousands of tests use to execute today. We’re also likely to start some investigative work to rebase Visual Studio F# tooling on LSP and working with the F# community to use a single component in both VS and VSCode. They already have a great LSP implementation, but a big merging of codebases and features needs to happen there. It’ll be really exciting, but we’re not quite sure what the work “looks like” yet.

## What's something about working on the F# team that you wish the community knew, but probably doesn't?

I think a lot of folks underestimate just how much work goes into adding a new language feature. Let’s consider something like `nameof`, which was requested a long time ago. Firstly, there needed to be a design for the obvious behaviors. But then there are non-obvious ones, like `nameof` when used as a pattern, or what the result of taking the name of an operator should be (there are two kinds of names for an operator in F#). 

>Language features need a high degree of orthogonality—they should work well with every other feature. And if they don’t, there needs to be a very good reason.

Firstly, that means a very large test matrix that takes a long time to write, but you also run into quirks that you wouldn’t initially anticipate. For example, F# has functions like `typeof` and `typedefof` that accept a type as a parameterized type argument, not an input to a function. Should nameof behave like that when taking the name of a type parameter? That means there are now two syntax forms, not just one. Is that the right call? We thought so, but it took a few months to arrive at that decision. 

Another quirk in how it differs from C# is that you can’t take a fully-qualified name to an instance member as if it were static.  Why not? Because `nameof` would be the only feature in all of F# that allows that kind of qualification. Special cases like this might seem fine in isolation, but if you have every language feature deciding to do things its own way rather than consider how similar behaviors work in the language, then you end up with a giant bag of parlor tricks with no ability to anticipate how you can or cannot use something.

Then there are tooling considerations: does it color correctly in all ways you’d use it? If I have a type and a symbol with the same name and I use nameof on it, what does the editor highlight? What name is taken? What is renamed when I invoke the rename refactoring? Does the feature correctly deactivate if I explicitly set my `LangVersion` to be lower than F# 5? And so on.

These things can be frustrating for people because they may try a preview, see that a feature works great for them now, and wonder why we haven’t just shipped it yet. Additionally, if it’s a feature that was requested a long time ago, there seems to be some assumption that because a feature was requested a long time ago, it should be “further along” in terms of design and implementation. I’m not sure where these kinds of things come from, but the reason why things take long is because they are extremely difficult and require a lot of focused time to hammer out all the edge cases and orthogonality considerations.

## Can you talk a little about the SAFE Stack and how it can be used in ASP.NET Core applications?

The SAFE stack [is a great combination](https://www.compositional-it.com/news-blog/introducing-the-safe-stack/) of F# technologies—minus the A for Azure or AWS, I guess—to do full-stack F# programming. It wasn’t the first to achieve this—I believe WebSharper was offering similar benefits many years ago—but by being a composition of various open source tools, it’s unique. 

The S and A letters mostly come into play with ASP.NET Core. The S stands for Saturn, which is an opinionated web framework that uses ASP.NET Core under the hood. It calls into a more low-level library [called Giraffe](https://github.com/giraffe-fsharp/Giraffe), and if you want to use Giraffe instead (GAFE), you can. The A is more of a stand in for any cloud that can run ASP.NET Core, or I guess it could just mean A Web Server or something. It’s where ASP.NET Core runs under the hood here. The F and E are for [Fable](https://fable.io/) and [Elmish](https://elmish.github.io/elmish/), which are technologies for building web UIs with F#.

I won’t get into the details of how to use SAFE, but I will say that what I love about it is that all the technologies involved are entirely independent of one another and square F# *technologies*. Yes, they use .NET components to varying degrees and rely on broader ecosystems to supply some core functionality. But they are technologies are made by and for F# developers first.

This is a level of independence for the language that I think is crucial for the long-term success of F#. People can feel empowered to build great tech intended mainly for F# programmers, combine that tech with other tech, and have a nice big party in the cloud somewhere. What SAFE represents to me is more important than any of the individual pieces of tech it uses.

## We're seeing a lot of F# inspiration lately in C#, especially with what's new in C# 9 (with immutability, records, for example). Where do you think the dividing line is between C# with FP and using F#? Is there guidance to help me make that decision?

I think the dividing line comes down to two things: what your defaults are and what you emphasize.

C# is getting a degree of immutability with records. But normal C# programming in any context is mutable by default. You can do immutable programming in C# today, and C# records will help with that. But it’s still a bit of a chore because the rest of the language is just begging you to mutate some variables. 

>They’re called *variables* for a reason! This isn’t a value judgement, though. It’s just what the defaults are. C# is mutable by default, with an increasing set of tools to do some immutability. F# is immutable by default, and it has some tools for doing mutable programming.

I think the second point is more nuanced, but also more important. Both C# and F# implement the .NET object system. Both can do inheritance, use accessibility modifiers on classes, and do fancy things with interfaces (including interfaces with default implementations). But how many F# programmers use this side of the language as a part of their normal programming? Not that many. OOP is possible in F#, but it’s just not emphasized. F# is squarely about functional programming first, with an object programming system at your disposal when you need it.

On the other hand, C# is evolving into a more unopinionated language that lets you do just about anything in any way you like. The reality is that some things work better than others (recall that C# is not immutable by default), but this lack of emphasis on one paradigm over the other can lead to vastly different codebases despite being written in the same language. 

Is that okay? I can’t tell. But I think it makes identifying the answer to the question, “how should I do this?” more challenging. If you wanted to do functional programming, you could use C# and be fine. But by using a language that is fairly unopinionated about how you use it, you may find that it’s harder to “get it” when thinking functionally than if you were to use F#. Some of the principles of typed functional programming may feel more difficult or awkward because C# wasn’t built with them in at first. Not necessarily a blocker, but it still matters.

What I would say is that if you want to do functional programming, you will only help yourself by learning F# when learning FP. It’s made for doing functional programming on .NET first and foremost, and as a general guideline it’s a good idea to use tools that were made for a specific purpose if you are aligned with that purpose. You may find that you don’t like it, or that you thought some things were cool but you’re ultimately happier with taking what you learned and writing C# code in a more functional style from now on. That’s great, and you shouldn’t feel any shame in deciding that F# isn’t for you. But it’ll certainly making writing functional C# easier, since you’ll already have a good idea of how to generally approach things.

## Something I ask everyone: what is your one piece of programming advice?

The biggest piece of advice I would give people is to look up the different programming paradigms, functional being one of them, and try out a language some of them. 

Most programmers are used to an ALGOL-derived language, and although they are great languages, they all tend to encourage the same kind of thought process for how you write programs. Programming can be a tool for thought, and languages from different backgrounds encourage different ways of thinking about solving problems with programming languages. I believe that this can make people better programmers even if they never use F# or other languages outside of the mainstream ones. 

Additionally, all languages do borrow from other ones to a degree, and understanding different languages can help you master new things coming into mainstream languages.

*You can reach Phillip Carter [on Twitter](https://twitter.com/_cartermp).*