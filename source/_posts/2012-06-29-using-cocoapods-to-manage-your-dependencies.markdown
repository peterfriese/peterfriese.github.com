---
author: Peter
comments: true
date: 2012-06-29 22:42:22
layout: post
slug: using-cocoapods-to-manage-your-dependencies
title: Using CocoaPods to manage your dependencies
wordpress_id: 913
categories:
- iOS
- Tools
- CocoaPods
- Dependency Management
asins:
- "1481812785" # iOS 6 By Tutorials: Volume 1 (Wenderlich)
- "1482092409" # iOS 5 by Tutorials: Volume 1 (2nd Edition) (Wenderlich)
teaserimage: /images/2012-06-29-using-cocoapods-to-manage-your-dependencies/cocoabean_150x150.png

---

One of the few things that I find really annoying about developing apps for the iOS platform is how cumbersome it is to include third party libraries in your projects. Depending on the complexity of the library (e.g. its respective dependencies and transitive dependencies) and the effort the authors put into the consumability of their library, the steps required to add a library to your project range from just copying a few files into your source folder to a whopping two-page description of drag'n'drop wizardry.

<!--more-->

Wouldn't it be awesome if you could just specify a list of dependencies and be done? You might have heard of RubyGems, Maven, Ivy, NuGet or npm that bring dependency management to the worlds of Ruby, Java, .NET and JavaScript developers.

Well, it turns out there finally is a solution for us Cocoa developers - CocoaPods. Getting started with CocoaPods is really easy and once you started using it, you won't look back again.

## Installation

CocoaPods itself is distributed as a Ruby gem, so installing it is as simple as:

``` bash
[sudo] gem install cocoapods
pod setup
```

## Usage

Using CocoaPods to manage your project's dependencies is a two-step process:

1. Specify the dependency in you project's dependency specification file, `Podfile`
2. Run `pod install`

Let me illustrate this by way of a simple example. Let's assume you want to build an application with the awesome [RestKit](http://www.restkit.org) library. If you have used RestKit before, you know that adding it to your project is a rather involved process. Over the years, it has become significantly easier, but it still needs a very detailed installation instruction with lots of screenshots - check out their [wiki page](https://github.com/RestKit/RestKit/wiki/Installing-RestKit-in-Xcode-4.x).

With CocoaPods, all you need to do is this:

* Create a new empty project, naming it `RestEasyWithCocoaPods`
* In your project folder, create a new file, `Podfile`
* Fill in the following information:

``` ruby Podfile
	platform :ios
	dependency 'RestKit'
	dependency 'RestKit/UI'
	dependency 'RestKit/Network'
	dependency 'RestKit/ObjectMapping'
	dependency 'RestKit/ObjectMapping/CoreData'
	dependency 'RestKit/ObjectMapping/XML'
	dependency 'RestKit/ObjectMapping/JSON'
```

* Run `pod install RestEasyWithCocoaPods.xcodeproj`
* Open the newly created workspace, `RestEasyWithCocoaPods.xcworkspace`
* Add the RestKit headers to `RestEasyWithCocoaPods-Prefix.pch`:

``` objc RestEasyWithCocoaPods-Prefix.pch
	#ifdef __OBJC__
	#import <UIKit/UIKit.h>
	#import <Foundation/Foundation.h>
	#import <RestKit/RestKit.h>
	#endif
```

* Done. Go and build awesome stuff with RestKit (more on this in another post).

## Where to go from here

Wasn't that a lot easier than all the other ways to install third party libraries?

If you are curious which libraries are supported by CocoaPods, go to [CocoaPods.org](http://cocoapods.org) - they have a nice little UI for searching their pod specification repository. If you are more include to using the command line, you are welcome to use `pod search <yoursearchterm>` to search for a specific library.

If your preferred library is not yet supported, why not help them out and build a pod spec? [CocoaPods](https://github.com/CocoaPods/CocoaPods) itself and the [pod spec repository](https://github.com/CocoaPods/Specs) are hosted on Github, so adding new pod specifications is really easy. The CocoaPods wiki has a section on [creating](https://github.com/CocoaPods/CocoaPods/wiki/The-podspec-format) and [publishing](https://github.com/CocoaPods/CocoaPods/wiki/Sharing-pod-specifications) new podspec files.

Now go and create awesome stuff!