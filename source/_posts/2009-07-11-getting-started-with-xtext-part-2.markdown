---
author: Peter
comments: true
date: 2009-07-11 22:48:56
layout: post
slug: getting-started-with-xtext-part-2
title: Getting started with Xtext, part 2
wordpress_id: 298
categories:
- DSLs
- Eclipse
tags:
- english
---

Last week, [I showed you how easy](http://www.peterfriese.de/getting-started-with-xtext/) it is to create a DSL with [Xtext](http://www.xtext.org). In this installment, we will have a look at how to leverage the models created with the DSL.



### Goal


Let's imagine we want to create an application for orders. People can sign in to the system, place orders for various items, check out and have them sent to their address. Very simple, but we can show a lot of things here.

As we expect to be writing more than one application of this type and as we also would like to be able to express the structure of the application on a business level (one of the major drivers for DSLs and MDSD for that matter), we come up with the idea of using a DSL to describe what the application does. Defining the DSL is what we did last week. This week, we need to map the concepts of the DSL to some code and some APIs we're going to program against.
<!-- more -->
So, we're going to create a set of code templates for a code generator that can then read our DSL models and create persistence code for us.



### Prepare


In order to draw any benefit from this post, you need to have a running version of the DSL we created last time round. If you're new to this series, please go back one week and follow along the steps outlined in last week's article. It shouldn't take you longer than 30 minutes, and I promise we'll wait here for you.



### Create a main template


As we're probably going to create more than one kind of class (we'll have Entity beans, DAOs and maybe some interfaces), it is a wise idea to separate all those templates into separate files. The code generator then needs to invoke all those template files in order to generate our application. To make things more maintainable, every code generator should have a main entry point, so let's create a main template file which serves as this main entry point.

First, please _delete_ everything in _org.xtext.example.entity.generator/templates_. Then, create a new _Xpand_ template in this directory by selecting _File -> New -> Other ... -> Xpand Template_. This is going to be our main template, so let's call it _Main.xpt_. An empty editor will open.

Insert the following line:

    
    «IMPORT entity»


In case you wonder how to insert the special quotation marks, use code completion (press _CTRL + Space_, or _CMD + Space_ on a Mac) to insert them. Upon activating code completion, a dialog will appear, asking you whether to add the _Xpand_ nature to the project. Please confirm this dialog.

The _IMPORT_ statement imports the metamodel of your DSL, thereby making it known to Xpand, which will enable us to use the metamodel in our templates.

Next, add the following lines:

    
    
    «DEFINE main FOR Model»
      «EXPAND Entity::entity FOREACH this.types.typeSelect(Entity)»
    «ENDDEFINE»
    


This will add a new template called _main_ to your template file. By specifying _FOR Model_, we specify that this template will later be bound to a variable representing your model. The _EXPAND...FOREACH_ statement will invoke a template called _entity_ in a template file called _Entity_ for all elements of the collection _types_ in your model. Remember, your model contains a number of entities and type definitions. These are stored in the collection _types_. The _typeSelect(Entity)_ statement makes sure the generator only considers elements which are of type _Entity_ (we do not want to create Java beans for the plain data types).



### Create a template for Java beans


Every data-oriented application needs a bunch of classes to hold the data. Usually referred to as _entities_, these classes are POJOs in our case. So, let's now create a template which helps us to create POJOs from our model.

Please add another _Xpand template_ to your project by selecting _File -> New -> Other ... -> Xpand Template_. Name the new file _Entity.xpt_, making sure to save it to the same folder as _Main.xpt_.

Again, start the file by importing the meta model:

    
    
    «IMPORT entity»
    



Next, add the following lines:

    
    
    «DEFINE entity FOR entity::Entity»
      «FILE this.name + ".java"»
        public class «this.name» {
          «EXPAND attribute FOREACH this.attributes»
        }
      «ENDFILE»
    «ENDDEFINE»
    


This template will create a new Java file for each entity instance it is invoked for. This is achieved by the _FILE_ statement, which takes the name of the output file as it's first parameter.

As you can guess, we also need a template to create the attributes, so please add the following lines:

    
    
    «DEFINE attribute FOR Attribute»
      private «this.type.name» «this.name»;
    	
      public void set«this.name.toFirstUpper()»(«this.type.name» «this.name») {
        this.«this.name» = «this.name»;
      }
    	
      public «this.type.name» get«this.name.toFirstUpper()»() {
        return this.«this.name»;
      }
    «ENDDEFINE»
    


This template will create a private field, a setter and a getter for each attribute it is invoked for.



### Create a template for a DAO


To load entities from a database and save them back, we will need to write some entities, but again, this is something the generator can do for us, so let's write a template:

Create a template _DAO.xpt_ and insert this text:

    
    
    «IMPORT entity»
    
    «DEFINE dao FOR entity::Entity»
      «FILE this.name + "DAO.java"»
        import java.util.Collection;
        import org.springframework.orm.hibernate3.support.HibernateDaoSupport;
        public class «this.name»DAO 
          extends HibernateDaoSupport {
          «EXPAND crud FOR this»
        }
      «ENDFILE»
    «ENDDEFINE»
    
    «DEFINE crud FOR Entity»
      public «this.name» load(Long id) {
        return («this.name»)getHibernateTemplate().get(«this.name».class, id);
      }
      
      @SuppressWarnings("unchecked")
      public Collection<«this.name»> loadAll() {
        return getHibernateTemplate().loadAll(«this.name».class);
      }  
      
      public «this.name» create(«this.name» entity) {
        return («this.name») getHibernateTemplate().save(entity);
      }
    
      public void update(«this.name» entity) {
        getHibernateTemplate().update(entity);
      }
    
      public void remove(«this.name» entity) {
        getHibernateTemplate().delete(entity);
      }
    «ENDDEFINE»
    



Do not forget to invoke this template from the master template file _Main.xpt_:

    
    
    «IMPORT entity»
    
    «DEFINE main FOR Model»
    	«EXPAND Entity::entity FOREACH this.types.typeSelect(Entity)»
    	«EXPAND DAO::dao FOREACH this.types.typeSelect(Entity)»
    «ENDDEFINE»
    





### Create a model


In your workspace, open _org.xtext.example.entity.generator/src/model/MyModel.entity_, making sure it contains the following model:

    
    
    typedef String
    typedef Integer
    typedef Double
    typedef Date mapsto java.util.Date
    
    entity Customer {
      String firstName
      String lastName
      String email
      Address shippingAddress
      Order* orders
    }
    
    entity Address {
      String line1
      String line2
      String city
      String postcode
      String state
      String country
    }
    
    entity Product {
      String description
      Double price
    }
    
    entity Book extends Product {
      String ISBN
      String author
    }
    
    entity Order {
      Date orderDate
      OrderLine* orderLines	
    }
    
    entity OrderLine {
      Product item
      Integer amount
    }
    





### Setting up the generator


Before we can actually start the generator and transform our model, we need to adjust some aspects of the generator project.

First, please open the generator workflow in _/org.xtext.example.entity.generator/src/workflow/EntityGenerator.mwe_. We need to change the name of the root template so it matches the name of the root template we chose. We also would like the generator to format the output properly, so we're going to add an output beautifier.

Change the generator component in the workflow file to match the following:

    
    
      <component class="org.eclipse.xpand2.Generator">
        <metaModel class="org.eclipse.xtend.typesystem.emf.EmfRegistryMetaModel"/>
        <fileEncoding value="MacRoman"/>
        <expand value="templates::Main::main FOR model"/>
        <genPath value="src-gen"/>
        <beautifier class="org.eclipse.xpand2.output.JavaBeautifier"/>
      </component>
    


**Note:** you need to leave the _fileEncoding_ attribute as-is. The above fragment is valid only on a Mac.

As the formatter is based on the Eclipse java formatter (see also [this post on the Eclipse code formatter](http://www.peterfriese.de/formatting-your-code-using-the-eclipse-code-formatter/) which I wrote some years ago and which still drives 200 reader/day to my blog), we have to add some bundle dependencies to the generator project.

Do to so, please open _/org.xtext.example.entity.generator/META-INF/MANIFEST.MF_, go to the _Dependencies_ and add the following bundles:



	
  * org.eclipse.jface.text

	
  * org.eclipse.core.runtime

	
  * org.eclipse.jdt.core

	
  * org.eclipse.xtext.log4j

	
  * com.ibm.icu





### Running the generator


To run the generator, simply select the workflow file _/org.xtext.example.entity.generator/src/workflow/EntityGenerator.mwe_ and choose _Run as... -> MWE Workflow_ from the context menu. You'll see a bunch of log messages in the console view, and after a few seconds, you'll find a bunch of just generated files in _/org.xtext.example.entity.generator/src-gen_.



### Using the DSL to drive the generator


In the above sample, we wrote the DSL model by hand (well, you probably just copied it from this post), but this certainly is not the normal usage pattern. So, how can you use the DSL editor to create models that are then being read by the generator?

Actually, it is very easy if you follow these steps:



	
  1. Start the DSL in a runtime workbench (refer to the last installment to see how)

	
  2. In the runtime workspace, create a new _Eclipse Plug-in Project_. Choose any name you like, e.g. _org.xtext.example.entity.mymodel_. Make sure you deselect _Generate an activator..._ on the second page of the wizard - we don't need one.

	
  3. Add all of the following dependencies to the newly created project's manifest file:
		
			
    * org.xtext.example.entity

			
    * org.eclipse.xpand

			
    * org.eclipse.xtend

			
    * org.eclipse.xtext

			
    * org.eclipse.emf.mwe.core

			
    * org.eclipse.emf.mwe.utils

			
    * org.eclipse.xtend.typesystem.emf
				
    * org.eclipse.jface.text

			
    * org.eclipse.core.runtime

			
    * org.eclipse.jdt.core

			
    * org.eclipse.xtext.log4j

			
    * com.ibm.icu

			
    * org.xtext.example.entity.generator

		
	

	
  4. Create a new workflow file in _/org.xtext.example.entity.mymodel/src/workflow/EntityGenerator.mwe_ and insert the following text:
		
    
    
    <workflow>
      <cartridge file="workflow/EntityGenerator.mwe" model="classpath:/model/MyModel.entity"/>
    </workflow>
    		


	

	
  5. Patch the workflow file in the generator: Go back to the original workspace and change the line containing _<component class="org.eclipse.xtext.MweReader" uri="..._ to _<component class="org.eclipse.xtext.MweReader" uri="${model}">_
	

	
  6. Go back to the runtime workspace and create a new file _MyModel.entity_ in _/org.xtext.example.entity.mymodel/src/model/MyModel.entity_.


You can now use the DSL editor to create your model. The generator can be started by running the generator workflow file _EntityGenerator.mwe_ in your newly created model project _/org.xtext.example.entity.mymodel_ in the runtime workspace.



### Conclusion


I hope the last section didn't bother you too much. The good news is that we're going to provide a wizard which will help you setting up DSL model projects - see [bug 281214](https://bugs.eclipse.org/bugs/show_bug.cgi?id=281214).

Due to space limitations, this example is not complete in a sense that you can generate a fully functional CRUD application from it. You should, however be able to see what's possible with textual DSLs and code generation.

If you're interested in seeing the whole generator, drop me a line (peter dot friese [at] itemis dot de) or DM me on Twitter (my Twitter ID is @peterfriese).



### Download the code


[Download the code](http://code.google.com/p/peterfriese/source/browse/xtext_getting_started/tags/xtext_getting_started_2/) from my SVN repository.
