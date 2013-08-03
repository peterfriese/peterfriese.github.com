---
author: Peter
comments: true
date: 2010-09-06 13:15:17
layout: post
slug: why-your-next-app-should-be-open-sourced
title: Why Your Next App Should be Open Sourced
wordpress_id: 575
categories:
- Apple
- Eclipse
- iPhone
- OSS
tags:
- iphone
- license
- open source
- OSS
teaserimage: /images/2010-09-06-why-your-next-app-should-be-open-sourced/bottled_water_150x150.png
---

I've been doing all sorts of software development over the past few years, from closed-source in-house software for companies to closed-source product development to open-source frameworks and tools development to closed-source app development. 

Looking back on my experience with the various drawbacks and benefits of each of those development modes, I hereby recommend your next app be open sourced. 

<!--more-->

Here is why:

### Reasons pro Open Source
We all heard it before, "Open Source will make the world a better place" - but why? Well, let me give you a few reasons:

**First**, by open sourcing your code, your code will become better. If everybody can see what your code looks like, you will do your best to make sure it's clean code. Jeff LaMarche calls this "[coding as if everybody is watching](http://iphonedevelopment.blogspot.com/2010/06/code-as-if.html)". Clean code has less errors than quick-and-dirty code, so that's a good thing. 

**Enabling others to contribute** will further improve your code. Should you decide to invite other developers to your project, you'll want to add some documentation to make it easier for them to get a jump start. Writing documentation will force you to think through your code and its overall structure. You might find you want to perform some refactorings before you actually let other developers work on your code. This will improve the quality of your code even more.

**Enabling others to participate will make your app more attractive**. Didn't you experience this before: the app you just downloaded is almost perfect - if it only had this one missing feature. For example, I really love [TweetDeck for iPhone](http://www.tweetdeck.com/iphone/), but I'm dearly missing an [Instapaper](http://www.instapaper.com/) integration. If TweetDeck was open source, I would've added Instapaper integration to the code and submitted a patch to the creators of TweetDeck for the benefit of the entire user base. But unfortunately, TweetDeck is not open source - what a pity.

By **building a team of skilled people**, you'll be able to deliver more features in a shorter amount of time than you can ever hope to achieve if you work alone. Successful open source projects are made up of a bunch of gifted individuals with diverse skills. This is even more important for mobile applications, as you not only have to develop the code that makes your app work. You also have to create a great looking UI, so might need a great designer. If you want to promote your app on a website, you might also consider teaming up with a web designer.

**Speaking of contributions**, you might be a bit hesitant to let other people work on your code. That's alright. Usually, contributors do not get commit rights right away. Instead, you ask them to contribute to the project by submitting patches via your bug tracker (you do use a bug tracker, don't you?). This enables you to review their code before actually committing it to the code base. If you're not satisfied, let them know (in a friendly way) what you would like them to improve. If a contributor delivers a number of great patches in a row, you can consider to promote him/her to be a committer.

### Reasons against Open Source
There are a few reasons why your next app maybe should not be open sourced:

**Others might steal your ideas!** I keep hearing this argument over and over again. Yes, if you open source your app, other developers might check out the code, re-brand it and sell it on the app store as their invention. Well, they can steal your ideas anyway - just by looking at your app and building an app that looks and feels the same. Granted, re-writing an app consumes considerably more time than re-branding an existing code base. But I doubt anybody dares to submit a blatant 1:1 copy of an app to the app store. At the very least, you should choose a suitable license for your code. There are a number of great open source licenses you might consider check out the [OSI website for a list of open source licenses](http://www.opensource.org/licenses). [Rob Rhyne](http://blog.robrhyne.com/) even had his [brother](http://jdrhyne.tumblr.com/) (who is a lawyer) create a [new app-store compatible license](http://github.com/capttaco/Briefs/blob/master/LICENSE) for his app, [Briefs](http://giveabrief.com/). Read his blog post "[Selling Open Source](http://blog.robrhyne.com/post/1043407467/selling-open-source)" for the rationale behind this step.

Even if other people don't copy your entire app, **they still might copy some cool UI tricks** you do. Maybe you've gone to great lengths to create some really cool frameworks that make your life easier or make your app behave in a very cool and new way. You might not want other apps to look as cool as your app does. Well, it's your right. In my opinion, the app store contains way too many badly designed apps. You would do the world a favour by releasing your great library to the public. Really.

And finally, you might not be able to open source your next app because your client or the **company you're working for is against open source**. Maybe they have good reasons for it, maybe they just don't know enough about open source. If the latter is the case, let them read this blog post or drop me a line - I'm available for consulting.

### Business Models for Open Source Apps
You might have a different point of view (and if you do, please leave a comment - I'm eager to hear your thoughts), but in my opinion open source is not a threat but a chance for your app. If we take this for granted, the question remains, how can you make money with an open sourced app? Here are some suggestions:

**Sell it on the App Store**. Yes, this might sound a little strange after all my raving about open source. If something is open source, how can you sell it? Well, it turns out selling apps on the App Store is a great idea, especially for open source apps. The app store is the only way how your clients can get hold of your application. Of course - they might check out your code, compile it and upload it to their iPhone. But to do so, they'd need to be registered iPhone developers, meaning they'd need to buy an iPhone developer certificate. Don't you think it is cheaper to just buy your app than shelling out 99 USD for the developer certificate?

If you're developing a library or framework, you might consider getting **funding on [Kickstarter](http://www.kickstarter.com/)**. Kickstarter is a great way to get funding for your project: you define the deliverable and how much funds you want to raise and people can back your project by pledging a variable amount of money. This way, [John Wain](http://www.penandthink.com/) of [Glyphish](http://glyphish.com/) fame [managed to raise more than 27.000 USD](http://www.kickstarter.com/projects/jpwain/great-icons-for-iphone-4-apps)  for developing an iPhone 4 compatible version of his great Glyphish icon set. His original goal was to raise 2.000 USD, by the way.

I hope I could encourage you to try open source as a strategy for your next application or framework. If I did, let me know! Chances are I might want to submit a patch to add that tiny little feature I think your app is lacking ;-)