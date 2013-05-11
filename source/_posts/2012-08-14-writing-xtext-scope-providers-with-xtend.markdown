---
comments: true
date: 2012-08-14 22:04:55
layout: post
slug: writing-xtext-scope-providers-with-xtend
title: Writing Xtext Scope Providers with Xtend
wordpress_id: 922
categories:
- DSLs
- Xtext
asins:
- "1481218581" # DSL Engineering: Designing, Implementing and Using Domain-Specific Languages
- "1935182455" # DSLs in Action
- "0321712943" # Fowler: Domain Specific Languages
---


{% img left /images/2012-08-14-writing-xtext-scope-providers-with-xtend/scope_150x150.jpg %}

[Scope][1] is an [important concept][2] in the design of programming language. In Xtext, scoping is used [to drive two major parts][3] of your DSL: [linking][4] and [content assist][5]. While Xtext applies the [80/20 rule][6] very successfully, thereby providing you with a decent scoping implementation out of the box, eventually you'll have to roll up your sleeves and write your own scope provider.

  [1]: http://en.wikipedia.org/wiki/Scope_(computer_science)
  [2]: http://phobos.ramapo.edu/~amruth/grants/problets/courseware/scope/home.html
  [3]: http://zarnekow.blogspot.de/2009/01/xtext-corner-2-linking-and-scoping.html
  [4]: http://www.eclipse.org/Xtext/documentation.html#linking
  [5]: http://www.eclipse.org/Xtext/documentation.html#contentAssist
  [6]: http://en.wikiquote.org/wiki/Alan_Kay

<!-- more -->

Over the years, implementing scope providers has become significantly easier, much thanks to the tiny internal scoping DSL provided by `org.eclipse.xtext.scoping.Scopes` ([src][7]). However, collecting the elements that make up a scope can be a very cumbersome task if you have to use Java. Let’s face it - [Java has not been built for traversing models.][8]

  [7]: https://github.com/eclipse/xtext/blob/v2.3.0/plugins/org.eclipse.xtext/src/org/eclipse/xtext/scoping/Scopes.java
  [8]: http://www.cafeaulait.org/slides/hope/02.html

A language much better suited to traversing models is [Xtend][9], the newest kid on the X block. If you didn’t yet give it a try, you should do so now - even if you’re not writing an Xtext DSL. Xtend directly translates to Java code and allows you to write very concise code by eliminating much of Java’s ceremony. You can use it to write everything you’d write in Java - from [Android UIs][10] to [web applications][11] - so why not write Eclipse plug-ins with Xtend?
  
  [9]: http://www.eclipse.org/xtend/
  [10]: http://blog.efftinge.de/2011/12/writing-android-uis-with-xtend.html
  [11]: http://www.eclipse.org/Xtext/7languagesDoc.html#httpRouting

To write the scope provider for your language using Xtend, you need to follow a few very simple steps:

1. Disable generating the scope provider stub. Open `Generate<YourDSL>.mwe2` in the DSL project and find the line 
    
		fragment = scoping.ImportNamespacesScopingFragment { }

2. Change this line to
    
		fragment = scoping.ImportNamespacesScopingFragment {
			generateStub = false
		}

3. If you already have a Java-based scope provider, copy the implementation to a safe place and delete the Java file to make room for the new Xtend-based scope provider.

4. Create a new Xtend class `<YourDSLPackage>.<YourDSL>ScopeProvider.xtend` in the DSL project. Make sure this class inherits from `org.eclipse.xtext.scoping.impl.AbstractDeclarativeScopeProvider`.

5. Start writing your scope provider using Xtend.

6. Don’t forget to smile :-)




