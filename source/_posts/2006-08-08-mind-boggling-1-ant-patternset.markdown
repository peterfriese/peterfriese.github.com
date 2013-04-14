---
author: Peter
comments: false
date: 2006-08-08 13:14:46
layout: post
slug: mind-boggling-1-ant-patternset
title: 'Mind-boggling #1: Ant patternset'
wordpress_id: 53
categories:
- Java
- Tools
---

If you're a Windows user like me, you are used to the idea that a filename consist of a first (the name) and a second part (the extension). Recently, I had to write a little [Ant](http://ant.apache.org/) file that copies an entire JRE to another directory. In order to do this, I used the following snippet:
`
<!-- copy JRE -->
<copy todir="${target.client.dir}/jre">
<fileset dir="${project.jre.dir}/jre">
<include name="**/*.*" />
</fileset>
`
Now, there is something wrong with this snippet. Can you spot the mistake? I didn't notice the mistake until my customer complained to me that the timezone information could not be read. It turned out that the timezone information is stored in files without an extension. 

But why weren't they copied? Didn't I tell Ant to copy all files by specifying "**/*.*"?

No, I didn't! The pattern "*.*" means "all files that contain a dot". Doh. Timezone files do not have a dot. The solution to the problem is quite simple: just remove the ".*", and you're done:
`
<!-- copy JRE -->
<copy todir="${target.client.dir}/jre">
<fileset dir="${project.jre.dir}/jre">
<include name="****/***" />
</fileset>
`

So, if you want to copy all files, remember to **not** use the "*.*" notation used to us Windows users, but use the "*" notation used to Linux users.

