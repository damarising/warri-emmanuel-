#!perl                                                                                      
$image = new mapscript::imageObj(600, 600);                                                 
my $img = $map->draw();                                                                     
$image->saveImage('out.png', $mapscript::MS_PNG, $map->{transparent}, $map->{interlace}, 0);
```                                                                                         
----                                                                                        
back to PerlMapScrip
