---
date: "2017-02-04"
title: "The AppCache API: Is It Worth It?"
tags: [javascript, html5]
---

The Application Cache (AppCache) API allows offline access to your application by providing the following benefits:

* **Performance** - Specified site resources come right from disk, avoiding any network trips
* **Availability** - Users can navigate to your site when they are offline
* **Resilience** - If a server breaks or something bombs and your site is inaccessible online, users can still use your offline site.

In this post, we'll explore how to implement AppCache and investigate its benefits and many drawbacks.

### Creating a manifest file ###

First, you'll need to create a manifest file, a static text file that tells the browser which assets to cache for offline availability. A manifest can have three different sections (the order and frequency of these sections do not matter):

* **Cache** - This default section lists the site assets that will be cached after an initial download.
* **Network** - Files listed here can come from the network if they aren't cached. Otherwise, the network isn't used even if the user is online.
* **Fallback** - A section, which is optional, that specifies pages to use if a resource is not available. The first URI is the resource, and the second is what is used if the network request fails or errors.

Keep in mind the following "gotchas" with the manifest: if the manifest file cannot be found, the cache is deleted. Also, if the manifest or a resource specified in the manifest cannot be found and downloaded, the entire offline caching process fails and the browser will keep using the old application cache.

So, when will the browser use a new cache? This occurs when a user clears their cache, a manifest file is modified, or programmatically (we'll get to that later).

Pay special attention to the second item. A common misconception is that when any resources listed within the manifest change, they will be re-cached. That is wrong. The manifest file itself needs to change. To facilitate this, it is a common practice to leave a timestamp comment at the top of the file that you can update whenever the manifest changes, such as in my example below.

```
CACHE MANIFEST
# v5 2016-08-15
index.html
css/main.css
scripts/script.js
images/hanna.jpg
images/emma.jpg

NETWORK:
*

FALLBACK:
offline.html
```

### Referencing the manifest file ###

Now that you've created the manifest file, you then need to reference it in your web page(s). To do this, you'll need to append the manifest attribute to the opening `<html>` tag of any page you want cached:

```html
<html manifest="manifest.appcache">
...
</html>
```

This bears repeating: the attribute must be included on every page that you want cached. The browser will not cache a page if the manifest attribute is not included on the specific page.

### Using the AppCache APIs ###

Now that you have created the manifest file and decided which pages you want to be cached, you can now talk to the AppCache programmatically from the global JavaScript `window.applicationCache` object. From this object, you can call the following methods:

* `abort` - kills the cache download process
* `addEventListener` - registers an event handler for a specific event type
* `dispatchEvent` - sends an event to a current element
* `removeEventListener` - removes a handler previously registered by addEventListener
* `swapCache` - swaps an old cache for a new cache
* `update` - triggers an update of the existing cache only if updates are available

From the [MDN documentation on AppCache](https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache), here's how you would see if your application has an updated manifest file.

```javascript
function onUpdateReady() {
  console.log('I found a new version!');
}

window.applicationCache.addEventListener('updateready', onUpdateReady);

if (window.applicationCache.status === window.applicationCache.UPDATEREADY) {
  onUpdateReady();
}
```

Note, as specified in the MDN documentation, that *"…since a cache manifest file may have been updated before a script attaches event listeners to test for updates, scripts should always test `applicationCache.status`."*

### The fine print ###

In [Application Cache is a Douchebag](http://alistapart.com/article/application-cache-is-a-douchebag), Jake Archibald brilliantly lays out the many limitations of the AppCache API. You should read that piece for the full details, but here are a few gotchas that I haven't mentioned yet:

* **Files come from the cache if you're online** – you'll first get a version of the site from your cache. After rendering, the browser then finds updates to the manifest. As noted in the article, it means the browser doesn't have to wait for timing out connections, but it's somewhat annoying.
* **Non-cached resources don't load on a cached page** – if you cache, for example, a web page but not an image on it, the image will not display on the page even when you are online. Really. You would get around this by adding the * to the Network section in your manifest. (However, these connections will fail anyway if you are offline.)
* **You can't access a cached file by its parameters** – accessing `index.html` by parameters such as `index.html?parameter=value` will not retrieve the cached page. It will fetch the page over the network.

As you can imagine, AppCache's limitations have spurned a "let's not use AppCache" movement across the Web. It has been removed from the Web standards in favor of service workers. When describing service workers, the MDN documentation summed up AppCache's rise and fall nicely:

The previous attempt — AppCache — seemed to be a good idea because it allowed you to specify assets to cache really easily. However, it made many assumptions about what you were trying to do and then broke horribly when your app didn't follow those assumptions exactly.

So to answer my initial question, it is not worth it; don't use AppCache. Unless you are completely aware of the limitations and able to live with them, AppCache's drawbacks outweigh its benefits. The community has spoken, and using local storage or service workers is the preferred approach.
