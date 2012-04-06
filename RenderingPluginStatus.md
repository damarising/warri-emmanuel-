This page documents the rendering plugin implementation status for MapServer version 6.0. To date work has accomplished primarily by Thomas Bonfort with the exception of the KML and OpenGL work.
                                                                                                                                                                                                  
What Works:                                                                                                                                                                                       
                                                                                                                                                                                                  
  * create image function (into an imageObj)                                                                                                                                                      
    * notes...                                                                                                                                                                                    
  * basic vector API is in place (e.g. draw line, fill polygon, etc...)                                                                                                                           
    * Cairo Graphics is implemented in trunk                                                                                                                                                      
    * AGG is minimally implemented in a sandbox                                                                                                                                                   
    * KML SoC work uses this API                                                                                                                                                                  
    * OpenGL is minimally implemented in trunk (status?)                                                                                                                                          
  * basic text API is in place                                                                                                                                                                    
    * performance concerns regarding font handling                                                                                                                                                
    * per glyph or per string?                                                                                                                                                                    
  * symbols support works for the most part                                                                                                                                                       
    * exception in PIXMAP symbols                                                                                                                                                                 
                                                                                                                                                                                                  
Remaining Tasks:                                                                                                                                                                                  
                                                                                                                                                                                                  
  * implement lazy PIXMAP loading                                                                                                                                                                 
  * write our own image loaders (into an imageObj), this is part of the rendering API                                                                                                             
    * using libpng, libjpeg and something for GIF                                                                                                                                                 
    * drivers could choose to implement their own (e.g. GD)                                                                                                                                       
  * write our own image writers (from an imageObj), again, this is part of the rendering API                                                                                                      
    * using libpng, libjpeg and something for GIF                                                                                                                                                 
    * drivers could choose to implement their own (e.g. GD)                                                                                                                                       
    * Thomas has implemented PNG and JPEG writers already, minus color pre-preocessing and some other output optionss                                                                             
  * raster API implementation                                                                                                                                                                     
    * how to deal with potentially different image buffers (e.g. GD vs. everything else)?                                                                                                         
  * implement color pre-processing                                                                                                                                                                
    * quantization                                                                                                                                                                                
    * is there still a need for a partial palette?                                                                                                                                                
  * existing renderer re-writes                                                                                                                                                                   
    * GD                                                                                                                                                                                          
    * vector drivers (PDF/SVG/Flash), may, in some cases, be replaced by Cairo                                                                                                                    
  * symbol API gaps                                                                                                                                                                               
    * use an imageObj instead of a gdImagePtr                                                                                                                                                     
  * vector API gaps                                                                                                                                                                               
    * polygon fills using a tile                                                                                                                                                                  
    * hatching support                                                                                                                                                                            
      * possibility exists to create a hatch generator (e.g. all the lines) and we'd pass those to the renderer                                                                                   
      * GD driver has code to compute the lines, but they aren't clipped                                                                                                                          
  * remove GD dependencies from certain functional areas, specifically scalebar, legend and reference map generation                                                                              
    * embedding may be difficult, however, if the symbolObj changes to use imageObj's for PIXMAP symbols then the current process can probably be retained                                        
                                                                                                                                                                                                  
                                                                                                                                                                                                  
 
