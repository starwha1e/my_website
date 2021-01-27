---
title: "Git"
date: 2020-11-12T11:49:06+08:00
---
## Concepts
working directory, repository, stage, branch.
`HEAD` means the current version
`HEAD^` means the previous version (1 version before)
`HEAD~100` 100 version before

## Create
`git init` initialise a git repository (inside that folder)  
Clone an existing repository:  
`git clone git@github.com:username/repository_name.git`  
`git clone ssh://user@domain.com/repo.git`  
`git remote add origin git@server-name:path/repo-name.git` connect a local repo  
`git push -u origin master` push master branch for the first time

## Local Changes
`git status` to check the status of the work space  
`git add <file>` add file, you can add multiple files  
`git diff` check the change you have made before commit  
`git add .` add all current changes to the next commit  
`git commit -m <message>` upload the change  
`git commit --amend` Change the last commit  
`git reset --hard HEAD^` back to 1 version before 
`git reflog` check future version after reset  
`git cherry-pick <commit number>` apply a commit to the current branch  

## Commit History
`git log` check the commit history  
`git log -p <file>` Show changes over time for a specific file  
`git blame <file>` Who changed what and when  

## Branches & Tags
`git branch <name>`  establish branch   
`git branch`  Check all branches   
`git switch <name>` Switch between branches   
`git switch -c <name>` establish and switch to a branch   
`git merge <name>` merge a branch  (fast forward cannot see the merge in `git log`)  
`git merge --no-ff -m "something" <branch name>` merge with no fast forward   
`git branch -d <name>` delete a branch   
`git tag <tage_name>` Mark the current commit with a tag  

## Undo
`git checkout -- <file>` discard changes in working directory (use the file in repository to replace the one in working directory)  
`git reset HEAD <file>` unstage

## Update & Publish
`git remote -v` List all currently configured remotes  
`git remote show <remote>` Show information about a remote  
`git fetch <remote>` Download all changes from \<remote\> but don’t integrate into head  
`git pull <remote> <branch>` Download all changes and directly merge/integrate  
`git push <remote> <branch>` Publish local changes on a remote  
`git branch -dr <remote/branch>` Delete a branch on the remote  
`git push --tags` Publish your tags  

## Store current state
`git stash` Store the current work state(that cannot be committed right now)   
`git stash list` check the stash list  
`git stash apply` to restore and `git stash drp` delete  
`git stash pop` can do two at the same time  
`git stash apply stash@{0}` if you have more than one stash  

## branch strategy
`Master` branch should be stable and only used to publish new version.  
work in `dev` branch and merge individual branches on `dev` branch.  
Use a new branch to develop a new feature, delete with `git branch -D <branch name>` if it is not merged.  
