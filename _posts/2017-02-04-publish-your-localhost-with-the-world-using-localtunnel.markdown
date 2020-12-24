---
date: "2017-02-04"
title: "Publish your localhost with the World using Localtunnel"
tags: [tools]
---

During your development process, you may need to show off your work from a browsable URL but you aren't quite ready for a deploy or even a check in. For example, you might want to do some easy mobile testing on your device of choice, or you may have an eager customer that wants to see your latest and greatest. Alternatively, your application might have webhooks to other services that require a public URL.

There are several options you could consider. You could use a cloud service like [Microsoft Azure](https://azure.microsoft.com/en-us/) or [Amazon Web Services](https://aws.amazon.com), but you'll need to register, configure, and eventually pay. [Now](https://www.npmjs.com/package/localhost-now) is great, but it only serves up static files. [Ngrok](https://ngrok.com/) is full-featured and robust, but if you're looking for a quick solution with minimal configuration you should look elsewhere.

I prefer [Localtunnel](https://localtunnel.github.io/www/) and its amazing simplicity. Its simplicity should not be taken as a deficiency, [as others have noted](https://news.ycombinator.com/item?id=7585056). Once I download the package, all I need to do is tell Localtunnel the port I am working on—then I get back a public URL I can share with anyone in the world.

You can get Localtunnel from a Node.js package. (If you need Node, download it from [this site](https://nodejs.org/en/download/), and of course confirm the installation by typing `node -v` in your shell.)

To get started, install Localtunnel from NPM:

```bash
npm install -g localtunnel
```

Then, once your localhost server is running, enter the following in the shell (change your port appropriately):

```bash
lt --port 8000
```

That's it! You'll get back a randomized subdomain URL to share:

```bash
your url is: https://cqjfkqyjve.localtunnel.me
```

Optionally, if you'd like a friendlier subdomain, you can use the subdomain parameter to specify one:

```bash
lt --port 8000 --subdomain dave
```

Localtunnel then gives you a custom subdomain. You can use your desired subdomain as long as **no one is using it when you are requesting yours**.

```bash
your url is: https://dave.localtunnel.me
```

You can share this link with anyone in the world, as long as the `lt` session remains active.

To find out more about Localtunnel, head over to [their GitHub repository](https://github.com/localtunnel/localtunnel). From there, you'll also see that you can use an API as well. If you're looking to share your localhost with little to no thinking or configuration, give Localtunnel a shot.
