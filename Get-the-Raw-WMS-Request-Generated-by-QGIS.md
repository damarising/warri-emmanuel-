  - download and install the software "Fiddler" (Google will help you find its current 
download location)
  - open it and leave it running
  - in QGIS goto /Settings/Options/Network
  - click on "Use proxy for web access"
  - for Proxy Type select "HttpProxy"
  - for Host enter: 127.0.0.1
  - for Port enter: 8888
  - presss "ok" button
  - now Add a WMS/WFS/WCS layer in QGIS again, but watch the Fiddler window
  - click on the request in Fiddler (such as GetCapabilities, GetMap, or GetCoverage)
  - to see the full requested url in Fiddler, check the 'Inspector' tab, 
and then select 'Raw'
  - then you can copy the raw url into a new browser window, load it, and examine the error and the url parameters

This is a handy trick especially when debugging MapServer services. 
(beware that QGIS caches the service though, see previous [warnings by me](https://lists.osgeo.org/pipermail/qgis-developer/2016-August/044000.html) for that).