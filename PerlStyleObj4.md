Illustrating use of new 4.x styleObj, two approaches:                                                   
                                                                                                        
== One - Using an existing style from map file. Default values will come from values set in map file. ==
{{{                                                                                                     
#!perl                                                                                                  
my $layerObj = $map->getLayerByName('data_points');                                                     
my $class = $layerObj->getClass(0);                                                                     
# we only have one style in this layer.                                                                 
my $style = $class->getStyle(0);                                                                        
# change some sytle values                                                                              
$style->{size} = 20;                                                                                    
$style->{color}->setRGB(255,0,0);                                                                       
$layerObj->draw($map,$img);                                                                             
}}}                                                                                                     
                                                                                                        
== Two - Creating a new style. All values must be set. ==                                               
{{{                                                                                                     
#!perl                                                                                                  
my $circle_idx = $map->getSymbolByName('circle');                                                       
my $layerObj = $map->getLayerByName('data_points');                                                     
my $class = $layerObj->getClass(0);                                                                     
my $style = new mapscript::styleObj();                                                                  
$style->{symbol} = $circle_idx;                                                                         
$style->{size} = 20;                                                                                    
$style->{color}->setRGB(255,0,0);                                                                       
$style->{outlinecolor}->setRGB(0,0,0);                                                                  
# insert the style in class                                                                             
$class->insertStyle($style, 0);                                                                         
$layerObj->draw($map,$img);                                                                             
# remove the style, this is important when in a loop. There is a maximum of 5 styles/class.             
$class->removeStyle(0);                                                                                 
}}}                                                                                                     
== Map file fragment ==                                                                                 
{{{                                                                                                     
LAYER                                                                                                   
  NAME "data_points"                                                                                    
  TYPE POINT                                                                                            
  STATUS ON                                                                                             
  TEMPLATE "bogus.html"                                                                                 
  TOLERANCE 5                                                                                           
  CLASS                                                                                                 
    STYLE                                                                                               
      SYMBOL "circle"                                                                                   
      SIZE 0                                                                                            
      COLOR 0 0 0                                                                                       
      OUTLINECOLOR 0 0 0                                                                                
    END                                                                                                 
  END                                                                                                   
  PROJECTION                                                                                            
    "proj=latlong"                                                                                      
  END                                                                                                   
END                                                                                                     
}}}                                                                                                     
                                                                                                        
== Access to styles has been changed in version 4.2. ==                                                 
                                                                                                        
$class->getStyle(0)->{symbol} = 0;                                                                      
                                                                                                        
(there is a hidden pen parameter that needs to be set)                                                  
                                                                                                        
$style->setRGB(255, 0, 0);                                                                              
                                                                                                        
----                                                                                                    
back to PerlMapScrip
