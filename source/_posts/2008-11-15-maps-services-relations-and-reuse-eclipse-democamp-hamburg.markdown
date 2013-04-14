---
author: Peter
comments: true
date: 2008-11-15 02:25:18
layout: post
slug: maps-services-relations-and-reuse-eclipse-democamp-hamburg
title: 'Maps, Services, Relations and Reuse: Eclipse DemoCamp Hamburg'
wordpress_id: 148
categories:
- Conferences
- Eclipse
---

On November 10th, the Hamburg Eclipse DemoCamp took place in the [stylish EAST Hotel](http://east-hamburg.de/).
[

![EAST Hotel](http://farm4.static.flickr.com/3040/3030032841_0940054426.jpg)

](http://www.flickr.com/photos/81029262@N00/3030032841)

We had 45 reservations and almost all of them made it to the Camp. I always get a little bit nervous 15 minutes before the event starts if no more than 10 people have shown up so far, but I guess that's alright.

Martin and I welcomed the crowd on behalf of our sponsors [(Eclipse Foundation](http://www.eclipse.org), [froglogic](http://www.froglogic.com/), [it-agile](http://www.it-agile.de/) and [itemis](http://www.itemis.de)):
[

![Martin Lippert and Peter Friese welcome the crowd](http://farm4.static.flickr.com/3024/3030045195_f9cefec1a1.jpg)

](http://www.flickr.com/photos/81029262@N00/3030045195)

After that, Harald Wellmann of Harman Becker told us that the world is a disc. Well, at least he and his company try to make it a disc again - Harald leads a team that develops a so called "map compiler". A map compiler takes map source data and condenses that data by extracting only the relevant parts of it. As you may guess this is a long-running process which can hugely benefit from parallelization. Harald and his team use OSGi to modularize their software and make sure they use computing resources efficiently. One thing worth noting is that OSGi is even being used in car entertainment systems: Harald told us about one entertainment system which makes use of OSGi to act as an intermediate / glue layer between the UI (written in Java) and the core (written in C++).
[

![3022446041_2273030e59_b](http://farm4.static.flickr.com/3226/3030054785_56b25b784a.jpg)

](http://www.flickr.com/photos/81029262@N00/3030054785)

[Gerd Wütherich](http://gerd-wuetherich.de) (Independent) continued where Harald stopped and showed us how to use Spring Dynamic Modules and OSGi in his excellent talk - interspersed with some neat demos:
[

![Eclipse DemoCamp Hamburg](http://farm4.static.flickr.com/3013/3030910128_c0d80a4c35.jpg)

](http://www.flickr.com/photos/81029262@N00/3030910128)
Together with Nils Hartmann, Mathias Lübken and Bernd Kolb, he wrote the   first German book on OSGi, so he really knows what he is talking about.

After those talks, we took a break to grab some refreshments and take the chance to get in touch with the other attending Eclipse enthusiasts. I had the impression that everybody had a good time discussing all things Eclipse - in fact Martin and I had to interrupt a lot of lively discussions for the second run of talks.

In the first talk after the break, Miguel Garcia and Rakesh Prithiviraj (both Technical University of Hamburg-Harburg) gave us an update of their research on how to integrate LINQ (Language Integrated Queries) in Java:
[

![Eclipse DemoCamp Hamburg November 2008](http://farm4.static.flickr.com/3005/3030138183_9d68a22655.jpg)

](http://www.flickr.com/photos/81029262@N00/3030138183)[

![Eclipse DemoCamp Hamburg November 2008](http://farm4.static.flickr.com/3171/3030138427_514e449208.jpg)

](http://www.flickr.com/photos/81029262@N00/3030138427)

The final demo was deliverd by Stephan Herrmann who showed us Object Teams / Equinox, an amazing piece of software that can be used to re-use existing Eclipse plug-ins in an aspect-oriented way. To get an idea of how powerful this approach is, have a look at the following screenshot -  this is the JDT, but enhanced by Object Teams in order to support their very own syntax extensions for Java:
[

![AddUnimplementedMethods](http://farm4.static.flickr.com/3025/3030986548_6a084a5c01.jpg)

](http://www.flickr.com/photos/81029262@N00/3030986548)
To learn more about Object Teams, browse to their web site at http://trac.objectteams.org/ot/wiki/OtEquinox.
[

![3023294590_3c5d9c364b_b](http://farm4.static.flickr.com/3277/3030160123_ae092a0af3.jpg)

](http://www.flickr.com/photos/81029262@N00/3030160123)
Everyone in the room was quite impressed with what is possible with Object Teams / Equinox, so you should check it out ([it's available for free](http://www.objectteams.org/distrib/otdt.html)). If you can manage to go the [DemoCamp in Berlin](http://wiki.eclipse.org/Eclipse_DemoCamps_November_2008/Berlin), you'll have the chance to see it live.

The feedback we received from the attendees was great - some people even sent emails thanking us for organizing the event, so I guess the DemoCamp can be considered a success!
