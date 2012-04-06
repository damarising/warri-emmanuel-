#!perl                                                                                          
$map = new mapscript::mapObj('demo.map');                                                       
                                                                                                
$minx = $map->{extent}->{minx};                                                                 
                                                                                                
$map->{interlace} = $mapscript::MS_TRUE;                                                        
                                                                                                
$map->{legend}->{keysizex} = 18;                                                                
                                                                                                
$map->{name} = 'example.map';                                                                   
                                                                                                
$num_layers = $map->{numlayers};                                                                
                                                                                                
$map->{scalebar}->{color} = $map->addColor(255,255,255);                                        
$map->{scalebar}->{label}->{color} = $map->addColor(0, 0, 0);                                   
                                                                                                
$map->{reference}->{minboxsize} = 5;                                                            
                                                                                                
$map->{extent}->{minx} = $map_minx;                                                             
                                                                                                
my $layer = $map->getLayerByName('parcel');                                                     
                                                                                                
my $img = $map->draw();                                                                         
                                                                                                
my $label = $map->nextLabel();                                                                  
                                                                                                
my $query_status = $st_poly_layer->queryByRect($map,$rectang);                                  
                                                                                                
# Prints the string and bounding polygon(s) for all labels in the cache that are actually drawn.
while ( $label = $map->nextLabel()) {                                                           
  if ( $label->{status} ) {                                                                     
    print $label->{string} . "\n";                                                              
    $shape = $label->{poly};                                                                    
    for ( $i=0; $i<$shape->{numlines}; $i++ ) {                                                 
      $part = $shape->get($i);                                                                  
      for ( $j=0; $j<$part->{numpoints}; $j++) {                                                
        $point = $part->get($j);                                                                
        print $point->{x} . "," . $point->{y} . " ";                                            
      }                                                                                         
      print "\n";                                                                               
    }                                                                                           
  }                                                                                             
}                                                                                               
```                                                                                             
----                                                                                            
back to PerlMapScrip
