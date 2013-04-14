---
author: Peter
comments: true
date: 2008-09-30 21:27:45
layout: post
slug: eclipse-on-macos
title: Eclipse on MacOS
wordpress_id: 127
categories:
- Apple
- Eclipse
---

By default, runtime instances of Eclipse will start with large fonts. To get rid of this behaviour, just add `-Dorg.eclipse.swt.internal.carbon.smallFonts` to the VM arguments of the respective launch configuration. 

I guess it might be a good idea if PDE UI could transparently add this parameter to Eclipse launch configurations - I [opened a bug](https://bugs.eclipse.org/bugs/show_bug.cgi?id=249179) to get this fixed.
