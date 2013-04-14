---
author: Peter
comments: true
date: 2009-04-23 23:34:54
layout: post
slug: getting-started-with-wikitext
title: Getting started with WikiText
wordpress_id: 229
categories:
- Eclipse
- Eclipse FAQ
- Stuff that rocks
---

Documentation is the bane of all developers. Nobody likes to write documentation, but most people know we need it. Even more so in Open Source projects. If you don't have a decent documentation, it will be hard to find contributors which will eventually stall the projects ability to acquire new committers.

While documenting code is discussed quite controversial (see [here](http://twitter.com/peterfriese/status/1573456711), [here](http://twitter.com/EelcoVisser/status/1573534530), [here](http://twitter.com/peterfriese/status/1574872897) and [here](http://twitter.com/EelcoVisser/status/1575307114)), most people seem at least to agree that a high level documentation outlining the concepts of the software in question **is** appropriate.

The options to write high-level documentation are:



	
  1. Word processors like Word, OpenOffice, Pages and the like

	
  2. LaTeX

	
  3. DocBook

	
  4. Wikis



Using word processors for writing high-level documentation has serious drawbacks, especially when a whole team needs to contribute to the documentation. Do you remember [master documents](http://word.mvps.org/fAQs/General/RecoverMasterDocs.htm)?

LaTeX is a great alternative, as it allows us to split documentation across several text files - this allows for efficient team work. Moreover, text files just cannot break and can be versioned / merged / diffed quite easily. However, LaTeX syntax is not for the faint of heart.

DocBook is great, too: it uses XML to describe documents and has a great number of transformations to about every output format you could think of ([PDF](http://www.sagehill.net/docbookxsl/PrintOutput.html), [HTML](http://www.sagehill.net/docbookxsl/HtmlCustomEx.html), [EclipseHelp](http://www.sagehill.net/docbookxsl/EclipseHelp.html), [HTML Help](http://www.sagehill.net/docbookxsl/HtmlHelp.html), [JavaHelp](http://www.sagehill.net/docbookxsl/JavaHelp.html) and even [man pages](http://www.sagehill.net/docbookxsl/RefentryToMan.html)! However, not everybody likes XML and in fact DocBook tends to be a little noisy.

So this leaves us with Wikis. They are great in many regards: you can write your documentation online, most wikis support versioning and diffing to a certain degree and most wikis have a decent collision detection that helps to work on documentation collaboratively. Thanks to offline Wiki editors for IDEs, we can even store wiki documents in local files. 



## Enter WikiText


[WikiText](http://wiki.eclipse.org/Mylyn/FAQ#WikiText) is such a tool. It actually is very flexible, as it allows you to use a multitude of wiki dialects to write your documentation. It also supports a number of output formats (HTML, Eclipse Help, DITA, DocBook and FOP). What's more, it features a really nice text editor that can display Wiki markup almost in a WYSIWYG fashion. WikiText is developed and maintained by [David Green](http://greensopinion.blogspot.com/).

So, without further ado, here is a quick introduction on how to install WikiText, writing your first document and transforming it to HTML.



## Installing


Make sure you have a recent version of Eclipse installed. Then:



	
  1. Point your update manager (_Help -> Install new software_) to the Mylyn Update Site (http://download.eclipse.org/tools/mylyn/update/weekly/e3.4)

	
  2. Select the most recent version of Mylyn WikiText (as of this writing, this is WikiText 1.1.0.I20090423-1700-e3x) and install


After the obligatory Eclipse restart, you can start using the WikiText editor.



## Writing your first document





	
  1. Create a new project (_File -> Project... -> General -> Project_

	
  2. Create a new file. We will be using the [Textile](http://textile.thresholdstate.com/) wiki syntax, so let's name it _HelloWorld.textile_

	
  3. Enter some text:
		
    
    
    h1(#id). An HTML first-level heading
    
    Textile syntax is really simple. You can _emphasize_ text or *emphasize it even more*.
    
    Scaled images:
    !{width: 50%}images/eiffelturm.jpg!
    
    Enumerations also are very easy:
    
    * An item in a bulleted (unordered) list
    * Another item in a bulleted list
    ** Second Level
    ** Second Level Items
    *** Third level
    
    # An item in an enumerated (ordered) list 
    # Another item in an enumerated list 
    ## Another level in an enumerated list 
    
    Let's have more headings:
    
    h2. An HTML second-level heading
    
    Here is a table:
    
    |_. Header |_. Header |_. Header |
    | Cell 1 | Cell 2 | Cell 3 |
    | Cell 1 | Cell 2 | Cell 3 |
    
    h2. An HTML third-level heading
    
    Here is some code:
    
    bc.. 
    package org.eclipse.workflow;
    
    public class Workflow {
    
    }
    
    p. Here is a plain old paragraph. It needs to start with "p." to mark the end of the code block above.
    
    
    h4. An HTML fourth-level heading
    
    Of course, we can also have hyperlinks: "Peter's homepage":http://www.peterfriese.de 
    
    h5. An HTML fifth-level heading
    
    h6. An HTML sixth-level heading
    		


	

	
  4. Create a new subfolder _images_ and place an image in his folder. I used [this](http://www.wiemann.eu/1/eiffelturm.jpg) one.



You can always switch over to the _preview_ pane, but you will notice that the source editor already does a decent job at rendering the text in a kind of WYSIWYG manner:
[

![WikiText markup and preview](http://farm4.static.flickr.com/3587/3469612438_469c476d6a.jpg)

](http://www.flickr.com/photos/81029262@N00/3469612438)



## Transforming your document to HTML


In order to convert your document to a nicely rendered HTML document, just right-click on _HelloWorld.textile_ in the package explorer and select _WikiText -> Generate HTML_. This will create anew file _HelloWorld.html_ in the same directory. Double-click it to open it with a browser:
[

![HelloWorld.html](http://farm4.static.flickr.com/3541/3469615654_a3fe0a9016.jpg)

](http://www.flickr.com/photos/81029262@N00/3469615654)



## Automating, PDF, splitting documents,...


I hope I could whet your appetite. In the next installment, I am going to show you how to automate the process of transforming WikiText documents into output documents, how to create PDF (by way of exploiting DocBook) and how to split your documents into smaller chunks so you can work on them as a team. Stay tuned!
