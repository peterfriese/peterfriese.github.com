---
author: Peter
comments: true
date: 2009-06-12 16:57:46
layout: post
slug: noisy-dsls
title: Noisy DSLs
wordpress_id: 272
categories:
- Eclipse
---

I recently came across some DSLs which had some defects. Let's look at a sample I quickly hacked together with [Xtext](http://www.xtext.org):[


![Resource - Demo/demo.mydsl - Eclipse SDK](http://farm4.static.flickr.com/3379/3618888297_f3a946f641.jpg)


](http://www.flickr.com/photos/81029262@N00/3618888297)

Here are two recommendations I'd like to give when designing a textual DSL:



	
  1. **Don't use uppercase** letters in the keywords of your DSL, unless you either really like them. In the sample DSL, the keywords (_Element_, _EndElement_, _Child_, _Endchild_ all start with an uppercase letter, which makes it cumbersome to edit the DSL script (you need to press the shift key a lot). Back in the days when we didn't have syntax highlighting, UPPERCASE keywords made a lot of sense as they helped to draw a better distinction between the keywords, the variables and the constants (strings and numbers) of a program. Nowadays, it is hard to find an IDE which does not offer syntax highlighting, so this argument does not hold any more.

	
  2. **Do not use noisy _begin_..._end_ syntax constructions.** They do not increase readability of your DSL script and they add to the amount of code to be typed.


Here is an improved version of the DSL script:  [


![Resource - Demo/demo.mydsl1 - Eclipse SDK](http://farm4.static.flickr.com/3647/3618914029_fdbe7d064c.jpg)


](http://www.flickr.com/photos/81029262@N00/3618914029)

By removing the _Endelement_ and _Endchild_ keywords, we have improved readability a lot. Using lowercase keywords has lowered the effort you need to invest to key in the DSL script. And - honestly - don't we all like to be lazy at times? After all, one of the goals of DSLs is to make your work easier and more efficient.  

That being said, have a great weekend!  

By the way, if you need help designing a DSL with [Xtext](http://www.xtext.org) - I and the entire Xtext team are happy to assist you. Just drop me a line (peter at itemis dot com) or hop on to [our professional support site](http://xtext.itemis.com/xtext/language=en/23946/professional-services)
