---
author: Peter
comments: true
date: 2008-11-28 12:42:26
layout: post
slug: using-xpand-in-your-eclipse-wizards
title: Using Xpand in your Eclipse Wizards
wordpress_id: 154
categories:
- Conferences
- Eclipse
- MDSD
---

At [ESE](http://www.eclipsecon.org/summiteurope2008/), I had a nice chat with [Chris](http://mea-bloga.blogspot.com/) (actually, I had a lot of nice chats with a lot of nice people - it was quite a challenge to attend any of the sessions), who told me that he was looking into template engines. I leave it up to you to make any assumptions on why he's interested in template engines. I happen to know at least three template engines: Velocity (which I had the joy of using in my active days at AndroMDA.org), the template, erhm, ... mechanism being used in PDE (I [once fixed a bug](https://bugs.eclipse.org/bugs/show_bug.cgi?id=241074) which had been caused by this engine) and - you might have guessed it - Xpand (which I am a committer on).

So, I gave a short demo of Xtext and Xpand to show Chris how easy it is to create (model-aware) code generation templates. Xpand comes with a nice editor which makes template editing a joy - it features code highlighting, hyperlink navigation, model-awareness and other editor goodness.

Instead of letting only Chris know how that works, here's a short guide on how to write your own Xpand template and use it in a wizard. To make the example more realistic, I chose to implement a wizard that creates an Ant build.xml file for a simple project (something which is missing in Eclipse, AFAIK). So, here we go.


## Prerequisites:





	
  * Eclipse 3.5M3 (3.4 might also do)

	
  * [EMF 2.5.0M3](http://www.eclipse.org/modeling/download.php?file=/modeling/emf/emf/downloads/drops/2.5.0/S200811032126/emf-sdo-xsd-SDK-2.5.0M3.zip)

	
  * [EMF Compare 0.8.1](http://www.eclipse.org/modeling/download.php?file=/modeling/emft/compare/downloads/drops/0.8.1/R200809170822/emft-compare-SDK-incubation-0.8.1.zip)

	
  * [MWE 0.7.0M3](http://www.eclipse.org/modeling/download.php?file=/modeling/emft/mwe/downloads/drops/0.7.0/S200811120303/emft-mwe-SDK-incubation-0.7.0M3.zip)

	
  * [Xpand 0.7.0M3](http://www.eclipse.org/modeling/download.php?file=/modeling/m2t/xpand/downloads/drops/0.7.0/S200811120513/m2t-xpand-SDK-incubation-0.7.0M3.zip)




## How to do it





	
  1. Download and install the prerequisites.

	
  2. Create a new plug-in project named "de.peterfriese.antwizard"

	
  3. Add a New File Wizard to the project. I used the Custom plug-in Wizard to get me started.

	
  4. Brush up the wizard page and add some fields for project name, source folder and binary folder.

	
  5. We want to transfer the values entered in this wizard into our template, so we need to create an Ecore model that describes our data model.[


![antwizard_metamodel](http://farm4.static.flickr.com/3047/3065637332_0f345eaf86_o.jpg)


](http://www.flickr.com/photos/81029262@N00/3065637332)

	
  6. Add _org.eclipse.emf_ and _org.eclipse.emf.edit_ to the dependencies of you plug-in.

	
  7. Create a genmodel and let EMF generate the model code for you.

	
  8. Add code to your wizard that transfers the values from the input fields to your model. One might consider using databinding for doing this, I used a straight forward read'n'write approach for the time being:

    
    public boolean performFinish() {
        final String containerName = page.getContainerName();
        final String srcDirName = page.getSrcDirName();
        final String binDirName = page.getBinDirName();
        // ...
    }
    private BuildSpecification createModel(IProject project, String srcDirName,
        String binDirName) {
        BuildSpecification buildSpecification = BuildspecificationFactory.eINSTANCE.createBuildSpecification();
        Project projectSpecification = BuildspecificationFactory.eINSTANCE.createProject();
        projectSpecification.setName(project.getName());
        projectSpecification.setBinaryFolder(binDirName);
        projectSpecification.setSourceFolder(srcDirName);
        buildSpecification.setProject(projectSpecification);
        return buildSpecification;
    }




	
  9. Create an Xpand template _de.peterfriese.antwizard/src/template/BuildTemplate.xpt_:
[


![Ant file template](http://farm4.static.flickr.com/3008/3064840225_f3f3b2e799_o.jpg)

](http://www.flickr.com/photos/81029262@N00/3064840225)
In case you wonder how to get those funky characters: make sure us create this file using the Xpand editor - it has code assist (CTRL + space) for those characters.


	
  10. Create an Xtend file _de.peterfriese.antwizard/src/template/GeneratorExtensions.ext_:
[


![Ant file extensions](http://farm4.static.flickr.com/3045/3065691432_b59ba29444_o.jpg)


](http://www.flickr.com/photos/81029262@N00/3065691432)

	
  11. Add _org.eclipse.xpand_, _org.eclipse.xtend_ and _org.eclipse.xtend.typesystem.emf_ to the depencies of your plug-in.

	
  12. Finally, we need to provide some code to invoke the generator:

    
    private void generate(BuildSpecification buildSpec, IProgressMonitor monitor) throws CoreException {
    
        // get project root folder as absolute file system path
        IWorkspaceRoot root = ResourcesPlugin.getWorkspace().getRoot();
        IResource resource = root.findMember(new Path(buildSpec.getProject().getName()));
        String containerName = resource.getLocation().toPortableString();
    
        // configure outlets
        OutputImpl output = new OutputImpl();
        Outlet outlet = new Outlet(containerName);
        outlet.setOverwrite(true);
        output.addOutlet(outlet);
    
        // create execution context
        Map globalVarsMap = new HashMap();
        XpandExecutionContextImpl execCtx = new XpandExecutionContextImpl(output, null, globalVarsMap, null, null);
        EmfRegistryMetaModel metamodel = new EmfRegistryMetaModel() {
            @Override
            protected EPackage[] allPackages() {
                return new EPackage[] { BuildspecificationPackage.eINSTANCE, EcorePackage.eINSTANCE };
            }
        };
        execCtx.registerMetaModel(metamodel);
    
        // generate
        XpandFacade facade = XpandFacade.create(execCtx);
        String templatePath = "template::BuildTemplate::main";
        facade.evaluate(templatePath, buildSpec);
    
        // refresh the project to get external updates:
        resource.refreshLocal(IResource.DEPTH_INFINITE, monitor);
    }





That's it!


## Taking it for a spin





	
  1. Launch a runtime workbench by selecting _de.peterfriese.antwizard/META-INF/MANIFEST.MF_ and invoking _Run As -> Eclipse Application_

	
  2. Create a new Java project in the runtime workspace.

	
  3. Invoke your wizard by selecting _File -> New -> Other... -> Ant -> Ant build file from existing project_

	
  4. Enter the required information (project name, source folder, binariy folder)

	
  5. After clicking on _finish_, you should get a fresh Ant file for your project:
[


![Ant file](http://farm4.static.flickr.com/3215/3064865073_3415a7b0c3_o.jpg)


](http://www.flickr.com/photos/81029262@N00/3064865073)

	
  6. Enjoy!


You can download the project [here](http://www.peterfriese.de/wp-content/downloads/plugins/model_driven_ant_wizard.zip). Feedback and comments are welcome!
