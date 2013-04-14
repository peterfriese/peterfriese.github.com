---
author: Peter
comments: true
date: 2010-04-26 10:24:03
layout: post
slug: osgi-servlets-flexibility-by-simplicity
title: 'OSGi & Servlets: Flexibility by Simplicity'
wordpress_id: 426
categories:
- Eclipse
- OSGi
tags:
- cloud
- Eclipse
- J2EE
- JEE
- OSGi
---

**Strangely enough**, simple things tend to be more flexible than complex things. I bet you too have seen people go to great lengths to ensure a certain solution provides utmost flexibility. <!-- more -->Often, this flexibility isn't needed, so you're introducing accidental complexity.




In [a recent post](http://www.peterfriese.de/osgi-servlets-a-happy-marriage/), I showed you how to create a plain servlet and register it in an OSGi environment. As both [Jeff](http://eclipsesource.com/blogs/author/jeff/) and [Scott](http://www.composent.com/) pointed out, my using a _ServiceTracker_ to register and unregister the servlet is a little bit clumsy and can be improved by using Declarative Services.





I highly recommend reading chapter 15 in "[OSGi and Equinox](http://my.safaribooksonline.com/9780321561510)", but in a nutshell Declarative Services allow you to define _components_ which can provide and consume _services_. _Binding_ and _unbinding_ references between components and services is performed by the DS runtime (also known as the Service Component Runtime).





Without further ado, here are the changes I had to make to DS-ify my simple servlet:






	
  1. Remove the activator. Yes, that's true: we don't need an Activator any more. Delete the class and also remove it from _META-INF/MANIFEST.MF_

	
  2. Delete _HttpServiceTracker_. Registering the servlet with the _HTTPService_ will be handled by the DS runtime.

	
  3. Implement a _component_ to register and unregister the servlet with the _HttpService_:

    
    public class SimpleComponent {
      private static final String SERVLET_ALIAS = "/hellods";
      private HttpService httpService;
    	
      public void setHttpService(HttpService httpService) {
        this.httpService = httpService;
      }
    	
      protected void startup() {
        try {
          System.out.println("Staring up sevlet at " + SERVLET_ALIAS);
          SimpleServlet servlet = new SimpleServlet();
          httpService.registerServlet(SERVLET_ALIAS, servlet, null, null);
        } catch (ServletException e) {
          e.printStackTrace();
        } catch (NamespaceException e) {
          e.printStackTrace();
        }
      }
    
      protected void shutdown() {
        httpService.unregister(SERVLET_ALIAS);
      }
    }


	

As you can see, the _HttpService_ will be injected into this component using it's setter method, _setHttpService_.


	

	
  4. Register this component with the DS runtime by adding a component description:

    
    
    <scr:component deactivate="shutdown" activate="startup" xmlns:scr="http://www.osgi.org/xmlns/scr/v1.1.0" name="simple.ds.component">
      <implementation class="simple.ds.servlet.SimpleComponent"></implementation>
      <reference interface="org.osgi.service.http.HttpService" bind="setHttpService"></reference>
    </scr:component>




I saved this file in _OSGI-INF/component.xml_ and added it to the _Service-Component_ section of _META-INF/MANIFEST.MF_. In fact, as I used the _Create New OSGi Component_ wizard, the wizard added the entry to the _ServiceComponent_ section - it's really easy to forget this if you do it manually!








That's it!




**Please note** that I did not change the servlet implementation at all (apart form issuing a different text to make it easier to tell the servlets apart)!





**Before launching**, please make sure to add _org.eclipse.equinox.ds_ and _org.eclipse.equinox.util_ to your launch config to enable Declarative Services.





The biggest advantage of this approach is that you do not have to take care of acquiring the _HTTPService_. The DS runtime will _only_ activate your component when all prerequisites have been met, i.e., all dependencies are available. If the _HttpService_ is not available for any reason, your component will not be started. This makes the code for registering the servlet simpler and cleaner.





You can [download the source for the DS-ified servlet](http://code.google.com/p/peterfriese/source/browse/#svn/osgi/trunk/simple.ds.servlet) from my SVN repository on Google Code.
