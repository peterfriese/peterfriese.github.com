---
author: Peter
comments: true
date: 2011-02-16 10:48:38
layout: post
slug: appropriate-use-of-mapkit
title: Appropriate Use of MapKit
wordpress_id: 689
categories:
- Apple
- iOS
- MapKit
- TOS
- terms of service
teaserimage: /images/2011-02-16-appropriate-use-of-mapkit/AppleMaps_150x150.png

---

I just had an app rejected because of violation of the Google Maps terms of service. While it certainly is kind of funny Apple rejects an app because you're violating Google's terms of service, I was wondering what in particular was wrong. 
<!-- more -->

At first sight, everything looked OK. Have a look at the following screenshot. It clearly violates [Google's terms of service for Maps](http://code.google.com/apis/maps/terms.html), but can you spot what is wrong?

{% fancyalbum 240x300 %}
/images/2011-02-16-appropriate-use-of-mapkit/maps_without_google_logo.png: Google Maps in your app - without Google Logo: you're doing it wrong!
{% endfancyalbum %}

Maybe you can better see what's wrong when I show you another screenshot, this time obeying the TOS:

{% fancyalbum 240x300 %}
/images/2011-02-16-appropriate-use-of-mapkit/maps_with_google_logo.png: Google Maps with logo: well done!
{% endfancyalbum %}

Can you spot the difference? It's the Google logo!

The reason why it is not shown in the first screenshot is that the bounds for the map are not set correctly. In the offending version of my app, I used a piece of code similar to this one:

``` objc
    - (void)viewDidLoad {
      [super viewDidLoad];
      mapView = [[MKMapView alloc] initWithFrame:self.view.bounds];
    	
      mapView.showsUserLocation=TRUE;
      mapView.mapType=MKMapTypeStandard;
      [self.view addSubview:mapView];		
    }
```


Nothing wrong with it, but it lacks one essential line:

``` objc    
    - (void)viewDidLoad {
      [super viewDidLoad];
      mapView = [[MKMapView alloc] initWithFrame:self.view.bounds];
    
      mapView.autoresizingMask = 
        (UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight);	
    
      mapView.showsUserLocation=TRUE;
      mapView.mapType=MKMapTypeStandard;
      [self.view addSubview:mapView];		
    }
```


So, next time you write an app that contains Google Maps, make sure the Google logo is visible. You can get the full source code for this example [on my GitHub page](http://github.com/peterfriese/MapKitSample).
