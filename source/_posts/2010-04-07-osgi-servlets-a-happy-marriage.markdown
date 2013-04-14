---
author: Peter
comments: true
date: 2010-04-07 12:40:41
layout: post
slug: osgi-servlets-a-happy-marriage
title: 'OSGi & Servlets: A Happy Marriage'
wordpress_id: 415
categories:
- Eclipse
- OSGi
tags:
- Amazon EC2
- cloud
- J2EE
- JEE
- OSGi
---

In this post, I'll show you how to create a simple OSGI-based servlet. Later, we will deploy this servlet to an Amazon EC 2 instance - this should be fun!


<!-- more -->



> **Update**: I updated the code sample to use Declarative Services, as suggested by Jeff and Scott in the comments of this post. After reading this article, [please refer to this other post to learn about](http://www.peterfriese.de/osgi-servlets-flexibility-by-simplicity/) the changes.






**First of all**, some preliminary steps. We need to create a new OSGi project and add some dependencies:




	
  1. Create a new plug-in project, naming it _simple.servlet_. Make sure to select _Equinox_ as the target OSGI framework on the first page of the wizard. Also make sure you've got an activator (that option is on by default).

	
  2. Open the manifest editor and add the following packages to the Imported Packages section on the Dependencies tab:
		
			
    * javax.servlet

			
    * javax.servlet.http

			
    * org.osgi.framework

			
    * org.osgi.service.http

			
    * org.osgi.util.tracker

		
	








**Next**, let's create the servlet. Servlets are plain Java classes that implement the javax.servlet.Servlet interface. As we want to serve information over HTTP, we can just create a subclass of javax.servlet.http.HttpServlet which already does some of the heavy lifting for us. All we need to do is override the doGet() method. This method does not have a return value. Instead, we need to fill its second parameter, `HttpServletResponse resp` with the text we want the web browser to display. We could return HTML, but for simplicity's sake, let's just return plain text (so make sure to set the content type to `text/plain`):


    
    package simple.servlet;
    import java.io.IOException;
    
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    public class SimpleServlet extends HttpServlet {
      private static final long serialVersionUID = 1L;
    
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
        throws ServletException, IOException {
        resp.setContentType("text/plain");
        resp.getWriter().write("Hello from the cloud!");
      }
    }








**In order for the servlet to be accessible** via HTTP, we need to register it. If you have done some JEE/J2EE development, you know that servlets are registered using the `web.xml` servlet descriptor. As we're not in the JEE world any longer, we can't go down this road. Instead, we will register the servlet using a `org.osgi.util.tracker.ServiceTracker`. It's a little bit more code to write, but let me tell you: it's worth it as you'll see very soon.







The service tracker will be registered for a specific class - in our case that's `org.osgi.service.http.HttpService`:





    
    package simple.servlet;
    
    import org.osgi.framework.BundleContext;
    import org.osgi.framework.ServiceReference;
    import org.osgi.service.http.HttpService;
    import org.osgi.util.tracker.ServiceTracker;
    
    public class HttpServiceTracker extends ServiceTracker {
    
      public HttpServiceTracker(BundleContext context) {
        super(context, HttpService.class.getName(), null);
      }
      // ...





When the service tracker is asked to return the `HttpService`, we can register our servlet:



    
      // ...
      public Object addingService(ServiceReference reference) {
        HttpService httpService = (HttpService) super.addingService(reference);
        if (httpService == null)
          return null;
    
        try {
          System.out.println("Registering servlet at /simple");
          httpService.registerServlet("/simple", new SimpleServlet(), null, null);
        } catch (Exception e) {
          e.printStackTrace();
        }
    
        return httpService;
      }
      /...





Likewise, we must make sure to unregister the servlet when the service tracker is asked to shut down the `HttpService`



    
      // ..
      public void removedService(ServiceReference reference, Object service) {
        HttpService httpService = (HttpService) service;
    
        System.out.println("Unregistering /simple");
        httpService.unregister("/simple");
    
        super.removedService(reference, service);
      }
    
    }





**Now** that we've got the servlet in place, we need to make sure the service tracker is started and stopped when our bundle is started and stopped. Those of you familiar with OSGi know where we're heading now: we need to implement the `start()` and `stop()` methods of our activator:




    
    package simple.servlet;
    
    import org.osgi.framework.BundleActivator;
    import org.osgi.framework.BundleContext;
    
    public class Activator implements BundleActivator {
    
      private HttpServiceTracker serviceTracker;
    
      public void start(BundleContext context) throws Exception {
        serviceTracker = new HttpServiceTracker(context);
        serviceTracker.open();
      }
      
      public void stop(BundleContext context) throws Exception {
        serviceTracker.close();
        serviceTracker = null;
      }
    
    }





**To test your servlet locally**, create a new launch config:



	
  1. In the main menu, select _Run -> Run Configurations..._

	
  2. Double click on _OSGi Framework_ in the tree on the left hand side to create a new OSGi-based launch config
and name it _Simple Servlet (OSGi)_
	
  3. Make sure you select the following **eight (8)** bundles on the _Bundles_ tab:
		
			
    1. simple.servlet

			
    2. javax.servlet

			
    3. org.eclipse.equinox.http.jetty

			
    4. org.eclipse.equinox.http.servlet

			
    5. org.eclispe.osgi

			
    6. org.eclispe.osgi.services

			
    7. org.mortbay.jetty.server

			
    8. org.mortbay.jetty.util

		
	

	
  4. Head over to the _Arguments_ tab, making sure the _VM arguments_ text box reads `-Declipse.ignoreApp=true -Dosgi.noShutdown=true -Dorg.osgi.service.http.port=8080`








**You can now start the server** by executing the launch config. After a very short time, the text `Registering servlet at /simple` should appear in the console window, indicating your OSGi-based server and your servlet have been started up (a lot faster that it would have taken on Tomcat or any other JEE server, for that matter).






**Open your web browser** at [http://localhost:8080/simple](http://localhost:8080/simple) to observe your servlet in action:



[![OSGi SimpleServlet, local deployment](http://farm3.static.flickr.com/2775/4499763290_00e3405795_m.jpg)](http://farm3.static.flickr.com/2775/4499763290_c0045c441b_o.jpg)





**Congratulations**, you've just built your first OSGi-enabled servlet. In the next post, I'll show you how to deploy this servlet in an Amazon EC2 instance. You can [download the source code](http://code.google.com/p/peterfriese/source/browse/#svn/osgi/trunk/simple.servlet) for this post from my SVN repository on Google code.

