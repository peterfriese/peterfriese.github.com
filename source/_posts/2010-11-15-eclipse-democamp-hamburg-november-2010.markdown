---
author: Peter
comments: true
date: 2010-11-15 07:28:17
layout: post
slug: eclipse-democamp-hamburg-november-2010
title: Eclipse DemoCamp Hamburg November 2010
wordpress_id: 624
categories:
- Conferences
- Eclipse
teaserimage: /images/2010-11-15-eclipse-democamp-hamburg-november-2010/buchhandlung-hamburg2-2_1_150x150.png
---

Last Friday was a happy day for Java developers in Hamburg: not only did Apple and Oracle announce their plan to continue development of Java on the Mac OSX platform, but we also had the pleasure to host this fall's instance of Eclipse DemoCamp Hamburg in a bookstore!
<!-- more -->

We had a nice lineup of four speakers - unfortunately not all of them were able to attend due to a cold, but we were lucky enough to find one additional speaker - thanks Jan!

After a casual meet-and-greet with [Fritz Kola](http://www.fritz-kola.de/) and Pretzels, [Dalibor Topic](http://twitter.com/#!/robilad) opened the official part of the evening with an overview of what's coming up in JDK7 and JDK8. Here's a list of things that sound quite interesting:
	
  * Support for dynamically-typed languages (InvokeDynamic) ([project page](http://openjdk.java.net/projects/mlvm))
  * Small language enhancements (Project Coin) ([project page](http://openjdk.java.net/projects/coin/))
  * An even newer NIO ([project page](http://openjdk.java.net/projects/nio/))

The good news of the day of course was a [press release announcing OpenJDK for Mac OSX](http://blogs.oracle.com/henrik/2010/11/oracle_and_apple_announce_openjdk_project_for_osx.html). Nevertheless, [Dalibor demoed a freshly built OpenJDK 7 BSD port](http://twitter.com/#!/robilad/status/3129373515321344) on his MacBook.

{% fancyimage right /images/2010-11-15-eclipse-democamp-hamburg-november-2010/democamp01.png 300x224 Dalibor Topic%}

After a short break, it was [Ralf Sternberg's turn](http://eclipsesource.com/blogs/author/rsternberg/) to give an overview of [RAP](http://www.eclipse.org/rap/) and explain what Single Sourcing is. With the help of RAP, it is rather easy to bring Eclipse RCP applications to the web. Of course, you'll have to pay attention to a few things, as Ralf pointed out: usually, RCP applications are single-user apps, whereas web applications are inherently multi-user enabled. Fortunately, RAP comes with a few utilities that cater for this fact.

One thing I was delighted to learn: not only do RAP applications run on the iPad, but also do they support drawing using the SWT API, as you can see in the following video:

{% vimeo 16829527 %}

In the last sessions, [Xtext](http://www.eclipse.org/Xtext/) committer [Jan Köhnlein](http://koehnlein.blogspot.com/) showed us some of the things coming up in Xtext 2.0. Being text-addicted, Jan refrained from using slides and used the IDE instead to deliver his talk:

{% fancyimage right /images/2010-11-15-eclipse-democamp-hamburg-november-2010/democamp02.png 300x224 Jan Köhnlein talking about Xbase and Xtext%}

Jan showed us how Xbase (no, not [xBase](http://en.wikipedia.org/wiki/Xbase)) can be used to develop DSLs (and even GPLs) that not only describe structural features but also behavior. He also demoed Xdoc, a newly invented documentation language, and the fancy new Xtext syntax view (featuring [railroad diagrams](http://en.wikipedia.org/wiki/Syntax_diagram)).

After the demos, we had some time for chats and impromptu hack sessions. Given the event took place in a book store (many thanks to Lehmanns for having us!), you could also browse and buy books.

Thanks to the fact we only had about 40 attendees, there were plenty of chances for networking which people really seemed to enjoy.

I'd like to thank everybody who attended the DemoCamp or gave a demo! [Follow me on twitter](http://twitter.com/#!/peterfriese) to be informed ahead of time for the next DemoCamp!
