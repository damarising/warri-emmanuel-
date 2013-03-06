This page will be used for the upcoming Boston code sprint agenda (see http://wiki.osgeo.org/wiki/Boston_Code_Sprint_2013).

* general discussion on 6.4 plans
  * timeline for release
  * major features/goals
  * who will do what (assigning resources)
* native spatialite driver
* labelcache optimizations
* autotest coverage
* oracle autotests, hosted CI server at USACE ?
* issue squashing, clean up outdated issues
* libmapserver / stable c api
* leaner shapeObj. split actual shapeObj into geometryObj (containing only vertices + bbox) and featureObj (containing geometryObj and other shapeObj attributes)
* UTFGrids
* only apply substitutions from VALIDATION blocks ? (performance related)
* rework xxx_enable_request (refactoring, integration with ip blocking?)
* OGC compliance
* OSGeo Live
* documentation
  * modify overall template, re: ideas submitted by sdlime (jmckenna)
  * change demo.mapserver.org to run off of 6.3-dev (jmckenna)
  * possible restructure of mapscript docs (jmckenna,havatv,sdlime) [ticket 4347](https://github.com/mapserver/mapserver/issues/4347)
* flesh out, perhaps implement, RFC 91 (FILTER normalization)