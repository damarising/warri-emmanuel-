                                                                                                                      
== Progress on incubation ==                                                                                          
    ... finally about ready to graduate                                                                               
                                                                                                                      
== 5.2 release in July 2008 ==                                                                                        
    * Nearly 200 tickets were closed as part of this effort.                                                          
    * Notable features and enhancements include:                                                                      
      * MapServer users manual (docs wrapped into a single downloadable PDF)                                          
      * Performance enhancements for large shapefile handling (following Geoserver comparison)                        
      * Performance enhancements for AGG rendering (an order of magnitude faster in some cases)                       
      * Rendering improvements:                                                                                       
        * fuzzy label outlines for AGG                                                                                
        * style-level opacity for AGG                                                                                 
        * quantization and palette support for PNGs and RGBA modes                                                    
        * pre-clip label point computation (helps in tile generation)                                                 
      * OGC Web Services:                                                                                             
        * WCS 1.1.0 support (RFC-41)                                                                                  
        * SOS 1.0.0 support                                                                                           
        * WFS 1.1.0 support                                                                                           
        * Compliance testing (WMS 1.1.1 ready to certify)                                                             
      * proxy and http authentication support for cascaded WMS requests                                               
      * Support for auth cookie forwarding (RFC-42)                                                                   
      * Direct tile generation for Google Maps API (RFC-43)                                                           
      * Native Microsoft SQL Server 2008 Support (RFC-38)                                                             
      * extent (e.g. shpext, mapext) template tag attribute handling                                                  
      * Simplified Template Support for Query Output (MS RFC 36)                                                      
    * Thanks to all contributors (devs, docs, testers, $$). Special thanks to the USACE for funding lots of that work.
                                                                                                                      
== What's coming ==                                                                                                   
    * 5.4 release early 2009                                                                                          
    * Tuning and performance optimizations                                                                            
    * New features / enhancements:                                                                                    
      - WMS 1.3.0                                                                                                     
      - KML output                                                                                                    
      - XML mapfile???                                                                                                
      - Rendering enhancements:                                                                                       
        - fractional line width (0.5 pixel lines with AGG)                                                            
        - label positioning enhancements (following comparison with Geoserver)                                        
        - and many others as detailed in RFC 45                                                                       
        - unified rendering API (merge GD, AGG, etc...), this is a *hopefully* pending GD 3.0                         
        - label text transformations RFC 40                                                                           
        - hoping that a OpenGL renderer will be donated to the project???                                             
      - URL-based configuration expansion (RFC-44)                                                                    
      - scale-dependent tile index support                                                                            
     * Docs/demos                                                                                                     
       - Docs: debugging and tuning guide                                                                             
       - Work on a new website or improve existing site                                                               
       - New mapserver demo (based loosely on the OpenLayers functional demos)                                        
                                                                                                                      
== Stats ==                                                                                                           
   * Number of members on lists:                                                                                      
      * users:  1843                                                                                                  
      * dev:    339                                                                                                   
   * Welcome 3 new committers:                                                                                        
      * Jeff !McKenna                                                                                                 
      * Paul Ramsey                                                                                                   
      * Alan Boudreault                                                                                               

