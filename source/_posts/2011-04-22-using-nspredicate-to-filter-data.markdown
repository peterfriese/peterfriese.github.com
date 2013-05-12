---
author: Peter
comments: true
date: 2011-04-22 22:15:36
layout: post
slug: using-nspredicate-to-filter-data
title: Using NSPredicate to Filter Data
wordpress_id: 725
categories:
- iOS
- NSPredicate
- NSArray
teaserimage: /images/2011-04-22-using-nspredicate-to-filter-data/coffee_filtered_150x150.png

---

Filtering data is one of the essential tasks in computing. With all the data available today, we need to apply certain limits and constraints to actually make it usable. What use is it to be able to scroll down a list of literally thousands of list items when you really care about one or two of them? Filtering and searching information make up a significant part of our work day - each time you use Google, you're applying a filter to the huge set of data we call the internet. 
<!-- more -->
Even on your local computer, you use services like Spotlight or the search field in your mail application all the time. Apps that lack decent searching and filtering capabilities sometimes are really hard to use, so as developers we should spend some time on thinking how to allow users to filter the data they're dealing with.

In this post, I am going to focus on ways to filter data. We won't be looking at the UI side of things - this is something I'll do in a subsequent blog post.



## Old-skool filtering


If you look at an arbitrary code base, chances are you'll sooner or later run into a piece of code similar to this one:

    
    NSMutableArray *oldSkoolFiltered = [[NSMutableArray alloc] init];
    for (Book *book in bookshelf) {
        if ([book.publisher isEqualToString:@"Apress"]) {
            [oldSkoolFiltered addObject:book];
        }
    }


It's a straight-forward approach to filtering an array of items (in this case, we're talking about books) using a rather simple `if`-statement. Nothing wrong with this, but despite the fact we're using a fairly simple expression here, the code is rather verbose. We can easily imagine what will happen in case we need to use more complicated selection criteria or a combination of filtering criteria.



## Simple filtering with NSPredicate


Thanks to Cocoa, we can simplify the code by using [NSPredicate](http://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSPredicate_Class/Reference/NSPredicate.html). `NSPredicate` is the object representation of an if-statement, or, more formally, a predicate.

Predicates are [expressions that evaluate to a truth value](http://en.wikipedia.org/wiki/Predicate_(mathematical_logic)), i.e. `true` or `false`. We can use them to perform validation and filtering. In Cocoa, we can use `NSPredicate` to evaluate single objects, filter arrays and perform queries against Core Data data sets.

Let's have a look at how our example looks like when using `NSPredicate`:

    
    NSPredicate *predicate = 
      [NSPredicate predicateWithFormat:@"publisher == %@", @"Apress" ];
    NSArray *filtered  = [bookshelf filteredArrayUsingPredicate:predicate];


Much shorter and better readable!



## Filtering with Regular Expressions


Regular Expressions can be used to [solve almost any problem ;-)](http://xkcd.com/208/) so it's good to know you can use them in `NSPredicate`s as well. To use regular expressions in your `NSPredicate`, you need to use the `MATCHES` operator.

Let's filter all books that are about iPad or iPhone programming:

    
    
    predicate = [NSPredicate predicateWithFormat:@"title MATCHES '.*(iPhone|iPad).*'"];
    filtered = [bookshelf filteredArrayUsingPredicate:predicate];
    dumpBookshelf(@"Books that contain 'iPad' or 'iPhone' in their title", filtered);
    



You need to obey some rules when using regular expressions in `NSPredicate`: most importantly, [you cannot use regular expression metacharacters inside a pattern set](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/Predicates/Articles/pUsing.html#//apple_ref/doc/uid/TP40001794-SW9).



## Filtering using set operations


Let's for a moment assume you want to filter all books that have been published by your favorite publishers. Using the `IN` operator, this is rather simple: first, we need to set up a set containing the publishers we're interested in. Then, we can create the predicate and finally perform the filtering operation:

    
    NSArray *favoritePublishers = [NSArray arrayWithObjects:@"Apress", @"O'Reilly", nil];
    predicate = [NSPredicate predicateWithFormat:@"publisher IN %@", favoritePublishers];
    filtered  = [bookshelf filteredArrayUsingPredicate:predicate];
    dumpBookshelf(@"Books published by my favorite publishers", filtered);





## Advanced filtering thanks to KVC goodness


`NSPredicate` relies on [key-value coding](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/KeyValueCoding/Articles/KeyValueCoding.html) to achieve its magic. On one hand this means your classes need to be [KVC compliant](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/KeyValueCoding/Articles/Compliant.html#//apple_ref/doc/uid/20002172-BAJEAIEE) in order to be queried using `NSPredicate` (at least the attributes you want to query). On the other hand, this allows us to perform some very interesting things with very little lines of code.

Let's for example retrieve a list of books written by authors with the name "Mark":

    
    predicate = 
      [NSPredicate predicateWithFormat:@"authors.lastName CONTAINS %@", @"Mark" ];
    filtered  = [bookshelf filteredArrayUsingPredicate:predicate];



In case we'd want to return the value of one of the aggregate functions, we don't need to use NSPredicate itself, but instead use KVC directly. Let's retrieve the average price of all books on our shelf:

    
    NSNumber *average = [bookshelf valueForKeyPath:@"@avg.price"];





## Conclusion


The Cocoa libraries provide some very powerful abstractions which can make your life and that of the people reading your code much easier. It pays off to know about them, so go ahead and browse the Cocoa documentation and hunt for those gems!

Apple's [NSPredicate programming guide](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/Predicates/predicates.html) provides an in-depth documentation for `NSPredicate` containing all the things I didn't cover in this post.

`NSPredicate` also plays an important role when querying data in Core Data, something we will need to have a look at in one of the next blog posts. Stay tuned!



## Code


The code for this post is available on my github page: [get forking](http://github.com/peterfriese/NSPredicateDemo)!
