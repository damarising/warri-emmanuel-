#!perl                                               
$point = new mapscript::pointObj();                  
                                                     
$point->{x} = 100;                                   
                                                     
$pointx = $point->{x};                               
$pointy = $point->{y};                               
                                                     
my $distance = $point->distanceToPoint($garagepoint);
                                                     
$point->project($in_proj,$out_proj);                 
                                                     
my $point = $label->{point};                         
                                                     
$box_minx = $point->{x} - 15;                        
                                                     
$line->add($point);                                  
}}}                                                  
----                                                 
back to PerlMapScrip
