---
author: Peter
comments: true
date: 2008-03-10 10:16:47
layout: post
slug: editors-how-to-open-an-eclassifier-in-the-ecore-editor-and-selecting-it
title: 'Editors: How to open an EClassifier in the ECore editor and selecting it'
wordpress_id: 84
categories:
- Eclipse
- Eclipse FAQ
---

I am currently working on an ECore Type Lookup Dialog that lets you look up EClassifiers in the same fashion we know from the JDT Type Lookup Dialog and the [PDE Artifact Lookup Dialog](http://mea-bloga.blogspot.com/2008/02/quickly-access-exported-packages.html).

Creating the dialog is rather easy - you just need to subclass FilteredItemsSelectionDialog. I'll blog more on this topic soon. Today, however, I wanted to document a tidbit that is not so well documented: How do I open the default editor on an EClassifier and select the EClassifier as soon as the editor has appeared.

First of all, let's find out more about the EClassifier:

    
    EObject eObject = (EObject) object; 
    URI uri = EcoreUtil.getURI(eObject); 
    IFile file = ResourcesPlugin.getWorkspace().getRoot().getFile(new Path(uri.toPlatformString(true)));


Now that we know the URI of the object of desire and the file it resides in, we can browse the EditorRegistry for the default editor that has been registered for this particular file:

    
    
    try { 
        // open default editor 
        Workbench workbench = PlatformUI.getWorkbench(); 
        EditorDescriptor editorDescriptor = workbench.getEditorRegistry().getDefaultEditor(file.getName());


Opening the editor is a no-brainer now:

    
    
        WorkbenchPage page = workbench.getActiveWorkbenchWindow().getActivePage(); 
        EcoreEditor ecoreEditor = (EcoreEditor) page.openEditor(new FileEditorInput(file), editorDescriptor.getId());


The hard part is how to select the EClassifier in the editor we just opened. The trick is to NOT just create an IStructuredSelection containing the EClassifier. You have to make sure you use the SAME EClassifier instance that is being shown in the editor. This can be ensured by retrieving the ECLassifier from the EditingDomain of the EcoreEditor:

    
    
        // set selection 
        EditingDomain editingDomain = ecoreEditor.getEditingDomain(); 
        EObject editObject = editingDomain.getResourceSet().getEObject(uri, true); 
        if (editObject != null) { 
            ecoreEditor.setSelectionToViewer(Collections.singleton(editObject)); 
        } 
    } catch (PartInitException e) { 
        Log.logError(e); 
    }


That's it. As with most things in EMF, the answer can be found by browsing the EMF source code and the code of the editors and other artifacts that it generates.
