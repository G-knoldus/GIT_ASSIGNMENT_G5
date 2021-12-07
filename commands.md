# Git Hub
## merging and pulling commands

[![G5|Git](https://miro.medium.com/max/640/1*75jvBleoQfAZJc3sgTSPQA.jpeg)](https://github.com/G-knoldus/GIT_ASSIGNMENT_G5)

The git pull command is used to fetch and download content from a remote repository and immediately update the local repository to match that content.

- Performance is Excellent
- Secure open source bucket
- Fexiblity is Good

## How it works

The git pull command first runs git fetch which downloads content from the specified remote repository. Then a git merge is executed to merge the remote content refs and heads into a new local merge commit. To better demonstrate the pull and merging process let us consider the following example. Assume we have a repository with a main branch and a remote origin.


## Installation of git

- To see if you already have git installed on the System
```sh
git version
```

- Make sure everything is up to date on the system
```sh
sudo apt-get update
```

- To install git use the following command in the terminal
```sh
sudo apt-get install git-all
```

## Commands

- Fetch the specified remote’s copy of the current branch and immediately merge it into the local copy. 
```sh
git pull <remote>
```
- Fetches the remote content but does not create a new merge commit.
```sh
git pull --no-commit <remote>
```

- Using git merge to integrate the remote branch with the local one, use git rebase.
```sh
git pull --rebase <remote>
```
- It displays the content being downloaded and the merge details.
```sh
git pull --verbose
```

## Git Merge
 
 Merging is Git's way of putting a forked history back together again. The git merge command lets you take the independent lines of development created by git branch and integrate them into a single branch.
 
## How it works

Git merge will combine multiple sequences of commits into one unified history. In the most frequent use cases, git merge is used to combine two branches. The following examples in this document will focus on this branch merging pattern. In these scenarios, git merge takes two commit pointers, usually the branch tips, and will find a common base commit between them. Once Git finds a common base commit it will create a new "merge commit" that combines the changes of each queued merge commit sequence.

## Commands
 
- Start a new feature
```sh
git checkout -b new-feature main
```
- Edit some files
```sh
git add <file>
```
- git commit -m "Start a feature"
```sh
git add <file>
```
```sh
git commit -m "Finish a feature"
```

- Merge in the new-feature branch
```sh
git checkout main
```
```sh
git merge new-feature
```
```sh
git branch -d new-feature
```
- This command merges the specified branch into the current branch
```sh
git merge --no-ff <branch>
```


 


