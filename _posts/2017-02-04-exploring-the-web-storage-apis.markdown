---
date: "2017-02-04"
title: "Exploring the Web Storage APIs"
tags: [javascript, html5]
---

For years, if we wanted to store local data we would look to using HTTP cookies—a convenient way to persist small amounts of data on a user's machine. And we do mean *small*: cookies are limited to about 4KB apiece. Also, the cookie is passed along with every request and response even if it isn't used, making for heavy HTTP messaging.

Enter the [web storage APIs](https://www.w3.org/TR/webstorage/), a way for you to store key/value paired string values on a user's machine. Local storage provides much more space—modern browsers support a minimum of 5MB, much more than the 4KB for cookies.

When using web storage, we can implement either `localStorage` or `sessionStorage` from the `Storage` object. Let's work through how to accomplish this.

### Checking web storage compatibility ###

Although most browsers [support web storage](http://caniuse.com/#feat=namevalue-storage), you still should be a good developer and verify that a user isn't using an unusually old browser like [IE6](http://www.saveie6.com/):

```javascript
function isWebStorageSupported() {
   return 'localStorage' in window;
}
```

### When to use `localStorage` or `sessionStorage` ###

The `localStorage` and `sessionStorage` APIs are nearly identical with one key exception: persistence.

The `sessionStorage` object is only available for the duration of the browser session, and is deleted automatically when the window is closed. However, it does stick around for page reloads.

Meanwhile, `localStorage` is persisted until it is explicitly by the site or the user. All changes made to `localStorage` are available for all current and future visits to the site.

Whatever the case, both `localStorage` and `sessionStorage` work well when working with **non-sensitive data** needed within an application since data stored in `localStorage` and `sessionStorage` can easily be read or updated from a user's browser.

### Web storage API methods and properties ###

The following is a [list of methods and properties](https://developer.mozilla.org/en-US/docs/Web/API/Storage) available on global `Storage` object's `localStorage` and `sessionStorage` variables.

* **`key(index)`** - finds a key at a given index. As with finding indexes in other code you write, you should check the length before finding an index to avoid any null or out-of-range exceptions.
* **`getItem(key)`** - retrieve a value by using the associated key.
* **`setItem(key, value)`** - stores a value by using the associated key. Whether you are setting a new value or updating an existing value, the syntax is the same.
* **`removeItem(key)`** - removes a value from local storage.
* **`clear()`** - removes all items from storage.
* **`length`** - a read-only property that gets the number of entries being stored.

### Storing objects ###

While you can only have string values in web storage, you can store arrays or even JavaScript objects using the JSON notation and the available utility methods, like in the following example.

```javascript
var player = { firstName: 'Kris', lastName: 'Bryant' };
localStorage.setItem('kris', JSON.stringify(player));
```

You can then use the `parse()` method to deserialize the `kris` object.

```javascript
var player = JSON.parse(localStorage.getItem('kris'));
```

### Keeping web storage synchronized ###

How do you keep everything in sync when a user has multiple tabs or browser instances of your site open concurrently? To solve this problem, the web storage APIs have a storage event that is raised whenever an entry is updated (add/update/removed). Subscribing to this event can provide notifications when something has changed. This works for both `localStorage` and `sessionStorage`.

Subscribers receive a `StorageEvent` object that contains data about what changed. The following properties are accessible from the `StorageEvent` object. The storage event cannot be canceled from a callback. The event is merely a notification mechanism: it informs subscribers when a change happens.

* **`key`** - gets the key. The key will be null if the event was triggered by `clear()`
* **`oldValue`** - gets the initial value if the entry was updated or removed. Again, the value will be null if an old value did not previously exist, or if `clear()` is invoked.
* **`newValue`** - gets the new value for new and updated entries. The value is null if the event was triggered by the `removeItem()` or `clear()` methods.
* **`url`** - gets the URL of the page on the storage action
* **`storageArea`** - gets a reference to either the `localStorage` or `sessionStorage` object

To begin listening for event notifications, you can add an event handler to the storage event as follows.

```javascript
function respondToEvent(event) {
 alert(event.newValue);
}

window.addEventListener('storage', respondToChange, false);
```

To trigger this event, perform an operation like the following in a new tab from the same site.

```javascript
localStorage.setItem('player', 'Kris');
```

### The fine print ###

While web storage offers many benefits over cookies, it is not the end-all, be-all solution, and comes with serious drawbacks as [others](https://hacks.mozilla.org/2012/03/there-is-no-simple-solution-for-local-storage/) have [noted](http://webreflection.blogspot.com/2012/03/whats-localstorage-about.html).

* **Web storage is synchronous** - because web storage runs synchronously, it can block the DOM from rendering while I/O is occurring.
* **No indexing or transactional features** - web storage does not have indexing, which may incur performance bottlenecks on large data queries. If a user is modifying the same storage data in multiple browser tabs, one tab could potentially overwrite the value in another.
* **Web storage does I/O on your hard drive** - because web storage writes to your hard drive, it can be an expensive operation depending on what your system is currently doing (virus scanning, indexing data, etc.) While you can store a lot more data in local storage, you'll need to be cognizant of performance.
* **Persistence** - if a user no longer uses a site and storage is not explicitly deleted, storage is still loaded when you start the browser session
* **First request memory loading** - because browsers load data into memory, it could use a lot of memory if many tabs are utilizing web storage mechanisms.

Web storage does offer you a simple, direct way to store user data, with caveats that it can degrade performance if you do not use it wisely thanks to its synchronous, I/O nature. Much like anything else you code, understand its utility and its nuances before using it, and **don't be greedy**. It very well can bite you in the rear end if you expect too much.
