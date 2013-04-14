---
author: Peter
comments: false
date: 2007-06-11 08:21:11
layout: post
slug: the-case-for-preferenceandpropertiespage
title: The case for PreferenceAndPropertiesPage
wordpress_id: 68
categories:
- Eclipse
---

In an article in the German [Eclipse Magazine](http://eclipse-magazin.de/), [Stefan Reichert](https://www.xing.com/profile/Stefan_Reichert5) and [Dirk Rademann](https://www.xing.com/profile/Dirk_Rademann) write about Wizards and Dialogs. Amongst other things, they briefly touch the topic of preference pages. This reminded me of a pet bug of mine: [https://bugs.eclipse.org/bugs/show_bug.cgi?id=173414](https://bugs.eclipse.org/bugs/show_bug.cgi?id=173414) (Make PropertyAndPreferencePage available to other plug-ins (i.e. make it API))). You might be aware that when dealing with preferences and properties, you quickly end up writing duplicate code just because you want to support a number of settings both on a global level (aka preferences) and at the same time a project specific level (aka properties).

The JDT team has come up with a very handy class called `PropertyAndPreferencePage` which does just what you might guess plus some very nice additional things:
First and foremost you use the same code for preference pages and property pages. This alone is a big benefit since it relieves you from the tedious and error prone task to re-implement things. Additionally, you get some additional controls in the headline: if the page is in property mode, it will add a checkbox that allows you to enabled project specific settings. Checking this box will automatically enable the client area of the property page. Unchecking the box will disable the client area consequently, but will enable the second control in the headline: a hyperlink
labelled _Configure Workspace settings_. Clicking this hyperlink will open the workspace preference dialog with a filter so that only your page will be visible - in preference mode!

If you open your page in the workspace preference dialog, you'll be able to configure settings on a global level. However, if you would like to quickly jump over in order to configure something special for just one or two projects, you can easily do so by using the _Configure Project Specific Settings..._ hyperlink. Since Eclipse cannot guess which project you might want to configure, it will present you with a project selection dialog before it will open your page in the properties dialog for the project you just selected.

As you can see, `PropertyAndPreferencePage` can save the day. If you have come to the conclusion that this nifty class is worthwhile to be included in the plattform, then do yourself a favour and vote for this bug: [https://bugs.eclipse.org/bugs/show_bug.cgi?id=173414](https://bugs.eclipse.org/bugs/show_bug.cgi?id=173414).
