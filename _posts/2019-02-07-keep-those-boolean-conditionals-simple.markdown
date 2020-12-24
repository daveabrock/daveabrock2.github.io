---
date: "2019-02-07"
title: "Keep Those Boolean Conditionals Simple"
tags: [csharp, quick-tips]
---

When working on legacy projects (or even new ones!) you are bound to come across code that is interesting—and trust me, your colleagues will say the same about your code from time to time!

There's always a balance between terseness and readability, and sometimes you can go overboard. Here's one I see sometimes that I don't particularly enjoy.

```csharp
// C# syntax
public bool CheckABoolean(int a)
{
    if (a > 42)
        return true;
    else
        return false;
}
```

Instead, do this. Your eyes and your colleagues will thank you.

```csharp
// C# syntax
public bool CheckABoolean(int a)
{
    return a > 42;
}
```
