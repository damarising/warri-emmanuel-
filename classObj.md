
```                                                                                          
#!perl                                                                                       
$class = new mapscript::classObj($layer);                                                    
                                                                                             
$class->{label}->{autoangle} = 1;                                                            
$class->{label}->{sizescaled} = 8;                                                           
                                                                                             
$class->setExpression("\"${string_variable}\"");                                             
$class->setExpression('/^{A-Z}/');                                                           
$class->setExpression('([LENGTH] gt 50 and [AREA] < 150000)');                               
$class->setExpression('N');                                                                  
                                                                                             
my $s_class = $s_layer->getClass(0);                                                         
                                                                                             
$class->{color} = $map->addColor(0,255,0);                                                   
                                                                                             
# Example of creating a "canvas" image for adding legend icons to & adding 1 really big icon.
my $legend_img =  $class->createLegendIcon($rmap,$layer,200,100);                            
$class->drawLegendIcon($map,$layer,150,50,$legend_img,25,25);                                
```                                                                                          
----                                                                                         
back to PerlMapScript                                                                        

