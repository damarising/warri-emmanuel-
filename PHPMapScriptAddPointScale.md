This shows you how to add a point like a city to your mapscript and to use min/max scaledenom to make the feature appear or disappear depending on your scale.
                                                                                                                                                              

```                                                                                                                                                           
<?php                                                                                                                                                         
                                                                                                                                                              
// example written by mdev and special thanks to ue and others on #mapserver that helped me resolve the issues                                                
                                                                                                                                                              
                                                                                                                                                              
ini_set("display_errors", 1); // show errors for debug                                                                                                        
                                                                                                                                                              
//dl('php_mapscript.so'); // if you need to load mapscript.so                                                                                                 
                                                                                                                                                              
$map_path="../mapfiles/"; // set these to your path                                                                                                           
$map = ms_newMapObj("../mapfiles/mapsimple.map"); // set to your mapfile                                                                                      
                                                                                                                                                              
// city layer                                                                                                                                                 
                                                                                                                                                              
$layer = ms_newLayerObj($map);                                                                                                                                
$layer->set("status", MS_ON);                                                                                                                                 
$layer->set("type", MS_LAYER_POINT);                                                                                                                          
                                                                                                                                                              
$layer->set("minscaledenom", 70); // <------ Shows up if denom<71 and doesn't show if denom>=71                                                               
// you can change min to max or add another if you want for the other way around                                                                              
                                                                                                                                                              
                                                                                                                                                              
// add new class to new layer                                                                                                                                 
$class = ms_newClassObj($layer);                                                                                                                              
                                                                                                                                                              
$class->label->set("font", "arial-bold");                                                                                                                     
$class->label->color->setRGB(0, 222, 31);                                                                                                                     
$class->label->set("size", 10);                                                                                                                               
$class->label->set("type", MS_TRUETYPE);                                                                                                                      
$class->label->set("position", MS_CR);                                                                                                                        
$class->label->set("antialias", TRUE);                                                                                                                        
                                                                                                                                                              
                                                                                                                                                              
$style = ms_newStyleObj($class);                                                                                                                              
$style->color->setRGB(0, 0, 0);                                                                                                                               
$style->set("size", 8);                                                                                                                                       
$style->set("symbol", 1);                                                                                                                                     
$style->set("antialias", TRUE);                                                                                                                               
                                                                                                                                                              
$point = ms_newPointObj();                                                                                                                                    
$point->setXY(-0.12, 51.50);                                                                                                                                  
                                                                                                                                                              
$line = ms_newLineObj();                                                                                                                                      
$line->add($point);                                                                                                                                           
                                                                                                                                                              
$pointShape = ms_newShapeObj(MS_SHAPE_POINT);                                                                                                                 
$pointShape->add($line);                                                                                                                                      
$pointShape->set("text", "London");                                                                                                                           
                                                                                                                                                              
$layer->addFeature($pointShape);                                                                                                                              
$image=$map->draw();                                                                                                                                          
                                                                                                                                                              
                                                                                                                                                              
echo $map->scaledenom . '<br>'; // to check what your denom is                                                                                                
$map->drawLabelCache($image);                                                                                                                                 
//$map->save("/tmp/test2.map"); // if you want to save mapfile to see your output                                                                             
$image_url=$image->saveWebImage();                                                                                                                            
                                                                                                                                                              
                                                                                                                                                              
?>                                                                                                                                                            
                                                                                                                                                              
<HTML>                                                                                                                                                        
 <HEAD>                                                                                                                                                       
     <TITLE>Example 1: Displaying a map</TITLE>                                                                                                               
 </HEAD>                                                                                                                                                      
 <BODY>                                                                                                                                                       
     <IMG SRC=<?php echo $image_url; ?> >                                                                                                                     
 </BODY>                                                                                                                                                      
</HTML>                                                                                                                                                       
                                                                                                                                                              
}}
