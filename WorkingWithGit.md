Here are a few hints to get started with hacking on mapserver with git. These can be considered "better" practices, and although they might seem heavier at the first glance, they really help in the long run.

### Never make your work-in-progress changes directly in the "master" or "branch-x-y" branches.

These branches should nearly always reflect the state of their equivalent on the central repository, i.e. you should be able to cleanly pull changes from the central repository into these branches.

here's an example session when working on a bugfix or new feature that will be applied to master:

```bash
git pull origin master  # make sure the master is uptodate
git checkout master
git checkout -b my-new-feature   # branch off of master
```

hack on some code, this can take some time. commit often.

When you're ready, prepare your my-feature-branch to be merged back into the up-to-date master.
This step can be skipped if master has not been updated on the central repo, or if you are positive that your changes will not interfere with the changes that happened on the main repo.

```bash
git checkout master
git pull origin master  
git checkout my-new-feature
git merge master
```

At this point, you may have conflicts to resolve. If fixing the conflicts takes some time, repeat the previous step until the merge completes cleanly.

You are now ready to publish your changes to the main repo:

```bash
git checkout master
git merge my-new-feature
git push origin master
```

Note in this case that your local "master" branch has diverged from the repo "master" branch only the time it took to run the merge command (which ran without conflict if you respected the previous steps) and the time taken to push the changes.

###Working with release branches
Following the workflow described here
http://www.net-snmp.org/wiki/index.php/Git_Workflow,
I've setup a script in our repository that changes the way fixes
should be applied to stable branches. The aim is to simplify the task
of applying a fix to multiple branches. So, in the case where a fix
needs to be applied to more than just master, this is the workflow to
use:

- make sure you have an uptodate clone, and that your master and
stable branches don't have any uncommited or unpushed changes

- pick the oldest branch your fix applies to (or that you are willing
to support), for the example let's say it is branch-5-4

- set your working copy to this branch:

```bash
git checkout branch-5-4
```
- create a temporary branch for your fix

```bash
git checkout -b bug1234
```

- code, test your fix, commit, restart,etc...

```bash
git add changed_file.c
git commit -m "fixing bug 1234, first step"
```

- once the fix is ready merge it into branch-5-4

```bash
git checkout branch-5-4
git merge bug1234
```
- you are now ready to apply this fix in the newer release branches. A
script automates this process:

```bash
sh stablemerge.sh
```

- what the script does is iteratively merge 4-10 into 5-0, then 5-0
into 5-2, etc, up till 6-0 into master. This means your fix is
propagated to all newer branches down into master.

- for more complex fixes and/or very old branches, the fix will likely
not always apply cleanly from one branch to the next. You'll be
returned back to your shell with a message indicating which file has a
conflict and asked to manually edit it to resolve. Once you've fixed
the conflict, commit your edited file, and rerun the stablemerge.sh
script to continue propagating your updated fix to newer branches.

- push your changes back to the repo

```bash
git push origin branch-5-4
git push origin branch-5-6
git push origin branch-6-0
git push origin master
```

Please try to avoid copy-pasting a fix in multiple branches to
accomplish the same task, as it will result in conflicts the next time
someone runs the process.
