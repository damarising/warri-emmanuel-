This page will be used for the upcoming Boston code sprint agenda (see http://wiki.osgeo.org/wiki/Boston_Code_Sprint_2013).

* general discussion on release plans
  * 6.2.1 release
  * timeline for 6.4 release
  * major features/goals
  * who will do what (assigning resources)
  * 7.0 release "moon shot" ideas
     * what are our dream features? (whiteboard session)
* native spatialite driver
* labelcache optimizations
* autotest coverage
* oracle autotests, hosted CI server at USACE ?
* issue squashing, clean up outdated issues
* libmapserver / stable c api
* switching from autotools to cmake
* fork cairo-svg to simplify svg symbol support
* leaner shapeObj. split actual shapeObj into geometryObj (containing only vertices + bbox) and featureObj (containing geometryObj and other shapeObj attributes)
* UTFGrids
* only apply substitutions from VALIDATION blocks ? (performance related)
* rework xxx_enable_request (refactoring, integration with ip blocking?)
* OGC compliance
* OSGeo Live
* documentation
  * modify overall template, re: ideas submitted by sdlime (jmckenna)
  * possible restructure of mapscript docs (jmckenna,havatv,sdlime) [ticket 4347](https://github.com/mapserver/mapserver/issues/4347)
  * change demo.mapserver.org to run off of 6.3-dev (jmckenna)
* flesh out, perhaps implement, RFC 91 (FILTER normalization)
* Mass RFC status updates (many don't reflect their real status)