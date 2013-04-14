---
author: Peter
comments: true
date: 2009-05-27 01:13:11
layout: post
slug: towels-models-and-bundles-eclipse-democamp-in-hamburg
title: 'Towels, Models and Bundles: Eclipse DemoCamp in Hamburg'
wordpress_id: 266
categories:
- Conferences
- Eclipse
---

Whether it was just pure coincidence or destiny - yesterdays [DemoCamp](http://wiki.eclipse.org/Eclipse_DemoCamps_Galileo_2009/Hamburg) in Hamburg happened to fall on the same day as [Towel Day](http://en.wikipedia.org/wiki/Towel_day). 

Although we had quite a bunch of [attendees (I counted 40)](http://wiki.eclipse.org/Eclipse_DemoCamps_Galileo_2009/Hamburg#Who_Is_Attending), only one brought his Towel.

[

![Heiko Behrens - Xtext](http://farm4.static.flickr.com/3637/3568313106_060ee9ff19.jpg)

](http://www.flickr.com/photos/81029262@N00/3568313106)

Thanks to our speakers, Martin and I were able to put together a really exciting program:



	
  1. Moritz Eysholdt, Patching Models and Evolving Meta Models

	
  2. Heiko Behrens, TMF Xtext

	
  3. Simon Zambrovski, Common Navigator Framework

	
  4. Marco Mosconi, Modular EMF/GMF customization with ObjectTeams/Java

	
  5. Markus Alexander Kuppe, Distributed OSGi (RFC 119) - The ECF way



Moritz presented the result of his master's thesis on the topic of metamodel evolution and explained how models can evolve together with their metamodels. This might also be of interest for data modeling.

[

![Moritz Eysholdt - Metamodel Evolution](http://farm4.static.flickr.com/3380/3568306510_deaab05a74.jpg)

](http://www.flickr.com/photos/81029262@N00/3568306510)

Heiko delivered a great presentation on the upcoming version of Xtext. He used a [well-known example](http://www.slideshare.net/HeikoB/xtext-und-was-man-damit-anstellen-kann) that everybody could relate to (no, not entities and services!) so it was really easy to understand how domain specific languages work and how Xtext can help to implement a domain specific tool chain.

[

![Heiko Behrens - Xtext](http://farm4.static.flickr.com/3329/3568309918_6eb0eed6f4.jpg)

](http://www.flickr.com/photos/81029262@N00/3568309918)

Simon presented on the Common Navigator Framework - a powerful framework which helps you to assemble tree views in a mostly declarative manner. Simon was bold enough to do a live coding session, but the [demo gods](http://www.rdrop.com/users/paulmck/DemoGods/) didn't smile on him, so he showed us the prepared solution, which was still very convincing. Simon prepared an article about the CNF on his blog, so if you're interested, [drop by](http://www.techjava.de/topics/2009/04/eclipse-common-navigator-framework/) - it's worth a read.
[

![Simon Zambrovski - CNF](http://farm4.static.flickr.com/3632/3568319930_a2c97d90af.jpg)

](http://www.flickr.com/photos/81029262@N00/3568319930)

After the break, Marco Mosconi showed how [ObjectTeams/Java](http://www.objectteams.org/) can be used to non-invasively customize generated code. As a real-world example he used the UML diagrams from the Eclipse [UML2Tools](http://www.eclipse.org/modeling/mdt/?project=uml2tools) project. The [UML2Tools](http://wiki.eclipse.org/MDT-UML2Tools) project uses [GMF](http://www.eclipse.org/modeling/gmf/) to generate the diagram code based on the UML2 metamodel (implemented as an ECore model). A rather large number of generated artifacts have to be customized to make sure the diagrams behave correctly. Marco showed how this is especially required for associations (I find it kind of funny how everybody uses associations to explain how complicated the UML metamodel is). The UML2Tools class diagram has been customized in at least 200 different locations, so you get a big maintenance problem (generated and non-generated code are mingled). With ObjectTeams, it is possible to extract all customization code into so called teams which then are able to enhance the generated code in an aspect oriented way. In a way, ObjectTeams is like AOP on steroids.
[

![Marco Mosconi - ObjectTeams](http://farm4.static.flickr.com/3644/3568322828_677d4a889a.jpg)

](http://www.flickr.com/photos/81029262@N00/3568322828)

The official part of the DemoCamp was concluded by a talk held by [Markus](http://www.ohloh.net/accounts/lemmy) [Kuppe](http://www.lemmster.de/blog/) on Distributed OSGi. Markus briefly explained the architecture behind Distributed OSGi and gave a demo of an Eclipse RCP application that communicated with an iPhone (which ran an Equinox server). Markus used the [Eclipse Communication Framework (ECF)](http://www.eclipse.org/ecf/) to achieve this - ECF contains a provider for Distributed OSGi. 
[

![Markus Kuppe - Remote OSGi](http://farm3.static.flickr.com/2483/3568326016_0e6b01375e.jpg)

](http://www.flickr.com/photos/81029262@N00/3568326016)

Eclipse DemoCamps are social events and aim at creating room for discussion and interaction. So, after the official part of the DemoCamp, we headed down to the EAST Bar to have some nice chats over some frosty beverages (I definitely recommend "Cool Mango").

Feedback has been very positive so far and I surely hope everybody enjoyed the DemoCamp as much as I did. Thanks to the [Eclipse Foundation](http://www.eclipse.org/), [it-agile](http://www.it-agile.de/) and [itemis](http://www.itemis.com/) for making this event possible by sponsoring the location and the drinks!

If you feel you missed out, drop [me](http://twitter.com/peterfriese) or [Martin](http://twitter.com/martinlippert) a mail so we can invite you the next time around.
