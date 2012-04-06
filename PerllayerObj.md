#!perl                                                                          
$layer = new mapscript::layerObj($map);                                         
                                                                                
$num_classes = $layer->{numclasses};                                            
                                                                                
$result_cache = $layer->{resultcache};                                          
                                                                                
$qryresult = $layer->queryByPoint($map,$point,$mapscript::MS_SINGLE,0);         
                                                                                
$resultmember = $layer->getResult(0);                                           
                                                                                
$query = "(([AREA]>500000 AND [AREA]<=1000000) AND ([CODE]=311 OR [CODE]=312))";
$layer->setFilter($query);                                                      
                                                                                
my $layer = $map->getLayerByName('parcel');                                     
                                                                                
$layer->{status} = $mapscript::MS_ON;                                           
                                                                                
$layer->{data} = "$scan.tif";                                                   
                                                                                
$layer->{transparency} = 100;                                                   
                                                                                
my $class = $layer->getClass(0);                                                
                                                                                
my $query_status = $st_poly_layer->queryByRect($map,$rectang);                  
my $result_mem = $st_poly_layer->getResult(0);                                  
my $result_cache = $st_poly_layer->{resultcache};                               
                                                                                
my $sing_layer_idx = $sing_layer->{index};                                      
$sing_layer->queryByRect($map,$rectang); # ** set of results **                 
$prcl_dim_layer->queryByFeatures($map,$sing_layer_idx);                         
}}}                                                                             
----                                                                            
back to PerlMapScrip
