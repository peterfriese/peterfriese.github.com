---
author: Peter
comments: true
date: 2010-09-19 20:15:30
layout: post
slug: apples-updated-developer-license-this-changes-everything-again
title: Apple’s Updated Developer License – This Changes Everything. Again.
wordpress_id: 585
categories:
- Apple
- DSLs
- Eclipse
- iPhone
- Xtext
tags:
- '331'
- apple
- iphone
- section331
teaserimage: /images/2010-09-19-apples-updated-developer-license-this-changes-everything-again/code_small_150x150.png
---

A few days ago, Apple made some small, but very important changes to the iOS Developer Program Agreement - a document which you must agree to before you can download the iOS SDK and start developing software for the iOS platform. These changes will drastically change the way we will build software for the iPhone, iPad and iPod and any other device that runs iOS.

<!-- more -->

In April 2010, together with the release of iOS 4, Apple changed the terms of the iOS Developer Program License in a way which basically prohibited creating software for iOS using languages other than Objective-C, C, C++ or JavaScript:

> 3.3.1 — Applications may only use Documented APIs in the manner prescribed by Apple and must not use or call any private APIs. Applications must be originally written in Objective-C, C, C++, or JavaScript as executed by the iPhone OS WebKit engine, and only code written in C, C++, and Objective-C may compile and directly link against the Documented APIs (e.g., Applications that link to Documented APIs through an intermediary translation or compatibility layer or tool are prohibited).

Back then, pretty much everyone was sure this update to section 3.3.1 of the license really was just one strike in [Apple's crusade against Flash](http://www.apple.com/hotnews/thoughts-on-flash/). Adobe had been working on a tool called [Flash Packager](http://labs.adobe.com/technologies/packagerforiphone/), which allows Flash designers to cross-compile their applications for the iPhone. While this must have been bad news for Adobe (they effectively needed to write off development costs for the entire development team working on Flash Packager), it posed a serious threat to companies whose business model relied upon creating iPhone apps using tools and languages other than Xcode and Objective-C. The future for Novell's [Mono Touch](http://monotouch.net/), [Appcelerator Titanium](http://www.appcelerator.com/), [PhoneGap](http://www.phonegap.com/) and [XMLVM/iPhone](http://www.xmlvm.org/iphone/) didn't look very bright.

No need to say there was quite an uproar and people came up with all sorts of creative interpretations of the terms stated in section 3.3.1 to avoid being rejected from the App Store.

Of course, if a monopolist starts endangering other, dependent companies, this soon will call the Federal Trade Commission to action. It doesn't come that much of a surprise Apple published a new, very relaxed version of the iOS Developer Program Agreement just a few days ago. The new version (which is publicly available at [http://developer.apple.com/programs/terms/ios/standard/ios_standard_agreement_20100909.pdf][1]) does not limit the programming languages you may use to create applications for the iOS platform any more. In particular, section 3.3.1 now reads:

  [1]: http://developer.apple.com/programs/terms/ios/standard/ios_standard_agreement_20100909.pdf


> 3.3.1	Applications may only use Documented APIs in the manner prescribed by Apple and must not use or call any private APIs.

You might think that a small paragraph in a license agreement doesn't mean that much, but I think the new section will fundamentally change the way we write software for the iPhone in the next few years. Let me explain:

The new section 3.3.1 means you may write iOS software using any language you like. Yes, ANY language - there is no if and when. This means you will not only be able to write iOS software in Objective-C (which is a great language as soon as you come to grips with it), but you will be able to use language like Java, Scala, Haskell, Ruby, etc. Some people are [even dreaming of using REALBasic](http://twitter.com/mattgemmell/status/24014145904) for iOS apps.

Vendors of tools such as Mono Touch, Titanium and PhoneGap will be glad, as their business models now have a solid foundation (well, Apple may of course change the license again but I doubt they'll ever restrict the usage of other languages again). Given the [announcement on Adobe Labs](http://labs.adobe.com/technologies/packagerforiphone/), I guess we will very soon see Adobe release their highly acclaimed Flash Packager for iPhone (I don't think people will seriously use this tool to create data-driven applications, but it's probably a great tool to develop games. If you're a Flash designer / developer, that is).

For the development community at large, this is great news: freedom to choose the language that fits your needs best has always been a cornerstone of successfully creating great software. People have expressed their desire to use languages such as Haskell and Ruby to build iPhone apps. Rumor has it Apple engineers have even been working on a version of MacRuby for iPhone - there you go.

At [itemis](http://www.itemis.com), we're very much into programming languages. We're even building our own language development toolkit to support development of programming languages - [Xtext](http://www.xtext.org). With this toolkit, it is very easy to create your own domain specific programming languages (DSLs). In the past, we've mainly been using it to create languages for enterprise and web applications. We're also helping others to (re-)build languages such as SQL. Of course, Xtext is being used to build itself - isn't that nice?  About one year ago, we started implementing a programming language tailored towards creating data-driven mobile applications for the iOS platform. We're not aiming at creating a language you can use to build arbitrary iOS applications. Instead, we're focussing on data-driven applications with a drill-down metaphor. Something you can find in many applications such as FaceBook, LinkedIn, Kayak and the like. Watch the following video to see it in action:

{% vimeo 15018235 %}

If you're interested in learning more about APPlause, get in touch with us by mail [heiko.behrens|peter.friese]@itemis.de, subscribe to [our](http://www.heikobehrens.net) [blogs](http://www.peterfriese.de) and follow us on Twitter ([@peterfriese](http://www.twitter.com/peterfriese), [@HBehrens](http://www.twitter.com/HBehrens), [@APPlauseDSL](http://www.twitter.com/applausedsl)).

In a few years, I guess, Objective-C will be just one language among many others you can choose from if you want to build iOS applications.

But why would you want to write iOS applications in languages other than Objective-C? There are many reasons:

* Use the language you feel most comfortable with. Maybe Objective-C's square brackets look scary to you?
* Use the language you have most experience with. Maybe you're a company and have a tight schedule. Writing your application using the language your developers have the most skills in helps you to meet your deadline. In fact, a friend just recently told me they used Appcelerator "because we've got JavaScript and HTML knowledge in-house, but none of our developers had experience with Objective-C"!
* use the language you use for developing your backend system. So you've got this huge backend system written C#, running on the .NT platform. Why not write all the front-end code in C# as well?
* Maybe you're a start-up in the social media realm and your website is written all in Ruby and Ruby on Rails. So why not write the iPhone app in Ruby as well?


It all boils down to being more productive and ensuring better sustainability. Using the right tool for the job helps you to achieve a faster time to market. If you're able to use the same language on the frontend and the backend, this will help you to secure your investment in this technology.

Please note that I'm not saying you should by all means write your frontend and your backend using the same technology and language. The first and foremost question you need to answer is, "what's the right tool for this job". Being able to make a real choice will help you to give a better answer to this question.
