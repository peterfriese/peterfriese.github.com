---
author: Peter
comments: false
date: 2005-10-10 00:28:52
layout: post
slug: how-to-filter-property-pages-according-to-the-project-nature
title: How to filter property pages according to the project nature
wordpress_id: 28
categories:
- Eclipse FAQ
---

Often, you need to hide property pages if a project doesn't have a specific nature. Here's how you can do that:

`
   <extension point="org.eclipse.ui.propertyPages">
      <page adaptable="false"
            class="org.andromda.android.ui.ideas.AndroidProjectPropertyPage"
            id="org.andromda.android.ui.ideas.AndroidProjectPropertyPage"
            name="Android Project Layout"
            objectClass="org.eclipse.core.resources.IProject">
         <filter name="nature"
               value="org.andromda.android.core.androidnature"/>
      </page>
   </extension>
`

How does it work? In order to make this work, the class you register the property page for must implement _IActionFilter_. Projects do not implement _IActionFilter_, but there is a class _WorkbenchProject_ that does so. This class is registered with the workbench adapter manager, so it will eventually be called (if no one else supplies a suitable adapter, that is) in order to find out whether the project has an attribute named _nature_ whose value is _org.andromda.android.core.androidnature_. In order  to resolve attributes, _IActionFilter_ requires clients to implement the method _public boolean testAttribute(Object target, String name, String value)_. Here is the implementation as supplied by _WorkbenchProject_:

`
    public boolean testAttribute(Object target, String name, String value) {
        IProject proj = (IProject) target;
        if (name.equals(NATURE)) {
            try {
                return proj.isAccessible() && proj.hasNature(value);
            } catch (CoreException e) {
                return false;
            }
        } else if (name.equals(OPEN)) {
            value = value.toLowerCase();
            return (proj.isOpen() == value.equals("true"));//$NON-NLS-1$
        }
        return super.testAttribute(target, name, value);
    }
`

As you can see, _WorkbenchProject_ will check whether the underlying project has a certain nature.
