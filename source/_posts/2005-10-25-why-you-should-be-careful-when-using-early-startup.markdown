---
author: Peter
comments: false
date: 2005-10-25 09:47:15
layout: post
slug: why-you-should-be-careful-when-using-early-startup
title: Why you should be careful when using early startup.
wordpress_id: 35
categories:
- Eclipse
---

Since version 2.0, Eclipse comes with a feature called _early startup_. Plug-ins using this mechanism will be loaded and activated when the workbench is starting up. One of the major design goals of Eclipse was to minimize startup time, which is why plugins are divided into  a declarative (i.e. the plugin.xml) and a dynamic part (the Java classes making up the plug-in). Using early startup is strongly discouraged, mainly due to performance reasons: if everybody wants to startup early, the whole idea of separating plug-ins into a declarative and a dynamic part gets perverted.



But there is another reason why using early startup is discouraged: if you are not careful, your plug-in might get instantiated twice! "What?", I hear you saying, "Plug-ins are singletons, how can they possibly be instantiated twice?" Well, that's what I thought as well. But have a look at [bug #91603](https://bugs.eclipse.org/bugs/show_bug.cgi?id=91603) and you'll find that it is true: if you specify your plug-in class to be the early startup class, your plug-in class will be instantiated twice. As the original reporter states, this will produce strange effects when you try to refer to the plug-in's bundle or other fields that get initialized during normal startup.






So, what can we do about it? I suggest the following best practice:



**Using early startup the safe way**



	
  * Do not use early startup.

	
  * Only use it if you absolutely have to.

	
  * If you use it, do not specify a class in the `class` element.



**Rationale**
Why shouldn't you specifiy a startup class? The [online help](http://help.eclipse.org/help31/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/extension-points/org_eclipse_ui_startup.html) says that _If the startup element has a class attribute, the class will be instantiated and earlyStartup() will be called on the result. Otherwise, this method will be called on the plug-in class._. To understand what the documentation writers want to tell us, review  this [comment by Nick Edgar](https://bugs.eclipse.org/bugs/show_bug.cgi?id=91603#c2). Now it becomes clear: if you specify a class, that class will be instantiated, regardless of whether it is the plug-in class itself or any other class. If you do not specify a class, then the singleton instance of your plug-in will be used instead, which is what we want.

