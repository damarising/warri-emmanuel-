{{{                                                                                          
#!perl                                                                                       
$shapefile = new mapscript::shapefileObj('lakes',-1) or die "Unable to open lakes shapefile";
                                                                                             
$extents = $shapefile->{bounds};                                                             
                                                                                             
$numshapes = $shapefile->{numshapes};                                                        
                                                                                             
$typeno = $shapefile->{type};                                                                
                                                                                             
$shapefile->add($shape);                                                                     
                                                                                             
This example saves a selected polygon shape into a new shapefile.                            
#                                                                                            
# Open the map.                                                                              
my $map = new mapscript::mapObj('your.map');                                                 
#                                                                                            
# Create the layer object to query.                                                          
my $layer = new mapscript::layerObj($map);                                                   
#                                                                                            
# Create a new shapefile for the selection set.                                              
my $newshpf = new mapscript::shapefileObj("$newshpfname",  $mapscript::MS_SHAPEFILE_POLYGON);
#                                                                                            
# Set index value for selected shape.                                                        
my $poly = 400;                                                                              
#                                                                                            
# Create a shapefile object for getting queried shape.                                       
my $shpf = new mapscript::shapefileObj("data_shapefile", -1);                                
#                                                                                            
# Create shape object for storing queried shape.                                             
my $shp = new mapscript::shapeObj(-1);                                                       
#                                                                                            
# Retrieve shape into shape object.                                                          
$shpf->get($poly, $shp);                                                                     
#                                                                                            
# Put shape into new shapefile.                                                              
$newshpf->add($shp);                                                                         
#                                                                                            
# Create dbf to go with new.                                                                 
@toucherror = system("touch $newshpfname.dbf");                                              
#                                                                                            
# Get the extent of the new shapefile.                                                       
my $newrect = $newshpf->{bounds};                                                            
my $newminx = $newrect->{minx};                                                              
my $newminy = $newrect->{miny};                                                              
my $newmaxx = $newrect->{maxx};                                                              
my $newmaxy = $newrect->{maxy};                                                              
#                                                                                            
# Close new shapefile.                                                                       
undef $newshpf;                                                                              
}}}                                                                                          
----                                                                                         
back to PerlMapScrip
