---
author: admin
comments: true
date: 2013-04-26 12:28:19+00:00
layout: post
slug: things-that-make-my-life-easier
title: Things That Make My Life Easier
wordpress_id: 871
categories:
- Insights
- Networking
---

As network engineers we live busy lives. We usually have multiple things happening and we have to remain agile to keep up with our daily work. I've slowly built a personal system which I use to help me stay on top of my mounting workload.


## Notes


[![The Post-It System](http://robertjuric.com/wp-content/uploads/2013/04/photo-300x225.jpg)](http://robertjuric.com/wp-content/uploads/2013/04/photo.jpg)

Some people rely on the post-it note system. For me, this doesn't cut it. I am constantly juggling multiple projects and tasks, many of which are hurry up and wait scenarios. I work on a project and then have to be able to come back to it and pick back up where I left off. Or sometimes I need save a config snippet I did, or templates I've used to deploy port configurations. Which brings me to one of my favorite (yet anti-climatic) tools, [Notepad++](http://notepad-plus-plus.org/).


## Notepad++


I use Notepad++ for multiple reasons, one of the biggest is tabs. I have created a folder (obviously called Notes), which I save all of my information to. When I get a call, or start to work a ticket, I immediately open a new tab (Ctrl+N) and start to work. When I'm done I can look at it and either close the tab without saving, or save it to my Notes folder. I like having multiple tabs open because I reference some things multiple times a day and it makes it much easier than constantly browsing for a file.

The next big reason I use Notepad++ is for the column editing mode. Instead of deleting or inserting at the beginning of each line 10 times, you can use Column Editing mode to do it all at once. To do this just Alt+Click in the document. This feature along with search and replace makes it easy to take the output from a show command, modify it with search+replace, and turn it into a script to paste back into the device.

There is also a handy [Compare Plugin for Notepad++](http://sourceforge.net/projects/npp-compare/) that can be useful for comparing config changes.


## CLI


[![Matrix Code](http://robertjuric.com/wp-content/uploads/2013/04/Matrix-in-CMD-300x225.jpg)](http://robertjuric.com/wp-content/uploads/2013/04/Matrix-in-CMD.jpg)

As network engineers we spend most of our time at the Command Line Interface. This involves a lot of time spent with terminal emulation programs. I like [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and it's great in a pinch, but if you work in a large environment I strongly suggest you check out [SecureCRT](http://www.vandyke.com/products/securecrt/). It is pricey ($100 for 1yr of updates), but I feel it is an investment well worth it. Well managed tabs and organized saved sessions are absolute life-savers.

One of the biggest things I've learned to do with SecureCRT is setup auto-logging. To do this you have to edit the session options and you can specify the log file name and how you want to save your logs using convenient variables. I do this in-case I fat-finger a command and generally having logs is always a good thing. It's also an easy way to capture the crazy output that is "show tech" for TAC cases.


## Task Management


The final tool I'm going to mention is a webapp I use to manage my project tasks. There are plenty of apps out there. The one I've settled on using is [Nirvana](https://www.nirvanahq.com/). It is a Getting Things Done (GTD) based web app and also has an iOS client. I like it primarily because I can group my tasks into projects. Being GTD based, it has a folder/view for Next items (stuff I need to be doing now) and Project folders/views (for stuff that isn't immediate). I also like that it has Contexts and areas to filter your tasks (great for separating home and work tasks and projects).


## The Take-Away


Build yourself a system. I'm sure you have plenty of issues, methods, and opinions that make every situation unique. These are the tools I've discovered that best suite me, but there are plenty more out there. Don't settle for mediocrity when it comes to tools you use on a daily basis, craftsmen don't and you shouldn't either.
