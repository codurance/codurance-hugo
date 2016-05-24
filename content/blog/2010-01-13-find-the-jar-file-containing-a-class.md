---
title: Find the jar file containing a class
author: Mashooq Badar
date: 2010-01-13T14:17:09Z

image:
  src: /assets/img/defaultBlogImg.png
tags:
  - java

type: blog
layout: post
slug: find-the-jar-file-containing-a-class
aliases: 
  - /2010/01/13/find-the-jar-file-containing-a-class/
---

I often need to know which jar file contains a particular class and I'm sure most people have that problem at some point. You can use [jarFinder](http://www.jarfinder.com) but it's not always up-to-date or the class might be in a jar private to your organisation. You can use the following command to do a search on your file systems for all jars, and search in them for the class you're trying to find:

~~~bash
    find -name *.jar -print -exec jar tvf {} \; | egrep "\.jar|<Class Name>"
~~~

Replace *Class Name* with the name of your class (Case sensitive). Use [cygwin ](http://www.cygwin.com/)if you are on windows.
