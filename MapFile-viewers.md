## Built-in viewer
Since MapServer 6.0 you can use [built-in viewer](http://mapserver.org/cgi/openlayers.html).

## Mapfile developer script
Minimalistic HTML viewer for UMN MapServer mapfiles in simple
standalone WSGI server.

* Works for MapServer < 6.0
* Doesn't require web server running on Your machine. It creates its
own WSGI server which is more like a development environments should look
like (Django dev server).
* Usable debug messages in console
* Possibilities to override CONNECTION setting - in production one can use
different credentials than in development.
* Possibility to override some other parameters line EXTENT. It creates
possibility to work with many projects with the same base mapfile. More
overriding options to come.
* Simple mapfile syntax check tool

Download: https://github.com/imincik/mapfile-viewer

## QGIS Mapfile tools
QGIS mapfile viewer plugin.

QGIS plugin repository: http://build.sourcepole.ch/qgis/plugins.xml
Source code: https://github.com/sourcepole/qgis-mapfile-tools