---
author: Peter
comments: true
date: 2009-10-27 16:55:43
layout: post
slug: xtext-tricks-1-enhancing-completion-proposals
title: 'Xtext Tricks #1: Enhancing Completion Proposals'
wordpress_id: 337
categories:
- DSLs
- Eclipse
- MDSD
---

Those of you who [follow me on Twitter](htp://twitter.com/peterfriese) might have noticed [I am working on an Xtext based DSL](http://twitter.com/peterfriese/status/4889552469) for [Behaviour Driven Development](http://en.wikipedia.org/wiki/Behavior_driven_development). Part of the DSL allows the DSL user to define actors and the verbs these actors can execute. Actors can have a hierarchy (much like a class hierarchy), meaning an actor will inherit all verbs of it's super actors. As the list of verbs can grow quite a bit, the content assist drop down menu becomes a bit overwhelming.

[

![XtextProposalProviderBefore](http://farm3.static.flickr.com/2581/4029808851_22fd514946.jpg)

](http://www.flickr.com/photos/81029262@N00/4029808851)

To alleviate  this situation, I decided to display the "owner" of a verb along with the name of the verb in the content assist drop down box. Here is my first try:
<!-- more -->

    
    
    public class StoryDslProposalProvider extends AbstractStoryDslProposalProvider {
      @Override
      protected ICompletionProposal createCompletionProposal(EObject element, String proposal, String displayString, ContentAssistContext contentAssistContext) 
      {
        if (element instanceof Verb) {
          Verb verb = (Verb) element;
          Subject owner = ((Subject)verb.eContainer());
          String myDisplayString = verb.getName() + ": " + owner.getName();
          return super.createCompletionProposal(element, proposal, myDisplayString, contentAssistContext);
        }
        return super.createCompletionProposal(element, proposal,  displayString, contentAssistContext);
      }
    }
    



With this piece of code in place, the content assist proposals for verbs will now display the name of the "owner" to the right of the replacement text:

[

![XtextContentAssistAfter](http://farm4.static.flickr.com/3526/4030575068_2358208f22.jpg)

](http://www.flickr.com/photos/81029262@N00/4030575068)

[Sebastian](http://zarnekow.blogspot.com/) pointed out that there is a `getDisplayString()` in `AbstractContentProposalProvider` that you should override in order to create a custom display string. It turned out, however, that this method will not be called as expected. I [filed a bug](http://bugs.eclipse.org/bugs/show_bug.cgi?id=293380), so expect to see a fix in one of the next milestones of the Xtext 0.8.0 stream.

