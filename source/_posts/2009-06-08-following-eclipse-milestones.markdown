---
author: Peter
comments: true
date: 2009-06-08 23:46:24
layout: post
slug: following-eclipse-milestones
title: Following Eclipse Milestones
wordpress_id: 270
categories:
- Eclipse
- Eclipse FAQ
- Stuff that rocks
---

With the Galileo Release coming up, you might find yourself having a hard time updating to the latest milestones AND keeping your favorite plug-ins up-to-date.

Did you know that you can migrate your additional plug-ins from one Eclipse install to another one? This can be a huge time-saver, especially for people who like to live on the bleeding edge.

Here's how:




	
  1. Install a fresh copy of Eclipse. Let's assume you install Eclipse 3.5 RC4 Cocoa 64bit (you're feeling lucky)

	
  2. Let's further assume you've got an existing install of Eclipse 3.5 RC3 Cocoa 32bit with some additional plug-ins, like [FindBugs](http://findbugs.sourceforge.net/), [WindowBuilder Pro](http://www.windowbuilderpro.com/), etc.

	
  3. After installing, start your newly installed instance of Eclipse

	
  4. Select _Help -> Install New Software..._

	
  5. In the _Install_ dialog, click the _Add..._ button to add a new update site: [

![Eclipse Install Dialog](http://farm4.static.flickr.com/3343/3608873664_f25891cb7f.jpg)

](http://www.flickr.com/photos/81029262@N00/3608873664)

	
  6. In the next dialog, click on _Local..._ to add a local update site:[

![Eclipse: Add Site](http://farm4.static.flickr.com/3409/3608070929_32df5d32cd.jpg)

](http://www.flickr.com/photos/81029262@N00/3608070929)

	
  7. using the file chooser, browse to _<path_to_your_OLD_eclipse_instance>/eclipse/p2/org.eclipse.equinox.p2.engine/profileRegistry/SDKProfile.profile/_ and click _Choose_ to select that directory.

	
  8. Click _OK_ to add the update site:[

![Eclipse: Add Local Update Site](http://farm3.static.flickr.com/2434/3608904194_5b6c4de3be.jpg)

](http://www.flickr.com/photos/81029262@N00/3608904194)

	
  9. The _Install_ dialog will now list all plug-ins installed in the old location (i.e. your old Eclipse instance), clearly highlighting the ones that are not already installed in the new instance:[

![Eclipse: Install from existing Eclipse install](http://farm4.static.flickr.com/3562/3608920684_9fc82065a9.jpg)

](http://www.flickr.com/photos/81029262@N00/3608920684)

	
  10. Check all features that you want to transport to the new location and continue the installation by clicking _Next>_.
	
  11. After confirming the license terms and clicking _Finish_, Eclipse will install the selected features from the old location into the new location. After the obligatory workbench restart you're good to go._


The only thing that I am wondering about is: why is there no first-class UI action (e.g. an import wizard) to do this?
