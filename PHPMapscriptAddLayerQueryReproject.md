== Example 1 ==                                                                                                                                                                    
                                                                                                                                                                                   
Here's a small example which:                                                                                                                                                      
                                                                                                                                                                                   
 * opens a mapfile                                                                                                                                                                 
 * adds a new layer                                                                                                                                                                
 * queries new layer                                                                                                                                                               
 * reprojects output map                                                                                                                                                           
 * draws and saves to file                                                                                                                                                         
                                                                                                                                                                                   
                                                                                                                                                                                   
Example based on http://dl.maptools.org/dl/omsug/osgis2004/php_mapscript_mum2.ppt in response to http://n2.nabble.com/Problems-with-MapScript-and-setProjection-tt2291579.html#none
                                                                                                                                                                                   
Contributed by Tom Kralidis.                                                                                                                                                       
                                                                                                                                                                                   
{{{                                                                                                                                                                                
<?php                                                                                                                                                                              
                                                                                                                                                                                   
// load mapscript                                                                                                                                                                  
dl("php_mapscript.so");                                                                                                                                                            
                                                                                                                                                                                   
// add map                                                                                                                                                                         
$map = ms_newMapObj("config.map");                                                                                                                                                 
                                                                                                                                                                                   
// add new layer to map                                                                                                                                                            
$layer = ms_newLayerObj($map);                                                                                                                                                     
$layer->set("name", "foo");                                                                                                                                                        
$layer->set("status", MS_ON);                                                                                                                                                      
$layer->set("data", "path/to/file.shp");                                                                                                                                           
$layer->set("type", MS_LAYER_POINT);                                                                                                                                               
$layer->setProjection("init=epsg:4326");                                                                                                                                           
$layer->set("template", "ttt");                                                                                                                                                    
$layer->set("dump", "true");                                                                                                                                                       
                                                                                                                                                                                   
// add new class to new layer                                                                                                                                                      
$class = ms_newClassObj($layer);                                                                                                                                                   
$class->set("name", "foo");                                                                                                                                                        
                                                                                                                                                                                   
// add new style to new class                                                                                                                                                      
$style = ms_newStyleObj($class);                                                                                                                                                   
$style->set("symbol", 2);                                                                                                                                                          
$style->set("size", 8);                                                                                                                                                            
$style->color->setRGB(255, 0, 0);                                                                                                                                                  
                                                                                                                                                                                   
// create new rect to query against the new layer                                                                                                                                  
$rect = ms_newRectObj();                                                                                                                                                           
$rect->setExtent(-95, 40, -50, 60);                                                                                                                                                
                                                                                                                                                                                   
// query new layer                                                                                                                                                                 
$layer->queryByRect($rect);                                                                                                                                                        
                                                                                                                                                                                   
// set projection of output map                                                                                                                                                    
$map->setProjection("init=epsg:42304", MS_TRUE);                                                                                                                                   
// draw                                                                                                                                                                            
$image=$map->drawQuery();                                                                                                                                                          
$image->saveImage("/tmp/out.png");                                                                                                                                                 
                                                                                                                                                                                   
?>                                                                                                                                                                                 
}}}                                                                                                                                                                                
                                                                                                                                                                                   
== Example 2 ==                                                                                                                                                                    
                                                                                                                                                                                   
The example below works under !MapServer 5.2.1 (tested under Fedora 10).  Contributed by Tim Wood.                                                                                 
                                                                                                                                                                                   
{{{                                                                                                                                                                                
<?PHP                                                                                                                                                                              
                                                                                                                                                                                   
/**                                                                                                                                                                                
 * Code to create a map from a shapefile                                                                                                                                           
 * and then (optionally) reproject the whole thing                                                                                                                                 
 *                                                                                                                                                                                 
 * If you just want to look at the reprojection stuff, start                                                                                                                       
 * with the "reproject (if needed) and draw" section at the end.                                                                                                                   
 * The rest                                                                                                                                                                        
 *                                                                                                                                                                                 
 * Created by Tim Wood from code and input                                                                                                                                         
 * by Tom Kralidis [Ontario], Pietro Gianninni and Stephen Jones.                                                                                                                  
 *                                                                                                                                                                                 
 * No copyright restrictions                                                                                                                                                       
 */                                                                                                                                                                                
                                                                                                                                                                                   
                                                                                                                                                                                   
// ---------------------------                                                                                                                                                     
// Configuration                                                                                                                                                                   
// aka set a bunch of variables and load the mapscript extension                                                                                                                   
//                                                                                                                                                                                 
                                                                                                                                                                                   
// Where my key directories are located                                                                                                                                            
$doc_root = '/var/www/html';                                                                                                                                                       
$rel_path = "example1/round2";                                                                                                                                                     
                                                                                                                                                                                   
$base_path = "$doc_root/$rel_path";                                                                                                                                                
                                                                                                                                                                                   
                                                                                                                                                                                   
// An empty map file                                                                                                                                                               
$base_map = "$base_path/data/empty.map";   // just the words MAP and END on dif. lines... really                                                                                   
                                                                                                                                                                                   
// The data I'm using                                                                                                                                                              
$shape_path    = "$base_path/data/zcta/zt08_d00_shp/";    // a goofy path I'm too lazy to fix                                                                                      
$shape_file = "gfsa000b06a_e";                            // works with or w/o the extension                                                                                       
$shape_file_projection = "+proj=longlat +datum=NAD83";                                                                                                                             
                                                                                                                                                                                   
// The Extents (area) I want in latitude, longitude                                                                                                                                
list( $ex0, $ex1, $ex2, $ex3 ) = array(-140, 40, -50, 55 );                                                                                                                        
                                                                                                                                                                                   
// Describe the rest of my output                                                                                                                                                  
$output_projection = '';                                                                                                                                                           
$output_projection = "+proj=tcc +lon_0=90w +ellps=GRS80";                                                                                                                          
$output_file_rel = "output/index2.png";                                                                                                                                            
$output_file_path = "$base_path/$output_file_rel";                                                                                                                                 
list($output_width, $output_height ) = array( 600, 500 );                                                                                                                          
                                                                                                                                                                                   
// Load MapScript extension                                                                                                                                                        
if (!extension_loaded("MapScript"))  dl('php_mapscript.'.PHP_SHLIB_SUFFIX);                                                                                                        
                                                                                                                                                                                   
                                                                                                                                                                                   
                                                                                                                                                                                   
                                                                                                                                                                                   
// ---------------------------                                                                                                                                                     
// Function(s)                                                                                                                                                                     
//                                                                                                                                                                                 
                                                                                                                                                                                   
/**                                                                                                                                                                                
 * Reproject a $map from $shape_file_projection to $output_projection                                                                                                              
 */                                                                                                                                                                                
function reproject_map( &$map, $shape_file_projection, $output_projection ) {                                                                                                      
    $origProjObj = ms_newProjectionObj( $shape_file_projection );                                                                                                                  
    $newProjObj = ms_newProjectionObj( $output_projection );                                                                                                                       
                                                                                                                                                                                   
    $oRect = $map->extent;                                                                                                                                                         
    $oRect->project( $origProjObj, $newProjObj );                                                                                                                                  
    $map->setExtent( $oRect->minx, $oRect->miny, $oRect->maxx, $oRect->maxy );                                                                                                     
    $map->setProjection( $output_projection  );                                                                                                                                    
}                                                                                                                                                                                  
                                                                                                                                                                                   
                                                                                                                                                                                   
                                                                                                                                                                                   
// ---------------------------                                                                                                                                                     
// The Main Code                                                                                                                                                                   
//                                                                                                                                                                                 
                                                                                                                                                                                   
// add map                                                                                                                                                                         
$map = ms_newMapObj( $base_map );                                                                                                                                                  
$map->set( 'name', 'my_map' );    // If we're using a blank map file, give it a name                                                                                               
                                                                                                                                                                                   
// Set the extent                                                                                                                                                                  
$map->setExtent( $ex0, $ex1, $ex2, $ex3 );                                                                                                                                         
                                                                                                                                                                                   
// Set the shapepath                                                                                                                                                               
$map->set( 'shapepath', $shape_path );                                                                                                                                             
// Set the output format and size                                                                                                                                                  
$map->selectOutputFormat( 'png' );                                                                                                                                                 
$map->setSize( $output_width, $output_height );                                                                                                                                    
                                                                                                                                                                                   
// add new layer to map                                                                                                                                                            
$layer = ms_newLayerObj($map);                                                                                                                                                     
$layer->set("name", "foo");                                                                                                                                                        
$layer->set("status", MS_ON);                                                                                                                                                      
$layer->set("data", $shape_file );                                                                                                                                                 
$layer->set("type", MS_SHAPE_POLYGON );                                                                                                                                            
$layer->setProjection( $shape_file_projection );                                                                                                                                   
$layer->set("template", "ttt");    // hide errors that appear iff $output_projection is set?                                                                                       
$layer->set("dump", "true");                                                                                                                                                       
                                                                                                                                                                                   
// add new class to new layer                                                                                                                                                      
$class = ms_newClassObj($layer);                                                                                                                                                   
$class->set("name", "foo");                                                                                                                                                        
                                                                                                                                                                                   
// add new style to new class                                                                                                                                                      
$style = ms_newStyleObj($class);                                                                                                                                                   
$style->color->setRGB(255, 0, 0);                                                                                                                                                  
$style->outlinecolor->setRGB( 128,128,128 );                                                                                                                                       
                                                                                                                                                                                   
// reproject (if needed) and draw                                                                                                                                                  
if( $output_projection != '' ) {                                                                                                                                                   
    reproject_map( $map, $shape_file_projection, $output_projection );                                                                                                             
    $image = $map->drawQuery();                                                                                                                                                    
} else {                                                                                                                                                                           
    // draw                                                                                                                                                                        
    $image = $map->draw();                                                                                                                                                         
}                                                                                                                                                                                  
                                                                                                                                                                                   
                                                                                                                                                                                   
                                                                                                                                                                                   
// ---------------------------                                                                                                                                                     
// Output the image                                                                                                                                                                
//                                                                                                                                                                                 
                                                                                                                                                                                   
$image->saveImage( $output_file_path );                                                                                                                                            
                                                                                                                                                                                   
?>                                                                                                                                                                                 
<html>                                                                                                                                                                             
<body>                                                                                                                                                                             
    <img src="<?PHP print $output_file_rel; ?>">                                                                                                                                   
</body>                                                                                                                                                                            
</html>                                                                                                                                                                            
}}}                                                                                                                                                                                
                                                                                                                                                                                   
----                                                                                                                                                                               
back to [wiki:PHPMapScript
