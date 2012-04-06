                                                                                
axl2map_xslt                                                                    
---------------                                                                 
                                                                                
A XSL Transform that converts ArcIMS .axl files into ~equivalent Mapserver      
.map files. This transform does the grunt work involved in translating ArcIMS   
.axl files into Mapserver .map files. The transform isn't perfect. Limitations  
of XSL and some differences between .axl format and .map format make it         
difficult to convert everything perfectly. Translation of layers with basic     
styling and scale restrictions works well. After the script is run, some        
manual effort is required to fill in the missing details in the output .map     
file. In particular, the layer DATA lines may be incomplete. How to run it:     
Run the XSL Transform through any XSL engine. I've always used Java Xalan, but  
it should work with other engines too.                                          
                                                                                
::                                                                              
                                                                                
    java org.apache.xalan.xslt.Process -IN foo.axl -XSL axl2map.xsl -OUT foo.map
                                                                                
}}
