---
author: Peter
comments: true
date: 2009-02-12 20:52:04
layout: post
slug: cvs-on-a-mac
title: CVS on a Mac
wordpress_id: 172
categories:
- Apple
- Eclipse
- Mac
tags:
- english
---

In order to be able to work on some of our [Xtext](http://www.xtext.org) / [Eclipse](http://www.eclipse.org) related build scripts, I needed to install a [CVS](http://www.nongnu.org/cvs/) command line client on my Mac. Now if you google for "[cvs mac](http://www.google.com/search?hl=en&q=cvs+mac)", you'll get a large list of result, basically telling you to get the [Apple Xcode SDK](http://developer.apple.com/technology/xcode.html). While the Xcode SDK is for free, and usually you don't even need to download it from the [Apple Developer Connection's website](http://developer.apple.com/) (as you already have it on your Mac install disks as [Lullabot](http://www.lullabot.com/videocast/install_cvs_mac_osx) points out), it occurred to me that installing a 1+GB space hog seems to be a bit of an overkill for getting a tiny application.

So I decided to give [Fink](http://www.finkproject.org/) a try. Here is what you need to do to get a CVS commandline client on your Mac:



	
  1. [Download Fink](http://www.finkproject.org/download/index.php?phpLang=en)

	
  2. Install Fink

	
  3. Copy FinkCommander to your Applications folder

	
  4. Start FinkCommander

	
  5. In the search box, type "cvs"
[


![Fink Commander](http://farm4.static.flickr.com/3513/3274121893_93b22306c0.jpg)


](http://www.flickr.com/photos/81029262@N00/3274121893)

	
  6. Click on the "install binary package" button (it's the leftmost, with the blue plus sign)

	
  7. In the lower pane, you can now watch Fink downloading and installing the CVS package.
[


![Fink Commander, CVS installed](http://farm4.static.flickr.com/3391/3274949728_0fbd4612b3.jpg)


](http://www.flickr.com/photos/81029262@N00/3274949728)

	
  8. Let's see if it works. Open a command line window and type "cvs":
[


![CVS command line](http://farm4.static.flickr.com/3397/3274132941_36c13ac586.jpg)


](http://www.flickr.com/photos/81029262@N00/3274132941)

Perfect!
