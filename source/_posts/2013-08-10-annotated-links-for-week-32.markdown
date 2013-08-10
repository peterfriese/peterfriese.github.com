---
layout: post
title: "Annotated Links for Week 32"
date: 2013-08-10 01:33
comments: true
categories: 
- Links
- Programming
teaserimage: /images/2013-08-10-annotated-links-for-week-32/chains.jpg

---

This week, we'll have a look at Apple's new system status page and another, more serious issue in a rather well-known computer-driven system. Other topics in this post: a very high-level view on programming (and why we failed so far) as well as a very detailed look at programming (and what you can do to excel at it). Finally, some spying.

<!-- more -->

### [Apple System Status Page](https://developer.apple.com/support/system-status/)
As an iOS developer, you rely on a surprisingly high number of Apple systems. Until recently, there wasn't a way to find out if a system was offline or if it was just you, "[holding it wrong](http://www.engadget.com/2010/06/24/apple-responds-over-iphone-4-reception-issues-youre-holding-th/)". As an immediate response to the ([alleged](http://www.theguardian.com/technology/2013/jul/26/apple-developer-site-hack)) hack of their developer support systems, Apple shut down pretty much all of their developer support systems and set up this system status page. As of now, all systems are back to normal, but this page might come in handy for future outages.

### [The Explosion of Ariane 5](http://www.ima.umn.edu/~arnold/disasters/ariane.html)
Due to popular belief, Ariane 5 exploded because a programmer mixed up a comma with a period, but it's not that easy**,**

In fact, the problem was someone used a 16 bit unsigned integer variable to hold the result of converting a 64 bit floating point value. I guess this is strong point for using type safe languages in safety-critical systems as well as clearly stating the pre- and post conditions of library functions. Some unit tests might also be in good order.

### [The Future of Programming](http://worrydream.com/dbx/)
A talk Bret Victor gave at the DropBox conference in ~~1973~~ 2013 on how he thinks the future of programming could have looked like if developers had been more courageous in embracing new ideas instead. Instead, we often see people rejecting new ideas, stating "that's not real programming". Thankfully, every now and then new ideas are strong enough to gain enough traction. Otherwise, we'd still be writing code in binary. Recently, we've seen a growing interest in programming languages and programming paradigms other than the well-known object-oriented / procedural approaches. I think this is a good sign, however, we need to be more open minded and think more about other, new ways of programming. If you haven't done so in this year, you should set aside some time to learn a new programming language. Why not try [Xtend](http://www.eclipse.org/xtend/)?

### [The NYTimes Objective-C Styleguide](http://open.blogs.nytimes.com/2013/08/01/objectively-stylish/?_r=0)
If you develop software for a living, you know that following a coding style guide makes sense. Sometimes, people do get religious about how to format their code - try to steer clear of this by using a widely-regarded style guide. The NYTimes Objective-C Styleguide is well-written, has the right amount of concise examples and can be [forked](https://github.com/NYTimes/objective-c-style-guide) in case you feel you need to adjust something to your liking. The fact that the New York Times mobile team [welcomes contributions](https://github.com/NYTimes/objective-c-style-guide/graphs/contributors) will probably make this styleguide one of the more popular ones.

### [SelfSpy](https://github.com/ttscoff/selfspy)
Whether you just want to [quantify yourself](http://en.wikipedia.org/wiki/Quantified_Self), something which [Stephen Wolfram seems to be doing to quite some extent](http://blog.stephenwolfram.com/2012/03/the-personal-analytics-of-my-life/) or you're curious what secret services might know about you if they installed a keylogger on your system: SelfSpy is a good tool to get started: it runs on Linux, Windows and Mac OSX. Besides, it's open source - so you can see what it does and you can extend it if needed.

[Chains image](http://www.flickr.com/photos/h20tubig/9376203024/) by [Haya Bernitez](http://www.flickr.com/photos/h20tubig/)