== Drawing lines with mapscript ==                                                                                    
(class definition requires a style - even if blank) (LINE type layer is rendered using COLOR and ignores OUTLINECOLOR)

```                                                                                                                   
CLASS                                                                                                                 
STYLE                                                                                                                 
END                                                                                                                   
```                                                                                                                   
--------------------------------------------------------------------                                                  

```                                                                                                                   
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
```                                                                                                                   
----                                                                                                                  
back to PerlMapScrip
