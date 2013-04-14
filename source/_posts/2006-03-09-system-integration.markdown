---
author: Peter
comments: false
date: 2006-03-09 13:09:30
layout: post
slug: system-integration
title: System integration
wordpress_id: 44
categories:
- Linux
---

Had to do some system integration this week. It's not what I really like. System integration should better be called "taming wild pieces of bits and bytes". However, here are some Linux commands I needed that I will likely forget if I do not write them down:



	
  * Log in to a remote system, specifying a user name: 
		`ssh hostname -l username`

	
  * Unpack a .tar.gz file: 
		`tar xvfz filename.tar.gz`

	
  * Restart sshd:
		`/etc/init.d/sshd restart`

	
  * Restart Tomcat:
		`/etc/init.d/tomcat restart`

	
  * Text based web browsers:
		
			
    1. w3m

			
    2. lynx

		
	

	
  * find out which process is listening on which ports:
		`netstat -p`
	

	
  * (really) kill a process:
		`kill -9 `
	

	
  * find out the process id of a java process:
		`ps aux | grep java`
	



Likewise, here are some Windows commands that might come in handy:

	
  * Need to start a number of PuTTY sessions from one batch file? Use the _start_ command:
		`start putty -load session_name
		start putty -load another_session_name`





