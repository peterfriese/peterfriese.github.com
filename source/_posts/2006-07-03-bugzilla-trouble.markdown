---
author: Peter
comments: false
date: 2006-07-03 11:06:01
layout: post
slug: bugzilla-trouble
title: Bugzilla trouble
wordpress_id: 50
categories:
- Computer
- Linux
---

Back from holidays, I discovered that our customer migrated our development server to a new hardware. Without telling anybody of the devlopment team! First thing I noticed was that Bugzilla didn't work any more: I just got error messages like this: `data/versioncache did not return a true value at globals.pl line 628`. Googling around returned [this](http://mozdev.org/pipermail/project_owners/2004-October/003149.html) mailing list entry which recommends rerunning checksetup.pl. Running `./checksetup.pl` in the installation directory (under `/etc/var/www/bugzilla`) did the trick: everything is fine again. On to the next problem!
