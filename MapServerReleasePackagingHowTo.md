##Step by step release packaging instructions

1. Make sure the source builds and works locally

2. Test with msautotest.

3. Edit mapserver.h and HISTORY.TXT to update version info

   Note that in HISTORY.TXT we remove the "Current Version (SVN trunk)" header and replace it with a version header, with the release number followed by the release date inside round brackets, e.g.                                      

   ``` 
Version 5.0.0 2007-09-17)
--------------------------
   ```

   ... then **after** the tag is created we come back and reinsert the "Current Version (GIT master)" above the release header in preparation for more history. 

4. Create release tag, e.g.

   Look in https://github.com/mapserver/mapserver to see how we usually name the tags, here are a few examples:

   ``` 
rel-5-0-0-beta1
rel-5-0-0-beta2
rel-5-0-0-beta6
rel-5-0-0-rc1
rel-5-0-0-rc2
rel-5-0-0
rel-5-0-1
rel-5-0-2
rel-5-0-3
rel-5-2-0-beta1
   ``` 

   If working from master (NEED TO UPDATE):

   ``` 
$ svn mkdir -m "Creating rel-5-0-0 release tag" https://svn.osgeo.org/mapserver/tags/rel-5-0-0/                                                                                                                                         
$ svn copy -m "Tagging source as rel-5-0-0" \ 
  https://svn.osgeo.org/mapserver/trunk/mapserver/ \                                                                                                                                                                             
         https://svn.osgeo.org/mapserver/tags/rel-5-0-0/mapserver/                                                                                                                                                                      
$ svn copy  -m "Tagging msautotest as rel-5-0-0" \                                                                                                                                                                                      
         https://svn.osgeo.org/mapserver/trunk/msautotest/ \                                                                                                                                                                            
         https://svn.osgeo.org/mapserver/tags/rel-5-0-0/msautotest/
   ```

   If working from a branch:

   ```
$ git checkout branch-6-0
$ git tag rel-6.0.3
$ git push origin branch-6-0 --tags
   ``` 

5. Prepare source package on projects.osgeo.osuosl.org (a.k.a. [http://wiki.osgeo.org/wiki/ProjectsVM ProjectsVM]) server (this server is used to package the releases, and then we copy the resulting archive to the downloads server):
``` 
$ ssh projects.osgeo.osuosl.org                                                                                                                                                                                                         
$ cd /osgeo/mapserver/
$ ./build_package.sh 6.0.3 rel-6-0-3
$ scp mapserver-6.0.3.tar.gz <your_id>@download.osgeo.org:/osgeo/download/mapserver/

```

6. Update download area on website:

http://www.mapserver.org/download.html

Make sure to move the link to the top of the list of packages so it is easy for users to find.

7. Announcements:

* Prepare and send announcement to mapserver-users and mapserver-dev, also include mapserver-announce for official releases (but not for betas).                                                                                       
 * Make sure announcement includes a summary of changes since last release and link to source package download at a minimum.                                                                                                            
 * Also add a news item for the release on the mapserver.org homepage (or ask Jeff to do it).                                                                                                                                           
 * Update version info on freegis.org (or ask Daniel to do it)                                                                                                                                                                          
 * Update version info on freshmeat.net (or ask Steve to do it)                                                                                                                                                                         
 * Depending on the importance of the release, consider sending an update to:                                                                                                                                                           
   * The OSGeo Discuss list                                                                                                                                                                                                             
   * The OSGeo news feed: email news_item at osgeo.org (see http://www.osgeo.org/content/news/submit_news.html)

8. When to create branches?                                                                                                                                                                                                             
                                                                                                                                                                                                                                        
Branches are created as late as possible in the release cycle, ideally after the final release (i.e. after all betas and RCs are done).                                                                                                 
For the list of existing branches, see https://svn.osgeo.org/mapserver/branches/                                                                                                                                                        
                                                                                                                                                                                                                                        
Here are a few examples of branch names:                                                                                                                                                                                                

```                                                                                                                                                                                                                                     
     branch-4-8                                                                                                                                                                                                                         
     branch-4-10                                                                                                                                                                                                                        
     branch-5-0                                                                                                                                                                                                                         
     branch-5-2                                                                                                                                                                                                                         
     ...                                                                                                                                                                                                                                
```                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                        
To create a branch, use:                                                                                                                                                                                                                

```                                                                                                                                                                                                                                     
$ svn mkdir -m "Creating branch-5-2 directory" \                                                                                                                                                                                        
         https://svn.osgeo.org/mapserver/branches/branch-5-2/                                                                                                                                                                           
$ svn copy -m "Branching source to branch-5-2" \                                                                                                                                                                                        
         https://svn.osgeo.org/mapserver/trunk/mapserver/ \                                                                                                                                                                             
         https://svn.osgeo.org/mapserver/branches/branch-5-2/mapserver/                                                                                                                                                                 
$ svn copy -m "Branching msautotest to branch-5-2" \                                                                                                                                                                                    
         https://svn.osgeo.org/mapserver/trunk/msautotest/ \                                                                                                                                                                            
         https://svn.osgeo.org/mapserver/branches/branch-5-2/msautotest/

```

## build_package.sh script

Here is what the build_package.sh script looked like as of this writing. The version on projects.osgeo.osuosl.org is the master:

```
#!/bin/sh

cd /osgeo/mapserver

##
## Check command args
##

if [ "$1" = "-q" ] ; then
  QUIET=1
  shift
else
  QUIET=0
fi

VERSION=$1
GITTAG=$2

if [ -z "$VERSION" -o -z "$GITTAG" ] ; then
  echo "Usage: $0 [-q] <version> <git_tag>"
  echo ""
  echo "e.g."
  echo "  $0 6.0.3 rel-6-0-3 "
#  echo "or"
#  echo "  $0 nightly https://svn.osgeo.org/mapserver/trunk/mapserver"
  echo ""
  exit
fi

ARCHIVE=mapserver-$VERSION.tar.gz
BASEDIR=mapserver-$VERSION

if [ "$QUIET" = "0" ] ; then
  echo "Packaging $ARCHIVE using git tag $GITTAG"
fi

##
## Build the actual package
##

if [ -d $BASEDIR ]:
then
    rm -rf $BASEDIR
fi

git clone git://github.com/mapserver/mapserver.git $BASEDIR

cd $BASEDIR
git checkout $GITTAG
rm -rf gdft

cd mapscript/perl
#swig -perl5 -shadow -outdir . -o mapscript_wrap.c ../mapscript.i >& /dev/null
swig -perl5 -shadow -outdir . -o mapscript_wrap.c ../mapscript.i 

cd ../python
swig -python -shadow -outdir . -o mapscript_wrap.c ../mapscript.i

cd ../csharp
swig -csharp -o mapscript_wrap.c ../mapscript.i

#cd ../java

#cd ../tcl

cd ../..

rm -rf .git
cd ..
tar -zcvf $ARCHIVE $BASEDIR

##
## And cleanup
## 

rm -rf $BASEDIR

```
