---
date: "2020-04-19"
title: "Tweeting New GitHub Pages Posts from GitHub Actions"
subtitle: Use the power of GitHub Actions to tweet GitHub pages post from a CI pipeline.
tags: [github, github-actions, ci-cd]
share-img: /assets/img/tweet-gh-actions.png
---

For the last few years, I hosted my blog on the [Ghost platform](https://ghost.org/). It was a fast, Node-powered CMS, which allowed for stupid-simple publishing: I could get in, write, and get out. However, I was looking at another annual bill for $220 and I wanted to find a better (and cheaper) way. I knew of the myriad of static site generators out there today. I eventually landed on [Jekyll + GitHub Pages](https://pages.github.com/). A month in, I'm happy that GitHub Pages gives me the flexibility to customize as I wanted, but also the simplicity. I can push a markdown file to GitHub, and then deploy to daveabrock.com automatically. All for just the cost of my domain name ($9 a year)!

After I publish my posts, I typically post them to Twitter. Ghost had a setting to do this. Could I do this from GitHub Pages? Not easily, it seemed, without leveraging external services. I could maybe toy with an RSS feed trigger from IFTTT or build something with Azure Logic Apps. Of course, it's a silly thing to obsess over but why should I manually do something repeatedly?

A thought occurred to me: if I'm using GitHub, shouldn't I be able to use their pipeline? I knew that I could leverage [GitHub Actions](https://github.com/features/actions) in my repo, which the docs say allow me to "discover, create, and share actions to perform any job you'd like, including CI/CD, and combine actions in a completely customized workflow." I would love to make this a deployment step.

Alas, being forced to manually tweet about a new post a few times a month wasn't one of my life's biggest regrets—but if I could spend a few hours learning about GitHub Actions while saving a few minutes every month, why not?

**This post is about learning**. Sure, you could look at my 16-line YAML file say, "looks pretty simple" and maybe it is, in retrospect. But when you try something new, you stumble and you learn—and that's what this post is about.

This post contains the following content.

- [The perfect world, and what I can do now](#the-perfect-world-and-what-i-can-do-now)
- [What you need before you get started](#what-you-need-before-you-get-started)
- [Add Twitter API secrets to GitHub](#add-twitter-api-secrets-to-github)
- [Create your first GitHub Action](#create-your-first-github-action)
- [How to get just the commit message from Git](#how-to-get-just-the-commit-message-from-git)
- [Passing commit message to my GitHub action](#passing-commit-message-to-my-github-action)
- [Wrapping up](#wrapping-up)

# The perfect world, and what I can do now

Without knowing much about GitHub Actions, here's the workflow I envisioned:

1. Every time I write a post, push my markdown file (and associated files, like images) to my repository.
2. Run a GitHub Actions workflow step from that push
3. In that step, get the post title and path to the post
4. Send a tweet with this information

As you can imagine, it was the third step that took some thought. In a perfect world, here's how I would grab the post and URL information:

1. Somehow, grab the Git commit data and parse the committed files
2. Assuming I'll only be pushing one markdown file, get the title metadata which is sitting inside the file
3. Get path, which more or less is the filename (would just have the replace the `.markdown` extension with `.html`)
4. Pass that string to the [Send Tweet Action](https://github.com/marketplace/actions/send-tweet-action)

Did I mention I didn't want to write any additional code or scripts, or spend more than a few hours on this? It was time to reset my expectations and think a little smaller. Thanks to some input from [my friend Isaac Levin from Microsoft](https://twitter.com/isaacrlevin)—[on Twitter, naturally](https://twitter.com/isaacrlevin/status/1251176465237868544)—I decided to just add the title and URL to my Git commit message. Then, I could just pass down the commit message to the Twitter action. This was manageable.

Let's get started.

# What you need before you get started

If you're playing along at home, you need to do the following before proceeding with this post:

- Register for a [Twitter developer account](https://developer.twitter.com). It wouldn't be a terrible idea to set up a test account first to avoid spamming your followers
- Once the registration is approved, head over to [developer.twitter.com/apps](https://developer.twitter.com/apps) and create a Twitter application. You'll need to do this so the Send Tweet Action can call the Twitter API on your behalf
- A GitHub repository (this demo works for any push, but the obvious use case is a GitHub Pages site)

# Add Twitter API secrets to GitHub

We will be using the [Send Tweet Action](https://github.com/marketplace/actions/send-tweet-action), developed by GitHub's [Edward Thomson](https://twitter.com/ethomson). This action handles connecting to Twitter API to send the tweet. We need to provide it the Twitter consumer API key, Twitter consumer API secret, Twitter access token, and Twitter access token secret.

To retrieve those, go to [developer.twitter.com/apps](https://developer.twitter.com/apps), click `Details` next to the application you created, then the `Keys and tokens` link.

You need to add those to GitHub as encrypted secrets. From GitHub, go to **Settings** > **Secrets**. I recommend calling them `TWITTER_CONSUMER_API_KEY`, `TWITTER_CONSUMER_API_SECRET`, `TWITTER_ACCESS_TOKEN`, and `TWITTER_ACCESS_TOKEN_SECRET`, so you can easily follow the code in this post. For details on storing encrypted secrets in GitHub, read [this GitHub article](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).

# Create your first GitHub Action

To get started, you'll need to set up a GitHub Action. To do that, click the `Actions` link at the top of your repository, right next to `Pull Requests`. From there, you'll see the power of GitHub Actions—there are so many CI workflows and automation processes to choose!

![Actions options]({{ site.url }}{{ site.baseurl }}/assets/img/actions-options.png)

For us, though, we'll use `Simple Workflow.` In that pane, click `Set up this workflow.` Once you do that, you will see that you are now editing a `blank.yml` file (which you can rename), sitting in a `.github/workflows` directory. We'll be updating this file.

Let's send a tweet! Replace the contents in `blank.yml` with this, right in GitHub, and commit the file. (We will explain particulars in a second.)

```yaml
name: Send a Tweet
on: [push]
jobs:
  tweet:
    runs-on: ubuntu-latest
    steps:
      - uses: ethomson/send-tweet-action@v1
        with:
          status: "NEW POST!"
          consumer-key: ${% raw %}{{ secrets.TWITTER_CONSUMER_API_KEY }}{% endraw %}
          consumer-secret: ${% raw %}{{ secrets.TWITTER_CONSUMER_API_SECRET }}{% endraw %}
          access-token: ${% raw %}{{ secrets.TWITTER_ACCESS_TOKEN }}{% endraw %}
          access-token-secret: ${% raw %}{{ secrets.TWITTER_ACCESS_TOKEN_SECRET }}{% endraw %}
```

After you commit the file, you can head over to the `Actions` page in GitHub to monitor the status.

![Job results]({{ site.url }}{{ site.baseurl }}/assets/img/job-results.png)

With any luck, you should see a tweet that says "NEW POST!" Let's break down what's happening. (You can also [review the docs](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions) for details on GitHub Actions workflow syntax.)

- `name` - the name of your workflow, which is displayed on the actions page
- `on` - the event that triggers the workflow. I included `push` but you can also do `pull_request` or other options
- `jobs` - a workflow comprises one or more jobs
  - `runs-on` - the type of machine to run the job on (required)
  - `steps` - one of a sequence of tasks in a job
    - `uses` - selects an action to run. In our case, we are using the Send Tweet Action from the GitHub Marketplace
    - `with`- the input that the action requires. For this action, the `status` is the tweet text, and the rest of the fields reference the secrets you created earlier.

So, this is great! However, your followers aren't mind readers. Tweeting "NEW POST!" doesn't do much. We now have to figure out how to tweet the commit message.

# How to get just the commit message from Git

To get the latest commit from Git, I know I can use `git log 1`. Here's what I get back:

```bash
commit cc789c6bd3032e0d28111b2515f3931cb5b95e4b (HEAD -> master)
Merge: e5b2597 a07f6b5
Author: Dave
Date:   Sun Apr 19 08:05:02 2020 -0500

Merge branch 'master' of https://github.com/daveabrock/daveabrock.github.io
```

I already see two problems: (1) this is a merge commit message, and (2) I *just* want the message.

How about `git log 1 --no-merges`?

```bash
commit cc789c6bd3032e0d28111b2515f3931cb5b95e4b (HEAD -> master)
Author: Dave
Date:   Sun Apr 19 08:05:02 2020 -0500

my commit
```

Better! But I just want to see `my commit.` Luckily, Git has a [`--pretty` flag](https://git-scm.com/docs/pretty-formats). I can pass `--pretty=%B`, where `%B` is the raw body of the commit message. So now, let's pass `git log --no-merges -1 --pretty=%B`. And what do we get?

```bash
my commit
```

Victory! (Or you could have googled it, but what fun is that?)

# Passing commit message to my GitHub action

So now, I know how to do things: how to tweet when I push and how to get my commit message. How do we connect them? From my `blank.yml` file, I want a way to run `git log` and save the output, then pass the output to the action's `status` field.

From what I could see, the `checkout` action allows me to [fetch commit details](https://github.com/actions/checkout). So, I can do something like this:

```yaml
name: Send a Tweet
on: [push]
jobs:
  tweet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - id: log
        run: echo "$(git log --no-merges -1 --pretty=%B)"
    # rest of file removed for brevity
```

If I look at the output from the Actions page, everything looks great. Now, how can I store this in a variable? Well, I was initially reading about a `steps.logs.outputs` object. If, for example, I changed my run command to `echo "::set-output name=message::$(git log --no-merges -1 --pretty=%B)"` I could access the value from using `steps.log.outputs.message`. Great, let's try it! Here's the full file. What do you think will happen?

```yaml
name: Send a Tweet
on: [push]
jobs:
  tweet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - id: log
        run: echo "::set-output name=message::$(git log --no-merges -1 --pretty=%B)"
      - uses: ethomson/send-tweet-action@v1
        with:
          status: "NEW POST: ${% raw %}{{ steps.log.output.message }}{% endraw %}"
          consumer-key: ${% raw %}{{ secrets.TWITTER_CONSUMER_API_KEY }}{% endraw %}
          consumer-secret: ${% raw %}{{ secrets.TWITTER_CONSUMER_API_SECRET }}{% endraw %}
          access-token: ${% raw %}{{ secrets.TWITTER_ACCESS_TOKEN }}{% endraw %}
          access-token-secret: ${% raw %}{{ secrets.TWITTER_ACCESS_TOKEN_SECRET }}{% endraw %}
```

This results in a **NEW POST:** tweet, sadly, with no commit data. This does not persist across actions or steps (in retrospect, the reason for it being in `steps`). Luckily, GitHub Actions has [environment variables](https://help.github.com/en/actions/configuring-and-managing-workflows/using-environment-variables), which come to our rescue.

While I was on the right track with my `run` statement, I should have used `set-env` instead. This injects the value into an `env` object that GitHub Actions uses for environmental variables. So: if I say `set-env name=POST_COMMIT_MESSAGE` I could access it later using `${% raw %}{{ env.POST_COMMIT_MESSAGE }}{% endraw %}`. Here is the updated file.

```yaml
name: Send a Tweet
on: [push]
jobs:
  tweet:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - id: log
        run: echo "::set-env name=POST_COMMIT_MESSAGE::$(git log --no-merges -1 --pretty=%B)"
      - uses: ethomson/send-tweet-action@v1
        with:
          status: "NEW POST: ${% raw %}{{ steps.log.output.message }}{% endraw %}"
          consumer-key: ${% raw %}{{ secrets.TWITTER_CONSUMER_API_KEY }}{% endraw %}
          consumer-secret: ${% raw %}{{ secrets.TWITTER_CONSUMER_API_SECRET }}{% endraw %}
          access-token: ${% raw %}{{ secrets.TWITTER_ACCESS_TOKEN }}{% endraw %}
          access-token-secret: ${% raw %}{{ secrets.TWITTER_ACCESS_TOKEN_SECRET }}{% endraw %}
```

It works! So now, in the future, if I push a commit message to `master` in the format `<Post title> <url>` it will push to Twitter right away. (I can now customize based on PR or other policies, as well.)

# Wrapping up

I hope you enjoyed reading this post as much as I did writing it. This was my first foray into this, so if you know of a better and simpler way to do this, leave a comment below ([or on Twitter, obviously](https://twitter.com/daveabrock)).
