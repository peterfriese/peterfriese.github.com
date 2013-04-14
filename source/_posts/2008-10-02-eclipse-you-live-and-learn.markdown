---
author: Peter
comments: true
date: 2008-10-02 00:56:18
layout: post
slug: eclipse-you-live-and-learn
title: Eclipse - You live and learn.
wordpress_id: 129
categories:
- Eclipse
---

[Yesterday, I complained](http://www.peterfriese.de/eclipse-on-macos/) about large fonts being used in Eclipse runtime instances on Mac OS X. Much to my delight, [Chris](http://mea-bloga.blogspot.com/) was kind enough to point out that you can define a launch configuration template for PDE-based launches. It is a bit hidden and doesn't have the "launch" keyword assigned. To globally define launch config parameters, open the preference dialog and navigate to Plug-in Development -> Target Platform -> Launching Arguments:

[caption id="attachment_130" align="alignnone" width="500" caption="Launch template for PDE launches"][![Launch template for PDE launches](http://www.peterfriese.de/wp-content/launchtemplate.png)](http://www.peterfriese.de/wp-content/launchtemplate.png)[/caption]

After ou have defined your favourite launching arguments, subsequently created launch configurations will be initialized with these arguments:
[caption id="attachment_133" align="aligncenter" width="500" caption="Launch configuration"][![Launch configuration](http://www.peterfriese.de/wp-content/launchconfig.png)](http://www.peterfriese.de/wp-content/launchconfig.png)[/caption]

Who would have thought you can learn something by just [filing a bug](https://bugs.eclipse.org/bugs/show_bug.cgi?id=249179)?
