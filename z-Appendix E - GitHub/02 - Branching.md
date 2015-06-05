#Branching#

A Git branch is a collection of commits and nothing more.  You'll sometimes hear people talking about 'branching strategies'.  All that means is "how do we manage to keep our mission progressing without obstructing independent development and then ship a product with all the blended features when complete?".

##A Typical Strategy##
Every repo has a `master` branch and most have some sort of `develop` or `staging` branch.  These branches are designed to live infinitely.  `master` is usually meant to house code that is 'always deployable' or even a mirror for what is on production right now.  `develop` is where *completed*  'to-be-released' features accumulate before the next release.

**Feature** branches are either research and development branches or features/fixes that are in development.  These feature branches are *branched* off of `develop` so they can at least be integrated with the features already completed.  If two features are being worked on in parallel and one feature is complete before the other, the feature branch is then *merged* with the `develop` branch.

It is at this time the `develop` branch can be merged with `master` and released or yet another feature can be started and branched off of `develop`.  Inevitably features will complete at different times.  When a completed feature gets merged to develop, other features in progress are out of date with `develop`.  Those working on their feature can merge `develop` into their feature branch to keep it up to date and ensure their feature still works with the recently completed feature.

It is very important to note that merging is directional.  You can merge `develop` into your feature branch or you can merge your feature branch into `develop`.

>Merging branches is Git's way of saying we're taking commits from one branch and applying them to another.

When a feature is completed and merged into `develop`, the feature branch can be deleted.  This makes it different from the `master` and `develop` branches because feature branches have a limited life span.

##Pull Requests##
Developers can freely merge their completed feature into `develop` (if they have commit rights), but a better way would be to do *pull request*.  When a pull request is created, a summary of the differences between the feature branch and `develop` is created.  It is a great way to gather opinions of your colleagues and is a general sanity check when you skim through the proposed changes.

When you don't have commit rights to a repo, you normally *fork* a repo which creates a copy into your GitHub account which provides you with commit rights to your local copy.  Once you modify your local copy, you can then send your changes to the original repo and ask that they pull in your changes, aka a **Pull Request**.

##Hotfixes##
A hotfix is just like a feature except instead we should branch off of `master`.  A hotfix is needed when something is broken on production and needs fixed really bad right now.  After a fix is finished, the hotfix branch is merged into `master` so the `master` branch contains the fix.  However the `develop` branch and any feature branches know nothing of this fix.  So after a hotfix is applied, merge `master` into `develop` then merge `develop` into each of your feature branches.

##Black Magic##
Branches do not (and usually never do) have the exact same files in them.  Meaning when you change from `master` to `develop`, you should expect that some files show up magically and some go away (alarmingly).  This is by design and you should pay attention to your Git Extensions when it warns you that you may lose *working directory* changes.  This means you have uncommitted edits detected that may be permanently lost unless you commit them or *stash* them.

##Stash##
Git has a convenient way to temporarily save **uncommitted** changes when changing branches.  Think of it as a built-in copy/paste.  When you want to change branches but want to keep uncommitted changes, store them in the stash, then pop them back out of the stash after you change branches.

##More Reading##
For more on Git Branching, check Google and read [one of my favorite articles](http://nvie.com/posts/a-successful-git-branching-model/) on this.

[<Back 01 - Terms](01 - Terms.md)

[Next> 03 - gitignore](03 - gitignore.md)
