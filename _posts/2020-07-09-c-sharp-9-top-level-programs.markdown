---
date: "2020-07-09"
title: "C# 9 Deep Dive: Top-Level Programs"
subtitle: In a C# 9 deep dive, we talk about how top-level programs work with status codes, async, arguments, and local functions.
tags: [csharp, csharp-9]
share-img: /assets/img/top-level-programs.png
---

This is the fourth post in a six-post series on C# 9 features in-depth:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 - [Records](https://daveabrock.com/2020/07/06/c-sharp-9-deep-dive-records)
- Post 3 - [Pattern matching](https://daveabrock.com/2020/07/06/c-sharp-9-pattern-matching)
- Post 4 (*this post*) - Top-level programs
- Post 5 - [Target typing and covariant returns](https://daveabrock.com/2020/07/14/c-sharp-9-target-typing-covariants)
- Post 6 - [Putting it all together with a scavenger hunt](https://daveabrock.com/2020/07/21/c-sharp-9-scavenger-hunt)

Typically, when you learn to write a new C# console application, you are required by law to start with something like this:

```csharp
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, world!");
    }
}
```

Imagine you're trying to teach someone how a program works. Before you even execute a line of code, you need to talk about:

- What are classes?
- What is a function?
- What is this `args` string array?

Sure, to you and me it likely won't be long before that needs to come up but the barrier for entry becomes higher—especially when you look at how simple it is to get started with something like Python or JavaScript.

This post covers the following topics.

- [The simple, obligatory "Hello, world" example](#the-simple-obligatory-hello-world-example)
- [Return a status code](#return-a-status-code)
- [Await things](#await-things)
- [Access command-line arguments](#access-command-line-arguments)
- [Local functions](#local-functions)
- [Wrapping up](#wrapping-up)

# The simple, obligatory "Hello, world" example

With C# 9 top-level programs, you can take away the `Main` method and condense it to something like this:

```csharp
using System;

Console.WriteLine("Hello, world!");
```

And, don't worry: I know what you're thinking. Let's make this a one-liner.

```csharp
System.Console.WriteLine("Hello, world!");
```

If you look at what Roslyn generates, from [Sharplab](https://sharplab.io), nothing should shock you:

```csharp
[CompilerGenerated]
internal static class $Program
{
    private static void $Main(string[] args)
    {
        Console.WriteLine("Hello, world!");
    }
}
```

No surprise here. It'll generate a `Program` class and the traditional main method for you.

Can we be honest? I thought this was where it ended: a nice, clean way to simplify a console app. But! As I read the [*Welcome to C# 9*](https://devblogs.microsoft.com/dotnet/welcome-to-c-9-0/) announcement a little closer, Mads Torgersen writes:

> If you want to return a status code you can do that. If you want to await things you can do that. And if you want to access command line arguments, args is available as a “magic” parameter...Local functions are a form of statement and are also allowed in the top level program.

That is super interesting. Let's try it out and see what happens in Sharplab.

# Return a status code

We can return anything from our top-level program. If we return 0 like we did in the good old days, let's do this:

```csharp
System.Console.WriteLine("Hello, world!");
return 0;
```

Roslyn gives us this:

```csharp
[CompilerGenerated]
internal static class $Program
{
    private static int $Main(string[] args)
    {
        Console.WriteLine("Hello, world!");
        return 0;
    }
}
```

# Await things

Mads says we can `await` things. Let's await something—let's call the [`icanhazdadjoke` API](https://icanhazdadjoke.com/api), shall we?

Let's try this code:

```csharp
using System.Net.Http;
using System;
using System.Net.Http.Headers;

using (var httpClient = new HttpClient())
{
    httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("text/plain"));
    Console.WriteLine(httpClient.GetStringAsync(new Uri("https://icanhazdadjoke.com")).Result);
}
```

As you can see, nothing it can't handle:

```csharp
[CompilerGenerated]
internal static class $Program
{
    private static void $Main(string[] args)
    {
        HttpClient httpClient = new HttpClient();
        try
        {
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("text/plain"));
            Console.WriteLine(httpClient.GetStringAsync(new Uri("https://icanhazdadjoke.com")).Result);
        }
        finally
        {
            if (httpClient != null)
            {
                ((IDisposable)httpClient).Dispose();
            }
        }
    }
}
```

OK, so I called `GetStringAsync` but I kinda lied—I haven't done an await or returned a `Task`.

If we do this thing:

```csharp
using System.Threading.Tasks;

await Task.CompletedTask;
return 0;
```

Watch what happens! We've got a `TaskAwaiter` and an `AsyncStateMachine`. What, you thought async was easy? Good thing: it's relatively easy with top-level functions.

```csharp
[CompilerGenerated]
internal static class $Program
{
    [StructLayout(LayoutKind.Auto)]
    private struct <$Main>d__0 : IAsyncStateMachine
    {
        public int <>1__state;

        public AsyncTaskMethodBuilder<int> <>t__builder;

        private TaskAwaiter <>u__1;

        private void MoveNext()
        {
            int num = <>1__state;
            int result;
            try
            {
                TaskAwaiter awaiter;
                if (num != 0)
                {
                    awaiter = Task.CompletedTask.GetAwaiter();
                    if (!awaiter.IsCompleted)
                    {
                        num = (<>1__state = 0);
                        <>u__1 = awaiter;
                        <>t__builder.AwaitUnsafeOnCompleted(ref awaiter, ref this);
                        return;
                    }
                }
                else
                {
                    awaiter = <>u__1;
                    <>u__1 = default(TaskAwaiter);
                    num = (<>1__state = -1);
                }
                awaiter.GetResult();
                result = 0;
            }
            catch (Exception exception)
            {
                <>1__state = -2;
                <>t__builder.SetException(exception);
                return;
            }
            <>1__state = -2;
            <>t__builder.SetResult(result);
        }

        void IAsyncStateMachine.MoveNext()
        {
            //ILSpy generated this explicit interface implementation from .override directive in MoveNext
            this.MoveNext();
        }

        [DebuggerHidden]
        private void SetStateMachine(IAsyncStateMachine stateMachine)
        {
            <>t__builder.SetStateMachine(stateMachine);
        }

        void IAsyncStateMachine.SetStateMachine(IAsyncStateMachine stateMachine)
        {
            //ILSpy generated this explicit interface implementation from .override directive in SetStateMachine
            this.SetStateMachine(stateMachine);
        }
    }

    [AsyncStateMachine(typeof(<$Main>d__0))]
    private static Task<int> $Main(string[] args)
    {
        <$Main>d__0 stateMachine = default(<$Main>d__0);
        stateMachine.<>t__builder = AsyncTaskMethodBuilder<int>.Create();
        stateMachine.<>1__state = -1;
        stateMachine.<>t__builder.Start(ref stateMachine);
        return stateMachine.<>t__builder.Task;
    }

    private static int <Main>(string[] args)
    {
        return $Main(args).GetAwaiter().GetResult();
    }
}
```

# Access command-line arguments

A nice benefit here is that, like with a command line program, you can specify command line arguments. This is typically done by parsing the `args[]` that you pass into your `Main` method, but how is this possible with no `Main` method to speak of?

The `args` are available as a "magic" parameter, meaning you should be able to access them without passing them in. MAGIC.

Let's say I wanted something like this:

```csharp
using System;

var param1 = args[0];
var param2 = args[1];

Console.WriteLine($"Your params are {param1} and {param2}.");
```

Here's what Roslyn does:

```csharp
[CompilerGenerated]
internal static class $Program
{
    private static void $Main(string[] args)
    {
        string text = args[0];
        string text2 = args[1];
        string[] array = new string[5];
        array[0] = "Your params are ";
        array[1] = text;
        array[2] = " and ";
        array[3] = text2;
        array[4] = ".";
        Console.WriteLine(string.Concat(array));
    }
}
```

# Local functions

Now, for my last trick, local functions.

Let's whip us this code to test out our top-level program.

```csharp
using System;

DaveIsTesting();

void DaveIsTesting()
{
    void DaveIsTestingAgain()
    {
        Console.WriteLine("Dave is testing again.");
    }
    Console.WriteLine("Dave is testing.");
    DaveIsTestingAgain();
}
```

I have to admit, I'm pretty excited to see what Roslyn decides to do on this one:

```csharp
[CompilerGenerated]
internal static class $Program
{
    private static void $Main(string[] args)
    {
        <$Main>g__DaveIsTesting|0_0();
    }

    internal static void <$Main>g__DaveIsTesting|0_0()
    {
        Console.WriteLine("Dave is testing.");
        <$Main>g__DaveIsTestingAgain|0_1();
    }

    internal static void <$Main>g__DaveIsTestingAgain|0_1()
    {
        Console.WriteLine("Dave is testing again.");
    }
}
```

Or not. They are just split out into different functions in my class. Carry on.

# Wrapping up

In this post, we've put top-level programs through its paces by seeing how it works with status code, async calls, command line arguments, and local functions. I've found that this can be a lot more powerful that slimming down lines of code.

What have you used it for, so far? Anything I miss? Let me know in the comments.
