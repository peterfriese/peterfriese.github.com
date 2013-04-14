---
author: Peter
comments: false
date: 2005-06-03 18:31:26
layout: post
slug: integrating-maven-in-eclipse
title: Integrating Maven in Eclipse
wordpress_id: 20
categories:
- Eclipse
- Java
- MDA
---

I am currently writing an Eclipse wizard that helps with creating a new [AndroMDA](http://www.andromda.org) project. AndroMDA already has two  wizards that help with creating a new project - they are both commandline based: one for [ANT](http://ant.apache.org/), the other one for [Maven](http://maven.apache.org/) ([AndroMDApp Maven Plug-in](http://www.andromda.org/maven-andromdapp-plugin/index.html)).

One of the biggest problems with integrating Maven is that there doesn't seem to be something like a main entry class that provides some method one can call and hand in some parameters to get things going. Instead, the only way to start maven is to start a new VM and run the main class. Which turns out to be quite annyoing. Here is an extract of the code that is needed to do the trick:

`
            // VM runner
            VMRunnerConfiguration vmConfig = new VMRunnerConfiguration(
                    "com.werken.forehead.Forehead", foreheadClasspath);

            vmConfig.setVMArguments(options);
            vmConfig.setProgramArguments(goals);
            vmConfig.setWorkingDirectory(projectParentDir.getAbsolutePath());
            vmConfig.setEnvironment(environment);

            String launchMode = ILaunchManager.RUN_MODE;
            IVMRunner vmRunner = getJRE().getVMRunner(launchMode);

            if (vmRunner != null)
            {
                // launch manager
                ILaunchManager manager = DebugPlugin.getDefault().getLaunchManager();
                ILaunchConfigurationType type = manager
                        .getLaunchConfigurationType(IJavaLaunchConfigurationConstants
                        .ID_JAVA_APPLICATION);

                ILaunchConfigurationWorkingCopy launchWorkingCopy = type.newInstance(null,
                        "Create AndroMDA project.");
                launchWorkingCopy.setAttribute(IDebugUIConstants.ATTR_PRIVATE, true);

                monitor.worked(2);
                monitor.subTask("Starting maven.");

                ILaunch newLaunch = new Launch(launchWorkingCopy, ILaunchManager.RUN_MODE, null);
                DebugPlugin.getDefault().getLaunchManager().addLaunch(newLaunch);
                vmRunner.run(vmConfig, newLaunch, monitor);
                monitor.worked(8);
            }
`

Right at the start you can see something called "forehead". This some funky launcher the Maven guys use to set up the class path and start the main class. I wonder why they are making things so complicated. One drawback of running a tool like this is that you cannot get feedback about how far processing has proceeded. I would have liked to provide a real progress monitor for the wizard, but unfortunately this is not possible. Maybe I'll take a look at Maven 2.
