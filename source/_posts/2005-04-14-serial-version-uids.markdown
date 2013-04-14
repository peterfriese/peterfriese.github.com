---
author: Peter
comments: false
date: 2005-04-14 13:36:22
layout: post
slug: serial-version-uids
title: Serial Version UIDs
wordpress_id: 11
categories:
- Java
- MDA
---

In my current project, we're struggling with [serial version UIDs](http://java.sun.com/j2se/1.4.2/docs/guide/serialization/spec/version.html). Here are some links that shed a little light on the whole topic.
[Portland Pattern Repository's Wiki](http://c2.com/cgi/wiki?AlwaysDeclareSerialVersionUid) states that all serializable classes should have a serial version UID assigned by the developer. An interesting observation ist that different compilers produce differnt UIDs for the same class.

[SerialVer](http://serialver.sourceforge.net/) is a [Sourceforge](http://sourceforge.net/) hosted project that provides [ANT](http://ant.apache.org/) tasks for generating serial version UIDs.

The [AndroMDA Hibernate Cartridge](http://www.andromda.org/andromda-spring-cartridge/) is currently lacking support for serial version UIDs, but [I have filed an issue](http://galaxy.andromda.org:8080/jira/browse/HIB-87) and hope it will be fixed soon.
