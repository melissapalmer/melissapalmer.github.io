---
layout: post
comments: true
title: "GIT Ticks and Tricks"
categories: tips and tricks
tags: 
- GIT
description: GIT
published: true
---

* [Moving commits between branches](#moving_commits_btw_branches)  

<a name="moving_commits_btw_branches"/>
#### Moving commits between branches
Branch A has commits (X,Y) that also need to be in Branch B. 
The cherry-pick operations should be done in the same chronological order that the commits appear in Branch A.

`cherry-pick` does support a range of commits, but if you have merge commits in that range, it gets really complicated

git checkout branch-B
git cherry-pick X
git cherry-pick Y

_Reference_ [https://gist.github.com/unbracketed/889c844473bcca1917e2](https://gist.github.com/unbracketed/889c844473bcca1917e2)