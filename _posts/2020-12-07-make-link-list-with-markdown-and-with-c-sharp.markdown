---
date: "2020-12-07"
title: "Automate a Markdown links page with Pinboard and C#"
subtitle: "In this post, we generate a Markdown links page using Pinboard and C#."
tags: [tools, csharp, dotnetstacks]
share-img: /assets/img/pinboard-post.png
---

*This is my contribution for the [C# Advent Calendar](https://www.csadvent.christmas/), a collection of awesome C# posts throughout December. Check it out!*

I run a weekly newsletter called *The .NET Stacks*. If you've read this blog for any period of time you know this, thanks to my shameless plugs. (Feel free [to subscribe](https://dotnetstacks.com)!) I love it and enjoy writing it every week, but it can take up a lot of my time. 

The biggest time spent is generating all the great community links. Luckily, I found a way to automate this process. I can click a button, and a console app writes all my links to my Markdown file. Then, all I have to do is fill in the rest of the newsletter.

It's down to a two-step process, really: throughout the week, I add links to my Pinboard, then my app writes them to a Markdown file. 

This post will show off how you can populate a Markdown links page using Pinboard and C#.

# How I curate links

The act of getting links has to be somewhat manual. I could say "whenever Person X posts, save it" but what if this person goes nuts and writes a post criticizing Santa Claus? My links are curated—while I don't agree with everything I share, I do want them to be relevant and useful. After getting links from my various RSS feeds and link aggregators, I can start the automation.

Where do I store my bookmarks? I'm a big fan [of Pinboard](https://pinboard.in/). For not even $2 a month, it's a no frills, fast, and secure way to save bookmarks. No ads, no tracking, and a great feature set. And it [comes with an API](https://pinboard.in/api/)! I knew this would save me hours a month and it makes the investment well worth it. 

After exploring the API docs, I found a hacky—but useful!—way to save me loads of time.

![A link in Pinboard]({{ site.url }}{{ site.baseurl }}/assets/img/markdown-link.png)

These are all retrievable fields from the API. The `title`, which is my name, is anything before the link and the `description` is my link text.

After I get all my links in, usually by Sunday, I'm ready to generate the links. Let's look at how I make that happen.

# Use C# to generate Markdown links page

This is all done with a simple console app. It isn't enterprise-ready; it's for me. So I didn't go crazy with any of it—the point of this is something quick to get my time back.

To interact with the Pinboard API, I'm using the [Pinboard.net NuGet package](https://github.com/shrayasr/pinboard.net), a wonderful C# wrapper that makes connecting to the API so easy. Thanks to [Shrayas Rajagopal](https://github.com/shrayasr) for your work on this!

I was able to use [C# 9 top-level programs](https://daveabrock.com/2020/07/09/c-sharp-9-top-level-programs) to avoid the `Main` method ceremony. 

At the top of the program, I do the following:

```csharp
using var pb = new PinboardAPI("my-api-key");
var bookmarksList = await pb.Posts.All();
await WriteMarkdownFile(bookmarksList);
```

I connect to Pinboard using my API key and get all my bookmarks (referred to as `Posts`). Then, everything happens in my `WriteMarkdownFile` method.

I define the `filePath` on my system, and include today's date in the name. 

```csharp
var filePath = $"C:\\path\\to\\site\\_drafts\\{DateTime.Now:yyyy-MM-dd}-dotnet-stacks.markdown";
```

All my links are categorized by tags. To avoid hardcoding, I have a `Tags` class to store the tag names (what I use in Pinboard) and the heading (what is in the newsletter):

```csharp
public static class Tags
{
    public const string AnnouncementsHeading = "📢 Announcements";
    public const string AnnouncementsTag = "Announcements";

    public const string BlazorHeading = "😎 Blazor";
    public const string BlazorTag = "Blazor";

    // and so on and so forth
}
```

Back to the `WriteMarkdownFile` method, I store these in a dictionary. Depending on the week, I change the order of these, so I want that flexibility. (I could have sorting logic, *I suppose*.)

```csharp
var tagInfo = new Dictionary<string, string>
{
    { Tags.AnnouncementsHeading, Tags.AnnouncementsTag },
    { Tags.CommunityHeading, Tags.CommunityTag },
    { Tags.BlazorHeading, Tags.BlazorTag },
    { Tags.DotNetCoreHeading, Tags.DotNetCoreTag },
    { Tags.CloudHeading, Tags.CloudTag },
    { Tags.LanguagesHeading, Tags.LanguagesTag },
    { Tags.ToolsHeading, Tags.ToolsTag },
    { Tags.XamarinHeading, Tags.XamarinTag },
    { Tags.PodcastsHeading, Tags.PodcastsTag },
    { Tags.VideoHeading, Tags.VideoTag }
};
```

Using a classic `StringBuilder`, I start to write out the file. I begin with my Jekyll front matter and the beginning heading, which is always the same:

```csharp
var sb = new StringBuilder("---");
sb.AppendLine();
sb.AppendLine($"date: \"{DateTime.Now:yyyy-MM-dd}\"");
sb.AppendLine("title: \"The .NET Stacks: <fill in later>\"");
sb.AppendLine("tags: [dotnet-stacks]");
sb.AppendLine("comments: false");
sb.AppendLine("---");
sb.AppendLine();

sb.AppendLine("## 🌎 Last week in the .NET world");
sb.AppendLine();
sb.AppendLine("### 🔥 The Top 3");
sb.AppendLine();
```

Here's the fun part, where I print out all the bookmarks:

```csharp
foreach (var entry in tagInfo)
{
    sb.AppendLine($"### {entry.Key}");
    sb.AppendLine();

    foreach (string bookmark in GetBookmarksForTag(entry.Value, bookmarksList))
    {
        sb.AppendLine($"- {bookmark}");
    }

    sb.AppendLine();
}
```

For each tag in the dictionary, I create a heading with the tag text, then do a line break. Then, for each `bookmark` object, I have a method that retrieves a filtered list based on the tag. For each bookmark, I construct a string with how I want to format the link, then add it to a list.

```csharp
static string[] GetBookmarksForTag(string tag, AllPosts allBookmarks)
{
    var filteredBookmarks = allBookmarks.Where(b => b.Tags.Contains(tag));
    var filteredList = new List<string>();

    foreach (var bookmark in filteredBookmarks)
    {
        var stringToAdd = $"{bookmark.Description} [{bookmark.Extended}]({bookmark.Href}).";
        filteredList.Add(stringToAdd);
    }

    return filteredList.ToArray();
}
```

Once I'm done with all the tags, I use a `TextWriter` to write to the file itself.

```csharp
static string[] GetBookmarksForTag(string tag, AllPosts allBookmarks)
{
    var filteredBookmarks = allBookmarks.Where(b => b.Tags.Contains(tag));
    var filteredList = new List<string>();

    foreach (var bookmark in filteredBookmarks)
    {
        var stringToAdd = $"{bookmark.Description} [{bookmark.Extended}]({bookmark.Href}).";
        filteredList.Add(stringToAdd);
    }

    return filteredList.ToArray();
}
```

In just 76 lines of code, I was able to come up with something that saves me a ridiculous amount of time. There are other improvements to be made, like storing backups to Azure Storage, but I like it.

Here's the full code, if you'd like:

```csharp
using pinboard.net;
using pinboard.net.Models;
using PinboardToMarkdown;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using var pb = new PinboardAPI("my_api_key");
var bookmarksList = await pb.Posts.All();
await WriteMarkdownFile(bookmarksList);

static async Task WriteMarkdownFile(AllPosts bookmarksList)
{
    var filePath = $"C:\\path\\to\\site\\_drafts\\{DateTime.Now:yyyy-MM-dd}-dotnet-stacks.markdown";

    var tagInfo = new Dictionary<string, string>
    {
          { Tags.AnnouncementsHeading, Tags.AnnouncementsTag },
          { Tags.CommunityHeading, Tags.CommunityTag },
          { Tags.BlazorHeading, Tags.BlazorTag },
          { Tags.DotNetCoreHeading, Tags.DotNetCoreTag },
          { Tags.CloudHeading, Tags.CloudTag },
          { Tags.LanguagesHeading, Tags.LanguagesTag },
          { Tags.ToolsHeading, Tags.ToolsTag },
          { Tags.XamarinHeading, Tags.XamarinTag },
          { Tags.PodcastsHeading, Tags.PodcastsTag },
          { Tags.VideoHeading, Tags.VideoTag }
    };

    var sb = new StringBuilder("---");
    sb.AppendLine();
    sb.AppendLine($"date: \"{DateTime.Now:yyyy-MM-dd}\"");
    sb.AppendLine("title: \"The .NET Stacks: <fill in later>\"");
    sb.AppendLine("tags: [dotnet-stacks]");
    sb.AppendLine("comments: false");
    sb.AppendLine("---");
    sb.AppendLine();

    sb.AppendLine("## 🌎 Last week in the .NET world");
    sb.AppendLine();
    sb.AppendLine("### 🔥 The Top 3");
    sb.AppendLine();

    foreach (var entry in tagInfo)
    {
        sb.AppendLine($"### {entry.Key}");
        sb.AppendLine();

        foreach (string bookmark in GetBookmarksForTag(entry.Value, bookmarksList))
        {
            sb.AppendLine($"- {bookmark}");
        }

        sb.AppendLine();
    }

    await using TextWriter stream = new StreamWriter(filePath);
    await stream.WriteAsync(sb.ToString());
}

static string[] GetBookmarksForTag(string tag, AllPosts allBookmarks)
{
    var filteredBookmarks = allBookmarks.Where(b => b.Tags.Contains(tag));
    var filteredList = new List<string>();

    foreach (var bookmark in filteredBookmarks)
    {
        var stringToAdd = $"{bookmark.Description} [{bookmark.Extended}]({bookmark.Href}).";
        filteredList.Add(stringToAdd);
    }

    return filteredList.ToArray();
}
```

# Wrap up

In this post, I showed how you can use Pinboard and C# to send links to a Markdown page automatically. I showed why I like to use Pinboard, and we also stepped through the code to see how it all works.

If you have any suggestions, please let me know!



