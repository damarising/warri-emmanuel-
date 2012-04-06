Database TILEINDEXes are a little odd in MapServer, they require 2 separate layers, a TILEINDEX layer and a RASTER layer that refers to the TILEINDEX layer.                                     
                                                                                                                                                                                                 
In Oracle, you'll want to create the TILEINDEX as a view to the GEORASTER table of interest.                                                                                                     
                                                                                                                                                                                                 
{{{create or replace view my_raster_tindex as select some_attribute, 'geor: user/pass@tns, raster_table_DATA,'||r.raster.rasterid location, R.RASTER.spatialextent shape from rasters_table r;}}}
                                                                                                                                                                                                 
This view will then be used for the TILEINDEX layer                                                                                                                                              
                                                                                                                                                                                                 
{{{                                                                                                                                                                                              
LAYER                                                                                                                                                                                            
  name raster_tindex                                                                                                                                                                             
  TYPE polygon                                                                                                                                                                                   
  PROJECTION                                                                                                                                                                                     
    "init=epsg:xxxx"                                                                                                                                                                             
  END                                                                                                                                                                                            
  CONNECTIONTYPE ORACLESPATIAL                                                                                                                                                                   
  CONNECTION user/pass@tns'                                                                                                                                                                      
  DATA "shape from my_raster_tindex using srid xxxx"                                                                                                                                             
END                                                                                                                                                                                              
}}}                                                                                                                                                                                              
                                                                                                                                                                                                 
Then you add a second layer that is the actual RASTER layer                                                                                                                                      
                                                                                                                                                                                                 
{{{                                                                                                                                                                                              
LAYER                                                                                                                                                                                            
  name db_raster_layer                                                                                                                                                                           
  TYPE RASTER                                                                                                                                                                                    
  PROJECTION                                                                                                                                                                                     
    "init=epsg:xxxx"                                                                                                                                                                             
  END                                                                                                                                                                                            
  TILEINDEX "raster_tindex"   #THIS NAME MUST MATCH THE TILEINDEX LAYER NAME                                                                                                                     
  TILEITEM "location"   #not actually needed if column is named location                                                                                                                         
  STATUS OFF                                                                                                                                                                                     
  OFFSITE 0 0 0                                                                                                                                                                                  
END                                                                                                                                                                                              
}}}                                                                                                                                                                                              
                                                                                                                                                                                                 
One last piece that is needed is to set the SHAPEPATH to null so that the paths to the raster files are treated as database connections and not files.                                           
                                                                                                                                                                                                 
Add                                                                                                                                                                                              
{{{                                                                                                                                                                                              
SHAPEPATH ""                                                                                                                                                                                     
}}
