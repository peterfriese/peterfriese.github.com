---
author: Peter
comments: true
date: 2008-08-05 13:13:21
layout: post
slug: some-thoughts-on-the-merits-of-model-driven-software-development
title: Some thoughts on the merits of model driven software development
wordpress_id: 119
categories:
- Eclipse
- MDA
- MDSD
---

Scanning my RSS feeds today, I came across [this](http://katastrophos.net/magnus/blog/2008/08/03/is-model-driven-development-faster/) post by Magnus Jungsbluth in which he deals with the question "Is Model Driven Development Faster?".  As Magnus points out, the question whether you can be faster by using model driven development points in a wrong direction. Just being faster is not a major goal of model driven development. It's the quality that counts. Model driven development helps you to concentrate on the relevant parts of your software (i.e. the business logic) instead of having to care too much about the architectural plumbing around the business logic. Yes, using a code generator saves you some time. Precious time you will be able to invest into other parts of the software you build. This will amount to a higher quality of your software.  Magnus mentions some more prejudices people have towards model driven software development. Here are a few:


#### "Higher level of abstraction = more complicated"


Now, this is plain wrong. Model driven approaches do not aim at making things more complicated. They aim at making things simpler and easier to achieve. This is what we mean by "raising the level of abstraction". I guess it's the word "abstract" which makes most people think things become more complicated.  [Wikipedia states that](http://en.wikipedia.org/wiki/Abstraction) "_**Abstraction** is the process or result of generalization by **reducing the information content** of a concept or an observable phenomenon, typically in order to **retain only information which is relevant** for a particular purpose._" This definition makes clear that abstraction helps to _simplify_ things in order to concentrate on only the relevant parts.

Consider a subway map: it is an abstraction of a geographical map, leaving out all the superfluous information, just retaining what is needed to find your way in the underground. The subway map is a **domain specific** map, explicitly aimed at a special purpose. If you need to find your way by car, you'd need a different kind of map.

[![Subway map](http://www.peterfriese.de/wp-content/subway.png)](http://www.peterfriese.de/wp-content/subway.png)

Abstraction in the context of model driven software development means to leave out the superfluous details and concentrate on the relevant parts. Let me give you an example: If you are writing  a software for your HR department, you really shouldn't need to care about writing getters and setters and how exactly to write that m:n relationship mapping in hibernate. Instead, you should concentrate on the names and types of the attributes and the fact that a certain relationship indeed is an m:n relationship.


#### "MDSD / DSLs enable business users to write software"


... do business users (i.e. the people who are going to be using your software) want to write software on their own at all? I don't think so. Modern business live is organized by division of labour, which is a good thing because it allows us to do the things we're good at and specialize ourselves.

Although I don't think business users should programm their own software using MDSD or DSLs, I believe that using MDSD and DSLs will help to bridge the gap between developers and business users. Much is gained if a developer and a business user can look at a graphical or textual model and discuss the business requirements in  language both understand. It is the task of software architects to ensure that the models being used can be written in this very domain specific language.

Oh, and please resist the temptation to provide a graphical language that tries to mirror a textual DSL one-to-one. Graphical syntaxes and textual syntaxes are very different. Things that can very easily be expressed in a graphical language  might look very awkward when mapped to a textual language and vice versa. Don't get me wrong - there's nothing evil about using different concrete syntaxes to provide views on one common / shared model. However, graphical languages are better suited to provide an overview of a system or module, whereas textual languages are better suited to flesh out the details.


#### "Versioning is tough, DSLs do not support versioning"


While it certainly is true that traditional UML tools are not that well suited for collaborative work (much due to the fact that most tools store their models either in a proprietary format or in monster XMI files), this is not true for external textual DSLs.

When using a textual external DSL, your model is plain text. Thus, your models can now be very easily stored in a version control system such as Subversion or CVS. And of course they can be diffed and merged. Using textual DSLs feels very "natural" to programmers after all.


#### Conclusion


So, to sum up: model driven development is neither some kind of obscure art which takes you years to learn and understand. Nor does it make things more complicated. It aims at aking your life easier. It does not neccesarily _save_ you time, but it will help you to better _use_ your time to build better software. One of the reasons your software will be better than before is that your business users will be able to discuss with you using a language you both understand.

That's what model driven software development is about.
