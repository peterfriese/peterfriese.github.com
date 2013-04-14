---
author: Peter
comments: true
date: 2010-04-16 01:04:09
layout: post
slug: osgi-servlets-deploying-on-amazon-ec2
title: 'OSGi & Servlets: Deploying on Amazon EC2'
wordpress_id: 422
categories:
- Eclipse
- OSGi
tags:
- Amazon EC2
- J2EE
- JEE
- OSGi
---

Last week, [I showed you how to create a very simple OSGi-based servlet](http://www.peterfriese.de/osgi-servlets-a-happy-marriage/) and run it locally on Jetty. Today, I will show you how to deploy this servlet to an Amazon EC2 instance.


<!-- more -->


We will first set up an Amazon EC2 instance and then export the servlet and install it in this instance.





## Setting up an Amazon EC2 instance




Scott Lewis [was kind enough to prepare an Amazon EC2 Machine Image (AMI)](http://eclipseecf.blogspot.com/2010/03/osgieclipsert-in-amazon-cloud.html) that already contains Jetty, Equinox and p2 - everything we need to get our servlet running. So let's use this AMI to provision our instance:






	
  1. Sign in to the [AWS web console](https://console.aws.amazon.com/ec2/home)

	
  2. Click on _Launch instance_ to create a new instance

	
  3. In the _Request Instances Wizard_, jump over to the _Community AMIs_ tab

	
  4. Type _OSGi_ in the search field to search for **_ami-69d93600_**

	
  5. Select the AMI

	
  6. In the next step, you get to configure the instance. A small instance will do for now.

	
  7. Leave the _Advanced Instance_ options as-is

	
  8. Next, you need to create a key pair (this will allow you to sign in to the instance using SSH). If you haven't created an EC2 instance before, select _Create a new Key Pair_ and follow the instructions. Otherwise, select an existing key pair

	
  9. To access the instance from outside using HTTP and SSH, you need to create and assign a _security group_. Make sure to add SSH and HTTP port configurations to this security group. Unfortunately, it is not possible to freely choose the port ranges, so we need to edit the security group after we're finished with the wizard.

	
  10. On the summary page, review the configuration and click on _Launch_ to actually start your instance.





After a little while, the newly created instance will show up in the list of AMIs. You can test your instance by opening a terminal window and SSH'ing to your instance:



    
    ssh -i path/to/your/privatekey.pem root@ec2-xxx-xx-xx-xx.compute-1.amazonaws.com




You should be greeted with a prompt like this:



    
            __|  __|_  )  Fedora 8
            _|  (     /    32-bit
           ___|\___|___|
    
    Welcome to an EC2 Public Image
                          :- )
       Base




Start the EclipseRT server as follows:



    
    cd /env/osgi_eclipsert36_sb_p2_jetty_7_0_1_linux_x86/jetty-distribution-7.0.1.v20091125/
    ./eclipsert36.sh




Your Eclipse RT Equinox server is now up and running. Time to export the servlet!





## Exporting the servlet


We will export our simple servlet using a feature based update site, so it can be installed using p2.



**First,** create a new feature project, naming it _simple.servlet.feature_. Provide the following details to the wizard:





	
  1. _ID:_ simple.servlet.feature

	
  2. _Version:_ 1.0.0.qualifier

	
  3. _Name:_ Simple Servlet Feature

	
  4. _Provider:_ Your name




Open the _Plug-ins_ page and add _simple.servlet_ to the list of packages plug-ins.





**Now,** we can create the update site. It will only contain the feature we just created. Using the _New Update Site_ wizard, create a new update site project, naming it _simple.servlet.updatesite_. Add the feature we just created to the list of features on the first page of the _site.xml_ Update Site Map.





**When you're done with that, **, you can build the update site by pressing the _Build All_ button. This will build the servlet plug-in, the feature and finally the update site. After that, we're ready to deploy.





## Installing the servlet


Installing the servlet into our Amazon EC2 instance is quite easy, as we can leverage p2:


**Using your favourite SFTP client,** copy the entire update into the _/tmp_ directory of your Amazon EC2 instance. When that's done, hop over to your terminal window that's connected to the OSGi console of your Eclipse RT server (remember, we started this server when setting up the Amazon EC2 instance). Issue the following commands:



    
    provaddrepo file:/tmp/simple.servlet.updatesite
    provliu
    provinstall simple.servlet  1.0.0.201004091446
    confapply


**Make sure you use the correct version identifier, it is likely to be different from the one I used here!**



You can now list the installed bundles by invoking _ss_:



    
    ss
    ...
    38      ACTIVE      org.eclipse.equinox.preferences_3.3.0.v20100208
    39      ACTIVE      org.eclipse.equinox.registry_3.5.0.v20100301
    40      ACTIVE      org.eclipse.equinox.security_1.0.200.v20100301
    41      ACTIVE      org.eclipse.equinox.server.examples.hello_1.0.0.201003111015
    42      ACTIVE      org.eclipse.equinox.simpleconfigurator.manipulator_2.0.0.v20100304
    43      ACTIVE      org.eclipse.equinox.util_1.0.100.v20090520-1800
    44      RESOLVED    org.eclipse.osgi.services_3.2.100.v20100108
    45      RESOLVED    org.sat4j.core_2.2.0.v20100225
    46      RESOLVED    org.sat4j.pb_2.2.0.v20100225
    47      RESOLVED    simple.servlet_1.0.0.201004081743





**Start** the servlet by typing _start 47_ (again, please use the bundle ID issued by _ss_).





**Open a webbrowser** and navigate to ec2-xxx-xx-xx-xx.compute-1.amazonaws.com:8080/simple (the DNS address is the same you used to SSH to your instance) and you should see the servlet output:



    
    Hello from the cloud!





## Conclusion




In this post, you saw how easy it is to create an Amazon EC2 instance and deploy an OSGi-based servlet on it. With this knowledge, you can now start sky-diving into the joys of cloud computing. Have fun!




Most of this information has been taken from the [Eclipse Wiki](http://wiki.eclipse.org/EclipseRT_for_Amazon_EC2). You are encouraged to add your own tips & tricks to this [wiki page](http://wiki.eclipse.org/EclipseRT_for_Amazon_EC2). All you need is an Eclipse Bugzilla account. Setting one up is easy, start [here](https://bugs.eclipse.org/bugs/createaccount.cgi).
