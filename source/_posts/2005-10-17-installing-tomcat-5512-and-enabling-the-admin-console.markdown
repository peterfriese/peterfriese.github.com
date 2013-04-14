---
author: Peter
comments: false
date: 2005-10-17 14:18:30
layout: post
slug: installing-tomcat-5512-and-enabling-the-admin-console
title: Installing Tomcat 5.5.12 (and enabling the Admin console)
wordpress_id: 31
categories:
- Tomcat
---

Setting up [Tomcat](http://tomcat.apache.org/) seems to be quite easy: just unpack the core ZIP file, and off you go! But as soon as you want to manage the server, you realize that Tomcat 5.5.x is missing the admin console. Setting up the admin console is easy as well, but is not well documented. So here we go:




	
  * Get the [latest binary builds](http://tomcat.apache.org/download-55.cgi) (apache-tomcat-5.5.12.zip, apache-tomcat-5.5.12-admin.zip and (if you're running on JDK 1.4) apache-tomcat-5.5.12-compat.zip)

	
  * 
		Unzip the files into the same directory, making sure you obey this order:
		
			
    1. apache-tomcat-5.5.12.zip

			
    2. apache-tomcat-5.5.12-compat.zip

			
    3. apache-tomcat-5.5.12-admin.zip

		
	

	
  * 
		Edit the file _$catalina_home/conf/tomcat-users.xml_, making sure it looks like this:
`
< ?xml version='1.0' encoding='utf-8'?>
<tomcat -users>
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <role rolename="admin"/>
  <role rolename="manager" />
  <user username="tomcat" password="tomcat" roles="tomcat"/>
  <user rusername="admin" password="admin" roles="tomcat,manager,admin" />
  <user username="both" password="tomcat" roles="tomcat,role1"/>
  <user username="role1" password="tomcat" roles="role1"/>
</tomcat>
`
	



This should do the trick.





