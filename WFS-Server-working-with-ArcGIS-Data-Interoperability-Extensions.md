The ArcGIS Data Interoperability Extensions allow ArcMAP and ArcCatalog to connect to a mapserver WFS and WMS server.  However the ESRI products are fussy, so mapserver needs to be configured just right to get it to work.  Here are elements of the map file that seem important to have a general WMS and WFS service that works with a wide range of client programs including the ESRI Data Interoperability Extensions.

IF YOU HAVE MORE INFORMATION PLEASE UPDATE THIS WIKI PAGE.  The initial version of this wiki page contains mapfile fragments cut-n-pasted from a mapfile known to work as both a WMS server and a WFS server in both QGIS and ArcMAP.

Map overview:  The MAXSIZE should be large enough if your users might have large monitors.  The Extent is very important otherwise ARCGIS will get confused.  Of course the projection should be specified.  For example:

`

        MAP
            NAME "w00_Residential_Spa_130206_141543"
            # Map image size
            SIZE 1000 800
            MAXSIZE 4096
            UNITS meters
        EXTENT -117.596180469 32.5342726369 -116.08088026 33.5053791791

            FONTSET 'F:\djangoProjects\web_output\fontset.txt'
        PROJECTION
            "init=epsg:4326"
        END
`

For WMS I've been using the agg output format:

`
           
           OUTPUTFORMAT
                NAME agg
                DRIVER AGG/PNG
                IMAGEMODE RGB
            END
`

In the WEB section I think IMAGEPATH and IMAGEURL are necessary:

`
        
        # Set IMAGEPATH to the path where MapServer should
        # write its output.
        IMAGEPATH 'F:\djangoProjects\web_output\images/'
    
        # Set IMAGEURL to the url that points to IMAGEPATH
        # as defined in your web server configuration
        IMAGEURL 'http://www.example.com/mapimages/'
`

The METADATA section within the web section is very important.  The titles need to be specified, the online resource is important, and it should end with a &.  I've found the abstract to be extremely useful so that I know what's actually mapped.  

`

   		#WFSE WFS needs metadata    
        METADATA
          'ows_title' 'w00_Residential_Spa_130206_141543'
          'wfs_title' 'w00_Residential_Spa_130206_141543'
          'wfs_onlineresource' 'http://www.example.com/cgi-bin/mapserv.exe?map=F:/path/to/mapfiles/luz_mapfile_scen1_130206_141543_.map&'
          'wfs_src' 'EPSG:4326'
          'wfs_abstract' 'Map of price for commodity generated from all_exchange_results at 130206_141543.'
          'wfs_enable_request' '*'
          'wms_title' 'w00_Residential_Spa_130206_141543'
          'wms_onlineresource' 'http://www.example.com/cgi-bin/mapserv.exe?map=F:/path/to/mapfiles/luz_mapfile_scen1_130206_141543_.map&'
          'wms_src' 'EPSG:4326'
          'wms_abstract' 'Map of price for commodity generated from all_exchange_results at 130206_141543.'
          'wms_enable_request' '*'
        END


`

Outside of the METADATA but still inside the WEB section it's useful to have a TEMPLATE.  Some users will look at your WMS/WFS URL and try to open it in the browser.  The TEMPLATE html should show the resulting map and also explain to the users that this URL is to be used inside a GIS program (WMS client or WFS client), not inside a web browser. 

`

        TEMPLATE 'F:\djangoProjects\web_output\templates/mapit/mapserv_template.html'    

`


Here is a layer definition that is known to work.  Critical elements seem to be DUMP true, specifying the extent, specifying the ows and wfs titles and online resource, and specifying the types of the data columns (for WFS)

`

     LAYER
        NAME 'luz_wgs84'
        TYPE POLYGON
        DUMP true
        EXTENT -117.596180469 32.5342726369 -116.08088026 33.5053791791
        CONNECTIONTYPE postgis
        CONNECTION "dbname='db_sandag' host=postgis.example.com port=5432 user='postgisuser' password='postgispassword'"
        DATA 'the_geom FROM analysis."w00_Residential_Spa_130206_141543_with_geom" USING UNIQUE gid USING srid=4326'    
        METADATA
          'ows_title' 'luz_wgs84'
          'wfs_title' 'luz_wgs84_data'
          'wfs_onlineresource' 'http://www.example.com/cgi-bin/mapserv.exe?map=F:/path/to/mapfiles/SANDAG/luz_mapfile_scen1_130206_141543_.map&'
          'wfs_src' 'EPSG:4326'
          'wfs_abstract' 'Map of price for commodity generated from all_exchange_results at 130206_141543.'
           "gml_delta10_5_type" "xsd:double"
           "gml_per10_5_type" "xsd:double"
           "gml_avg2005price_type" "xsd:double"
           "gml_avg2006price_type" "xsd:double"
           "gml_avg2007price_type" "xsd:double"
           "gml_avg2008price_type" "xsd:double"
           "gml_avg2009price_type" "xsd:double"
           "gml_avg2010price_type" "xsd:double"
                     "gml_include_items" "delta10_5,per10_5,avg2005price,avg2006price,avg2007price,avg2008price,avg2009price,avg2010price"
          'wfs_enable_request' '*'
          'wms_title' 'luz_wgs84'
        END
        STATUS OFF
        TRANSPARENCY 100
        PROJECTION    
           "init=epsg:4326"
        END
        CLASS
           NAME 'luz_wgs84' 
           STYLE
             SYMBOL 0 
             SIZE 7.0 
             OUTLINECOLOR 0 0 0
             COLOR -1 -1 -1
           END
        END
      END

`