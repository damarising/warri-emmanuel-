This page is currently a parking place for collecting information and thoughts on what must be done to provide OGC Web Feature Service 2.0.0 (also ISO 19142) support within !MapServer. It is still at an early stage: if you have more information then please add it! Once the requirements are clear, a RFC will be prepared.                                                                                                     

MapServer already supports WFS1.1, so this page is largely focussed on the differences between this and WFS2.0, and what impact this has on MapServer.                                                                                                                                                                                                                  Note that support for WFS2.0 is a prerequisite for support of the [wiki:InspireDownloadService INSPIRE Download Service].

## Summary of changes from WFS1.1

### Joins 
WFS2.0 adds support for joins as part of a !GetFeature request.

This goes beyond the joins currently supported by !MapServer (which are 1:n from spatial objects to attributes/non-spatial objects) to include spatial and attribute joins between layers. Could have a significant impact, but also offers the opportunity for true support of complex spatial objects since many of the requirements and internals should be identical.

### FilterEncoding2.0
The filter encoding is updated, including an extension to cover temporal filters.                                                                                                      Parser needs to be updated and support for new filter types added as necessary.

### Stored queries
Filters may be stored and used in queries, including placeholder values.                                                                                                       Could maybe be implemented using query layers? Requires a persistance mechanism between queries if the (optional) !CreateStoredQuery (and !DropStoredQuery) should be supported. The (very) poor-man's implementation would be to just process the (required) !ListStoredQueries and !DescribeStoredQueries requests and return "empty" responses (i.e. add minimal support for the interface without actually implementing any internals).

### GetPropertyValue
A new mandatory request !GetPropertyValue is defined for getting attribute values.
Should be relatively straightforward: the basic functionality is already available it just needs the interface to it.

### Output formats
The support for output formats other than GML (3.2.1 / ISO19136) is more explicitly defined, although not required. Would be a nice-to-have using OGR to do the donkey work.

### Reference
[http://www.opengeospatial.org/standards/wfs WFS page @OGC]

Other resources on the web
- [http://geoserver.org/display/GEOS/GSIP+61+-+WFS+2.0 GeoServer GSIP] how the Java folks did it                                                                                                            - [http://www.weichand.de/tag/wfs-2-0/ Blog posts by Jürgen Weichand] introducing WFS 2.0 (in German
