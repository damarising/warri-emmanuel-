## New features in 7.2
* RFC XX: Vector tile generation.
* RFC-113 Extension: compositing filters (blurring, shadows)
* RFC-112: Better placement of follow labels

## Other notable enhancements include:
* TBD

## Deprecated Features
* None

See also:
* https://github.com/mapserver/mapserver/blob/branch-7-2/HISTORY.TXT
* http://mapserver.org/MIGRATION_GUIDE.html#mapserver-7-0-to-7-2-migration
                                                                   
##Planned Dates

We will plan for a sufficient number of betas and release candidates (RC) aiming for a final release sometime around mid-October 2016:

* branch-7-2 creation, 7.2.0-beta1 : TBD
* 7.2.0-beta2
* ...
* 7.2.0-rc1
* ...
* 7.2.0 (final, tentative: mid-October 2016)

## Release Manager (see http://mapserver.org/development/rfc/ms-rfc-34.html) 
TBD: 
                                                          
## Git Tags/Branches

* The current master branch is currently the 7.1 development version that we plan to release as 7.2
* The stable branch for this release will be called "branch-7-2" (not created yet).
* Once branch-7-2 is released, all commits targeted for 7.2 **must** be pushed to branch-7-2 only. Merging
these fixes to master can/will be done by merging branch-7-2 into master. All commits to master should only concern features targeted for 7.4 (or 8.0).
* The betas will be tagged in SVN as "rel-7-2-0-beta1", "rel-7-2-0-beta2", ... and the release candidates as "rel-7-0-rc1", "rel-7-0-rc2", etc...

## Managing HISTORY.TXT and Release Changelogs

* Our practice of dealing with HISTORY.TXT needs to be updated in order to account for the more frequent merging practices that come with git usage.
* Once 7.2 has been branched, commits to the stable branches are *not* reflected into HISTORY.TXT. Instead of this, the commit message *must* be sufficiently explicit to be incorporated in the stable branch Changelog that will be more or less automatically generated on release day. A typical commit message in a release branch *should* follow the following best practices (c.f. http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html ):
 * Use a short single line summary as the first line
 * include the ticket number in this first line
 * if additional information needs to be added, write them in a paragraph, ideally wrapped at 72 chars, after skipping a blank line.

An example commit message could be:
```
Fix incorrect handling of foobar (#XXXX)

Optional detailed message as to why this was needed,
along with any information that might seem relevant.
This commit message will appear when annotating the
source code, so any caveats or tricky implications can
be mentioned here to.
```
 
## Ticket Conventions
In order to facilitate querying the Gihub issue database for tickets that still need to be addressed for this release, we try to stick to the following conventions:

* Tickets to be addressed for this release must have their target milestone set to "7.2 release" 
* Bugs/Enhancements? that can't make it in this release but that we may want to address at a later time should be set to no milestone with a comment explaining that the bug is postponed and if possible a quick analysis
* The target milestone on a ticket should be set by the developers (bug owners) and not by the users (reporters).

Other good practices when dealing with tickets:

* Please file tickets for any non-trivial bugfix or change to the software. This is so that we keep a trace for future reference of all bugfixes and changes that were made (why and how).
* Please assign yourself to an issue as soon as you start working on it.
* Please include the bug number in the commit message, with the #xxxx notation. This will automatically link the commit to the ticket itself for future reference.
* Issues can be automatically closed from a commit message. Include a line with "closes #xxxx" in your commit message.
* Keep documentation in mind when fixing/changing things: if you cannot update the documentation yourself then please create a documentation bug describing the new feature/change and which document(s) should be updated.                                                                            

The following query returns all currently open bugs that are tagged with the "7.2 release" target milestone:
https://github.com/mapserver/mapserver/issues?milestone=38&state=open

You can further limit the issues assigned to you or targeting a specific component by clicking on the links to the left of the issue list
                                                                          
##QA
TBD

##Open Tasks

- [ ] Open Pull Requests
- [ ] Write migration guide
- [ ] Update HISTORY.TXT