---
layout: post
title: "Annotated Links for Week 33"
date: 2013-08-19 09:30
comments: true
categories: 
- Links
- Programming
teaserimage: /images/2013-08-10-annotated-links-for-week-32/chains.jpg

---

This week's annotated links are more coding- and code-related than last week. We'll have a look at some data mapping frameworks for Objective-C, a way to set up your projects in a faster, more reproducible way and - as iOS 7 is around the corner, a feature comparison of iOS 7 on the various devices out there.

<!-- more -->

### [iOS 7 Compatibility Chart](http://www.cultofmac.com/231632/the-ultimate-ios-7-compatibility-chart/)

With the release of iOS 7 just around the corner, you might be wondering which devices you should test your apps on in order to make sure you cover all required combinations of services that might or might not be available to your app (yes, iOS 7 is one of those releases that won't make all features available on all devices). Cult of Mac has a nice overview chart, worth checking out.

### [Crafter - Easily Setup Xcode Projects](http://www.merowing.info/2013/05/crafter-setup-your-cocoa-projects-with-ease/)
Github has made it really easy to check out new libraries or demos: just clone the repo and off you go. But what about creating new projects? If you find yourself constantly creating new projects, adjusting the build configuration to suit your needs, adding a Podfile for [CocoaPods](http://www.cocoapods.org), then [Crafter](https://github.com/krzysztofzablocki/crafter) might be for you. You can configure it with your defaults, while at the same time staying flexible by adding options. Definitely worth a try.

### [Object](https://github.com/krzysztofzablocki/KZPropertyMapper) [Mapping](https://github.com/uacaps/NSObject-ObjectMap) [in](https://github.com/dchohfi/KeyValueObjectMapping) [Objective-C](https://github.com/mattiaslevin/ObjectMapper)
Most apps require you to map data from a text-based presentation (mostly JSON or XML) to  Objective-C objects. Chances are you've written mapping code by hand in at least one of your projects. "Its's just one or two classes, how hard can it be?", was probably what you thought when you set out to implement you home-grown mapping. YOu might have ended up with either pulling your hair out while maintianing your code whenever the API team decided to implement some "minor changes" or using a full-blown framework like [RestKit](http://www.restkit.org) that gave you more than you needed. If you just need to map data without the need for all the fancy add-ons like automatic CoreData persistence, in-memory and persistent caching and so on, you should have a look at the following frameworks. The all follow different approaches, some apply automagic mapping by matching field names, others by using an internal DSL to help you express your mapping. Take a look yourself and decide based on what you need for your current project.

* [KZPropertyMapper](https://github.com/krzysztofzablocki/KZPropertyMapper) - uses an internal DSL to express data mappings
* [NSObject-ObjectMap](https://github.com/uacaps/NSObject-ObjectMap) - uses introspection to map properties directly unto properties. 
* [KeyValueObjectMapping](https://github.com/dchohfi/KeyValueObjectMapping) - Uses a combination of both introspection and configuration to map data unto objects,
* [ObjectMapper](https://github.com/mattiaslevin/ObjectMapper) - Uses introspection and a callback method to map data.

### [And finally... Instacodes](http://instacod.es)
Did you ever wonder how to create those geeky looking source code screenshots that look like you used a camera to take a picture of your screen from an angle? Well, here's a tool that let's you create those screenshots without the need to take out your camera: [Instacodes](http://instacod.es)

{% fancyalbum 600x250 %}
/images/2013-08-17-annotated-links-for-week-33/instacodes.png: Istancodes sample
{% endfancyalbum %}

That's it for this week, I hope you liked it. Feel free to send suggestions for next week!

[Chains image](http://www.flickr.com/photos/h20tubig/9376203024/) by [Haya Bernitez](http://www.flickr.com/photos/h20tubig/)