---
date: "2020-08-29"
title: "Dev Discussions - Luis Quintanilla (1 of 2)"
subtitle: In the latest interview, we talk with Luis Quintanilla about ML.NET.
tags: [dotnet-stacks, dev-discussions]
share-img: /assets/img/dev-discussions-luis.png
---

This is the full interview from my discussion with Luis Quintanilla in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com)!
{: .box-note}

Machine learning is a fascinating world and, to many, a complicated one. As .NET developers, we definitely see the benefit in training our data but between the learning curve and using other languages like Python for machine learning—a language .NET devs might not be familiar with—ML is often sent to a developer's "I should look into that sometime" queue.

That changed in 2018, when Microsoft launched ML.NET—a free, open source, x-plat machine learning framework for .NET. With ML.NET, you can use your favorite languages like C# or F# to work with your custom machine learning models. The idea is to meet you where you are and make ML more accessible.

There's no one better to talk to about this than Luis Quintanilla. Luis has been with ML.NET since the beginning and was eventually scooped up by Microsoft to work on the docs for ML.NET. Luis had so much great stuff to share that we'll split this interview up into two parts. Today, we'll talk about his path to Microsoft, the value of ML.NET, and how to get started. Next week, [we'll talk about using ML.NET over something like Azure Cognitive Services, use cases for ML.NET, and more](https://daveabrock.com/2020/09/05/dev-discussions-luis-quintanilla-2).

![Luis Quintanilla]({{ site.url }}{{ site.baseurl }}/assets/img/luisquintanilla-picture.jpg)

## How did you get to Microsoft, and what do you do there these days?

Before joining Microsoft, I was consulting in the artificial intelligence space for companies in various industries. I was fortunate enough to work on a wide variety of projects using different technologies. Most of these technologies were not .NET technologies. At Build 2018, [ML.NET was announced](https://www.youtube.com/watch?v=OhCysVU5RDA). I had done limited .NET Framework development in the past. Coincidentally, around the same time, .NET Core 2.1 was released so I thought it was a good time to jump in with both feet. I was immediately hooked and since I wanted a break from the technologies I used every day at work, I started using it in my spare time.

I found myself being productive with ML.NET and would document what I was experimenting with [on my blog](https://www.luisquintanilla.me/archives/). Additionally, I gave several talks at local user groups and conferences on ML.NET. Around the same time, Microsoft was looking for someone to join the Microsoft Docs team to focus on ML.NET so I happened to naturally fit the role. I was fortunate enough to connect with the folks on the hiring team and the rest is history.

The past year and a half, while my focus has been ML.NET, I have recently expanded my responsibilities to also create content for Azure Machine Learning and .NET for Apache Spark. Like with ML.NET, I've had a great experience working with these technologies which are integral of data and machine learning workflows.

## What made you focus on ML.NET over other development tech?

I could write an entire essay on why ML.NET but all of the reasons can be summarized in a single word, .NET. Now, to expand on that, here are a few reasons why I enjoy ML.NET so much!

### Languages

.NET is made up of several fantastic languages, C#, F#, and Visual Basic.

Though not unique to .NET, I like statically-typed languages. I'm sure many of the readers are able to build their applications and successfully run them without errors on the first try 😉. That, however, is usually not my experience. Therefore, I prefer catching as many errors as possible at compile time. Another reason I like types is they provide a way of documenting your code.

Sometimes, it may be the case that you step away from your code for a day or two and have to refresh your memory. Types provide some form of annotation that at the very least provide you with a hint of what data types are used in functions and their overall logic. Annotations by themselves are useful, but having the compiler check those type annotations makes me feel extra safe. This is of extreme importance when working in data science and machine learning scenarios. Although ultimately the data used by machine learning algorithms to train models is encoded as numbers, knowing your data schema and checking it at compile time may help reduce the number of errors in your code as you transform your data.

With languages like Python, although you can do type checking and conversions at runtime as well as use type annotations and static type checkers, it's not the default way of working with the language. In fact, I’ve noticed various Python libraries working to introduce type annotations. I’m sure this is no easy task since it’s all being done retroactively but in my opinion it’s a worthwhile effort in the long term.

Lately I've been doing more F# development and the more I use it, the more I like it. F# for me provides a nice balance between Python and C#. F# gives you the productivity and succinctness of a language like Python, while still having the compiler and many other neat features at your disposal.

Typically, data is represented as a list or collection of items. In F#, functions and collections are integral features of the language. Therefore, you can work with them in easy and powerful ways. When performing data transformations, you can leverage the ​fact that your data is immutable by default and by using function composition, you can chain together multiple operations into a data transformation pipeline while limiting side effects. 

Although F# is a functional-first language, that doesn't mean you can't take advantage of object-oriented concepts like interfaces and classes. Because F# is a language that runs on .NET, you're also able to use existing .NET libraries and NuGet packages, such as ML.NET in your applications.

### Runtime

The .NET runtime is fast and performant. This is important in two scenarios, training machine learning models and deploying them. A good part of training machine learning models involves performing operations on vectors and matrices. .NET provides Single Instruction Multiple Data (SIMD) enabled types via the [System.Numerics](https://docs.microsoft.com//dotnet/standard/simd)  namespace. ML.NET leverages these types where possible to increase the throughput of the training operations making training fast and efficient.

A common deployment target for machine learning models is the web. In .NET, this means deploying your models in an ASP.NET application. In benchmarks, such as [TechEmpower](https://www.techempower.com/benchmarks/#section=data-r19&hw=ph&test=composite), ASP.NET Core ranks in the top 10—making it a great option to run fast and scalable machine learning applications on.

### Tooling

.NET has world class tooling across the board and you can't go wrong with any of your choices. [Visual Studio](https://visualstudio.microsoft.com/) is an excellent IDE packed with tons of functionality to help developers be more productive. Alternatively, another great IDE for .NET is [Jetbrains Rider](https://www.jetbrains.com/rider/). If you're looking for a more lightweight development environment, you can also use [Visual Studio Code](https://code.visualstudio.com/). When working with F#, you can use the [Ionide](http://ionide.io/) extension which makes F# development a pleasant experience.

Data science and machine learning workflows are very experimental. This means that you sometimes may want to have an interactive environment where you get feedback in near real-time of the outputs generated by your code. You’d also like a way to visualize your data inline. Within the data science community, a popular interactive computing environment is Jupyter Notebooks. You can leverage this interactive environment in .NET through .NET Interactive, which provides among many things, a kernel for you to run .NET code interactively.

### Deployment Targets

.NET Core was a big step towards .NET everywhere. That's not to say you couldn't run .NET outside of Windows prior to .NET Core, but .NET Core was able to cohesively package things together so that you didn't have that extra cognitive load of choosing where you want your applications to run.

Now, if you have .NET everywhere, this means at least from a deployment standpoint, you can run your machine learning models across various platforms, depending on where your users are. As mentioned previously, a common deployment scenario is hosting your machine learning model in a web API. .NET, however gives you the ability to take that same model and also run it in desktop, mobile, IoT and many other deployment targets.

The other neat thing about .NET everywhere is, you're using the languages and tooling you're used to. Whether you're developing WPF, Xamarin or Unity applications, you're typically using the same .NET languages and tooling. Therefore, you're able to leverage your existing skills and be productive almost immediately.

### Modernization

While the latest and greatest features are going into .NET Core, there are still many applications built in .NET Framework that are key to the operations of many organizations. Because ML.NET is 
built on .NET Standard, developers are able to add machine learning to their existing .NET Framework applications to enhance and modernize the capabilities of that application.

### Extensible

Although .NET is great, a large portion of the data science and machine learning space is predominantly made up of libraries and frameworks built in Python. That however does not limit 
ML.NET because it is extensible. ML.NET supports working with TensorFlow and Open Neural Network Exchange (ONNX) models. [TensorFlow](https://www.tensorflow.org/) is a popular platform for building machine learning
models. Using [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), a set of C# bindings for TensorFlow, users can train and deploy TensorFlow models with ML.NET. [ONNX](https://onnx.ai/) is an open format built to represent machine learning models. This means that you can train a model in other popular tools and frameworks like Azure Custom Vision, PyTorch, Scikit Learn. Then, you can export or convert that model to ONNX and consume it using ML.NET.

### Open Source & Community

ML.NET, like .NET, is open source. This allows for the community to collaborate and contribute to it. Users have various ways of contributing to ML.NET, whether it's raising issues, updating documentation or submitting pull requests, they're all valuable contributions that only help make the framework that much better for everyone to use.

ML.NET has a vibrant and passionate community. Being open source gives visibility into the work being done which allows community members to identify opportunities to improve the core ML.NET code, build solutions around it, or provide training materials. One example of this is [MLOps.NET](https://github.com/aslotte/MLOps.NET), an open source library for tracking the lifecycle of ML.NET machine learning models. Others include [videos](https://www.youtube.com/channel/UCrDke-1ToEZOAPDfrPGNdQw), [blog posts](https://xamlbrewer.wordpress.com/2020/02/20/getting-started-with-ml-net-in-jupyter-notebooks/), and even its very own [conference](https://www.youtube.com/channel/UClv1sloNF4mzWQiQbemHXRw) (make sure to stay tuned for updates on an upcoming hackathon).

These are only a few of the many reasons why I enjoy ML.NET. While it's still relatively new, I foresee healthy growth and a bright future as more individuals use and contribute to it.

## Correct me if I'm wrong, but I believe a big mission of ML.NET is making machine learning accessible—that is, I shouldn't have to be an expert in machine learning to do it in .NET. Even still: how much should I know before I get started?

That's right! ML.NET provides many ways of interacting with it depending on what you're most comfortable with. The easiest way to get started is by using the tooling. The tooling provides a low-code way of training and consuming ML.NET models. If you prefer a graphical user interface, you can try [Model Builder](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet/model-builder), a Visual Studio extension that guides you through the steps involved in training a machine learning model. As long as you have a general sense of the problem you're trying to solve (classify text, predict a number, categorize images) and you have a dataset, Model Builder takes care of the rest. 

Once your model is trained, code to retrain and consume your model is automatically generated for you, making it even easier to integrate your model into your end ​user applications. Some scenarios like image classification are resource intensive. As a result, you have the option of using a GPU locally or in Azure.

Alternatively, if you prefer working on the command line you can use the [ML.NET CLI](https://www.nuget.org/packages/MLNet/), a .NET command line tool for training ML.NET model models and generating consumption code. The idea is very much the same as Model Builder, except now you interact with the tooling via the command line. The CLI is also a great choice for Machine Learning Operations (MLOps) scenarios where model training and deployment is done as part of a continuous integration (CI) or continuous deployment (CD) pipeline.

For folks who want more control, prefer a code-first approach, or are more familiar with machine learning concepts, there's other ways of using ML.NET. One is with the ML.NET Automated ML (Auto ML) API. The AutoML API is leveraged by the tooling to try to find the "best" model. The best model for your problem depends on many factors such as the quantity and distribution of your data and time to train. Therefore, it helps to try different algorithms with different parameters.

Exploring various algorithms can be time consuming and inefficient if done by brute force. By using the AutoML API, you can automate this exploratory phase to find you the best model while at the same time gaining access to various settings provided by the API. Often times, it's good to use the AutoML API as a starting point to help guide you in choosing the best algorithm to solve your problem. You can then choose to deploy the trained model or further refine the model using the ML.NET API.

If you want full control over your machine learning pipeline, you can use the ML.NET API. The API provides you with direct access to data loaders, transformations, trainers, and prediction components that you can configure as needed to solve your problem.

One of the nice things is, none of the ways of using ML.NET is mutually exclusive. You can start off with the tooling to bootstrap the model training process and from there use the ML.NET API to make further refinements. In the end, it's all about choice and depending on your experience with machine learning and preferred workflow, there's an option for you.

*You can connect with Luis Quintanilla [at his blog](https://www.luisquintanilla.me/) or [on Twitter](https://twitter.com/ljquintanilla).*
