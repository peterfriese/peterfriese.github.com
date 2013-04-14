---
author: Peter
comments: true
date: 2009-03-31 15:44:18
layout: post
slug: modeling-is-dead-long-live-modeling
title: Modeling is dead. Long live Modeling!
wordpress_id: 218
categories:
- Conferences
- Eclipse
- EMF
tags:
- english
---

Although [some people seem to be convinced](http://kenn-hussey.blogspot.com/2009/03/on-death-of-eclipse-and-modeling.html) that [Eclipse](http://www.eclipse.org/) and [Eclipse Modeling](http://www.eclipse.org/modeling/) are dead, I have been sensing quite healthy life signals of both on EclipseCon 2009 (or rather [#eclipsecon](http://search.twitter.com/search?q=%23eclipsecon)).

On Monday, [Bernd](http://www.flickr.com/photos/sza/3385548232/), [Kenn](http://kenn-hussey.blogspot.com/) and I hosted the [official Modeling BOF](http://www.eclipsecon.org/2009/sessions?id=780):
[

![Modeling BOF](http://farm4.static.flickr.com/3648/3383866773_ffa5c03596.jpg)

](http://www.flickr.com/photos/81029262@N00/3383866773)
The topics discussed were focussing on consumability and long-term sustainability of EMF:

**Documentation and Marketing**

We all agreed that the entire modeling project lacks a decent documentation. While of course all of the projects and subprojects do have their own documentation, there is so such thing as an overview documentation. Modelers as we are, we quickly realized we're lacking a documentation superstructure. The idea is to provide a guide for users (newbies and seasoned experts alike) to navigate the Eclipse Modeling world. The modeling project offers a great variety of technologies, so it is not always easy to find out which set of technologies / frameworks you might want to use - especially if you are new to EMF.

We also agreed that we need more success stories (in the form of blog entries, articles and shiny flyers for the [modeling landing page](http://www.eclipse.org/home/categories/index.php?category=modeling)). A quick poll revealed that more than 90% of all people attending have been working on projects in which modeling was successfully applied, so there should be enough material to create success stories!

Kenn mentioned the Eclipse Foundation wants to start a documentation project (much like [Babel](http://www.eclipse.org/babel/)) to improve overall documentation, we might eventually join this project. In the meantime, we decided to form a working group comprised of members of the Eclipse Modeling Project that starts to create this overview documentation. The initial members of this working group will be Kenn, Bernd, Peter, Tamer, Pierre, Marcello, Thibault and Anthony (guys, please send me your email addresses). The working group will start its work on the [EMF dev mailing list](https://dev.eclipse.org/mailman/listinfo/emf-dev). You are welcome to join us there.

**Consuming Modeling Technology**

I think there is no other technology that causes more emotions in an Eclipse committers life than P2. So we also had our little discussion about how to best provision Modeling technology. No big news here - we just re-iterated the pros and cons of using ZIPs vs. P2 and also had a short discussion about the Friends of Eclipse Download Wizard (which, rumor has it, will allow you to assemble a custom distro by just selecting the pieces you need). Several people mentioned that [Amalgamation](http://www.eclipse.org/modeling/amalgam/) is intended to be a place for modeling distros.

**EMF 3.0**

We also discussed whether we should have a new major release of EMF that allows us to break API. Though not everybody agreed that we need a new major version (because the current version works smoothly for most people) we decided to set up an additional BOF to discuss this topic in more detail. I will write about the EMF 3 BOF later (yes, I am teasing)

**Miscellanea**

Someone asked if we need something like a model bus, the answer was no - we've got the [modeling workflow engine (MWE)](http://www.eclipse.org/modeling/emft/?project=mwe).

We also briefly discussed the recent change of leadership on the MDT OCL component ([Christian Damus](http://give-a-damus.blogspot.com/) [revoked](http://dev.eclipse.org/mhonarc/lists/modeling-pmc/msg01042.html) his committer and component lead status recently). Everybody agreed that to lead a component, a long-term committment is needed (which has been met by Christian in the most excellent way). We also agreed that we need to make avoid situations in which a whole bunch of projects / components suddenly are without a leader. This will be a task for the PMC.
[

![Modeling BOF](http://farm4.static.flickr.com/3629/3383867215_982c6592ac.jpg)

](http://www.flickr.com/photos/81029262@N00/3383867215)

After the official part of the BOF, there still was time to chat and shake hands with people you never met in person before. I had the pleasure to shake hands with Anthony Hunter, who works for IBM. From our discussions on mailing lists and bugs, you could guess that we have a totally different view on the marketing of software, but it turns out we're actually not that far apart :-)

[BOFs](http://en.wikipedia.org/wiki/Birds_of_a_Feather_(computing)) are a great way to interact with like-minded people at conferences. So, next time you go to a conference, make sure you participate in a BOF to make yourself be heard and take part in the discussion.
