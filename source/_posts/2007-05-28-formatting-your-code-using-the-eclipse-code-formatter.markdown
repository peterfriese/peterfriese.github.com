---
author: Peter
comments: true
date: 2007-05-28 21:14:23
layout: post
slug: formatting-your-code-using-the-eclipse-code-formatter
title: Formatting your code using the Eclipse Code Formatter
wordpress_id: 67
categories:
- Eclipse
- Tools
---

In case you needed to format a batch of code using a command line utility, you probably went for [Jalopy](http://jalopy.sourceforge.net/) or [JIndent](http://www.jindent.com/). Thanks to Ben Konrath of Red Hat, you no longer need to do so: since Eclipse 3.2, you can use the built-in code formatter to format your Java code using the command line.

Here's how:

The hardest part of it all is to create the config file for the formatter. To create it, select one of your existing projects, and activate project specific formatter settings (_Properties_ ->_ Java Code Style_ -> _Formatter_ -> _Enable project specific settings_):

![](http://www.peterfriese.de/wp-content/downloads/images/formatter_project_specific_settings.jpg)
Configure the code formatter as desired. Click _OK_ when you're done.

Using a file explorer, navigate to **<path to your workspace>/<yourproject>/.settings** and copy **org.eclipse.jdt.core.prefs** to a new location. This file contains all your formatting settings.

To invoke the code formatter using the command line issue the following command:
`
<path-to-eclipse>\eclipse.exe -vm <path-to-vm>\java.exe -application **org.eclipse.jdt.core.JavaCodeFormatter** -verbose -config <path-to-config-file>\org.eclipse.jdt.core.prefs <path-to-your-source-files>\*.java
`
In case the formatter complains about your code, your code probably contains Java 5 constructs and you have to add the following lines to your config file to make everything work:

`org.eclipse.jdt.core.compiler.compliance=1.5
org.eclipse.jdt.core.compiler.codegen.targetPlatform=1.5
org.eclipse.jdt.core.compiler.source=1.5
`
