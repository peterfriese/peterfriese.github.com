---
author: Peter
comments: false
date: 2005-08-14 21:24:24
layout: post
slug: podcasting-with-a-palm-device
title: Podcasting with a Palm Device
wordpress_id: 24
categories:
- Stuff that rocks
- Tools
---

[Podcasting](http://en.wikipedia.org/wiki/Podcasting) seems to get momentum. Today, I dropped by [Frank Westphal's](http://www.frankwestphal.de/Uebermich.html) web site. Frank has a very nice podcast named "[Tonabnehmer](http://www.frankwestphal.de/Tonabnehmer.html)" dealing with [Agile Software Development](http://agilemanifesto.org/).

Since I do not own an [iPod](http://www.apple.com/ipod/), but instead call a [Palm Tungsten E](http://www.palm.com/us/products/handhelds/tungsten-e/) my own, I wondered whether I could use my Palm for listening to podcasts. Yes, I can! Here's how I did it:




	
  1. First, get a podcast aggregator. I use [Doppler](http://www.dopplerradio.net/).

	
  2. Get an MP3 player for your Palm. I use [Real One Player for Palm](http://www.realnetworks.com/industries/mobile/operators/products/player/palm/), which came with my Tungsten E.

	
  3. Now it is about time to subscrie to a podcast feed. Here is the link to Frank's Tonabnehmer: [http://frankwestphal.podhost.de/rss](http://frankwestphal.podhost.de/rss)

	
  4. Doppler supports sending all downloaded files to an external programm, e.g. [iTunes](http://www.apple.com/itunes/). That's where we can plug-in!

	
  5. Download [this ZIP file](http://f3.tobject.de/wp-content/downloads/podcasting/podcast.zip) and place the contained batch file in a directory of your choice.

	
  6. You'll have to adjust the path information in the file, of course.

	
  7. Open the Doppler config dialog and associate MP3 files with your new command file. The application field must contain the path to the batch file you just extracted, the parameters field must contain the text `"{1}"`.

	
  8. Now, place an SD card in an SD writer attached to your computer and start downloading a podcast. After downloading has finished, the batch file will copy the MP3 files to your Palm's SD card.

	
  9. Just place the SD card back into your palm and you're ready to go!






