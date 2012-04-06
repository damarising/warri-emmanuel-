For the release of MapServer 5.6, a XML schema has been defined to encode mapfiles in XML format, this is discussed in more details in [http://mapserver.org/development/rfc/ms-rfc-51.html MS RFC 51].                                                                                                                 
                                                                                                                                                                                                                                                                                                                        
MapServer 5.6 does not implement direct support for reading and writing that XML mapfile format, but a XSLT is included with the distribution to convert an XML mapfile into a regular mapfile usable with MapServer.                                                                                                   
                                                                                                                                                                                                                                                                                                                        
We hope that the ability for outside contributors to build Mapfile Editors will be facilitated by the existence of this XML mapfile format. If XML mapfile editors become available and popular, then we may consider implementing direct read/write support for the XML format in MapServer in a future release.       
                                                                                                                                                                                                                                                                                                                        
== Examples ==                                                                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                        
The gmap demo xml mapfile: [http://trac.osgeo.org/mapserver/attachment/wiki/XMLMapfiles/gmap75.xml gmap75.xml][[BR]]                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                                                        
The gmap demo xml mapfile transformed in a normal mapfile: [http://trac.osgeo.org/mapserver/attachment/wiki/XMLMapfiles/gmap75.map gmap75.map]                                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                        
== Getting Started: where to get the XML schema and XSL file? ==                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                                        
The XML schema and the XSL file are included with the distribution in the '''xmlmapfile''' directory.[[BR]]                                                                                                                                                                                                             
 * mapfile.xsd : The XML schema used to validate a XML mapfile.[[BR]]                                                                                                                                                                                                                                                   
 * mapfile.xsl : The XSL file used by XSLT to convert a XML mapfile into a regular mapfile.                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                        
== What kind of XML files can I create using the schema? ==                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                        
You can create 3 XML file types:                                                                                                                                                                                                                                                                                        
 * A complete XML mapfile: <Map ...></Map>                                                                                                                                                                                                                                                                              
 * A XML file for your layers: <LayerSet ...><Layer ...></Layer>...</LayerSet>                                                                                                                                                                                                                                          
 * A XML file for your symbols: <SymBolSet><Symbol ...></Symbol></SymbolSet>                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                        
'''Note:''' Due to some limitations of XML Schema recommendation (1.0), all tags have to be written in alphabetical order.[[BR]]                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                                        
There is one exampe of each type in the '''xmlmapfile/tests''' directory.                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                        
== How to use the XML schema? ==                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                                        
If you are using a XML editor like XMLSpy, you can easily specify the XML schema used to validate your current XML document. Furthermore, those editors will provide XML completition by listing allowed tags/attributes, automatic validation and more..                                                               
                                                                                                                                                                                                                                                                                                                        
If you prefer, you can also use a normal text editor. On Linux, there is a few tools to validate a XML file with its schema. On Ubuntu, you can do the following command:                                                                                                                                               

```                                                                                                                                                                                                                                                                                                                     
apt-get install xmlstarlet                                                                                                                                                                                                                                                                                              
xmlstarlet val -e --xsd mapfile.xsd my-new-mapfile.xml                                                                                                                                                                                                                                                                  
```                                                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                                        
== How to convert an XML mapfile to regular mapfile using the XSLT? ==                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                                                        
If you are using a XML editor, you can easily specify the XSL file to use and transform the document.                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                        
On Ubuntu, you can do the following command:                                                                                                                                                                                                                                                                            

```                                                                                                                                                                                                                                                                                                                     
apt-get install xsltproc                                                                                                                                                                                                                                                                                                
xsltproc mapfile.xsl my-new-mapfile.xml > /tmp/my-regular-mapfile.map                                                                                                                                                                                                                                                   
```                                                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                                        
Note: if you use XMLSpy or the MSXML xslt processor, you may need to use an external xslt processor for the transformation. These processors don't seem to support the use of the http://exslt.org/ xslt extension. xsltproc is also available for Windows: http://www.sagehill.net/docbookxsl/InstallingAProcessor.html
