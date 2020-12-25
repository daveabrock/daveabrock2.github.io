---
date: "2020-05-08"
title: "First Look: C# Source Generators"
subtitle: An initial look at the power of C# source generators.
tags: [csharp]
---

Last week, Microsoft introduced a preview of [C# Source Generators](https://devblogs.microsoft.com/dotnet/introducing-c-source-generators/), to be released with the C# 9 release. While the tooling isn't great (yet), it's available for curious developers to play with—so long as you are on the [latest version of Visual Studio 2019 preview](https://dotnet.microsoft.com/download/dotnet/5.0) and the [latest .NET 5 preview](https://dotnet.microsoft.com/download/dotnet/5.0), too. If you've been geeking out on C# for a while, you may remember this [was proposed as early as C# 6.](https://github.com/dotnet/roslyn/blob/12bd769ebcd3121b88f535e8559f5a42d9c0e873/docs/features/generators.md)

**Heads up!** as you can imagine, this is in preview so this content is definitely subject to change. Be aware, especially with feedback [from](https://twitter.com/andrewlocknet/status/1255963536041402369) [the](https://twitter.com/ChaseAucoin/status/1256259137291333632) [community](https://twitter.com/daveabrock/status/1257847659525804032), that the samples aren't always working 100%.
{: .notice--warning}

This post contains the following content.

- [Source generators overview](#source-generators-overview)
- [The ISourceGenerator interface](#the-isourcegenerator-interface)
- [Try it out](#try-it-out)
  - [Your first source generator](#your-first-source-generator)
  - [Implement the INotifyPropertyChangedPattern](#implement-the-inotifypropertychangedpattern)
- [Wrapping up](#wrapping-up)
- [References](#references)

# Source generators overview

From the Microsoft blog post, they define source generators as "a piece of code that runs during compilation and can inspect your program to produce additional files that are compiled together with the rest of your code." It's a compilation step that generates code *for you* based on your existing code. The benefit here, [straight from Microsoft's design document](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.md), is that source generators can read the contents of the compilation before runtime and access any additional files—meaning C# can now read both C# code and files specific to source generation.

So, in a step-by-step process, your application:

1. Kicks off compilation
2. Runs the source generator compilation step, analyzing your code and then generating code added as compilation input
3. Completes the source generator step and continues and eventually completes the compilation process

If you've ever leaned on reflection in your projects, you might begin to see many use cases for these solutions—C# source generators provide a lot of advantages that reflection currently offers and few, if any, drawbacks. Reflection is extremely powerful when you want to query properties and attributes you don't know about when you typically compile. Of course, getting type information at runtime can incur a large performance cost, so offloading this to compilation is definitely a game-changer in C#.

How else can this help you? Microsoft is developing a [source generators cookbook](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.cookbook.md) that walks through a bunch of real-world scenarios, such as class generation, file transformation, interface implementation, and more.

It is important to note that source generators are *additive only*, meaning generators add new code to your compilation but do not modify existing user code. You can [reference the design document](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.md) for details.

# The ISourceGenerator interface

Source generators implement the `ISourceGenerator` interface, in the `Microsoft.CodeAnalysis` namespace. It looks like this:

```csharp
public interface ISourceGenerator
{
    void Initialize(InitializationContext context);
    void Execute(SourceGeneratorContext context);
}
```

The `Initialize` method is called once by the host (in this case, the IDE or compiler). The passed in `InitializationContext` registers callbacks for future generation calls. Many times, you probably won't mess with this. The `Execute` method, meanwhile, is where the magic happens: the passed-in `SourceGeneratorContext` provides access to the current compilation using the following properties.

```csharp
public readonly struct SourceGeneratorContext
    {
        public ImmutableArray<AdditionalText> AdditionalFiles { get; }

        public CancellationToken CancellationToken { get; }

        public Compilation Compilation { get; }

        public ISyntaxReceiver? SyntaxReceiver { get; }

        public void ReportDiagnostic(Diagnostic diagnostic) { throw new NotImplementedException(); }

        public void AddSource(string fileNameHint, SourceText sourceText) { throw new NotImplementedException(); }
    }
```

OK, enough explanation—let's try it out!

# Try it out

In this post, we will get our feet wet by trying out:

- A simple, "hello world" example
- Implementing the `INotifyPropertyChanged` pattern

## Your first source generator

After you confirm you're on the [latest version of Visual Studio 2019 preview](https://dotnet.microsoft.com/download/dotnet/5.0) and the [latest .NET 5 preview](https://dotnet.microsoft.com/download/dotnet/5.0), crack open Visual Studio 2019 preview and create a new .NET Standard 2.0 class library. Call it something like *MyFirstGenerator*.

So, tooling isn't great. For now, you'll need to edit the project file to the following structure:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
        <LangVersion>preview</LangVersion>
    </PropertyGroup>
    <PropertyGroup>
        <RestoreAdditionalProjectSources>https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet5/nuget/v3/index.json ;$(RestoreAdditionalProjectSources)</RestoreAdditionalProjectSources>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.CodeAnalysis.CSharp.Workspaces" Version="3.6.0-3.20207.2" PrivateAssets="all" />
        <PackageReference Include="Microsoft.CodeAnalysis.Analyzers" Version="3.0.0-beta2.final" PrivateAssets="all" />
    </ItemGroup>
</Project>
```

In your new class, add a `Generator` annotation to your class. If you have Visual Studio implement the interface for you, it'll now look like this.

```csharp
using System;
using Microsoft.CodeAnalysis;

namespace MyFirstGenerator
{
    [Generator]
    public class Generator : ISourceGenerator
    {
        public void Initialize(InitializationContext context)
        {
            throw new NotImplementedException();
        }

        public void Execute(SourceGeneratorContext context)
        {
            throw new NotImplementedException();
        }
    }
}
```

Now, we can generate a silly class with some silly properties in our `Execute` implementation. We'll leave `Initialize` alone.

```csharp
public void Execute(SourceGeneratorContext context)
{
    var sourceText = SourceText.From(@"
        namespace GeneratedClass
        {
            public class SeattleCompanies
            {
                public string ForTheCloud => ""Microsoft"";
                public string ForTheTwoDayShipping => ""Amazon"";
                public string ForTheExpenses => ""Concur"";
            }
        }", Encoding.UTF8);
    context.AddSource("SeattleCompanies.cs", sourceText);
}
```

Now, build it — with any luck the generated class should be added to your compilation.

To test this out, create a new console project (let's call it `MyFirstGeneratorTest` or something similar). Then, add a reference to the source project we just created (`MyFirstGenerator`). To get anything to run, you need to manually edit the project file to include `preview` as a `LangVersion` and `OutputItemType` and `ReferenceOutputAssembly` to your `ProjectReference`. It should look like this:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
	<TargetFramework>netstandard2.0</TargetFramework>
	<LangVersion>preview</LangVersion>
  </PropertyGroup>
  <ItemGroup>
    <ProjectReference Include="..\MyFirstSourceGenerator\MyFirstSourceGenerator.csproj"                OutputItemType="Analyzer"                         ReferenceOutputAssembly="false" />
  </ItemGroup>
</Project>
```

In your `Program` file, if you add your generated class to the using statement, you should be able to have IntelliSense pick up your generated class! Here's my code:

```csharp
using System;
using GeneratedClass;

namespace MyFirstSourceGeneratorConsole
{
    class Program
    {
        static void Main(string[] args)
        {
            var companies = new SeattleCompanies();
            Console.WriteLine("Running code from a generated class!");
            Console.WriteLine($"My favorite cloud: {companies.ForTheCloud}");
            Console.WriteLine("We're done here.");
            Console.ReadLine();
        }
    }
}
```

**Note**: In many cases, as of now you may need to restart Visual Studio to get IntelliSense support.

## Implement the INotifyPropertyChangedPattern

In the C# community, a common request is the ability to automatically implement interfaces for classes with an attribute attached to them. Source generators will make this possible, with the use of the `INotifyPropertyChanged` pattern. Let's look at a basic use of this.

Let's say you have a class with some properties you need to monitor. You can decorate it with a source generator implementation of this, like so:

```csharp
using NotifyMe;

public partial class MyClass
{
    [NotifyMe]
    private bool _boolProp;

    [NotifyMe]
    private string _stringProp;
}
```

Then, the generator can give us this code:

```csharp
using System;
using System.ComponentModel;

namespace NotifyMe
{
    [AttributeUsage(AttributeTargets.Field, Inherited = false, AllowMultiple = false)]
    sealed class AutoNotifyAttribute : Attribute
    {
        public AutoNotifyAttribute()
        {
        }
        public string PropertyName { get; set; }
    }
}

public partial class MyClass : INotifyPropertyChanged
{
    public bool BoolProp
    {
        get => _boolProp;
        set
        {
            _boolProp = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("UserBool"));
        }
    }

    public string StringProp
    {
        get => _stringProp;
        set
        {
            _stringProp = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("UserString"));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

For a deeper dive, the Roslyn team [has a good example of this in their GitHub repository](https://github.com/dotnet/roslyn-sdk/blob/master/samples/CSharp/SourceGenerators/SourceGeneratorSamples/AutoNotifyGenerator.cs).

# Wrapping up

This is really early, but I hope you can see the obvious benefits C# source generators can offer you. To be honest: the developer experience is not refined yet, but once that happens you'll really see the power they provide.

# References

- [Introducing C# Source Generators](https://devblogs.microsoft.com/dotnet/introducing-c-source-generators/) (Microsoft)
- [Source Generators design document](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.md) (Roslyn GitHub repo)
- [Source Generators cookbook](https://github.com/dotnet/roslyn/blob/master/docs/features/source-generators.cookbook.md) (Roslyn GitHub repo)
- [Discussion in r/csharp](https://www.reddit.com/r/csharp/comments/gbox73/faster_than_reflection_microsoft_preview_source/) (Reddit)
- [C# Source Generators: Less Boilerplate Code, More Productivity](https://dontcodetired.com/blog/post/C-Source-Generators-Less-Boilerplate-Code-More-Productivity) (Jason Roberts)