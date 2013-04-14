---
author: Peter
comments: true
date: 2009-04-02 15:21:50
layout: post
slug: how-twitter-solved-my-p2-problems-during-my-lunch-break
title: How Twitter solved my P2 problems during my lunch break
wordpress_id: 224
categories:
- Eclipse
- Eclipse FAQ
- Stuff that rocks
tags:
- english
---

Today, I wanted to migrate from Eclipse 3.5M5 to Eclipse 3.6M6 (I know, it's a bit late for that already - but then, I had been busy with [#eclipsecon](http://www.eclipsecon.org/) and preparing the [Stupid Modeling Talk](http://www.eclipsecon.org/2009/sessions?id=358) during the past weeks).

As of course I've got quite a number of additional bundles installed (e.g., EMF, Window Builder, Mylyn, Xpand and Xtend), I wanted to re-use them. I recalled that a while back someone (probably Chris?) blogged that it basically is possible to use an existing Eclipse installation as a local update site. However, I couldn't find the blog posting describing how to do that.

Given the fact it was about lunch time, I decided to let the #wisecrowd figure out how to solve my problem. So I twittered "[desperately looking for a hint how to use an existing Eclipse install as a P2 repo. Goal:transport features from my M5 install to a fresh M6](http://twitter.com/peterfriese/statuses/1437609552)". 

So, during my lunch break, the wise crowd ([Kai](http://twitter.com/kaitoedter), [Ekke](http://twitter.com/ekkescorner), [Boris](http://twitter.com/bokowski) and [Paul](http://twitter.com/paulweb515)) sent me their opinions on the matter:

[

![How Twitter solved my P2 problem during my lunch break](http://farm4.static.flickr.com/3650/3407041702_4b788ff887.jpg)

](http://www.flickr.com/photos/81029262@N00/3407041702)

At first, it seemed like there is no solution, but in the end Paul's [tweet](http://twitter.com/paulweb515/status/1437925330) reveals the solution I was looking for: 

Add eclipse/p2/org.eclipse.equinox.p2.engine/profileRegistry/SDKProfile.profile/ as a local update site.

All of you out there who still doubt the practical use of Twitter: you really should give it another try.
