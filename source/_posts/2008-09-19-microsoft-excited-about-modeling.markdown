---
author: Peter
comments: true
date: 2008-09-19 14:04:59
layout: post
slug: microsoft-excited-about-modeling
title: Microsoft excited about modeling
wordpress_id: 125
categories:
- Eclipse
- MDA
- MDSD
---

Microsoft [seem to be](http://clemensreijnen.nl/post/2008/09/Microsoft-joins-the-OMGhellip%3b.aspx) [quite excited](http://www.pluralsight.com/community/blogs/dbox/archive/2008/09/06/oslo.aspx) about modeling these days. Just recently, [they announced that](http://www.microsoft.com/presspass/press/2008/sep08/09-10OMGModelingPR.mspx) [they have joined the OMG](http://blogs.msdn.com/stevecook/archive/2008/09/11/microsoft-joins-the-omg.aspx). OMG does NOT stand for Old Modeling Guys, but for [Object Management Group](http://www.omg.org/). This is the standards body that brought you specifications like [CORBA](http://www.corba.org/), [UML](http://www.uml.org/), [MDA](http://www.omg.org/mda/) and [BPMN](http://www.omg.org/bpmn-logo/), to name just a few. 





But Microsoft does not just talk about modeling, they also put their money where their mouth is: during the last few years, a number of people including [Steve](http://blogs.msdn.com/stevecook/) [Cook](http://www.domainspecificdevelopment.com/Bios/SteveCook.aspx) and [Stuart](http://blogs.msdn.com/stuart_kent/) [Kent](http://www.domainspecificdevelopment.com/Bios/StuartKent.aspx) have been working on a toolchain which first has seen the light of day under the somewhat cryptic name [VSX DSL Tools](http://msdn.microsoft.com/en-us/vsx/cc677256.aspx).  





VSXDSL Tools are really good at rapidly creating (simple) graphical modeling tools. However, when it came to model transformations and code generation, you were stuck with a somewhat dated technology called [T4 (Text Templating Transformation Toolkit)](http://msdn.microsoft.com/en-us/library/bb126445.aspx) - a template language quite similar to JSP and ASP. [T4](http://www.olegsych.com/2007/12/text-template-transformation-toolkit/) seriously lacks some important features, like polymorphic dispatch, support for multiple file output and - believe it or not - a decent editor. Well, you can get one from [http://www.t4editor.net/](http://www.t4editor.net/), but who likes to shell out 99 dollars for just an editor?  





Now, Microsoft seem to have listened to their users and are coming up with an updated and extended version of the DSL Tools, code-named Oslo.  





So what is Oslo? [Douglas Purdy states that Oslo is](http://douglaspurdy.com/2008/09/06/what-is-oslo/):



	
  * _A tool that helps people define and interact with models in a rich and visual manner _

	
  * _A language that helps people create and use textual domain-specific languages and data models _

	
  * _A relational repository that makes models available to both tools and platform components  Looks to me like they are addressing all the issues MS DSL tools had._








At [PDC 2008](http://www.microsoftpdc.com/), the team will reveal Oslo and all its nice features. 





Although I am an [Eclipse fan](http://www.eclipse.org/pde/pde-ui/committers/committers.php#contributors) and an [openArchitectureWare](http://www.openarchitectureware.org/) committer, I am quite thrilled so see Microsoft make this move to embrace modeling. In my opinion, this will advance MDSD and DSLs quite a deal. A lot of people who may have never heard of DSLs and MDSD will now get in touch with those techniques due to the sheer marketing power of Microsoft. This will also bring some nice competition to the market, which always is a Good Thing (tm).  





The [press release](http://www.microsoft.com/presspass/press/2008/sep08/09-10OMGModelingPR.mspx) [states that](http://blogs.msdn.com/architectsrule/archive/2008/09/10/microsoft-joins-omg.aspx) "to make model-driven development a reality, Microsoft is focused on providing a model-driven platform and visual modeling tools [...]"  - So, welcome to the modeling world, Microsoft!



