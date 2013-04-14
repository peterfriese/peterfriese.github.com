---
author: Peter
comments: true
date: 2010-08-06 22:22:10
layout: post
slug: watching-dvds-on-your-ipad
title: Watching DVDs on your iPad
wordpress_id: 541
categories:
- Apple
- iPad
- iPhone
tags:
- dvd
- iPad
- video
---

I've been wanting to watch DVDs on my iPad since I had it. As the iPad doesn't have an optical drive, you need to jump through several loopholes to get the trick done, but in the very moment you watch your first DVD on your iPad, something magical happens...

<!-- more -->At first, I came up with a solution that would allow me to watch DVDs without re-encoding them - I just streamed them from my Mac. Actually, this is very easy with the help of some great open source tools, some of which are already installed on your Mac. But I guess this is a story for another blog post.

If you want to take your DVDs with you on your iPad, streaming them from your Mac isn't an option. So I browsed the web to see if I could find an affordable ripper. If you google for "[dvd ipad](http://www.google.com/search?q=dvd+ipad)", you'll be given an enormous list of hits linking to commercial tools - obviously encoding DVDs for the iPad isn't that simple, so companies can make a good business with selling tools. But I didn't want to buy any of those tools, so I looked for a solution involving freely available open source tools.

So, if you want to encode your DVDs for your iPad, here are the steps you need to take. It's very simple and doesn't cost you a single dime:




	
  1. [Download](http://handbrake.fr/downloads.php) and install Handbrake.

	
  2. In order to encode for the iPad, you need to use an adjusted profile: [iPad.plist](http://www.peterfriese.de/wp-content/downloads/handbrake/iPad.plist).

	
  3. Install this profile into Handbrake (_Preset -> Import..._ on the main menu)

	
  4. Insert a DVD into your optical drive

	
  5. Click the _Source_ button in Handbrake and select your DVD

	
  6. Handbrake will now scan the DVD for what it thinks is the main feature - you might need to verif this.

	
  7. Select one of the profiles in the profile selection dialog (on the Mac, press the _Toggle Presets_ button on the toolbar). _iPad full_ worked fine for me.

	
  8. You can now fine-tune the settings. For example, the _audio_ tab allows you to add multiple audio tracks to your video

	
  9. Once you're done, press _Start_

	
  10. Now it's time to go for a walk or having a chat with your friends - ripping the DVD will take some time, depending on your computer

	
  11. When Handbrake is done encoding the video, just double-click the resulting _.m4v_ file to import it to iTunes.

	
  12. As soon as the video shows up in iTunes, you can sync it to your iPad (the iPhone plays the video fine, too)



Most DVDs do not take up more than 1GB after encoding for the iPad, so you can load quite a number of films even onto small iPads.

Enjoy! Oh, and should I have made your life easier, feel free to [flattr](http://flattr.com/thing/2943/Peter-Frieses-Blog) me!

