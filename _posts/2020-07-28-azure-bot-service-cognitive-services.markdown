---
date: "2020-07-28"
title: "Talk with Groot using the Microsoft Bot Framework and Azure sentiment analysis"
subtitle: In this post, we use the Bot Framework to detect how a user really feels.
share-img: /assets/img/groot-card.png
tags: [csharp]
---

Like any good adventure, this one started on Twitter.

My friend (and [friend of the newsletter](https://daveabrock.com/2020/06/28/dev-discussions-shahed-chowdhuri)) Shahed Chowdhuri took his lunch break to create a simple chat bot using the Microsoft Bot Framework. A user enters something, and it responds with *I am Groot*.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Full source code for Groot Chatbot: <br><br>if (true) <br> echo “I am Groot.” <a href="https://t.co/NEpWhXbYEX">pic.twitter.com/NEpWhXbYEX</a></p>&mdash; Shahed Chowdhuri @ Microsoft (@shahedC) <a href="https://twitter.com/shahedC/status/1283799107597996035?ref_src=twsrc%5Etfw">July 16, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

If you aren't familiar with Groot, he is a tree-like being [from the Marvel universe](https://www.youtube.com/watch?v=f_bJZGbSI1s) who only responds with *I am Groot* - leaving much up to interpretation. Here's an idea: how about we change how Groot responds based on the sentiment of what a user types? For example, if a user types something happy, or sad, can we respond in kind? Luckily, [we can have some fun with Azure's Text Analytics Service](https://azure.microsoft.com/services/cognitive-services/text-analytics/) to analyze sentiments.

From here on out, however, we will address him at Gruut*. Why?

<blockquote class="twitter-tweet" data-conversation="none"><p lang="en" dir="ltr">... and for anyone wondering why it&#39;s spelled Gruut <a href="https://t.co/jskJrY5hLL">pic.twitter.com/jskJrY5hLL</a></p>&mdash; Shahed Chowdhuri @ Microsoft (@shahedC) <a href="https://twitter.com/shahedC/status/1283831870405382152?ref_src=twsrc%5Etfw">July 16, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**please don't sue us*

In this post, we'll do the following:

* Create a chatbot using the Microsoft Bot Framework
* Integrate the Azure Text Analytics service to detect a user's sentiment
* Learn how to debug bots
* Safely store our credentials to access our Azure services

Many [thanks to Shahed](https://twitter.com/shahedC) for the inspiration for this post.

This post covers the following topics.

- [Before you get started](#before-you-get-started)
  - [Install Bot Framework SDK templates](#install-bot-framework-sdk-templates)
  - [Install Bot Framework Emulator](#install-bot-framework-emulator)
- [Create a Text Analytics Service](#create-a-text-analytics-service)
- [Create your bot project](#create-your-bot-project)
- [Introducing your EchoBot](#introducing-your-echobot)
- [First pass: getting it working](#first-pass-getting-it-working)
- [Let's try it out](#lets-try-it-out)
- [Second pass: working with credentials safely](#second-pass-working-with-credentials-safely)
  - [Add secrets to Key Vault](#add-secrets-to-key-vault)
  - [Set up app registration in Azure Active Directory](#set-up-app-registration-in-azure-active-directory)
    - [Create client secret](#create-client-secret)
  - [Give your app rights to the Key Vault](#give-your-app-rights-to-the-key-vault)
  - [Update your app](#update-your-app)
- [Wrapping up](#wrapping-up)

# Before you get started

While there are multiple ways to develop with the Bot Framework, we'll use Visual Studio tooling. So before you get started with this tutorial, you'll need to install Bot Framework templates and the Bot Framework Emulator.

## Install Bot Framework SDK templates

To make project generation easier, you'll need to install the Bot Framework v4 SDK Templates for Visual Studio. In your Visual Studio, go to **Extensions** > **Manage Extensions,** and search for **bot**. It should be the first result.

![bot framework extension]({{ site.url }}{{ site.baseurl }}/assets/img/bot-framework-extension.png)

Alternatively, you can find the [direct download at this location](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4).

## Install Bot Framework Emulator

To test and debug your bots (either locally or remotely), you'll need to install the Bot Framework Emulator. Head on over to the GitHub releases [to install the emulator for your specific environment](https://github.com/Microsoft/BotFramework-Emulator/releases).

# Create a Text Analytics Service

Now, we can head out to Azure to create a Text Analytics instance. Assuming you have an Azure account (if not, you can [go to the Azure site to sign up and get free credits](https://azure.microsoft.com/)), head on over to the Azure Portal at *[portal.azure.com](https://portal.azure.com/).*

In the search box, enter **Text Analytics,** and select **Text Analytics** from the Marketplace section.

![bot framework extension]({{ site.url }}{{ site.baseurl }}/assets/img/text-analytics-marketplace.png)

Enter a name, select your subscription, a location, pricing tier (the Free tier should be fine for this tutorial), and a resource group, then click **Create**.

![bot framework extension]({{ site.url }}{{ site.baseurl }}/assets/img/create-text-analytics.png)

After the deployment completes, click **Go to resource**. Then, click **Keys and Endpoint**. You'll need to grab a key (Key 1 or Key 2 is fine) and the endpoint for your application. Copy these values somewhere, like a text file.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/sentiment-keys.png)

Excellent! Let's move on to Visual Studio to create our chatbot.

# Create your bot project

After you install the Bot Framework v4 Templates for Visual Studio, create a new project as you typically would.

From the Project Types drop-down, select **AI Bots** and select the **Echo Bot** template. This will give us the basic functionality we need (since Gruut does a lot of echoing).

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/echo-bot-template.png)

# Introducing your EchoBot

When your project loads, navigate over to `EchoBot.cs` (in your `Bots` folder). You'll see the `EchoBot` implements the [Microsoft.Bot.Builder.ActivityHandler interface](https://docs.microsoft.com/dotnet/api/microsoft.bot.builder.activityhandler?view=botbuilder-dotnet-stable), and has two methods implemented for you.

* `OnMessageActivityAsync` - this is where you'll include code specific to your conversational logic. *We'll override this and do our work here.*
* `OnMembersAddedAsync` - this is where you'll provide logic when new members join the conversations (like welcome logic). *You can remove this method if you wish, since we aren't using it.*

We'll be working with the `Azure.AI.TextAnalytics` package. You can install it now from the NuGet Package Manager or get Visual Studio assistance to install when you get errors. Your choice. 😎

# First pass: getting it working

Now, still in `EchoBot.cs`, find the Text Analytics key and endpoint you copied when you created your instance in Azure. At the beginning of your class, declare a `credentials` and `endpoint` static class variable:

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<your key>");
private static readonly Uri endpoint = new Uri("<your endpoint>");
```

**Hard-coding your credentials is reckless.** Don't do this for any apps that have actual users. We will fix this soon—we want to get this working first.
{: .notice--danger}

Moving on to our `OnMessageActivityAsync` method, we'll first get the text the user entered. We get this by using our `turnContext` that is passed in, that's an `ITurnContext<MessageActivity>`. The `Activity.Text` gets the contents of the sent message as a `string`.

```csharp
string userInput = turnContext.Activity.Text;
```

Now, we're ready to work with our Text Analytics service. We will first get an instance of the Text Analytics client, passing in our credentials:

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

Then, we want to call the client's `AnalyzeSentiment` method. When we call this, we get a [TextAnalytics.DocumentSentiment back](https://docs.microsoft.com/dotnet/api/azure.ai.textanalytics.textanalyticsclient.analyzesentiment?view=azure-dotnet), with [tons of data to work with](https://docs.microsoft.com/dotnet/api/azure.ai.textanalytics.documentsentiment?view=azure-dotnet).

We can work with confidence scores, a sentiment for each sentence, or any warnings that occur. For our purposes, we want to work with the `Sentiment` property. This property has a `TextSentiment`, [an enum with valid values as Mixed, Negative, Neutral, or Positive](https://docs.microsoft.com/dotnet/api/azure.ai.textanalytics.textsentiment?view=azure-dotnet). This should be fine for our purposes. Then, we can make a decision on what to send back to the user.

```csharp
var sentiment = client.AnalyzeSentiment(userInput).Value.Sentiment;
```

Now, we can write a local function in C# ([thanks for the inspiration, David Pine](https://github.com/shahedc/GruutChatbot/pull/2)) that leverages switch expressions. We send back specific responses for `Positive`, `Negative`, and `Neutral` results. If we get anything else back, we have a fallback.

```csharp
static string GetReplyText(TextSentiment sentiment) => sentiment switch
{
    TextSentiment.Positive => "I am Gruut.",
    TextSentiment.Negative => "I AM GRUUUUUTTT!!",
    TextSentiment.Neutral => "I am Gruut?",
    _ => "I. AM. GRUUUUUT"
};
```

Now, all that's left is to invoke the `GetReplyText` function and call `SendActivityAsync` to send the message back to the user.

```csharp
var replyText = GetReplyText(sentiment);
await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
```

That should do it! For your reference, here's the entire class.

```csharp
using System;
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Azure;
using Azure.AI.TextAnalytics;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Schema;

namespace GruutChatbot.Bots
{
    public class EchoBot : ActivityHandler
    {
        private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<my key>");
        private static readonly Uri endpoint = new Uri("<my endpoint>");

        protected override async Task OnMessageActivityAsync(ITurnContext<IMessageActivity> turnContext, CancellationToken cancellationToken)
        {
            var client = new TextAnalyticsClient(endpoint, credentials);
            string userInput = turnContext.Activity.Text;
            var sentiment = client.AnalyzeSentiment(userInput).Value.Sentiment;

            static string GetReplyText(TextSentiment sentiment) => sentiment switch
            {
                TextSentiment.Positive => "I am Gruut.",
                TextSentiment.Negative => "I AM GRUUUUUTTT!!",
                TextSentiment.Neutral => "I am Gruut?",
                _ => "I. AM. GRUUUUUT"
            };

            var replyText = GetReplyText(sentiment);
            await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
        }
    }
}
```

# Let's try it out

Let's see how this works. Before we debug this, set a breakpoint at the declaration of `GetReplyText`. Now, start debugging your app in Visual Studio. A browser will launch—take note of the port number after localhost (`https://localhost:xxxx`). 

Finally, we can now work with the Bot Framework Emulator. At the initial screen, click `Open Bot`. For your bot URL, enter `http://localhost:xxxx/api/messages` and replace `xxxx` with your port number you are using. Then, click `Connect`.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/open-a-bot.png)

You are connected! Enter some text to try it out and hit your breakpoint. I'll type **I'm really mad**.

With my breakpoint hit, hover over `sentiment` and you'll see it comes back as `Negative`.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/negative.png)

Great! Hit *Continue* in Visual Studio, go back to the Bot Framework Emulator, and you'll see that Gruut matches your anger.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/really-mad.png)

Excellent! This is great—however, before we ship an app with hard-coded credentials in the source code, we should probably clean that up. Let's do that now.

# Second pass: working with credentials safely

Clearly, we do not want to hard-code our credentials. Instead, here's what we'll do:

* Store our key and endpoint in the Azure Key Vault
* In Azure Active Directory, create a new app registration
* Give this new registration rights to access the Key Vault
* In our app, use the configuration provider to access key vault using our provided client ID and secret

Of course, there are even more, enterprise-y ways to do it, like managed identities—like everything else, it's a balance of security and overkill for what we're doing.

Before we proceed, you'll need to create a Key Vault or use an existing one. For details on how to create a Key Vault, [check out the docs](https://docs.microsoft.com/azure/key-vault/general/quick-create-portal) and come back when you're done. I'll wait.

## Add secrets to Key Vault

From your Key Vault, click **Secrets** and then **Generate/Import** to create two secrets: `AzureKeyCredential` and `CognitiveServicesEndpoint` I won't be showing you my setup for security reasons, but it's pretty straight-forward.

## Set up app registration in Azure Active Directory

We're now ready to set up our app registration. Head on over to Azure Active Directory (I just use the search box), and click **App Registrations**, then **New registration**. Give your registration the name and accept the default supported account type, then click **Register**.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/app-registration.png)

Copy the `Application (client) ID`, as we will need it later.

### Create client secret

Of course, this registration does nothing by its own, so we'll need to create a client secret so our app can know about it. While still in the App Registrations section, click **Certificates & secrets**, then click **New client secret**.

Enter a name, leave the default expiration policy, and click **Add**.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/client-secret.png)

You should see your new client secret. Copy this value somewhere—we will also need it soon.

![echo bot template]({{ site.url }}{{ site.baseurl }}/assets/img/key-vault-client-secret.png)

## Give your app rights to the Key Vault

Now, we're ready to head over to the Key Vault to give our chatbot access to use it via the app registration. Once you get to the Key Vault, click Access policies, then **+ Add Access Policy**.

Accept the defaults other than the following, then click **Add**:

* **Secret permissions** - Get, List
* **Select principal** - Select the client secret you used earlier (in my case, `gruut-bot`)

We're done with Azure. Now, all we need to do is update our app.

## Update your app

In your `appsettings.json` file, update it to include the name of your key vault (whatever is before *.vault.azure.net*) and the client ID and secret you just copied.

```json
{
  "KeyVault": {
    "Name": "my-key-vault-name",
    "ClientId": "my-client-id",
    "ClientSecret": "my-client-secret"
  }
}
```

In your `Program.cs` file, in `CreateHostBuilder`, we need to update it to grab the values in question once the app starts.

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((context, config) =>
        {
            var builtConfig = config.Build();
            config.AddAzureKeyVault(
                $"https://{builtConfig["KeyVault:Name"]}.vault.azure.net",
                builtConfig["KeyVault:ClientId"],
                builtConfig["KeyVault:ClientSecret"]);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Now, if you go back to `EchoBot.cs`, remove the hard-coded strings and replace it with a constructor [that references the `IConfiguration` interface](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration.iconfiguration?view=dotnet-plat-ext-3.1) using some dependency injection. This allows us to access our configuration properties.

```csharp
private readonly IConfiguration _configuration;

public EchoBot(IConfiguration configuration)
{
    _configuration = configuration;
}
```

Then, all you need to do is change the `TextAnalyticsClient` parameters to reference your configuration.

```csharp
var client = new TextAnalyticsClient(
             new Uri(_configuration["CognitiveServicesEndpoint"]),
             new AzureKeyCredential(_configuration["AzureKeyCredential"]));
```

Run your app again and you'll see it should be working the same as before—except this time, our Text Analytics credentials are stored away in our Key Vault.

# Wrapping up

In this post, we got our feet wet with the Microsoft Bot Framework. We incorporated Azure Cognitive Services to detect the sentiment of a user, and also worked on safeguarding our credentials using the Azure Key Vault and Azure Active Directory.

You can [access the source code at Shahed's repo](https://github.com/shahedc/GruutChatbot), and PRs/GIFs (or both) are always welcome.
