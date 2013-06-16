---
author: Peter
comments: true
date: 2011-01-05 00:11:11
layout: post
slug: your-windows-ide-sucks-replace-it-with-your-favorite-editor-on-the-mac
title: Your Windows IDE sucks? Replace it with Your Favorite Editor on the Mac!
wordpress_id: 643
categories:
- Mac
- Tools
tags:
- IDE
- Samsung
- symlink
- textmate
- TV
- VMware
- Windows
teaserimage: /images/2011-01-05-your-windows-ide-sucks-replace-it-with-your-favorite-editor-on-the-mac/chains_150x150.png
---

For a [current project](http://www.samsungsmarttvchallenge.eu/), I need to use a Windows-based IDE that really sucks. So instead of letting the IDE degrade my productivity, I decided to use some combined Windows/Mac wizardry to solve the problem.

<!-- more -->

The IDE in question is Samsung's TV App SDK. Basically, it's just a Windows-based Visual-Studio look-alike IDE with editor support for CSS, JavaScript and HTML and an Emulator for Samsung TV kits. Nothing too complicated, really. In fact, the editor is quite OK. 

{% fancyimage right /images/2011-01-05-your-windows-ide-sucks-replace-it-with-your-favorite-editor-on-the-mac/Samsung-TV-App-SDK.png 1040x722 Samsung TV App SDK %}

The part about it that really sucks is the project management. How come tool vendors still think it's good style in 2010 to place the projects under _Program Files\Samsung TV Apps SDK\Apps_? There's no way to store your project files in a different location. The project open dialog just won't let you navigate to some other place:

{% fancyimage right /images/2011-01-05-your-windows-ide-sucks-replace-it-with-your-favorite-editor-on-the-mac/Samsung_TV_App_SDK_ProjectManagement.png 482x352 Samsung TV App SDK Project Management Dialog%}

So I wanted to be able to edit my files on the Mac (using [TextMate](http://macromates.com/)) while still using the good parts of the Samsung TV SDK (i.e., the Emulator).

I'm using VMware Fusion to run Windows 7 and the Samsung SDK (no, there is no version for the Mac). Most virtualization solutions offer a mechanism to share folders between the host and the guest OS. So I quickly set up folder sharing between my Mac and the guest OS, in this case Windows 7.

Now that I can see the project files both on the Mac and on the Windows machine, how can I make sure I can open the project in the Samsung TV SDK IDE? As I mentioned before, there's no way to tell the IDE to open projects form other locations than _Program Files\Samsung TV Apps SDK\Apps_!

After playing around with some more or less usable folder synchronization utilities, I came up with something most MacOS / Linux users should be familiar with: [symbolic links](http://en.wikipedia.org/wiki/Symbolic_link)! While symlinks have been around in Unix-like OSes for ages, they have been rarely known to Windows users for most of the time. However, [starting with Windows NT](http://en.wikipedia.org/wiki/NTFS_symbolic_link), you can create symbolic links, hard links and junctions using a nifty little tool called _[mklink](http://www.howtogeek.com/howto/windows-vista/using-symlinks-in-windows-vista/)_. Unfortunately, you're not allowed to run _mklink_ if you're not an administrator. Using _runas_ (which is Windows' equivalent of _sudo_), didn't help as the shared folders weren't visible to the admin user.

To cut a long story short, I found [Symlinker](http://code.google.com/p/symlinker), a UI tool that helps in creating symlinks on Windows. As it is a UI tool, you can run it with administrator privileges (by selecting Run as Admin from the context menu). Using a [UNC][1] path, you can create a [symlink][2] to a VMware shared folder and place this symlink in the location the Samsung IDE expects it to be.

  [1]: http://en.wikipedia.org/wiki/Path_(computing)
  [2]: http://en.wikipedia.org/wiki/NTFS_symbolic_link

Finally, I can edit my files on the Mac and run the app in Samsung's Emulator on my hosted Windows machine. And as the files on my Mac are mapped to the hosted Windows machine via a symlink, I do not suffer a synchronization lag - all files are updated instantaneously :-)
