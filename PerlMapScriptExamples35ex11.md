#!perl                                                                                                       
#!/usr/bin/perl                                                                                              
                                                                                                             
# File: project.pl                                                                                           
#                                                                                                            
# Purpose: This script takes a shapefile as input along with 2 Proj.4                                        
# projection descriptions.                                                                                   
#                                                                                                            
# Syntax: project.pl [from projection] [to projection] [in shapefile] [out shapefile]                        
#                                                                                                            
# Author: Stephen Lime (08/05/02)                                                                            
                                                                                                             
use mapscript;                                                                                               
                                                                                                             
sub show_syntax() {                                                                                          
  print "Syntax: project.pl [from projection] [to projection] [in shapefile] [out shapefile]\n";             
  exit 0;                                                                                                    
}                                                                                                            
                                                                                                             
&show_syntax() unless $#ARGV == 3;                                                                           
                                                                                                             
#                                                                                                            
# open the shapefiles                                                                                        
#                                                                                                            
$in_shapefile = new shapefileObj($ARGV[2], -1) or die "Unable to open input shapefile.";                     
$out_shapefile = new shapefileObj($ARGV[3], $in_shapefile->{type}) or die "Unable to open output shapefile.";
                                                                                                             
#                                                                                                            
# make a copy of the dBase file                                                                              
#                                                                                                            
system("cp ". $ARGV[2] .".dbf ". $ARGV[3] .".dbf");                                                          
                                                                                                             
#                                                                                                            
# create a couple of projection objects                                                                      
#                                                                                                            
$from_projection = new projectionObj($ARGV[0]) or die "Unable to initialize \"from\" projection.";           
$to_projection = new projectionObj($ARGV[1]) or die "Unable to initialize \"to\" projection.";               
                                                                                                             
#                                                                                                            
# and finally loop through the shapefile                                                                     
#                                                                                                            
$shape = new shapeObj($in_shapefile->{type});                                                                
for($i=0; $i<$in_shapefile->{numshapes}; $i++) {                                                             
  $status = $in_shapefile->get($i, $shape);                                                                  
  die "Error reading shape $i." unless $status == $mapscript::MS_SUCCESS;                                    
  $shape->project($from_projection, $to_projection);                                                         
  $out_shapefile->add($shape);                                                                               
}                                                                                                            
                                                                                                             
undef $out_shapefile;                                                                                        
exit;                                                                                                        
}}}                                                                                                          
----                                                                                                         
back to PerlMapScrip
