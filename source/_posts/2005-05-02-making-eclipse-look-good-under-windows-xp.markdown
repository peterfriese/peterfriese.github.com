---
author: Peter
comments: false
date: 2005-05-02 21:09:22
layout: post
slug: making-eclipse-look-good-under-windows-xp
title: Making Eclipse look good under Windows XP
wordpress_id: 16
categories:
- Eclipse
---

You may have noticed that Eclipse doesn't really look native under Windows XP. This can easily be fixed by creating a manifest file for the Java VM used to start Eclipse, see [here](http://wiki.schubart.net/How_to_Make_Eclipse_Use_the_Windows_XP_Skin). The author mentions that it might not be easy to find out which JVM is used to start Eclipse. This, too, can easily be fixed by copying an entire **JRE** into the _eclipse_ directory. Eclipse will automatically use this JRE to start up.
