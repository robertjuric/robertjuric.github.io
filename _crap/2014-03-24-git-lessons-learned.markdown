---
author: admin
comments: true
date: 2014-03-24 15:08:23+00:00
layout: post
slug: git-lessons-learned
title: Git Lessons Learned
wordpress_id: 956
categories:
- Code
tags:
- branch
- commit
- git
- github
- merge
- push
---

Wow, Git can get out of hand quickly. I thought I'd share some of my lessons learned from actually using Git since my last [Using GitHub post](http://robertjuric.com/2014/03/17/using-github/).

**Do no track large files, such as /data/**
My app started out simple enough, but then I installed MongoDB, and maybe through sheer ignorance I decided to install the data folder in the same folder as my repository. Well I added the folder to the repository and committed. When I tried to push I found out there is a file size limit. Live and learn.

**Can use exlude file to ignore those folders, or place data in other folder**
So I'm not sure if you're supposed to include the node modules in the repository or not, but to avoid the non-tracking errors you can just stop tracking them in the exclude file.

**Use 'git status' to make sure no local commits haven't been pushed**
After I committed my too large data/ folder, I tried to push to the remote repo. I didn't noticed that it failed. I continued making commits until I caught my error 4 commits in. If I had checked the git status I could have caught the error and resolved it much easier than being 4 commits in.

**Conflicts suck, commit and push regularly, check status often**
Biggest lesson learned right here. Resolving conflicts or correcting a mistake is a huge headache, one best just avoided. Commit often, push after commits, and check status to verify.

**CLI > GUI**
This one is probably obvious to most, but since I come from a Windows background I started out trying to use the GUI. Luckily I'm a network engineer by day and I feel at home on the CLI. It doesn't take long to learn the CLI syntax and then you have much better control of Git.

Here are some of the Git commands I use quite often. This may well just be a future reference for myself:
`//Remove file from tracking but keep actual file
git rm --cached file.txt`

`//Create branch
git checkout -b Branch-Name`

`//Commit, force add files, comment
git commit -a -m 'Commit comment'`

`//Push to remote branch
git push origin Branch-Name`

`//Merging a branch:
//Move back to the master branch
git checkout master
//Merge master with branch
git merge Branch-Name
//Delete local branch
git branch -d Branch-Name
//Push to local, merged, master to remote master
git push origin master
//Delete remote branch
git push origin :Branch-Name`

So there you have it, Git isn't as simple as it first appears. However, it can be easy enough if you follow some simple rules and pay attention to what you're doing. If I run into anything else I'll be sure to write that up as well. These posts tend to usually be references for myself anyway.
