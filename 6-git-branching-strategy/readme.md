## What is Git
The most widely used modern version control system in the world today is Git. Git is a mature, actively maintained open source project originally developed in 2005 by Linus Torvalds, the famous creator of the Linux operating system kernel

Git is an example of a DVCS (hence Distributed Version Control System). Rather than have only one single place for the full version history of the software as is common in once-popular version control systems like CVS or Subversion (also known as SVN),in Git, every developer's working copy of the code is also a repository that can contain the full history of all changes.

In addition to being distributed, Git has been designed with performance, security and flexibility in mind.

## Basic Commands
```git
# Configure name and email
$ git config --global user.name "Sam Smith"
$ git config --global user.email sam@example.com

# Create new local repo
$ git init

# Checkout repo
$ git clone /path/to/repository

# Add files
$ git add <filename>
$ git add *

# Commit Changes
$ git commit -m "Commit message"
$ git commit -a

# Push
$ git push origin master

# Status
$ git status

# Connect to remote repo
$ git remote add origin <server>

# Branches
$ git checkout -b <branchname> # new
$ git checkout <branchname> # switch
$ git branch # list
$ git branch -d <branchname> # delete
$ git push origin <branchname> # push
$ git push --all origin # push all origin
$ git push origin :<branchname> # delete origin

# Update from remote repository
$ git pull # fetch
$ git merge <branchname> # merge

# Diff between branches
$ git diff
$ git diff --base <filename>
$ git diff <sourcebranch> <targetbranch>

# Add files
$ git add <filename>
$ git add . # all files

# Tags
$ git tag 1.0.0 <commitID> # create a tag
$ git log # log tags
$ git push --tags origin # push tag to origin

# Undo local changes
$ git checkout -- <filename>
$ git fetch origin 
$ git reset --hard origin/master

# Search
$ git grep "foo()"
```

## Why Git
- cheap and simple merging and branching

## Decentralized but centralized
- origin
- each developer pulls and pushes to origin
- pull changes from other peers to form sub teams. 


![Decentralized but centralized](https://nvie.com/img/centr-decentr@2x.png)

## Main Branches

The central repo holds two main branches with an infinite lifetime:
- master
- develop

`origin/master`: HEAD always reflects a production-ready state.

`origin/develop`: HEAD always reflects a state with the latest delivered development changes for the next release.

`develop` branch reaches a stable point and is ready to be released, all of the changes should be merged back into `master` somehow and then tagged with a release number.

![Main Branches](https://nvie.com/img/main-branches@2x.png)

## Supporting branches
supporting branches to aid parallel development between team members, ease tracking of features, prepare for production releases and to assist in quickly fixing live production problems.

Unlike the main branches, these branches always have a limited life time, since they will be removed eventually.

The different types of branches we may use are:
- Feature branches
- Release branches
- Hotfix branches

### Feature branches 
May branch off from: <br/>
> develop <br/>

Must merge back into: <br/>
> develop <br/>

Branch naming convention:
>anything except master, develop, release-*, or hotfix-* <br/>

![Feature Branch](https://nvie.com/img/fb@2x.png)

**Creating a feature branch**
```git
$ git checkout -b myfeature develop
Switched to a new branch "myfeature"
```

**Incorporating a finished feature on develop**
```git
$ git checkout develop
Switched to branch 'develop'

$ git merge --no-ff myfeature
Updating ea1b82a..05e9557
(Summary of changes)

$ git branch -d myfeature
Deleted branch myfeature (was 05e9557).

$ git push origin develop
```

The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.

![no-ff](https://nvie.com/img/merge-without-ff@2x.png)

### Release branches
May branch off from:
> develop

Must merge back into:
> develop and master

Branch naming convention:
> release-*

Release branches support preparation of a new production release.

**Creating a release branch**
```git
$ git checkout -b release-1.2 develop
Switched to a new branch "release-1.2"

$ ./bump-version.sh 1.2
Files modified successfully, version bumped to 1.2.

$ git commit -a -m "Bumped version number to 1.2"
[release-1.2 74d9424] Bumped version number to 1.2
1 files changed, 1 insertions(+), 1 deletions(-)
```

**Finishing a release branch**
```git
$ git checkout master
Switched to branch 'master'

$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)

$ git tag -a 1.2
```

To keep the changes made in the release branch, we need to merge those back into develop, though. In Git:

```git
$ git checkout develop
Switched to branch 'develop'

$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)
```
This step may well lead to a merge conflict (probably even, since we have changed the version number). If so, fix it and commit.

Now we are really done and the release branch may be removed, since we don’t need it anymore:

```git
$ git branch -d release-1.2
Deleted branch release-1.2 (was ff452fe).
```

### Hotfix branches 
May branch off from: 
> master

Must merge back into:
> develop and master

Branch naming convention:
> hotfix-*

Hotfix branches are very much like release branches in that they are also meant to prepare for a new production release, albeit unplanned.

A hotfix branch may be branched off from the corresponding tag on the master branch that marks the production version.

**Creating the hotfix branch**
Hotfix branches are created from the master branch

```git
$ git checkout -b hotfix-1.2.1 master
Switched to a new branch "hotfix-1.2.1"

$ ./bump-version.sh 1.2.1
Files modified successfully, version bumped to 1.2.1.

$ git commit -a -m "Bumped version number to 1.2.1"
[hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
1 files changed, 1 insertions(+), 1 deletions(-)
```

Don’t forget to bump the version number after branching off!

Then, fix the bug and commit the fix in one or more separate commits.

```git
$ git commit -m "Fixed severe production problem"
[hotfix-1.2.1 abbe5d6] Fixed severe production problem
5 files changed, 32 insertions(+), 17 deletions(-)
```

**Finishing a hotfix branch**
When finished, the bugfix needs to be merged back into master, but also needs to be merged back into develop

```git
$ git checkout master
Switched to branch 'master'

$ git merge --no-ff hotfix-1.2.1
Merge made by recursive.
(Summary of changes)

$ git tag -a 1.2.1
```
Next, include the bugfix in develop, too:

```git
$ git checkout develop
Switched to branch 'develop'

$ git merge --no-ff hotfix-1.2.1
Merge made by recursive.
(Summary of changes)
```
The one exception to the rule here is that, when a release branch currently exists, the hotfix changes need to be merged into that release branch, instead of develop

Finally, remove the temporary branch:
```git
$ git branch -d hotfix-1.2.1
Deleted branch hotfix-1.2.1 (was abbe5d6).
```

Reference List:
===
[1. a-successful-git-branching-model](https://nvie.com/posts/a-successful-git-branching-model/) <br/>
[2. what-is-git](https://www.atlassian.com/git/tutorials/what-is-git) <br/>
