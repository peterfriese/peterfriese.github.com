---
author: Peter
comments: true
date: 2008-10-25 08:42:47
layout: post
slug: mdsd-today-2008-recap
title: MDSD Today 2008 Recap
wordpress_id: 139
categories:
- Conferences
- Eclipse
- MDSD
---

From October 15 to October 17, [MDSD Today 2008](http://mdsd08.techjava.de) took place at the [Nordakademie](http://www.nordakademie.de) in [Elmshorn](http://www.google.com/maps?f=q&hl=de&geocode=&q=elmshorn,+k%C3%B6llner+chaussee+11&ie=UTF8&ll=53.753973,9.672866&spn=0.001922,0.005686&t=h&z=18) near Hamburg, Germany. The conference, which had been organized by [Frank Zimmermann](https://www.xing.com/profile/Frank_Zimmermann74) (Nordakademie), [Simon Zambrovski](https://www.xing.com/profile/Simon_Zambrovski) and [yours truly](https://www.xing.com/profile/Peter_Friese), was intended to bring together practioners, researchers and people from both industry and business. 

We were fortunate enough to aquire a number of well-known speakers, most notably [Axel Uhl](https://www.xing.com/profile/Axel_Uhl) (SAP) and [Ed Merks](http://ed-merks.blogspot.com/) (of EMF fame).

On the first day, Axel and Ed delivered their excellent keynotes on the current state of affairs in modeling. 

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3182/2944191296_a759638f4a.jpg)

](http://www.flickr.com/photos/81029262@N00/2944191296)

Ed commented on a number of misconceptions he has been confronted with during the last few years such as "modeling is too complicated", "modeling will just get into my way" or "modeling will limit my creativity". The bottom line of his talk was that although you might not realize it, models drive most software (just think of all the data models you're dealing with in business applications). However, using source code as a representation for your models might not always be the best solution - to gain both a better overview of your software and be more efficient with respect to software development, you should think about raising the level of abstraction, e.g. by using domain specific modeling tools that let you focus on the structure of your model instead of having to deal with (basically irrelevant) syntactical details of the target language. If working at a higher level of abstraction makes things more complicated for you, you're doing something wrong.

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3245/2944294118_0a18937386.jpg)

](http://www.flickr.com/photos/81029262@N00/2944294118)

Axel picked up this idea in his keynote on "Current Challenges for Industrial Software Development Tools and Languages" stating that any computer program can be conceived as a model. Since models are independent of their visual representation, they can be represented in various different forms, the most common being text, trees and diagrams of any kind. The question, however, whether to choose text or the abstract model as the primary artifact for storing. A textual representation clearly has advantages with respect to diffing and merging and can be handled with commonly used tools such as CVS nd plain text editors. Using the abstract model as the primary artifact allows for overlapping partial views. Since this model can persisted in some kind of repository, access to the model very much feels like working with a database. In particular, there's no need to parse any text back and forth between the model and an editor. The decision whether to store models as text or in a repository largely depends on boundary decisions, like the availability of the repository technology, the robustness of the mapping between the textual representation and the model, language characteristics, the number of developers who need to work on the model in parallel, the size of the software system built with the model and the life cycle of the software. Axel concluded that on one hand, modeling is not that much different from coding and that, on the other hand, DSLs are becoming increasingly important due to the complexity of UML.

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3028/2943396623_8d23d52dbf.jpg)

](http://www.flickr.com/photos/81029262@N00/2943396623)

One of the nice thing about conferences is you get to talk to people who are interested in the same topics as you are. MDSD Today has been no different - during the breaks you could see people involved in all kinds of discussions.

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3173/2943699993_a1b90778c6.jpg)

](http://www.flickr.com/photos/81029262@N00/2943699993)

Later that day, [Stefan Reichert](http://www.wickedshell.net/blog/) and Birger Garbe of [Lufthansa Systems](http://www.lhsystems.com) shared their experiences regarding model driven software projects with us. The topic of their talk was "Model Driven Software Development in Business Projects: Chance or Risk?" Their anwser to this question was that if've got the right tools, MDSD is a chance and can help you to build better software.

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3250/2944461460_d812725a1a.jpg)

](http://www.flickr.com/photos/81029262@N00/2944461460)[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3165/2944442780_b2c3a74a75.jpg)

](http://www.flickr.com/photos/81029262@N00/2944442780)

[Thomas Stahl](https://www.xing.com/profile/Thomas_Stahl4) of b+m concluded the day with his talk on how MDSD, BMP and SOA fit together:

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3067/2944562230_b78c3c74e0.jpg)

](http://www.flickr.com/photos/81029262@N00/2944562230)

The second and third day of the conference were crammed with hands-on tutorials on MDSD tools like [EMF](http://www.eclipse.org/modeling/emf/), [Xtext](http://www.xtext.org), [Xpand](http://www.openarchitectureware.org) and [GMF](http://www.eclipse.org/modeling/gmf/). Quite a lot of people attended the tutorials as you can see from the pictures. 

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3189/2948348881_8915cce730.jpg)

](http://www.flickr.com/photos/81029262@N00/2948348881)

Arno had to overcome the hardship of a broken beamer, so he had to use arms and legs to explain:

[

![MDSD Today 08, Elmshorn, Germany](http://farm4.static.flickr.com/3036/2949342540_2f5641809e.jpg)

](http://www.flickr.com/photos/81029262@N00/2949342540)

Overall, the conference was a real success and I am looking forward to seeing you all again at MDSD Today 2009!
