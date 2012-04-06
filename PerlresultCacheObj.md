{{{                                                            
#!perl                                                         
$resultcache = $layer->{resultcache};                          
                                                               
$resultsfnd = $resultcache->{numresults};                      
                                                               
$sing_layer->queryByRect($map,$rectang); # ** set of results **
$prcl_dim_layer->queryByFeatures($map,$sing_layer_idx);        
}}}                                                            
----                                                           
back to PerlMapScript                                          

