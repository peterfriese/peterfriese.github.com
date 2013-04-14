---
author: Peter
comments: true
date: 2010-03-17 16:05:05
layout: post
slug: getting-started-with-code-generation-with-xpand
title: Getting started with Code Generation with Xpand
wordpress_id: 390
categories:
- Eclipse
- EMF
- MDSD
tags:
- MDSD
- xpand
---

Have you heard about model driven software development (MDD / MDSD) and are thinking "what's all this fuzz about models"? "Why should models help me to be more productive," might be another thought you have.

People have been asking how to leverage models on and off on the web and in meetings I attended, so I thought I might share this little tutorial with you. In this tutorial, we will develop a little code generator that helps you to create (HTML) forms from models. 



## A short Overview


Xpand is a template engine, similar to FreeMarker, Velocity, JET and JSP. However, it features some very unique properties that makes using Xpand very well suited for generating code from models, such as **[type safety](http://en.wikipedia.org/wiki/Type_safety)** and **polymorphic dispatch**. If you haven't heard those terms before, fear not! I'll show you how to use Xpand by way of an easy-to-follow example.

<!-- more -->

The usual process of writing a code generator with Xpand is as follows:



	
  1. Define the structure of the model you want to process. This is called a _metamodel_

	
  2. Define one or more template(s) that teach the code generator how to translate your model into code.


Easy, isn't it?

Using the code generator is even easier:

	
  1. Create a model

	
  2. Start the code generator



It is worth mentioning that you can use Xpand to generate code for almost any known programming language. Everything you can express in text can be generated using Xpand. So, while we will be generating HTML and Java code in the following example, you can easily write code templates that generate code for C#, Basic, Lua, SmallTalk, ABAP, or any other programming language. You can also generate manuals and other documentation artifacts from your models using Xpand. I've successfully used Xpand to create DocBook files from models. Those DocBook files have then been converted to PDF files and online help files.



## Preparing your IDE





	
  1. Grab and install a recent copy of Eclipse. At the time of writing, I am using [Eclipse 3.6 M6](http://download.eclipse.org/eclipse/downloads/drops/S-3.6M6-201003121448/index.php).

	
  2. Install the latest version of Xpand (_Help -> Install New Software ..._)
	
  3. Add the Xpand update site (at the time of this writing, I am using Xpand 1.0 nightly builds from [http://download.eclipse.org/modeling/m2t/xpand/updates/nightly/](http://download.eclipse.org/modeling/m2t/xpand/updates/nightly/))

	
  4. Add the MWE update site ([http://download.eclipse.org/modeling/emft/mwe/updates/nightly/](http://download.eclipse.org/modeling/emft/mwe/updates/nightly/))

	
  5. Select MWE and Xpand:

[![Install MWE and Xpand](http://farm5.static.flickr.com/4001/4439298864_40b913f0e5.jpg)](http://www.flickr.com/photos/81029262@N00/4439298864)


	

	
  6. After the obligatory restart, do yourself the favour and set the platform encoding to **UTF-8**!





## Creating a Generator Project


Xpand code generators are hosted in Eclipse plug-ins, mainly because this makes handling the classpath a lot easier. People have reportedly used Maven to run Xpand code generators, but we won't go down this road today. Let's create a simple generator project:



	
  1. Open the new project wizard and choose _Xpand Project_ from the list

	
  2. Choose a meaningful name for your project (e.g. _org.xpand.example.gettingstarted_

	
  3. Select _Create a sample EMF based Xpand project_


After clicking _Finish_, the wizard will create an sample generator project for you:


[![Xpand project layout](http://farm3.static.flickr.com/2794/4439735747_e7385b2cef_o.png)](http://www.flickr.com/photos/81029262@N00/4439735747)



Before we can start working with this project, we need to perform some clean-up actions:



	
  1. Open _src/metamodel/Checks.chk_ and delete all its contents

	
  2. Open _src/metamodel/Extensions.chk_ and delete all its contents

	
  3. Open _src/template/GeneratorExtensions.ext_ and delete all its contents

	
  4. Delete _src/Model.xmi_

	
  5. Don't forget to save all modified files





## Creating the Metamodel


As mentioned before, we need to define the structure of our models before we can actually start writing the code template.
Xpand is capable of understanding a variety of metamodel types. For example, if you have an XML schema file, you can use this as a metamodel and thereby enable Xpand to use XML files which are compliant to your schema as input models. Or, if you already have a bunch of Java files making up your data model, you can use those to drive Xpand code generation. In this tutorial, however, we will be using an Ecore metamodel to define the structure of our models. The project has already been configured to support Ecore metamodels, so all we need to do is open _metamodel.ecore_ and define the structure:




	
  1. Please open _src/metamodel/metamodel.ecore_
.
	
  2. Remove the following elements from the metamodel: _Feature_, _Entity_, _Datatype_, _Type_, _Model_. The metamodel should now be empty:

[![Empty metamodel](http://farm3.static.flickr.com/2555/4439788989_42665597db.jpg)](http://www.flickr.com/photos/81029262@N00/4439788989)


	
  3. Select package _metamodel_ (as depicted in the screenshot above) and add a new _EClass_ (_context menu -> New Child -> EClass_). Use the properties view to change the name of the newly created EClass to _Model_.

	
  4. Create a new EClass _Form_

	
  5. Select the newly created EClass _Form_ and add the following _EAttribute_s:
		
			
    * _name_, set the EType to _EString_

			
    * _description_, EType = _EString_

			
    * _title_, EType = _EString_

		
	

	
  6. Select EClass _Model_ and add a new _EReference_, setting its attributes as follows:
		
			
    * name = _forms_

			
    * EType = _Form_

			
    * Containment = _true_

			
    * Upperbound = _-1_ (meaning: unlimited)

		
	

	
  7. Create a new EClass _Field_ and add the following EAttributes to it:
		
			
    * _name_, EType = _EString_

			
    * _label_, EType = _Estring_

		
	

	
  8. Create another EClass _TextField_, setting its properties as follows:
		
			
    * name = _TextField_

			
    * ESuper Types = _Field_

		
	

	
  9. Add one EAttribute _text_ to _Textfield_:
		
			
    * name = _text_, EType = _EString_

		
	

	
  10. Add an EClass _MultiLineTextField_ to the metamodel:
		
			
    * name = _MultiLineTextTield_

			
    * ESuper Types = _TextField_

		
	

	
  11. Now that we have everything in place, we finally need to add a reference from _Form_ to _Field_ so we can later add fields to a form. Select EClass _Form_ and add an EReference to it, setting its properties as follows:
		
			
    * name = _fields_

			
    * EType = _Field_

			
    * Containment = _true_

			
    * Upperbound = _-1_ (meaning: unlimited)

		
	





## Creating a Model


Let's now create a model that follows the structure of the metamodel:



	
  1. In _metamodel.ecore_, select EClass _Model_

	
  2. Create a model instance by choosing _Create Dynamic Instance..._ from the context menu

	
  3. Save the model file to _src/Model.xmi_



The model file editor will now open and you can use the tree editor to input the following model:

	
  * Add a _Form_ to the model, seeting the following properties:
		
			
    * Name: _context_

			
    * Description: _Send your feedback_

			
    * Title: _Contact form_

		
	

	
  * Add a _TextField_ to the _Form_, setting the following properties:
		
			
    * Name: _name_

			
    * Label: _Name_

		
	

	
  * Add another _TextField_ to the _Form_, setting the following properties:
		
			
    * Name: _email_

			
    * Label: _EMail_

		
	

	
  * Add a _MultiLineTextField_ to the _Form_, setting the following properties:
		
			
    * Name: _message_

			
    * Label: _Message_

		
	


Your model should now look like this:


[![Contact model](http://farm3.static.flickr.com/2719/4440711442_354f817303.jpg)](http://www.flickr.com/photos/81029262@N00/4440711442)





## Creating a Code Generator


As mentioned before, we will create a code generator for simple forms. Nothing too fancy, but enough to give you an idea of how to create generator templates.

The result will look like this:


[![Contact Form](http://farm3.static.flickr.com/2745/4440844786_46824ce7ea.jpg)](http://www.flickr.com/photos/81029262@N00/4440844786)



Open _src/template/Template.xpt_ and replace its contents with the following text:

    
    
    «IMPORT metamodel»
    «DEFINE main FOR Model»
    «EXPAND form FOREACH forms»
    «ENDDEFINE»
    



On the first line, we import the metamodel so that the generator (and the editor as well) knows about the structure of our model. On line 2 - 4 we define a code template named _main_, making sure it is bound to model elements of type _Model_. The template doesn't do much, except to call another template (which we will define in a minute) named _form_ with the collection of _Form_s, contained in the reference _forms_ of the current form.

Add the following lines to the template file, defining the template for the HTML file:

    
    
    «DEFINE form FOR Form»
    «FILE name + ".html"»
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
      <title>«this.title»</title>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
      <link rel="stylesheet" type="text/css" href="../static/style.css" />
    </head>
    



In the first line, we start the template by specifying its name (_form_) and the type it is bound to (_Form_). On the next line, we use the _FILE_ statement to specify the file the output is going to be written to. The name of the file is derived by concatenating the attribute _name_ of the current _Form_ and the string literal _".html"_.

Continue the template by appending the following text:

    
    
    <body>
      <div id="page-wrap">
        <h1>«this.title»</h1><br /><br />
        <p>«this.description»</p>
        <div id="form-area">
    



Obviously, this piece of template code will create part of the body of the HTML page. Again, we will access attributes of the current _Form_ (as read from the model) and insert their values into the template (in this case, the title and the description). By the way, you can omit the _this._ prefix in front of the variable names.

The template goes on with the following text:

    
    
          <form method="post" action="form.php">
            «EXPAND field FOREACH this.fields»
            <input type="submit" name="submit" value="Submit" class="submit-button" />
          </form>
          <div style="clear: both;"></div>
        </div>
      </div>
    </body>
    </html>
    «ENDFILE»
    «ENDDEFINE»
    



The _EXPAND_ statement will invoke yet another subtemplate with the name _field_. This sub template will be called for each element in the _fields_ attribute (reference) of the current _Form_.

You might recall that we we defined three different kinds of _>Field_s in the metamodel: 



	
  1. Field (which is the super class for the other two field types

	
  2. TextField

	
  3. MultiLineTextField



As their names imply, a _TextField_ will be a single line text entry field, whereas _MultiLineTextField_ will be a multiline text input field. We somehow need to be able to render different HTMNL code for each of these different text field types.

As mentioned in the introduction, Xpand is not only type safe, but also supports **polymorphic dispatch**. This basically means we will create three templates (one for each of the different field types) _with the same name_. When evaluating the code template, the Xpand generator will dispatch to the appropriate template by matching the most concrete type of the current model element.

Add the following code to the template file:

    
    
    «DEFINE field FOR Field»
    «ERROR "should not happen"»
    «ENDDEFINE»
    «DEFINE field FOR TextField»
            <label for="«this.name»">«this.label»:</label>
            <input type="text" name="«this.name»" id="«this.name»" />
    «ENDDEFINE»
    «DEFINE field FOR MultiLineTextField»
            <label for="«this.name»">«this.label»:</label>
            <textarea name="«this.name»" id="«this.name»" rows="20" cols="20"></textarea>
    «ENDDEFINE»
    



As you can see, all three templates have the same name. Only the type they are bound to differs. This is enough to let Xpand know which template to choose according to the type of the current model element. Let's suppose Xpand is iterating the model and the current model element is a _TextField_. Although _Field_ is a direct super type of _TextField_, Xpand will not invoke the first template (_«DEFINE field FOR Field»_), but the second template (_«DEFINE field FOR TextField»_), as this is the most concrete match for the type of the model element.



## Running the Code Generator


If you have followed the above steps, running the code generator is a piece of cake: 

Open the context menu on _src/workflow/workflow.mwe_ and select _Run As -> MWE Workflow_

This will start the code generator. You will see some log messages in the console view. If all went well, the output in the console reads something like this:

    
    
    ...
    1503 INFO  Generator          - Written 1 files to outlet [default](src-gen)
    1503 INFO  WorkflowRunner     - workflow completed in 650ms!
    



The result of the code generation can be found in _src-gen/contact.html_. As this file has some dependencies to CSS/image files, please download the files from the _static_ folder ([here](http://code.google.com/p/peterfriese/source/browse/#svn/org.xpand.example.gettingstarted/trunk/org.xpand.example.gettingstarted/static)) and place them in your project before opening _contact.html_ in your browser.



## Where to go from here


If you want to learn more about Xpand, be sure to attend my talk [Use models and let the computer do the grunt work with Xpand](http://www.eclipsecon.org/2010/sessions/?page=sessions&id=1129) at EclipseCon 2010. I'll show some more advanced topics in this talk (such as generator cartridges, using Xtend to augment your models, partitioning your code templates, using other metamodels to define your models).

Xpand comes with an extensive documentation (just go to _Help -> Help Contents -> Xpand Documentation_ in Eclipse). You can also get help on Xpand in the [Eclipse Community Forums](http://www.eclipse.org/forums/index.php?t=tree&th=163643&)

Should you need help, [itemis (the company I work with)](http://www.itemis.com) offers training and consulting for Xpand and a host of other modeling related technologies.

No doubt you have heard about [Xtext](http://www.xtext.org). Xpand and Xtext go together great: you can use Xtext to define the structure of your models and create great-looking text editors to edit your models. Then, use Xpand to create code generators that take your textual models and turn them into running software. Actually, Xtext comes with a wizard that you to create a code generator project for your DSL.



## Downwloads


The code for this tutorial can be found in [my SVN repository on Google Code](http://code.google.com/p/peterfriese/source/browse/#svn/org.xpand.example.gettingstarted/trunk/org.xpand.example.gettingstarted).
