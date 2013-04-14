---
author: Peter
comments: true
date: 2009-06-30 00:17:56
layout: post
slug: getting-started-with-xtext
title: Getting Started with Xtext
wordpress_id: 284
categories:
- DSLs
- Eclipse
tags:
- english
---

[Xtext](http://www.xtext.org) has been released as a part of the Eclipse Galileo release train on June 24th, 2009. Xtext is a framework for building DLSs (**d**omain **s**pecific **l**anguages). In fact, it can be seen as a DSL for defining DSLs. 

In this article, we will develop a small DSL for defining entities. 



## Download and Install



Hop over to [http://xtext.itemis.com](http://xtext.itemis.com) and download an Xtext distribution matching your platform. We've got all major platforms: Windows, Mac OSX Carbon, Mac OSX Cocoa 64, Mac OSX Cocoa 32, Linux Gtk 64, Linux Gtk 32.

To install, unzip the distribution file to a directory of your liking. 

**Windows users, please make sure you unzip to a directory near to your filesystem root! Eclipse contains files and folders with long names that might be hard to handle for Windows.**
<!-- more -->


## Create a new project



After launching Eclipse and creating a new workspace (or using an existing one), create a new Xtext project by selecting _File -> New... Project... -> Xtext Project_. In this article, we're creating a DSL for describing entities, so let's go with the following settings:



	
  * Main project name: _org.xtext.example.entity_

	
  * Language name: _org.xtext.example.Entity_

	
  * DSL-File extension: _entity_

	
  * Create generator project: _yes_


Click _Finish_ to let Xtext create the three projects that make up your DSL:



	
  * org.xtext.example.entity - this project contains the DSL itself, including all back end infrastructure like the parser and the meta model.

	
  * org.xtext.example.entity.ui - as the name implies, this project contains all UI centric code of the DSL: the editor, the outline, content assist and so forth

	
  * org.xtext.example.entity.generator - this project contains a code generator which will transform the DSL scripts (aka models) you write in your DSL into something useful. In our example, we will create some POJOs and DAOs from our models.


Upon finishing creating these three projects, Xtext opens a file _Entity.xtext_, which contains a sample grammar, which we will change in a second.



## Define the grammar for your DSL


Next up, we need to define the grammar for our DSL. To make things easier for us, let's first write down a sample model. Please open _org.xtext.example.entity.generator/src/model/MyModel.entity_ and key in the following text:

    
    
    typedef String
    typedef Integer
    typedef Date mapsto java.util.Date
    
    entity Person {
      String name
      String surName
      Date birthDay
      Address home
      Address work
    }
    
    entity Boss extends Person {
      Person* employees
    }
    
    entity Address {
      String street
      String number
      String city
      String ZIP
    }
    


Most of this model should be pretty obvious, but there are two things worth noting:



	
  * We define our own list of data types to gain a certain degree of flexibilty, i.e. to map them to different types in the host language, as can be seen for the _Date_ data type, which gets mapped to _java.util.Date_ (we might also decide to map it to _java.sql.Date_)

	
  * The star (*) denotes multiplicity. We might also have chosen square brackets (_Person[] employees_) or something completely different - it's largely a matter of taste.



Let's derive the grammar for this model. Open _org.xtext.example.entity/src/org/xtext/example/Entity.xtext_, erase its entire contents and enter the following:

    
    
    grammar org.xtext.example.Entity with org.eclipse.xtext.common.Terminals
    generate entity "http://www.xtext.org/example/Entity"
    


The first line indicates we want the new grammar to be derived from the (built-in) grammar _Terminals_, which defines some basic terminal rules (like STRING, ID and so forth). If you're interested, _CTRL+Click_ on the language name to navigate to its definition.

The second line defines the name and namespace URI for our own grammar.

Let's now define that our DSL supports types. Add the following lines:

    
    
    Model:
      (types+=Type)*;
    
    Type:
      TypeDef | Entity;
    


This tells Xtext that our _Model_ contains any number (i.e. _0..N_, as declared by the _*_) of _Type_s. What exactly a _Type_ is needs to be specified. Apparently, a _Type_ can be either a _TypeDef_ or an _Entity_:

    
    
    TypeDef:
      "typedef" name=ID ("mapsto" mappedType=JAVAID)?;	
    


A _TypeDef_ starts with the keyword _typedef_, followed by an _ID_ making up its name. Following the name, we can optionally (hence the question mark at the end) add a _mapsto_ clause. The fragment _mappedType=JAVAID_ specifies that the _TypeDef_ will later have an attribute named _mappedType_ of type JAVAID. As JAVAID is not yet defined, we need to do so:

    
    
    JAVAID:
      name=ID("." ID)*;
    


So, a _JAVAID_ is a sequence of _ID_s and dots, making up a qualified Java type name, such as _java.util.Date_.

Next, let's define how to model entities:

    
    
    Entity:
      "entity" name=ID ("extends" superEntity=[Entity])?
      "{"
        (attributes+=Attribute)*
      "}";
    


As you might have guessed, _Entity_s start with the keyword _entity_, followed by an _ID_ as their name. They may optionally extends another entity. Surrounding a rule call with square brackets means "this is a cross reference", i.e. a reference to an already existing element.

_Entity_s do have _Attribute_s (any number, to be precise), thus we have to define how _Attribute_s look like:

    
    
    Attribute:
      type=[Type] (many?="*")? name=ID;
    


By now, you should be able to read this rule: an _Attribute_ has a _type_ which is a cross reference to a _Type_ (which is either a _TypeDef_ or an _Entity_), it has an optional multiplicity indicator (the star) and - of course - if has a name.

Your grammar should look like this by now:

    
    
    grammar org.xtext.example.Entity with org.eclipse.xtext.common.Terminals
    
    generate entity "http://www.xtext.org/example/Entity"
    
    Model:
      (types+=Type)*;
    
    Type:
      TypeDef | Entity;
      
    TypeDef:
      "typedef" name=ID ("mapsto" mappedType=JAVAID)?;
      
    JAVAID:
      name=ID("." ID)*;
      
    Entity:
      "entity" name=ID ("extends" superEntity=[Entity])?
      "{"
        (attributes+=Attribute)*
      "}";
      
    Attribute:
      type=[Type] (many?="*")? name=ID;
    





## Compiling the DSL


Now it is time to see the fruits of our labor. But first, we need to compile our grammar. Xtext will create:



	
  * a parser

	
  * a serializer_
	
  * an Ecore meta model

	
  * a full blown Eclipse editor


from this grammar. To make this happen, please select _org.xtext.example.entity/src/org/xtext/example/GenerateEntity.mwe_ and select _Run As -> MWE Workflow_ from the context menu. Xtext will now generate the entire infrastructure for your DSL and after a few seconds you should have a shiny new DSL including a great editor.



## Taking it for a spin


Seeing is believing, so let's take the DSL editor for a test drive. Select the DSL project _org.xtext.example.entity_ and, from the context menu, select _Run As -> Eclipse Application_. A second instance of Eclipse will be started.

In this new instance, create a new, empty project (_File -> New -> Project... -> General -> Project_. Create a new file _Sample.entity_ in the new project.

You can now insert the model we designed above or enter a new model:
[

![Getting Started With Xtext](http://farm3.static.flickr.com/2574/3673405506_09fed234e1.jpg)

](http://www.flickr.com/photos/81029262@N00/3673405506)



## Leveraging the model


Now that we've got a fancy editor for our DSL, we want to transform the models we can create with this editor into something meaningful. This, however, will be the topic of the next installment.



## More info


Feel free to discuss this article in the comments section of [my blog](http://www.peterfriese.de). Should you have any technical questions regarding Xtext, we've got an [excellent newsgroup](http://www.eclipse.org/newsportal/thread.php?group=eclipse.modeling.tmf) where the committers answer your questions. We (itemis) also offer [professional (commercial) support](http://xtext.itemis.com/xtext/language=en/23946/professional-services), i.e. customized trainings to get your team up to speed. Just [drop us a note](mailto:peter.friese@itemis.de), we're happy to discuss the details with you.



## Download the code


You can check out the code from [this SVN repository location](http://code.google.com/p/peterfriese/source/browse/xtext_getting_started/tags/xtext_getting_started_1/).
