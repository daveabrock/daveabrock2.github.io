---
date: "2017-02-04"
title: "Exploring the Geolocation API"
subtitle: Use the HTML geolocation API to get and monitor a user's current location.
tags: [javascript, html5]
---

Building location-aware applications is a snap with the [HTML5 Geolocation APIs](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/Using_geolocation). These APIs allows you to retrieve a user's location – with the user's permission – as a one-time request or over a period of time. This post will walk you through how to implement the Geolocation API.

### Checking for support ###

When implementing the Geolocation APIs, the first thing you'll want to do is see if geolocation is supported by a user's browser. (You'll notice that the geolocation functionality is [supported by virtually all browsers](http://caniuse.com/#feat=geolocation), but you know what they say about assuming.) You can check by writing code that [uses the in operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in); this returns `true` if the geolocation property exists in the window's navigator object.

```javascript
function supportsGeolocation() {
    return 'geolocation' in navigator;
}
```

Now that you can use the Geolocation API, you can use the `position.coords` property to retrieve some of the following values:

* The `latitude` and `longitude` attributes are geographic coordinates specified in decimal degrees.
* The `altitude` attribute denotes the height of the position, specified in meters. If the implementation cannot provide altitude information, the value of this attribute must be null.
* The `accuracy` attribute denotes the accuracy level of the latitude and longitude coordinates. It is specified in meters and must be supported by all implementations.
* The `altitudeAccuracy` attribute is specified in meters. If the implementation cannot provide altitude information, the value of this attribute must be null.
* The `heading` attribute denotes the direction of travel of the hosting device and is specified in degrees, where 0° ≤ heading < 360°, counting clockwise relative to the true north. If the implementation cannot provide heading information, the value of this attribute must be null. If the hosting device is stationary, then the value of the heading attribute must be NaN.
* The `speed` attribute denotes the magnitude of the horizontal component of the hosting device's current velocity and is specified in meters per second. If the implementation cannot provide speed information, the value of this attribute must be null.

From here, you can decide if you want to get a user's current location (one-time event), or watch a current position (specified time).

### Getting current location ###

There are many scenarios where you just want to get a user's location once – like, where is a user's closest movie theater or grocery store? If this is what you're after, then you can call the `getCurrentLocation` method from the `navigator.geolocation` object.

This method sends an async request to detect the user's position. When the position is determined, a callback function is executed. You can optionally provide a second callback function to be executed if an error occurs and also a third parameter as an `options` object. In the `options` object, you can set the following attributes:

* `enableHighAccuracy` – get the best possible result, even if it takes longer (default is false)
* `timeout` – timeout, in milliseconds, that the browser will wait for a response (the default is -1, meaning there is no timeout)
* `maximumAge` – specifies that a cached location is acceptable, so long as it isn't longer than the milliseconds that you specify (the default is 0, meaning a cached location is not used)

In the following example, I'll build a simple page that asks a user to click a button that will execute a function to find a user's location. The information will be displayed in the `location` div.

```html
<html>
    <head>
        <script src="js/current-location.js"></script>
    </head>
    <body>
        <p><button onclick="findMe()">Show my location</button></p>
        <div id="location"></div>
    </body>
</html>
```

The following example retrieves the `latitude`, `longitude`, and `accuracy` of the current user, given that they give permission to access their location. After getting this information, we'll display a Google Maps image of their location, easily accessible by using the Google Maps API, which takes `latitude` and `longitude` as parameters.

```javascript
function findMe() {
  var output = document.getElementById("location");

  function success(position) {
    var latitude  = position.coords.latitude;
    var longitude = position.coords.longitude;
    var accuracy = position.coords.accuracy;

    output.innerHTML = '<ul> \
                        <li>Latitude: ' + latitude + ' degrees</li> \
                        <li>Longitude: ' + longitude + ' degrees</li> \
                        <li>Accuracy: ' + accuracy + 'm</li> \
                        </ul>';

    var img = new Image();
    img.src = "https://maps.googleapis.com/maps/api/staticmap?center=" + latitude + "," + longitude + "&zoom=13&size=300x300&sensor=false";
    output.appendChild(img);
  };

  function error() {
    output.innerHTML = "Unable to retrieve your location!";
  };

  output.innerHTML = "Getting location ...";

  var options = {
      enableHighAccuracy: true,
      timeout: 3000,
      maximumAge: 20000
  };

  navigator.geolocation.getCurrentPosition(success, error, options);
}
```

### Monitoring current location ###

If you see a user's position changing frequently, like with turn-by-turn directions,  you can set up a callback function that you call with the updated position information. You can accomplish this by using the `watchPosition` function. This function has the same parameters as `getCurrentPosition`.

The `watchPosition` method returns an ID that can be used to identify who or what is watching the position. Then, when you wish to stop watching a user's location, you can use the `clearWatch` method which takes the ID as a parameter.

In this case, we have another simple page that has buttons to start and stop watching a user's position. The position information will display in the `message` div.

```html
<html>
    <head>
        <script src="js/jquery-3.1.0.min.js"></script>
        <script src="js/watch-position.js"></script>
    </head>
    <body>
        <div id="message"></div>
        <button id="startLocation">Start</button>
        <button id="stopLocation">Stop</button>
    </body>
</html>
```

In the JavaScript file, we'll start by initializing a `watchId`, using jQuery to initiate click events (more on that in a second), checking to see if the API is supported, and then writing a utility method that will show the information in the `message` div.

```javascript
var watchId = 0;

$(document).ready(function() {
    $('#startLocation').on('click', getLocation);
    $('#stopLocation').on('click', endWatch);
})

function supportsGeolocation() {
    return 'geolocation' in navigator;
}

function showMessage(message) {
    $('#message').html(message);
}
```

And now, here's where the magic happens: if a user's browser supports geolocation, we call `watchPosition`, which in this case take a success callback, an optional error callback, and an optional options parameter.

```javascript
function getLocation() {
    if (supportsGeolocation()) {
        var options = {
            enableHighAccuracy: true
        };
        watchId = navigator.geolocation.watchPosition(showPosition, showError, options);
    }
    else {
        showMessage("Geolocation is not supported by this browser.");
    }
}

function showPosition(position) {
    var datetime = new Date(position.timestamp).toLocaleString();
    showMessage("Latitude: " + position.coords.latitude + "<br />"
              + "Longitude: " + position.coords.longitude + "<br />"
              + "Timestamp: " + datetime);
}

function showError(error) {
    switch (error.code) {
        case error.PERMISSION_DENIED:
            showMessge("User denied Geolocation access request.");
            break;
        case error.POSITION_UNAVAILABLE:
            showMessage("Location information unavailable.");
            break;
        case error.TIMEOUT:
            showMessage("Get user location request timed out.");
            break;
        case error.UNKNOWN_ERROR:
            showMessage("An unknown error occurred.");
            break;
    }
}
```

Finally, when the user clicks the Stop button, the `clearWatch` method is called and the browser stops tracking the user's location.

```javascript
function endWatch() {
    if (watchId != 0) {
        navigator.geolocation.clearWatch(watchId);
        watchId = 0;
        showMessage("Monitoring complete.");
    }
}
```
