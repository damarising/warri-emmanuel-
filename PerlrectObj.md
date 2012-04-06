#!perl                                                        
my $rectangle = new mapscript::rectObj();                     
$rectangle->{minx} = 0;                                       
$rectangle->{miny} = 0;                                       
$rectangle->{maxx} = 10;                                      
$rectangle->{maxy} = 10;                                      
                                                              
my $query_status = $st_poly_layer->queryByRect($map,$rectang);
                                                              
my $rectang = $sing_shpfile->{bounds};                        
```                                                           
----                                                          
back to PerlMapScrip
