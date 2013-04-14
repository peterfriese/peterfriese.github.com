---
author: Peter
comments: true
date: 2009-05-05 19:18:47
layout: post
slug: advanced-wikitext
title: Advanced WikiText
wordpress_id: 240
categories:
- Eclipse
- Eclipse FAQ
- Publishing
- Stuff that rocks
---

In my [recent posting](http://www.peterfriese.de/getting-started-with-wikitext/), I showed you how to use [Mylyn WikiText](http://wiki.eclipse.org/Mylyn/Incubator/WikiText) to easily create documentation for your project.

In this installment, we're going to look at four things:



	
  1. How to create PDFs

	
  2. How to automate your publishing process

	
  3. How to split your documents to facilitate team work

	
  4. How to use DocBook as an intermediary format to add even more flexibility to your publishing process


Lots of stuff, so let's get started!


## How to create PDFs


The WikiText documentation states you can create DocBook, HTML and Eclipse Help from Wiki markup. So, how to create PDFs? Well, there are many ways to create PDFs. If you just want to create PDFs inside the IDE, you can download the [Textile-J PDF add-on](https://textile-j.dev.java.net/builds/wikitext/index.html) for WikiText. Textile-J is the predecessor of WikiText. The reason why the PDF output is not included in WikiText is that some of the required frameworks need to transform to PDF are not yet IP-approved (see bug [258349](https://bugs.eclipse.org/bugs/show_bug.cgi?id=258349) for more information).

To install the Textile-J PDF add-on, proceed as follows:



	
  1. Make sure you have WikiText 1.1.0 or better installed

	
  2. Add the Textile-J update site to your Update Manager: _https://textile-j.dev.java.net/builds/wikitext/site_

	
  3. Refresh the Update Manager. You should now see _Textile-J WikiText PDF_ in the list of available features.

	
  4. Select _Textile-J WikiText PDF_ and click _Install_

	
  5. Restart Eclipse as requested


You can now right-click on any wiki file (_*.textile_, _*.mediawiki_, _*.confluence_, _*.twiki_, _*.tracwiki_ and select _Textile-J -> Generate PDF_ from the context menu.


## How to automate the process


If you want to create PDFs outside the IDE (e.g. on your CI server using ANT), you need to create a build file:

    
    <project name="how-to-use-wikitext" default="wikitext2pdf" basedir=".">
    <property name="wikitext.standalone" value="lib"/><!-- path to wikitext standalone package -->
    <path id="wikitext.classpath">
    <fileset dir="${wikitext.standalone}">
    <include name="org.eclipse.mylyn.wikitext.*core*.jar"/>
    <include name="net.java.dev.textilej.wikitext.*core*.jar"/>
    <include name="net.java.dev.textilej.wikitext.*lib*.jar"/>
    </fileset>
    </path>
    <taskdef classpathref="wikitext.classpath"
            resource="org/eclipse/mylyn/wikitext/core/util/anttask/tasks.properties" />
    <taskdef classpathref="wikitext.classpath"
            resource="net/java/dev/textilej/wikitext/pdf/core/util/anttask/tasks.properties" />
    <target name="wikitext2pdf" description="Generate PDF from textile">
    <wikitext-to-pdf markupLanguage="Textile">
    <fileset dir="${basedir}">
    <include name="documentation.textile"/>
    </fileset>
    </wikitext-to-pdf>
    </target>
    </project>


By the way, there are ANT tasks for all other output formats as well, so you can automate the creation of Eclipse Help, HTML and DocBook, too:

    
    <wikitext-to-docbook markupLanguage="Textile">
    <fileset dir="${basedir}">
    <include name="documentation.textile"/>
    </fileset>
    </wikitext-to-docbook>




## How to split your documents to facilitate team work


Now that you know how to transform Wiki markup to various output formats, you might wonder how to split your Wiki files into smaller chunks so the members of your team can work on them separately. This is something which is very easy with, say, LaTeX or DocBook because they both have the concept of importing other documents. In Wikis, there usually is no such thing as importing another Wiki page - you rather _link_ to that other page. If you're writing a documentation, this isn't sufficient, however. You really need to be able to import document fragments.

Instead of adding the notion of imports to WikiText, I decided to control the assembling of the master document by using a little bit of ANT wizardry and a text file.

The text file (named _index.txt_ controls the order in which the document fragments ned to be glued together:

    
    introduction.textile
    grammar_language.textile
    newnoteworthy.textile


The _build.xml_ now needs to be enhanced so that it reads the control file and use this information to assemble the Wiki files in the order requested:

    
    <target name="assemble">
    <loadfile srcfile="doc/index.txt" property="inputfiles">
    <filterchain>
    <tokenfilter>
    <replacestring from="\n" to=","/>
    </tokenfilter>
    </filterchain>
    </loadfile>
    <concat destfile="documentation.textile" append="false" fixlastline="yes">
    <filelist dir="doc" files="${inputfiles}"/>
    </concat>
    </target>


So what happens here is that we read in the contents of the _index.txt_ file, concatenate all file names, separated with a comma to one large line and feed that large line into the _concatenate_ task that will then concatenate the contents of all those files in the given order so we get one big file.

After that, the big file can now be processed as usual:

    
    <target name="generate-pdf" depends="assemble" description="Generate PDF from textile">
    <wikitext-to-pdf markupLanguage="Textile">
    <fileset dir="${basedir}">
    <include name="documentation.textile"/>
    </fileset>
    </wikitext-to-pdf>
    </target>




## How to use DocBook to gain more flexibility


DocBook has been around for quite some years now and together with LaTeX still will give you the most flexibility when it comes to documentation engineering. I have been using both LaTeX and DocBook to write all sorts of documentation, ranging from my diploma thesis to open source project documentation and have found both the be versatile and reliable, but also quite verbose. Regarding flexibility, both give you the possibility to choose a document template / style of your liking. If none of the existing document styles match your needs, you can more or less easily create you own.

As WikiText supports creating DocBook from your Wiki files, we'll have a look at how to use this DocBook output to further process it. Here is what we need to do:



	
  1. Enhance our build script so it downloads the relevant DocBook files (libraries, basic styles and XSL transformations)

	
  2. Add targets to our build script that allow you to choose the output to be created: PDF, HTML or Eclipse Help


The whole process will look like this:
[


![WikiTextDocBook ToolChain](http://farm4.static.flickr.com/3599/3494324023_c4cf66ba24_o.png)


](http://www.flickr.com/photos/81029262@N00/3494324023)

Let's have a look at some crucial parts of the script.

We already saw how to assemble multiple Wiki files into one big Wiki file. The next step is to convert this file to DocBook:

    
    <target name="wikitext2docbook" depends="assemble" description="Generate DocBook from textile">
    <wikitext-to-docbook markupLanguage="Textile">
    <fileset dir="${basedir}">
    <include name="documentation.textile"/>
    </fileset>
    </wikitext-to-docbook>
    </target>


After that, we need to convert the DocBook file to PDF:

    
    <target name="docbook2pdf">
    <echo>Converting article to PDF...</echo>
    <delete file="${dest.dir}${file.separator}${documenat.name}.pdf"/>
    <delete file="${dest.dir}${file.separator}${documenat.name}.fo"/>
    <xslt in="${documenat.name}.xml"
            extension="xml"
            out="${dest.dir}${file.separator}${documenat.name}.fo"
            style="${document.stylesheet}">
    <factory name="org.apache.xalan.processor.TransformerFactoryImpl">
    <attribute name="http://xml.apache.org/xalan/features/optimize" value="true"/>
    </factory>
    <xmlcatalog>
    <entity
                    publicId="docbook.xsl"
                    location="${docbook.dir}${file.separator}fo${file.separator}docbook.xsl"/>
    </xmlcatalog>
    <param name="generate.toc" expression="book toc" />
    <param name="show.comments" expression="0" />
    <param name="header.rule" expression="1" />
    <param name="admon.graphics.extension" expression=".gif"/>
    <param name="admon.textlabel" expression="0"/>
    <param name="admon.graphics" expression="1"/>
    </xslt>
    <docbook2pdf
            source="${dest.dir}${file.separator}${documenat.name}.fo"
            target="${dest.dir}${file.separator}${documenat.name}.pdf"/>
    <delete file="${dest.dir}${file.separator}${documenat.name}.fo" />
    </target>


You can now control the output by supplying different stylesheets. DocBook XSLT comes with a variety of stylesheets for articles, books and online help systems. You can now easily create printed documentation AND online help from one source. DocBook XSLT supports EclipseHelp, JavaHelp, HTMLHelp (the Windows Help System) and even man pages!

So, next time you need to write documentation for a project you're working on, don't let it get you down. Instead, set up a trusty WikiText / DocBook toolchain and write down that documentation in Wiki markup in no time!

The whole build script and the sample document are available for [drain file 17 url download] for free. If you need help setting up a tool chain or want to book me for consulting, drop me a line (peter dot friese at itemis dot com).

Many thanks to [David Green](http://greensopinion.blogspot.com/) and [Steffen Pingel](http://steffenpingel.de/) of WikiText and Mylyn who tirelessly fixed bugs I found, applied patches and created EA builds!
