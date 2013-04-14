---
author: Peter
comments: true
date: 2008-11-20 11:49:27
layout: post
slug: ese-building-refactoring-tools-with-ltk-and-ludwig
title: 'ESE: Building Refactoring Tools with LTK and Ludwig'
wordpress_id: 150
categories:
- Conferences
- Eclipse
---

[Jeffrey Overbey](http://jeff.over.bz/) talked about building refactoring tools using the LTK and his research work "Ludwig".

I guess you all agree with me that refactoring support is crucial for any programing language, as you need to restructure your code rom time to time. 

Although Eclipse [LTK](http://www.eclipse.org/articles/Article-LTK/ltk.html) offers a decent [API](http://help.eclipse.org/stable/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/api/org/eclipse/ltk/core/refactoring/package-summary.html) and UI to realize refactoring tools, all the heavy lifting (parsing the text in question, analyzing the AST, creating rewrite rules) is left to you.

Jeff's research tool Ludwig can help to derive refactoring support from a BNF-style grammar very easily. Ludwig essentially derives a lexer/parser from the grammar and provides an interface to the AST which gets constructed by the parser. Which gives you the chance to walk / visit the AST and (partially) rewrite the AST.

I really enjoyed Jeff's presentation style. He used a set of slides and pre-recorded screenvideos to drive his talk. I think this is great idea, as it basically eliminates any problems you might run into if you do a live demo. Plus, it gives the presenter the chance to face the audience while demoing, instead of mumbling into the screen :-)

[

![Eclipse Summit Europe](http://farm4.static.flickr.com/3154/3045729300_77548dceed.jpg)

](http://www.flickr.com/photos/81029262@N00/3045729300)
