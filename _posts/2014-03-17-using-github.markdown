---
author: admin
comments: true
date: 2014-03-17 16:18:14+00:00
layout: post
slug: using-github
title: Using GitHub
wordpress_id: 953
categories:
- Code
tags:
- branch
- git
- github
---

Not much has left me with more confusion on how I should use a tool than GitHub. I've banged my head and messed up my repository (I'll explain soon) more times than I can count. Until I read the 5 minute [Understanding the GitHub Workflow](http://guides.github.com/overviews/flow/) guide. Wow I should have just read that on the front end....

My summary of how to use GitHub (high-level):
`1. Create a repository
2. Create a branch of that repository
(Write code)
3. Commit changes to branch
4. Push changes to remote repository
5. Merge branch into master`

GitHub is based on Git, a _distributed _revision control system. So you have your GitHub account online, and then you install Git on your local machine to do your coding. Those are two separate repositories (I didn't realize this at first). When you make commit changes on your local machine, you are committing them to your local repository, which is why #4 is required to push those changes to the remote repository (distribution!).

Branches are also important, because the master branch is supposed to be your deployable code. If you are going to work on a new feature, you should create a branch to work on that feature, commit changes to it, and then if it tests out ok, merge the branch back with the primary.

That's GitHub in a nutshell. There is soo much more, such as collaboration, issue tracking, mark-up, forks, but this is good enough to get you started. We can look at adding this additional features as the need arises. For now let's get started coding!
