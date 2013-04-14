---
author: Peter
comments: true
date: 2010-03-08 21:42:38
layout: post
slug: running-applescript-from-java
title: Running AppleScript from Java
wordpress_id: 387
categories:
- Apple
- Eclipse
- iPhone
- Java
- Mac
tags:
- AppleScript
- Eclipse
- Java
---

In my current project, I need to launch an external application and maybe execute some additional commands on this external application. Due to the very nature of the project, the whole system will always be run on Mac OS X. So I thought, "why not use AppleScript"?<!-- more -->
Turns out using AppleScript to launch applications is fairly easy, all you have to do is 

    
    tell application "name of your app" to launch



If you want to try this from the command line, _osascript_ comes in handy:

    
    osascript -e 'tell app "iTunes" to launch'



So far so good. Should be easy to do this from Java, shouldn't it? Turns out it's not so easy at all. Let's try this:

    
      String launchCmd = "osascript -e 'tell application \"iTunes\" to play'";
      process = Runtime.getRuntime().exec(launchCmd);
    
      BufferedReader bufferedReader = new BufferedReader(
        new InputStreamReader(process.getErrorStream()));
      while ((lsString = bufferedReader.readLine()) != null) {
        System.out.println(lsString);
      }


I'm not sure why, but it results in a nasty "_0:1: syntax error: A unknown token can't go here. (-2740)_" error message.

But there is another signature for Runtime.exec:

    
      String[] cmd = { "osascript", "-e",	"tell app \"iPhone Simulator\" to launch" };
      process = Runtime.getRuntime().exec(cmd);
    
      BufferedReader bufferedReader = new BufferedReader(
        new InputStreamReader(process.getErrorStream()));
      while ((lsString = bufferedReader.readLine()) != null) {
        System.out.println(lsString);
      }


... and this works out just fine!
