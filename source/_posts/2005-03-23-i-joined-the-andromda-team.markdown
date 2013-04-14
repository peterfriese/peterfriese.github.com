---
author: Peter
comments: false
date: 2005-03-23 19:56:14
layout: post
slug: i-joined-the-andromda-team
title: I joined the AndroMDA team
wordpress_id: 7
categories:
- MDA
---

Have you ever read stories about machines building other machines and computer programs writing other computer programs? Science-fiction, you say? Reality today, I say!

Take a look at AndroMDA. It is a tool that lets you build software from a model (designed in UML) and reduce the amount of hand-written code by great lengths. 

I have been using AndroMDA since mid-2003 in several projects (a large one is currently running) and have started to submit patches for the AndroMDA Spring Cartridge. After having submitted patches for [SPRING-34](http://galaxy.andromda.org:8080/jira/browse/SPRING-34) (Spring remoting), [SPRING-44](http://galaxy.andromda.org:8080/jira/browse/SPRING-44) (support for rich clients), [SPRING-38](http://galaxy.andromda.org:8080/jira/browse/SPRING-38) (hibernate criteria search support, together with Stefan Reichert) and [SPRING-47](http://galaxy.andromda.org:8080/jira/browse/SPRING-47) (criteria search support for null parameters), the team invited me to join them and promoted me to be an AndroMDA commiter.

My next tasks will be to enhance the criteria search facility, enable finders to return `java.util.Map`s instead of `java.util.Collection`s ([SPRING-48](http://galaxy.andromda.org:8080/jira/browse/SPRING-48)) and adding a feature for limiting the size of the result set of a finder ([SPRING-50](http://galaxy.andromda.org:8080/jira/browse/SPRING-50)).
