This page provides step by step instructions to setup a Ubuntu 12.04 LTS server from scratch to render OSM data with MapServer 6.x.

The following instructions assume that we start with a brand new Ubuntu 12.04 server with Apache installed but none of the MapServer related packages installed.

The setup will include:

 * MapServer 6.x (from the UbuntuGIS repository)
 * PostgreSQL/PostGIS
 * OSM data to be downloaded from http://download.geofabrik.de/ (up-to-date from OSM) or http://downloads.cloudmade.com/ (2011 datas from OSM)
 * Use of "imposm" to load the data in PostGIS: http://imposm.org/
 * Map configured in EPSG:3857 projection, with data loaded for the state of Rhode Island only, and using a Google-like style
 * MapFile generated using "basemaps": https://github.com/mapserver/basemaps
 * Tile caching using MapCache: https://github.com/mapserver/mapcache

## Create working directory

 * All the steps that follow assume that the data and mapfiles will be installed in a directory called "osm-demo" in your home directory
 * Commands:

```bash
    mkdir ~/osm-demo
    cd ~/osm-demo/
```

## Install UbuntuGIS MapServer and PostGIS packages

 * Relevant docs:  http://trac.osgeo.org/ubuntugis/wiki/UbuntuGISRepository
 * At the time of this writing, the current version of MapServer is 6.0.3 (Should also work with 6.2)
 * Commands:

```bash
  sudo apt-get install python-software-properties
  sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable
  sudo apt-get update
  sudo apt-get install cgi-mapserver mapserver-bin

  sudo apt-get install postgresql-9.1-postgis postgresql-server-dev-9.1
```

 * Install other non-GIS packages that will be required later on:

```bash
  sudo apt-get install mercurial subversion git
  sudo apt-get install zip unzip
```

## Download OSM data

 * You can download data for your region of interest from http://download.geofabrik.de/ (up-to-date from OSM) http://downloads.cloudmade.com/ (2011 datas from OSM). The rest of these instructions assume that we work with the data for the state of Rhode Island.
 * Commands:

```bash
  cd ~/osm-demo/
  wget http://downloads.cloudmade.com/americas/northern_america/united_states/rhode_island/rhode_island.osm.bz2
```

## Install imposm, using virtualenv

 * Relevant docs: http://imposm.org/docs/imposm/latest/install.html
 * Install dependencies and create python virtual env:

```bash
  cd ~/osm-demo/
  sudo apt-get install build-essential python-dev protobuf-compiler \
                      libprotobuf-dev libtokyocabinet-dev python-psycopg2 \
                      libgeos-c1

  sudo apt-get install python-virtualenv
  virtualenv venv
  source venv/bin/activate
```
 * Install shapely speedups: (shapely 1.2.11 has bugs, do not use it)

```bash
  sudo apt-get install libgeos-dev
  pip install Shapely
```
 * Install imposm:
Note that up until recently imposm did not create the generalized tables used by the basemaps mapfile. Since version 2.3.0 this is not the case anymore, so there is no need
to install a forked version of imposm as was the case in the first versions of this document

```bash
  pip install imposm
```

## Create database

 * Relevant docs: http://imposm.org/docs/imposm/latest/tutorial.html
 * Commands:

```bash
  imposm-psqldb > create-db.sh
  vi ./create-db.sh # cross check if all path are set properly
... remove the following line:
-------------------8<--------------
createlang plpgsql osm
------------------->8--------------

  sudo su postgres
  sh ./create-db.sh
  exit
  sudo service postgresql restart
```


## Load data using imposm

 * Relevant docs: http://imposm.org/docs/imposm/latest/tutorial.html
 * Commands:

```bash
  cd ~/osm-demo/
  imposm --proj=EPSG:3857 --read rhode_island.osm.bz2
  imposm --proj=EPSG:3857 --write --database osm --host localhost --user osm
  (... if prompted for db password, the default is osm)
  imposm  --optimize -d osm
```
 * Note: multiple .osm.pbf files can be loaded in separate commands using the --merge-cache argument.

## Install basemaps mapfile generator

 * You have two choices of Basemaps you can use 
   * the official release https://github.com/mapserver/basemaps 
   * the Mapgears modified version https://github.com/mapgears/basemaps (Currently only working with 6.0.1 and 6.2)

### Using the Official release from Github:
```bash
  cd ~/osm-demo/
  git clone https://github.com/mapserver/basemaps.git
  cd basemaps

... Update basemaps' osmbase.map and Makefile as follows:
  vi osm-base.map
-------------------8<------------------------
  WEB
...
    IMAGEPATH "/tmp/ms_tmp/"
    IMAGEURL "/ms_tmp/"
 END
...
-------------------->8-----------------------

  vi Makefile
-------------------8<------------------------
OSM_SRID=3857
OSM_UNITS=meters
OSM_EXTENT=-20000000 -20000000 20000000 20000000
...
STYLE=google
-------------------->8-----------------------
```
 * With trunk, the '''STYLE''' parameter possible values are the keys of the style_aliases hash, which can be found in the generate_style.py file.  For example: google
 * Create MapServer temp dirs (required by mapserv CGI for testing with openlayers template)

```bash
  mkdir /tmp/ms_tmp
  chmod 777 /tmp/ms_tmp
```
 * Initialize the data and generated folder. You only need to do this once. It may take a while to download everything.

```bash
  cd basemaps
  mkdir generated
  cd data
  make
```

 * Execute the basemaps makefile to generate the mapfile.

```bash
  cd basemaps
  make
```
 

### Using the Mapgears release from Github:
```bash
  cd ~/osm-demo/
  git clone https://github.com/mapgears/basemaps.git
  cd basemaps
```
 * With trunk, the '''STYLE''' parameter possible values are the keys of the style_aliases hash, which can be found in the generate_style.py file.  For example: google
 * Create MapServer temp dirs (required by mapserv CGI for testing with openlayers template)

```bash
  mkdir /tmp/ms_tmp
  chmod 777 /tmp/ms_tmp
```
 * Configure rc.local to recreate the ms_tmp folder at startup.
```bash
sudo vi /etc/rc.local
```

 * Add the following lines to the rc.local file

```bash
mkdir /tmp/ms_tmp
chown www-data:www-data -R /tmp/ms_tmp
```

 * Initialize the data and generated folder. You only need to do this once. It may take a while to download everything.

 NOTE : 10m-admin-0-boundary-lines-land.zip is not use anymore 

```bash
  cd basemaps
  mkdir generated
  cd data
  make
```

 * Execute the basemaps makefile to generate the mapfile.

```bash
  cd basemaps
  make
```
 * The 'make' command will have generated the mapfile based on the parameters in the generate_style.py script.
 * More information about tweaking the map styles is available at https://github.com/mapserver/basemaps/wiki/Tweaking-Map-Styles

 * Access your map online using MapServer's built-in template=openlayers mode:
                                                                                                                                                                                                                                                                                                                                                                                                                                            http://yourserver.tld/cgi-bin/mapserv?map=/path/to/osm-demo/basemaps/osm-google.map&mode=browse&template=openlayers&layers=all

## Setup MapCache

 * Read more about Mapcache : http://mapserver.org/mapcache/index.html
 * With UbuntuGIS previously setup, you can install mapcache quickly

```bash
  sudo apt-get install libmapcache mapcache-cgi mapcache-tools libapache2-mod-mapcache
```


 * Create 'cache' directory writable by Apache (www-data) user

```
  mkdir ~/osm-demo/mapcache/cache
  sudo chown www-data ~/osm-demo/mapcache/cache/
```

 * You need to create the Mapcache configuration file.   

```bash
  sudo vi /path/to/directory/mapcache-osm.xml
```

 * Add this in your mapcache.xml and edit your path setup.  You can read more about all configuration option in this sample https://github.com/mapserver/mapcache/blob/master/mapcache.xml.sample.

```
<?xml version="1.0" encoding="UTF-8"?>
<!-- see the accompanying https://github.com/mapserver/mapcache/blob/master/mapcache.xml.sample for a fully commented configuration file -->
<mapcache>

   <cache name="disk" type="disk">
      <base>/path/to/osm-demo/mapcache/cache</base> <!-- Change Path here -->
      <symlink_blank/>
   </cache>

   <source name="osm" type="wms">
      <getmap>
         <params>
            <FORMAT>image/png</FORMAT>
            <LAYERS>default</LAYERS>
	    <MAP>/path/to/osm-demo/basemaps/osm-google.map</MAP> <!-- Change Path here -->
         </params>
      </getmap>
      <http>
         <url>http://yourserver.tld/cgi-bin/mapserv?</url> <!-- Change server here -->
      </http>
   </source>

   <tileset name="osm">
      <metadata>
	 <title>OSM MapServer served map</title>
	 <abstract>see https://github.com/mapserver/mapserver/wiki/RenderingOsmDataUbuntuv2</abstract>
      </metadata>
      <source>osm</source>
      <cache>disk</cache>
      <grid>WGS84</grid>
      <grid>g</grid>
      <format>PNG</format>
      <metatile>5 5</metatile>
      <metabuffer>10</metabuffer>
      <expires>10000</expires>
      <auto_expire>86400</auto_expire>	
   </tileset>


   <default_format>JPEG</default_format>

   <service type="wms" enabled="true">
      <full_wms>assemble</full_wms>
      <resample_mode>bilinear</resample_mode>
      <format>JPEG</format>
      <maxsize>4096</maxsize>
   </service>

   <service type="wmts" enabled="true"/>
   <service type="tms" enabled="true"/>
   <service type="kml" enabled="true"/>
   <service type="gmaps" enabled="true"/>
   <service type="ve" enabled="true"/>
   <service type="demo" enabled="true"/>

   <errors>report</errors>
   <lock_dir>/tmp</lock_dir>

</mapcache>
```

 * Add directives to Apache config:

```bash
  sudo vi /etc/apache2/sites-available/default
```

 * Add the following lines to the end of the default VirtualHost (update the '/path/to/' directory name):

```bash
<IfModule mapcache_module>
   <Directory /path/to/directory>
      Order allow,deny
      Allow from all
   </Directory>
   MapCacheAlias /mapcache "/path/to/directory/mapcache-osm.xml"
</IfModule>
```

* Finally, restart Apache to take the modified configuration into account

```bash
 sudo apache2ctl restart
```



 * Ready to test server, see URL docs at http://code.google.com/p/mod-geocache/wiki/RequestingTilesFromService#TMS_service
 * Demo site at http://yourserver.tld/mapcache/demo/
 * TMS service available at http://yourserver.tld/mapcache/tms/1.0.0/
   * Sample tile:  http://yourserver.tld/mapcache/tms/1.0.0/osm@g/3/2/5.png
 * Test using the sample OpenLayers app to test TMS: http://openlayers.org/dev/examples/tms.html
   * URL of TMS (Should end in /): http://yourserver.tld/mapcache/tms/
   * layer_name: osm@g
   * Format: PNG
   * ... and hit submit. You should see a browsable map of the world. If not then check your Apache error logs for errors.

 * You can optionally run the cache seeder, see: http://mapserver.org/mapcache/seed.html