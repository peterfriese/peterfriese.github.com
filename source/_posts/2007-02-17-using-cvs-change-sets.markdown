---
author: Peter
comments: false
date: 2007-02-17 01:30:25
layout: post
slug: using-cvs-change-sets
title: Using CVS change sets
wordpress_id: 66
categories:
- Eclipse
---

It seems to be a little known fact that the Eclipse CVS integration supports CVS change sets. Now, what is this? Have a look at the following figure:

![CVS synchronisation without change sets](/wp-content/downloads/images/cvs_synch_without_change_sets_small.PNG)
What we see here is a typical situation: you have been working on a new feature in your local workspace while your teammates weren't lazy either and have been hacking away some code. The synchronize view gives you an impression of which changes are outgoing ones and which ones are incoming ones.

However, there are several problems with this view. First, you cannot see which changes belong to each other. You also cannot see _who_ has been performing the changes. And last, you cannot see _why_ the changes have been made.

All these shortcomings can be overcome by using CVS change sets. The same situation with change sets activated looks like this:

![CVS synchronisation with change sets](/wp-content/downloads/images/cvs_synch_with_change_sets_small.PNG).

Now you can easily see who has been changing what for which reason.

Here's how to use change sets:



	
  1. Make sure that change set models are activated in the synchronisation preference dialog:![CVS change set preferences](/wp-content/downloads/images/cvs_changesets_preferences_small.PNG)

	
  2. Activate the changeset model in the sychronize view models dropdown menu:![Activate CVS change set model](/wp-content/downloads/images/cvs_changesets_activate_small.PNG)

	
  3. Last, but not least: make sure you and your teammates use commit comments! It is a good habit to comment each and every commit. For CVS change sets to be useful, you should NOT use a different commit comment for each single file, but rather use one comment for all files that belong to a logical change (e.g. a bug fix).


Give it a try. I am sure you won't look back.
