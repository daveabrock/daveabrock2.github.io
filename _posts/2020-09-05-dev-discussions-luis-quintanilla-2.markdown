---
date: "2020-09-05"
title: "Dev Discussions - Luis Quintanilla (2 of 2)"
subtitle: We continue our talk with Luis Quintanilla about ML.NET.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/dev-discussions-luis.png
---

This is the full interview from my discussion with Luis Quintanilla in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com)!
{: .box-note}

Machine learning is a fascinating world and, to many, a complicated one. As .NET developers, we definitely see the benefit in training our data but between the learning curve and using other languages like Python for machine learning—a language .NET devs might not be familiar with—ML is often sent to a developer's "I should look into that sometime" queue.

That changed in 2018, when Microsoft launched ML.NET—a free, open source, x-plat machine learning framework for .NET. With ML.NET, you can use your favorite languages like C# or F# to work with your custom machine learning models. The idea is to meet you where you are and make ML more accessible.

There's no one better to talk to about this than Luis Quintanilla. Luis has been with ML.NET since the beginning and was eventually scooped up by Microsoft to work on the docs for ML.NET. Luis had so much great stuff to share that we'll split this interview up into two parts. Last time, [we talked about his path to Microsoft, the value of ML.NET, and how to get started](https://daveabrock.com/2020/08/29/dev-discussions-luis-quintanilla-1). Today, we're talking about using ML.NET over something like Azure Cognitive Services, use cases for ML.NET, and more.

![Luis Quintanilla]({{ site.url }}{{ site.baseurl }}/assets/img/luisquintanilla-picture.jpg)

## Where is the dividing line for when I should use machine learning, or use Azure Cognitive Services?

This is a really tough one to answer because there's so many ways you can make the comparison. Both are great products and in some areas, the lines can blur. Here are a few of them that I think might be helpful.

### Custom Training vs Consumption

If you're looking to add machine learning into your application to solve a fairly generic problem, such as language translation or identifying popular landmarks, Azure Cognitive Services is an excellent option. The only knowledge you need to have is how to call an API over HTTP. Being able to work via HTTP also provides you with flexibility over what you use to make the requests to the API. If you want to run your machine learning workflow with Azure Cognitive Services via cURL as part of a background Cron job, that's perfectly acceptable.

Azure Cognitive Services provides a set of robust, state-of-the-art, pretrained models for a wide variety of scenarios. However, there’s always edge cases. Suppose you have a set of documents that you want to classify and the terminology in your industry is rare or niche. In that scenario, the performance of Azure Cognitive Services may vary because the pretrained models most likely have never encountered some of your industry terms. At that point, training your own model may be a better option, which for this particular scenario, Azure Cognitive Services does not allow you to do.

That's not to say Azure Cognitive Services does not allow you to train custom models. Some scenarios like language understanding and image classification have training capabilities. The difference is, training custom models is not the default for Azure Cognitive Services. Instead, you are encouraged to consume the pretrained models. Conversely, with ML.NET, for training purposes, you're always training custom models using your data.

### Online vs Offline

By default, Azure Cognitive Services requires some form of internet connectivity. In scenarios where there is strong network connectivity, this is not a problem. However, for offline or air-gapped environments, this is not an option. Although in some scenarios, you can deploy your Azure Cognitive Services model as a container, therefore reducing the number of requests made over the network, you still need some form of internet connectivity for billing purposes. Additionally, not all scenarios support container deployments—therefore, the types of models you can deploy is limited.

While Azure in general makes sure to responsibly protect the privacy and security of user data in the cloud, in some cases—whether it's a regulatory or organizational decision—putting your data in the cloud may not be an option. In those cases, you can leverage Azure Cognitive Services container deployments. Like with the offline scenario, the types of models you can deploy is limited. Additionally, you most likely would not be training custom models since you wouldn't want to send your data to the cloud.

ML.NET is offline first, which means you can train and deploy your models locally without ever interacting with a cloud environment. That being said, you always have the option to scale your training and consumption by provisioning cloud resources. Another benefit of being offline first is, you don't have to containerize your application in order to run it locally. You can take your model and deploy it as part of a WPF application without the additional overhead of a container or connecting over to the internet.

### Cost

Cost is always a tricky thing to talk about because what exactly do you account for when calculating cost? With Azure Cognitive Services, you pay for your consumption. While there are very different tiers you can subscribe to depending on your usage and the free tier is fairly generous, you always pay for what you use. Also, because all the resources are managed for you, you don't have to spend time maintaining any of them. Because most of the models you use are pretrained, it also means you don’t have to spend your time or resources training a model.

ML.NET is always free—both the library and the tooling. Since the resources for training and deployment are not managed for you, you have to think about that. However, if your machine learning model is deployed alongside your existing applications, perhaps the management, resources, and overhead may be negligible. In terms of training, since you're always training a ​custom model, the time and resources devoted to that have to be taken into account. The benefit of that, though, is that your model is fine tuned to your specific problem.

Revisiting the offline scenario, if your model is deployed in an offline environment or perhaps inside of an existing WPF application, your costs at that point are practically zero because any usage of the model isn't costing you anything extra. So again, there's many ways you can look at cost and it all depends on what you decide to take into account.

## Do you have some practical use cases for using ML.NET in business apps today?

Definitely! If you have a machine learning problem, the framework you use is fairly agnostic as long as the scenario is supported. Since ML.NET supports classical machine learning as well as deep learning scenarios, it practically can help solve a wide variety of problems. Some examples include:

* Sentiment analysis
* Sales forecasting
* Image classification
* Spam detection
* Predictive maintenance
* Fraud detection
* Web ranking
* Product recommendations
* Document classification
* Demand prediction
* And many others...
  
I would suggest for users interested in seeing how companies are using ML.NET in production today, visit the [ML.NET customer showcase](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet/customers).

## What kind of projects are you hacking away at now?

My interests are too many. If only there was enough time to do them all! Most recently we’ve started doing the [Machine Learning Community Standup](https://www.youtube.com/watch?v=2gjMrZ9XbRQ). This is a place where folks who are interested in machine learning within the .NET ecosystem can come and meet other members of the community, learn what’s new in the space, and ask questions. You can tune in every other Wednesday to check that out. If you’re working on projects with ML.NET or other .NET machine learning libraries, we’d love to hear from you!​

Also, I mentioned it earlier but folks should stay tuned for the next [Virtual ML.NET Conference](https://www.youtube.com/channel/UClv1sloNF4mzWQiQbemHXRw). The first one took place earlier this year and it was a great success. Not only was engagement high during the conference, but interest and excitement has increased since then—which can only mean good things for ML.NET. For this next version, instead of workshops and presentations, it’ll be hands-on in the form of a hackathon. Again, at the moment there aren’t many details, but make sure to stay in touch on [Twitter](https://twitter.com/virtualmlnet) or join the [Slack](https://join.slack.com/t/virtual-mlnet/shared_invite/zt-galp4khg-gUJiri5yvEfTD2vkLa9v0w) community to get the latest updates.

Personally, I've been dabbling in reinforcement learning, which is another form of artificial intelligence. Though many of the concepts don't transfer over, there are some areas where my prior knowledge is applicable, especially when it comes to the use of neural networks as part of deep reinforcement learning.

Much of that work takes me away from .NET, which I always try to get a healthy dose of. As a result, I've been doing more F# development. I still have a ways to go, but I really like the language and always try to get better at it.

For throwaway projects and learning exercises, I've been playing around with Racket. In some ways, my learnings from F# have made me a better Lisp developer. I've always been fond of the language considering for the longest time it was THE language for artificial intelligence. Though I'm far from building Lisp bindings for ML.NET (which I think is feasible), it's always fun to get outside of your comfort zone.

Of course, ML.NET is never far away either and I'm always trying to find new ways of creatively using ML.NET.

From time to time, I'll post some of the things I'm experimenting with on my [blog](https://www.luisquintanilla.me/) so feel free to check it out.

## What kind of stuff do you work on over at your Twitch stream? When is it?

Currently I stream on Monday and Wednesday mornings at 10 AM EST. My stream is a live learning session where folks sometimes drop in to learn together. On the stream, I focus on building data and machine learning solutions using .NET and other technologies. The language of choice on stream is F# though I don’t strictly abide by that and will use what works best for getting the solution working.

Most recently, I built a deep learning model for malware detection using ML.NET. I've been trying to build .NET pipelines with Kubeflow, a machine learning framework for Kubernetes, on the stream. I've had some trouble with that, but that's what the stream is about: learning from mistakes.

Inspired by a reddit post which asked how to get started with data analytics in F#, I’ve started working on using F# and various .NET tools like .NET Interactive to analyze the arXiv dataset.

If any of that sounds remotely interesting, feel free to check out and follow the channel on [Twitch](https://www.twitch.tv/lqdev1). You can also catch the recordings from previous streams on [YouTube](https://www.youtube.com/channel/UCkA5fHdQ4cf3D1J19UNgV7A).

## What is your one piece of programming advice?

Just do it! Everyone has different learning styles, but I strongly believe no amount of videos, books or blog posts compare to actually getting your hands dirty. It can definitely be daunting at first, but no matter how small or basic the application is, building things is always a good learning experience. Often there is a lot of work that goes into producing content, so end users typically get the polished product and the happy path. When you're not sure of where to start or would like to go more in depth, these resources are excellent. However, once you stray from that guided environment, you start making mistakes. Embrace these mistakes because they’re a learning experience.

*You can connect with Luis Quintanilla [at his blog](https://www.luisquintanilla.me/) or [on Twitter](https://twitter.com/ljquintanilla).*