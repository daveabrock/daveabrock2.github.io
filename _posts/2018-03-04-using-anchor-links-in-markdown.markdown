---
date: "2018-03-04"
title: "Using Anchor Links in Markdown"
tags: [blogging, markdown, quick-tips]
---

This is a short post that might help you.

I wasn't sure how to use anchor links in Markdown. These come in handy when you have a long post and want to link to different sections of a document for easy navigation.

The nice thing about Markdown is that it plays so well with straight HTML - so I was pleased to get it working on the first try.

First, add an anchor as regular HTML in your Markdown element. Here, it is right at a heading.

`### <a id="MyHeading"></a>My Heading ###`

Now, I can link it using a standard Markdown link.

`Where is my [heading](#MyHeading)?`
