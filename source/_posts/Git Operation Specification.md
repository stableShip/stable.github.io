---  

title: Git Operation Specification
date: 2019-02-22 17:19:32  
tags: [git]  

---

## Now

all the bugfix are commit direct to 'bugfix' branch, all members commit in the same branch. it make the commit history unreadable and everyone have to pull&merge others's code which is not the current work in their hand frequently and make git hard to handle.

## How to change

### Use Github Flow

[git flow guide ](https://gitversion.readthedocs.io/en/latest/git-branching-strategies/githubflow/)

1.  Update master to latest  upstream code
2.  Create a feature branch from master branch  `git checkout -b feature/myFeatureBranch`
3.  Do the feature/work
4.  Push feature branch to origin
5.  Create pull request from origin/  -> upstream/master
6.  Review, fix raised comments, merge your PR or even better, get someone else to.

The main rule of GitHub Flow is that master should  _always_  be deployable.

  

### Automatic Deploy For Test

1.  set github project hook
2.  commit pr to develop branch, tigger github project hook
3.  the hook send request to an url to the deploy server: jenkin or program you build.
4.  the deploy server pull the newest code and restart server
5.  test!

### deploy to production

1.  commit pr to master
2.  others review the pr
3.  merge pr
4.  deploy to production

### Name Rule:

Bugfix: bugfix/fix_xxxx

Feature: feature/add_xxx

if your project contains frontend and backend.

Frontend: add frontend/ , such as: front/bugfix/fix_xxxx

Backend: add backend/ , such as: backend/bugfix/fix_xxxx