---
author: Peter
comments: true
date: 2009-03-06 23:22:42
layout: post
slug: using-teneo-and-emf-to-store-your-data
title: Using Teneo and EMF to store your data
wordpress_id: 181
categories:
- Eclipse
- Eclipse FAQ
- EMF
- MDSD
tags:
- english
---

Most of you know that I am working as a committer for various Eclipse-related projects (such as [Xtext](http://www.xtext.org), [Xpand](http://www.eclipse.org/modeling/m2t/?project=xpand) and the [Modeling Workflow Engine](http://www.eclipse.org/modeling/emft/?project=mwe#mwe)). You might not know, however, that I also work as a consultant for [itemis](http://www.itemis.com). On one of my [recent consulting assignments](http://www.itemis.com/itemis-ag/language=en/2621/portfolio) in Ottawa, Canada, I was asked "How can we use EMF to store our data in a database?"

Well, it turns out EMF can help a long way to store data in a database. Here is how.
**Prepare your development environment**



	
  1. Get Eclipse 3.5 M5 ([click here to download](http://download.eclipse.org/eclipse/downloads/drops/S-3.5M5-200902021535/index.php))

	
  2. Unpack and start Eclipse

	
  3. Bring up the "Install New Software" dialog (Help -> Install New Software)

	
  4. Select _Teneo EMF Hibernate Runtime_ and _Teneo EMF Hibernate SDK_, version 1.0.3

	
  5. Cick _Finish_

	
  6. You most probably will be asked to restart Eclipse.


**Create a target definition that includes Hibernate and HSQLDB**
In order to keep things simple, we will store the data in an [HSQLDB](http://hsqldb.org/) database. We will use the [Hibernate](http://www.hibernate.org/) OR Mapper to perform the mapping between your data objects and the database. As you might guess, quite a number of libraries will be involved to get the task accomplished. Instead of creating a bunch of plug-in projects containing the respective libraries, or - even worse - copying all libraries into our project, we'll set up a target definition. Target definitions help to maintain a common set of dependencies for all developers on a team, which is a Good Thing.



	
  1. Create a new project _library.target_ (File -> New -> Project... -> General -> Project)

	
  2. Create three folders in this project: _hibernate_, _dependencies_, _hsqldb_

	
  3. Go to the [SpringSource Bundle Repository](http://www.springsource.com/repository/app/) and download the following OSGi bundles:







file
save in folder





com.springsource.org.hibernate-3.2.6.ga.jar


library.target/hibernate






com.springsource.org.apache.commons.logging-1.1.1.jar


library.target/dependencies






com.springsource.org.dom4j-1.6.1.jar


library.target/dependencies






com.springsource.org.apache.commons.collections-3.2.0.jar


library.target/dependencies






com.springsource.javax.transaction-1.1.0.jar


library.target/dependencies






com.springsource.antlr-2.7.7.jar


library.target/dependencies






com.springsource.org.hsqldb-1.8.0.9.jar


library.target/hsqldb





	
  4. Create a new target definition library.target in this project (File -> New -> Other... -> Plug-in Development -> Target Definition)

	
  5. Open the target definition and add the three directories to it's contents. As the GUI does not allow you to work with relative paths, you might consider to use a text editor to paste the following text:

    
    <?xml version="1.0" encoding="UTF-8"?>
    <?pde version="3.5"?>
    <target description="Teneo-related stuff, mostly Hibernate" name="Library Target Definition">
        <locations>
            <location path="${eclipse_home}" type="Profile"/>
            <location path="${workspace_loc}/library.target/hibernate" type="Directory"/>
            <location path="${workspace_loc}/library.target/dependencies" type="Directory"/>
            <location path="${workspace_loc}/library.target/hsqldb" type="Directory"/>
        </locations>
    </target>




	
  6. Open the target definition in the Target Definition Editor and click on the _Set as Target Platform_ hyperlink in the upper right area. This will activate the target definition. All contained bundles are now available and can be referenced as dependencies.


**Create a model for your data**
The data model will be based on the [well-known library tutorial](http://help.eclipse.org/ganymede/index.jsp?topic=/org.eclipse.emf.doc/references/overview/EMF.html) that ships with EMF. If you are interested in a more in-depth look, I recommend taking this tutorial. However, to shortcut things, here is the ultra-slim version of the EMF-Tutorial:



	
  1. Download the Rose class model and save it on your computer

	
  2. Create a new EMF project (File -> New -> Other... -> Eclipse Modeling Framework -> EMF Project)

	
    * Project name: library

	
    * Model importer: Rose class model

	
    * Browse to the Rose class model _library.mdl_ mentioned before

	
    * Click on Load to load the model

	
    * Click Next, then Finish




	
  3. In the library.genmodel editor, right-click on the Library node and select Generate Model Code


**Create the library main application**
In order to demonstrate how to use the data model and how to perform CRUD operations with your data, we will create a simple Java class. In the good spirit of encapsulation and components, we will create a new plug-in project to host this class:



	
  1. Create a new Plug-in project library.main (File -> New -> Project... -> Plug-in Project)

	
  2. Open the manifest and add the following dependencies:

	
    * _library_ (this is the bundle which contains our data model)

	
    * _org.eclipse.emf.teneo.hibernate_

	
    * _org.eclipse.emf.ecore.xmi_

	
    * _com.springsource.org.hibernate_

	
    * _com.springsource.org.apache.commons.logging_

	
    * _com.springsource.org.dom4j_

	
    * _com.springsource.org.apache.commons.collections_

	
    * _com.springsource.javax.transaction_

	
    * _com.springsource.antlr_

	
    * _com.springsource.org.hsqldb_




	
  3. Create a new Hibernate configuration file _hibernate.properties_ in _library.main/src_ and paste the following lines:

    
    hibernate.connection.driver_class=org.hsqldb.jdbcDriver
    # use the following line to run the embedded db:
    # hibernate.connection.url=jdbc:hsqldb:file:/some/path/on/your/computer/dbname
    # the following line will connect to a standalone (local) DB server:
    hibernate.connection.url=jdbc:hsqldb:hsql://127.0.0.1/library
    hibernate.connection.username=sa
    hibernate.connection.password=
    hibernate.dialect=org.hibernate.dialect.HSQLDialect
    hibernate.hbm2ddl.auto=true





**Implement the library main application**
With all the boilerplate in place, we're finally ready to write some code. We will create a new class and add some code to create an author and his book and store both in a library.



	
  1. Create a new class LibraryDemo, making sure it has a main method

	
  2. In order to use Teneo to persist our data, we first need to create a datastore and register our model package with it:

    
        // create the data store
        String dataStoreName = "LibraryDataStore";
        HbDataStore dataStore = HbHelper.INSTANCE.createRegisterDataStore(dataStoreName);
    
        // register the model package with the data store
        dataStore.setEPackages(new EPackage[] { LibraryPackage.eINSTANCE });
    
        // initialize the data store, which creates the tables
        dataStore.initialize();




	
  3. Next, we need to get hold of a session factory and request a new session form it:

    
        SessionFactory sessionFactory = dataStore.getSessionFactory();
        {
            Session session = sessionFactory.openSession();
            session.beginTransaction();




	
  4. Now, let's create a new library and save it to the session:

    
            // create a library
            Library library = LibraryFactory.eINSTANCE.createLibrary();
            library.setName("Developer's bookshelf");
    
            // store the library
            session.save(library);




	
  5. In the following part, we will create an author and his book, link them to each other and add them to the library. There is no specific Teneo aspect to this part of the code, it is just straightforward usage of the API EMF generated for your datamodel:

    
            // create an author
            Writer writer = LibraryFactory.eINSTANCE.createWriter();
            writer.setName("A. K. Dewdney");
    
            // create a book
            Book book = LibraryFactory.eINSTANCE.createBook();
            book.setTitle("The New Turing Omnibus");
            book.setPages(480);
            book.setCategory(BookCategory.MYSTERY); // oh well, let's hope it's not mystery to most readers!
            book.setAuthor(writer);
    
            // add book and writer to library
            library.getBooks().add(book);
            library.getWriters().add(writer);




	
  6. Finally, we need to commit our changes to the database and close the session:

    
            // commit changes to the database and close the session
            session.getTransaction().commit();
            session.close();
        }





**Start the DB server and run the application**



	
  1. Open a command line and navigate to the directory that contains _hsqldb.jar_

	
  2. Start the HSQLDB server using this command line:

    
    java -cp com.springsource.org.hsqldb-1.8.0.9.jar org.hsqldb.Server -database.0 file:library -dbname.0 library




	
  3. Finally (!) go back to Eclipse and start _LibraryDemo_. You should get an output similar to this one:

    
    Mar 6, 2009 1:50:26 PM org.eclipse.emf.teneo.hibernate.HbHelper createRegisterDataStore
    INFO: Creating emf data store and registering it under name: LibraryDataStore
    ...
    Mar 6, 2009 1:50:28 PM org.hibernate.tool.hbm2ddl.SchemaUpdate execute
    INFO: schema update complete




	
  4. To actually see the data stored in the database, navigate to the directory containing the database and open _library.log_:

    
    CREATE USER SA PASSWORD "" ADMIN
    /*C1*/SET SCHEMA PUBLIC
    CONNECT USER SA
    ...
    INSERT INTO "library" VALUES(1,'Library',0,'Developer''s bookshelf')
    INSERT INTO "writer" VALUES(1,'Writer',0,'A. K. Dewdney',NULL,NULL,'Library','1',-2)
    INSERT INTO "book" VALUES(1,'Book',0,'The New Turing Omnibus',480,'Mystery',1,NULL,NULL,'Library','1',-3)
    DELETE FROM "writer" WHERE E_ID=1
    INSERT INTO "writer" VALUES(1,'Writer',0,'A. K. Dewdney',1,0,'Library','1',-2)
    DELETE FROM "book" WHERE E_ID=1
    INSERT INTO "book" VALUES(1,'Book',0,'The New Turing Omnibus',480,'Mystery',1,1,0,'Library','1',-3)
    INSERT INTO "writer_books" VALUES(1,1,0)
    COMMIT
    DISCONNECT





In the next installment, I will show you how to retrieve objects from the database and query the database using Hibernate Query Language (HQL).

**Download the source**
If you are interested in the source for the solution so far, you can [drain file 16 url download] it here: [drain file 16 url (click to download)].
