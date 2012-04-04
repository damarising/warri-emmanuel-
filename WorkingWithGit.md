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



