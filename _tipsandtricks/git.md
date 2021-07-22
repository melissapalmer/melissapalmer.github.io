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
* [Different Git Config at a Folder Level](#different_git_config_at_a_folder_level)   

<a name="moving_commits_btw_branches"/>
# Moving commits between branches
Branch A has commits (X,Y) that also need to be in Branch B. 
The cherry-pick operations should be done in the same chronological order that the commits appear in Branch A.

`cherry-pick` does support a range of commits, but if you have merge commits in that range, it gets really complicated

```bash
git checkout branch-B
git cherry-pick X
git cherry-pick Y
```
_Reference_ [https://gist.github.com/unbracketed/889c844473bcca1917e2](https://gist.github.com/unbracketed/889c844473bcca1917e2)

# Different Git Config at a Folder Level

(for Work and Personal)

- create a separate folders where clone work vs. personal repos to. eg: 

  - `~/work/mp/repository`  for personal repositories
  - `~/work/company-name/repository` for work related repositories

- split your git config into three parts - a shared config in home, and per-directory configs 

- in the shared config (i.e.:  `.gitconfig`) add any settings you want to share between git hosts. 

  - ```bash
    [user]
    name = Melissa Palmer
    	
    [includeIf "gitdir:~/work/company-name/"]
    path = ~/work/company-name/.gitconfig
    
    [includeIf "gitdir:~/work/mp/"]
    path = ~/work/mp/.gitconfig
    ```

- then configure your email address per folder (*git hosts*)

  - in the file `~/work/mp/.gitconfig`

    - ```bash
      [user]
      	email = melissa.palmer@personalemail.com
      ```

  - in the file `~/work/company-name/.gitconfig`

    - ```bash
      [user]
      	email = melissa.palmer@company-name.com
      ```

*References* 

- https://dev.to/davidjones418/per-directory-git-config-3ec9 
- https://www.codeproject.com/Articles/5297072/How-to-Set-Different-Git-Config-user-email-and-use (Windows related)

