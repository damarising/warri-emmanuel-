##Step by step release packaging instructions

1. Make sure the source builds and works locally

2. Test with msautotest.

3. Create a branch ( only for a major version release)
```
 $ git checkout master
 $ git checkout -b branch-x-y
```

4. Inside the release branch, edit mapserver.h and HISTORY.TXT to update version info
```
$ git checkout branch-6-2
```
Note that in HISTORY.TXT we remove the "Current Version (Git master)" header and replace it with a version header, with the release number followed by the release date inside round brackets, e.g. ```Version 6.2.0-beta1 (20012-06-29)```
```
 $ git add mapserver.h HISTORY.TXT
 $ git commit -m "updated mapserver.h and HISTORY.TXT for 6-2-0-beta1 release"
```
4. Create a release tag, following usual tag naming conventions (see output of ```git tag```), e.g: ```rel-5-0-0-beta1```,```rel-5-0-0-rc1```,```rel-5-0-0```,```rel-5-0-1```
```
 $ git tag -a rel-6-2-0-beta1 -m "Create 6-2-0-beta1 tag"
```

5. Push your changes back to github
```
 $ git push origin branch-6-2 --tags
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
