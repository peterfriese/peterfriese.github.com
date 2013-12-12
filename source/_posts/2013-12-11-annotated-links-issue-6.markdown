---
layout: post
title: "Annotated Links, Issue 6"
date: 2013-12-11 12:24
comments: true
categories: 
- Links
- Programming
- JavaScript
- Node
- iOS
- Google
- Chrome
- Open Source
- Mobile
teaserimage: /images/2013-08-10-annotated-links-for-week-32/chains.jpg
---

After a few weeks of silence (and on popular demand by a [single person](http://heikobehrens.net)), I decided to pick up publishing my annotated links again. This time, we'll look at wiretapping APIs, donating code to your favourite projects on Github, Node.js in your iOS apps, and a racing game that renders its track across multiple devices.

<!-- more -->

### [Runscope](http://www.runscope.com)

Runscope is an online tool for inspecting, debugging and testing API traffic. The traffic inspector allows you to easily set up a "wire tap" for your app, basically allowing you to monitor and record any traffic between your app and any API that you're using. In addition to recording traffic for inspection, you can also compare responses of two calls and find out where the differences are - very handy! You can even share individual recorded requests with your co-workers, e.g. to discuss a problem with your back-end team.

Runscope also provides a tool for automated testing of your API, dubbed _Runscope Radar_. This tool allows you to set up a sequence of API calls, along with the expected outcome for each of the calls. Radar provides an easy-to-use UI for setting up simple, yet powerful assertions that allow you to specify how the header and body of the response should look like. Variables help you to keep things flexible. Runscope Radar supports JSON and XML, so it is easy to build expressions like `[2].number` to navigate JSON and XML responses and specify expectations. You can then schedule your tests to run at given intervals and be notified via e-mail in case a test fails. This allows you to easily set up intelligent tests that monitor the validity of your API.

[Wiretap](http://httpkit.com/wiretap) by [Kris Jordan](https://twitter.com/KrisJordan) looks very promising as well. It acts as a reverse proxy, allowing you to record HTTP traffic between your app and an API very easily. In fact, setting up a wiretap seems to be easier than setting up Runscope. Unfortunately, still is in beta, but Kris has set up a video to give you a goood understanding of how it works. Wiretap and Runscope look strikingly similar, but I have the impression Wiretap has been around for longer...

### [24 Pull Requests Until Christmas](http://24pullrequests.com)

Christmas is upon us - and this is your chance to give back some love to some of the many open source projects out there you might be using in your own work. The idea is to submit one pull request each day for any project on Github. A number of projects are already listed on the [projects](http://24pullrequests.com/projects) page, which allows you to filter projects by programming language to find a few that you might be interested in. Your favourite project isn't listed? No problem at all - just [submit a suggestion](http://24pullrequests.com/projects/new) and fork away!

I found it very interesting to see [Xtend](http://www.eclipse.org/xtend/) is officially listed as a programming language on Github - great job, [guys](https://twitter.com/xtendlang)!

### [Node.js in your iOS App](http://nodeapp.org)

If you wanted to use JavaScript in your iOS apps prior to iOS 7, you basically had two options: use an embedded `UIWebView` and invoke `-stringByEvaluatingJavaScriptFromString:`, or recompile [JavaScriptCore for iOS](https://github.com/phoboslab/JavaScriptCore-iOS).

Starting with iOS 7, Apple [opened access to JavaScriptCore](https://developer.apple.com/library/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS7.html) (I really like _"For information about the classes of this framework, see the framework header files."_), basically enabling iOS developers to use JavaScript to script their applications.

The NodeApp interpreter goes one step further and allows developers to re-use existing Node modules. Even if you might not be interested in using any of the [50.000+ module listed on npm.org](https://npmjs.org), this is good news as it allows you to re-use the Node module system, thereby enabling you to structure your JavaScript apps in a more concise way.

Check out the [NodeApp interpreter app](https://github.com/node-app/Interpreter) and impress your coworkers with a real JavaScript interpreter running on your iPhone!


### [Racer - A multiplayer game for your mobile browser](http://g.co/racer)

Racer is a Chrome Experiment that shows off various web technologies, like [WebSockets](http://www.html5rocks.com/en/tutorials/websockets/basics/), the [Web Audio API](http://www.html5rocks.com/en/tutorials/webaudio/intro/), CSS Animations and [CSS Sprites](http://css-tricks.com/css-sprites/).

Watch the [making of](http://www.youtube.com/watch?v=17P67Uz0kcw), read the back story of how it was made on [HTML 5 rocks](http://master.html5rocks-hrd.appspot.com/en/tutorials/casestudies/racer/). Then, go find some friends with mobile devices and [play Racer online](http://g.co/racer). The [soundtrack](http://www.youtube.com/watch?v=YT0k99hCY5I) has been composed by [Giorgio Moroder](https://myspace.com/giorgiomoroderpage), who composed a number of well-known songs for events like Olympia 1984 and 1988 or the 1990 Football World Cup. Some of his songs were used in racing games before, but the soundtrack for Racer is the first one to be composed specifically for the purpose of being used in a racing game.

I hope you enjoyed this week's links - let me know if you would like me to continue compiling them by sending me a mail or - better yet - using the tweet button below to let your friends know about this post!

Thanks for reading!