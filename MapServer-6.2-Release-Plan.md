## New features in 6.2:
* RFC 70 : TinyOWS integration
* RFC 71 : MapCache integration
* RFC 73 : SVG symbology support
* RFC 75 : INSPIRE view services 
* RFC 76 : XMP Metadata support
* RFC 77 : Multiple labels or collision-detected symbols per feature
* RFC 78 : Vector Field rendering
* RFC 79 : Layer Masks
* RFC 80 : Multiple font fallback support
* RFC 81 : Offset labels with leader lines
* RFC 84 : Code and issues migrated to github

## Other notable enhancements include:
* GD support disabled by default. Enable with --with-gd
* labelcache: speedups and reduced memory usage
* postgis input: reduced memory usage
* proj: use the unlocked thread free if proj >= 4.8.0
* optional fastpath 4326->3857 reprojections
* libtool builds: make install is now required. the mapserv and shp2img tools now link to a shared libmapserver library
* python and php mapscript build: todo
* support for ELEVATION and DIM_* WMS dimensions
* precise symbol placement: ANCHORPOINT
* support for named group layer
* basic multi-language support in WMS

## Deprecated Features
* ANNOTATION layers are deprecated. All their functionality can be obtained using LABEL level STYLES with "labelpnt" GEOMTRANSFORM. Current annotation layer functionality is kept for the time-being, but new features (layer masking, multiple labels per feature, label offsetting) will bail out if applied to an annotation layer.
* TODO

See also the HISTORY.TXT for this release:
https://github.com/mapserver/mapserver/blob/branch-6-2/HISTORY.TXT

A 6.0 -> 6.2 migration guide is being worked on. For now it lives in the source tree but should become part of official documentation soon:
https://github.com/mapserver/mapserver/blob/branch-6-2/MIGRATION_GUIDE.TXT                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                           
##Planned Dates
                                                                                                                                                                                                                                                                                           
We will plan for 3 betas and 2 release candidates (RC) over a 6 week period after the feature freeze (1 beta/RC per week each Wednesday). This will lead us to a final release sometime around TBD:                                                                                                        
                                                                                                                                                                                                                                                                                                           
    * branch-6-2 creation : Wed. April 18, 2012                                                                                                                                                                                                                                                                 
    * 6.2.0-beta1 -                                                                                                                                                                                                                                                                                        
    * 6.2.0-beta2 -                                                                                                                                                                                                                                                                                        
    * 6.2.0-beta3 -                                                                                                                                                                                                                                                                                        
    * 6.2.0-rc1 -                                                                                                                                                                                                                                                                                          
    * 6.2.0 (final) -                                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                                           
## Release Manager (see http://mapserver.org/development/rfc/ms-rfc-34.html)                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                                                           
Thomas Bonfort
                                                                                                                                                                                                                                                                                                           
## Git Tags/Branches

* The current master branch is currently the 6.1 development version that we plan to release as 6.2
* The stable branch for this release will be called "branch-6-2" (not created yet).
* Once branch-6-2 is released, all commits targeted for 6.2 **must** be pushed to branch-6-2 only. Merging
these fixes to master can/will be done by merging branch-6-2 into master. All commits to master should only concern features targeted for 6.4.
* The betas will be tagged in SVN as "rel-6-2-0-beta1", "rel-6-2-0-beta2", ... and the release candidates as "rel-6-2-rc1", "rel-6-2-rc2", etc...

## Ticket Conventions                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                           
In order to facilitate querying the Gihub issue database for tickets that still need to be addressed for this release, we try to stick to the following conventions:

* Tickets to be addressed for this release must have their target milestone set to "6.2 release"                                                                                                                                                                                                       
* Bugs/Enhancements? that can't make it in this release but that we may want to address at a later time should be marked with the "FUTURE" target milestone with a comment explaining that the bug is postponed and if possible a quick analysis
* The target milestone on a ticket should be set by the developers (bug owners) and not by the users (reporters).

Other good practices when dealing with tickets:

* Please file tickets for any non-trivial bugfix or change to the software. This is so that we keep a trace for future reference of all bugfixes and changes that were made (why and how).
* Please assign yourself to an issue as soon as you start working on it.
* Please include the bug number in the commit message, with the #xxxx notation. This will automatically link the commit to the ticket itself for future reference.
* Issues can be automatically closed from a commit message. Include a line with "closes #xxxx" in your commit message.
* Keep documentation in mind when fixing/changing things: if you cannot update the documentation yourself then please create a documentation bug describing the new feature/change and which document(s) should be updated.                                                                            
                                                                                                                                                                                                                                                                                                           
The following query returns all currently open bugs that are tagged with the "6.2 release" target milestone:
https://github.com/mapserver/mapserver/issues?milestone=31&state=open

You can further limit the issues assigned to you or targeting a specific component by clicking on the links to the left of the issue list
                                                                                                                                                                                               
##QA
TODO

##Open Tasks
Update release procedure from svn->git