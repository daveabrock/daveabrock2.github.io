---
date: "2018-10-27"
title: "GitHub tip: You don't need that .git extension to clone"
subtitle: Read more to clone from the command line with the .git extension!
tags: [git, quick-tips]
---

If you want to clone a GitHub repository, the most common (and [documented](https://help.github.com/articles/cloning-a-repository/)) way is to browse to it and click that green **Clone or download** button. In this case, I am cloning the [.NET machine learning project](https://github.com/dotnet/machinelearning):

![CloneOrDownload]({{ site.url }}{{ site.baseurl }}/assets/img/CloneOrDownload.png)

Typically, I would copy the Git repository path and clone it in a command line window, like so:

`git clone https://github.com/dotnet/machinelearning.git`

As it turns out, this is a waste of clicks. All you have to do is grab the URL from the address bar of your favorite browser and ignore the `.git` extension altogether.

Try this:
![CloneWithoutExtension]({{ site.url }}{{ site.baseurl }}/assets/img/CloneWithoutExtension.png)
