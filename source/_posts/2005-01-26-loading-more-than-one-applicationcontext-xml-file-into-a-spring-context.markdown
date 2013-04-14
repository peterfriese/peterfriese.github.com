---
author: Peter
comments: false
date: 2005-01-26 15:32:42
layout: post
slug: loading-more-than-one-applicationcontext-xml-file-into-a-spring-context
title: Loading more than one applicationContext XML file into a Spring context
wordpress_id: 5
categories:
- Java
- Spring
---

You can use this command to load more than one application context file into a spring context:

`String serviceContextLocation = "service.xml";
String serverContextLocation = "server.xml";
ClassPathXmlApplicationContext combinedContext = new ClassPathXmlApplicationContext(new String[] {serviceContextLocation, serverContextLocation});`
