#!perl                                
$line = new mapscript::lineObj();     
$numpoints = $line->{numpoints};      
for($j=0; $j<$numpoints; $j++) {      
  $point = $line->get($j);            
}                                     
                                      
my $line = new mapscript::lineObj();  
my $point = new mapscript::pointObj();
while (<POINTS>) {                    
  $point->{x} = $long;                
  $point->{y} = $lat;                 
  $line->add($point);                 
}                                     
}}}                                   
----                                  
back to PerlMapScrip
