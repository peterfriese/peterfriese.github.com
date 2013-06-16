---
author: Peter
comments: true
date: 2010-11-25 12:00:43
layout: post
slug: how-to-use-the-gyroscope-of-your-iphone-in-a-mobile-web-app
title: How to Use the Gyroscope of Your iPhone in a Mobile Web App
wordpress_id: 633
categories:
- Eclipse
- HTML5
- iPhone
tags:
- accelerometer
- gyrosope
- html5
- iphone
- safari
teaserimage: /images/2010-11-25-how-to-use-the-gyroscope-of-your-iphone-in-a-mobile-web-app/gyroscope_150x150.png
---

This week's release of iOS 4.2 for iPad and iPhone comes with some nice little features most people will not immediately become aware of as they're neither directly visible in the iOS UI nor are they mentioned in Apple's official release notes. You have to dig a little deeper to find them. One of them is a JavaScript API for the iPhone's gyroscope. Read on to see it in action and learn how to use it.

<!-- more -->

Your iPhone has a number of sensors, some of which are rather essential for the phone's operation (such as the microphone). While the accelerometer and the gyroscope might not be the most essential sensors for a phone, they're certainly the most exciting ones. While accelerometer measures the acceleration you induce on the phone, the gyroscope gives a rather precise feedback on the orientation of the phone.

Until now, web developers didn't have access to the accelerometer sensor and the gyroscope sensor. With this week's release of iOS 4.2, this has changed and we can now use [DeviceMotionEvent](https://developer.apple.com/library/safari/#documentation/SafariDOMAdditions/Reference/DeviceMotionEventClassRef/DeviceMotionEvent/DeviceMotionEvent.html) and [DeviceOrientationEvent](https://developer.apple.com/library/safari/#documentation/SafariDOMAdditions/Reference/DeviceOrientationEventClassRef/DeviceOrientationEvent/DeviceOrientationEvent.html) to determine the acceleration and orientation data of the phone.

Let's first determine whether the current browser supports device orientation sensing:
    
``` javascript
if (window.DeviceMotionEvent==undefined) {
}
```

We can then read the sensor data by registering the respective callbacks. Here's how you read the accelerometer's data:
    
``` javascript
window.ondevicemotion = function(event) {
  ax = event.accelerationIncludingGravity.x
  ay = event.accelerationIncludingGravity.y
  az = event.accelerationIncludingGravity.z
  rotation = event.rotationRate;
  if (rotation != null) {
    arAlpha = Math.round(rotation.alpha);
    arBeta = Math.round(rotation.beta);
    arGamma = Math.round(rotation.gamma);
  }
}
```

The gyroscope's data can be read like this:
   
``` javascript    
window.ondeviceorientation = function(event) {
  alpha = Math.round(event.alpha);
  beta = Math.round(event.beta);
  gamma = Math.round(event.gamma);
}
```
    
I've put together a little demo that uses the sensor data to color some boxes on the phone's screen. Here's a short video showing it in action:

{% vimeo 17182364 %}

If you want to take it for a spin, open this URL in mobile safari on your phone: [http://demos.peterfriese.de/gyro/gyro.html](http://demos.peterfriese.de/gyro/gyro.html).

(image of Gyroscope by stop that pigeon! taken from http://www.flickr.com/photos/25312309@N05/2651042796/)


