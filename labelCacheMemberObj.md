#!perl                                                            
my ($label,$box_minx,$box_miny,$box_maxx,$box_maxy,$text);        
while ( $label = $map->nextLabel() ) { # ** labelCacheMemberObj **
  if ( $label->{status} ) {                                       
    my $point = $label->{point};                                  
    $box_minx = $point->{x} - 15;                                 
    $box_miny = $point->{y} - 10;                                 
    $box_maxx = $point->{x} + 15;                                 
    $box_maxy = $point->{y} + 10;                                 
    $text = "String = $label->{string}";                          
  }                                                               
}                                                                 
```                                                               
----                                                              
back to PerlMapScrip
