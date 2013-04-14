---
author: Peter
comments: true
date: 2009-03-14 22:55:49
layout: post
slug: using-teneo-emf-and-hibernate-to-update-and-query-your-data
title: Using Teneo, EMF and Hibernate to update and query your data
wordpress_id: 215
categories:
- Eclipse
- Eclipse FAQ
- EMF
- MDSD
tags:
- english
---

[Last week I showed you](http://www.peterfriese.de/using-teneo-and-emf-to-store-your-data/) how to use Teneo, EMF and Hibernate to store your data in a database.

This week, we're going to have a look at how to update the data, add more records and how to query the database for information using HQL, the Hibernate Query Language.

If you haven't done so, you might consider [taking last week's tutorial](http://www.peterfriese.de/using-teneo-and-emf-to-store-your-data/) in order to get your environment set up and understand the basic concepts. If you're lazy, you can as well download the code of the tutorial to get started more quickly. It is available [drain file 16 url here].

As it turns out, in order to change anything in the library database, we must first fetch the to-be-changed data, so we need to have a look at querying first.

**Retrieving and updating data**
Let's assume we'd want to the update the number of pages in a book, because the new edition has an additional chapter.




	
  1. So, first of all we need to open a session and begin a transaction:
		
    
    
        {
            Session session = sessionFactory.openSession();
            session.beginTransaction();


	

	
  2. Next, let's create an HQL query. We want to find any books that are written by an author by the name "A. K. Dewdney" that contain "Omnibus" in their title.
		
    
    
            Query query = session.createQuery(
                "SELECT book from " +
                "    Book book, " +
                "    Writer writer " +
                "WHERE " +
                "    book.title like '%Turing Omnibus%' " +
                "AND " +
                "    writer.name = 'A. K. Dewdney'");


	

	
  3. We can now execute the query and display the results:
		
    
    
            List<book> books = query.list();
            Book book = books.get(0);
            System.out.println(book.getTitle());


	

	
  4. Updating the page count is pretty obvious:
		
    
    
            book.setPages(520);


	

	
  5. Finally, don't forget to commit the transaction and close the session:
		
    
    
            session.getTransaction().commit();
            session.close();
        }


	



**Adding data**
Let's now assume we want to add more books (and authors) to the library.




	
  1. By now, you should be pretty familiar with the pattern of opening a new session:
		
    
    
        {
            Session session = sessionFactory.openSession();
            session.beginTransaction();


	

	
  2. As we want to add new items to the library, we need to retrieve the library instance first of all:
		
    
    
            Query query = session.createQuery("from Library");
            List<library> libraries = query.list();
            Library library = libraries.get(0);


	

	
  3. Creating a new book and its author is easy, as we just have to use the API EMF so kindly generated for us:
		
    
    
            Writer writer = LibraryFactory.eINSTANCE.createWriter();
            writer.setName("J.R.R. Tolkien");
            Book book = LibraryFactory.eINSTANCE.createBook();
            book.setTitle("The Hobbit");
            book.setPages(320);
            book.setAuthor(writer);
            book.setCategory(BookCategory.MYSTERY);		
            library.getBooks().add(book);
            library.getWriters().add(writer);


	

	
  4. Finally, commit the transaction and close the session:
		
    
    
            session.getTransaction().commit();
            session.close();
        }


	


