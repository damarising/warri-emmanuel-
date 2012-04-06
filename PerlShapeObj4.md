== drawing lines with mapscript ==                                                 
                                                                                   
...class definition in the map file needed a style section (even if it is empty)...
{{{                                                                                
#!perl                                                                             
$layer = $map->getLayerByName('line_layer');                                       
$layer->{status} = 1;                                                              
my $linePoint1 = new mapscript::pointObj();                                        
$linePoint1->{x} = 272048; $linePoint1->{y} = 199772;                              
my $linePoint2 = new mapscript::pointObj();                                        
$linePoint2->{x} = 272450; $linePoint2->{y} = 200074;                              
my $line = new mapscript::lineObj();                                               
$line->add($linePoint1);                                                           
$line->add($linePoint2);                                                           
my $shape = new mapscript::shapeObj($mapscript::MS_SHAPE_LINE);                    
$shape->add($line);                                                                
$shape->{text} = "LINE TEXT";                                                      
$shape->draw($map, $layer, $img);                                                  
}}}                                                                                
----------------------------------                                                 
{{{                                                                                
LAYER                                                                              
NAME "line_layer"                                                                  
STATUS ON                                                                          
PROJECTION                                                                         
"init=epsg:26958"                                                                  
END                                                                                
TYPE LINE                                                                          
CLASS                                                                              
STYLE                                                                              
END                                                                                
LABEL                                                                              
ANGLE AUTO                                                                         
FONT arial                                                                         
TYPE TRUETYPE                                                                      
POSITION UC                                                                        
SIZE 7                                                                             
COLOR 0 0 0                                                                        
END                                                                                
COLOR 0 0 0                                                                        
SIZE 10                                                                            
END                                                                                
END                                                                                
}}}                                                                                
----                                                                               
back to PerlMapScript                                                              

