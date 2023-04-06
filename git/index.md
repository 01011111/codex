---
title: Git
---

# Git

[Git](https://git-scm.com/) is a distributed version control system (VCS).

Version control is the practice to keep track of changes overtime, allowing to manipulate those changes, test new things, and eventually revert them if necessary.

A distributed version control system shares this tracking of those changes with everyone working with the system. Everybody involved can access the history of changes, but also can work 'offline', only to connect with the rest later, and this alleviate the risk of having the content and its history in only one location.

Git uses `branches` for context switching. Every repository starts with a branch named `main` (formerly `master`).
We are free to create a new branch from any existing one - creating a new context - for the work we want to do.
There are several branching strategies and conventions, please refer to the _Branching Strategies_ page.  
_See commands `checkout` and `branch`_

After the changes made, we can _stage_ those changes - understand: select them to be saved, added to the history - and it's done, we can _commit_ those changes - effectively adding them to the history of our branch.  
_See commands `add` and `commit`_

Once all the work we wanted to do in that branch is done, we can _merge_ those changes into another branch. This specifically allow collaboration, as everyone can work in their own contexts without affecting the others as we expirement, and centralize those changes in one common context when finished.  
_See commands `cherry-pick`, `rebase` and `merge`_

Since this is a distributed VSC, to share our branches with everybody we need to _push_ them - that will share the history of the changes with everyone else. Of course, it might happen that several persons work on the same branch at the same time, and you may need at some point to _fetch_ their changes to have them in your _local_ context. To do that you _pull_ the changes and that will merge the history locally.
_see commands `fetch`, `pull`, `rebase` and `push`_

## Commands

- [add]({{ site.baseurl }}/git/add)
- [branch]({{ site.baseurl }}/git/branch)
- [checkout]({{ site.baseurl }}/git/checkout)
- [cherry-pick]({{ site.baseurl }}/git/cherry-pick)
- [commit]({{ site.baseurl }}/git/commit)
- [fetch]({{ site.baseurl }}/git/fetch)
- [merge]({{ site.baseurl }}/git/merge.mg)
- [pull]({{ site.baseurl }}/git/pull)
- [push]({{ site.baseurl }}/git/push)
- [rebase]({{ site.baseurl }}/git/rebase)
- [reset]({{ site.baseurl }}/git/reset)

## Practices

- [Branching Strategies]({{ site.baseurl }}/git/branching/ 
