---
title: Configuring a WAR project in Eclipse & Setting up Jetty to Run/Debug
author: Mashooq Badar
date: 2009-04-17T14:43:54Z

image:
  src: /assets/img/defaultBlogImg.png

type: blog
layout: post
slug: configuring-a-war-project-in-eclipse-setting-up-jetty-to-rundebug
aliases: 
    - /2009/04/17/configuring-a-war-project-in-eclipse-setting-up-jetty-to-rundebug/
---

Generate eclipse project: mvn eclipse:eclipse
Import the project into eclipse

In Eclipse
- Create a new Run/Debug config
- Set main class toorg.codehaus.classworlds.Launcher

Go to the argument tab:
- Set Program arguments to jetty:run
- Set VM arguments to -Xmx512M -Dclassworlds.conf=${M2_HOME}/bin/m2.conf -Dmaven.home=${M2_HOME}

Go to the classpath tab:
- Remove the application from the user entries
- Add the ${M2_HOME}/boot/classworlds-1.1.jar to the user entries

Go to the source tab:
