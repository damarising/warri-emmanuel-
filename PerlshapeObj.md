
```                                                                          
#!perl                                                                       
$shape = new mapscript::shapeObj(-1);                                        
                                                                             
$shape = new mapscript::shapeObj($mapscript::MS_SHAPE_POLYGON);              
                                                                             
$extents = $shape->{bounds};                                                 
                                                                             
$minx = $shape->{bounds}->{minx};                                            
$miny = $shape->{bounds}->{miny};                                            
$maxx = $shape->{bounds}->{maxx};                                            
$maxy = $shape->{bounds}->{maxy};                                            
                                                                             
$numlines = $shape->{numlines};                                              
                                                                             
$street_poly->get($result_mem->{shapeindex},$selected_shape);                
                                                                             
my $result_shape = $prcl_dim_layer->getResult(0);                            
                                                                             
for($i=0; $i<$numshapes; i++) {                                              
  $shapefile->get($i, $shape);                                               
  ...                                                                        
}                                                                            
```                                                                          
This example retrieves all of the x,y's for all the lines in a polygon shape.

```                                                                          
#!perl                                                                       
#                                                                            
# Create shape object.                                                       
$shp = new mapscript::shapeObj(-1);                                          
#                                                                            
# Retrieve shape into shape object.                                          
$shpfile->get(1,$shp);                                                       
#                                                                            
# How many lines in shape.                                                   
my $num_lines = $shp->{numlines};                                            
#                                                                            
# Loop through each line.                                                    
for ( $line_num=0; $line_num<$num_lines; $line_num++ ) {                     
  #                                                                          
  # Get line.                                                                
  my $line = new mapscript::lineObj();                                       
  $line = $ishp->get($line_num);                                             
  print "Got Line #$line_num\n";                                             
  #                                                                          
  # How many points in line.                                                 
  my $num_points = $line->{numpoints};                                       
  #                                                                          
  # Loop through each point.                                                 
  for ( $point_num=0; $point_num<$num_points; $point_num++ ) {               
    #                                                                        
    # Get the point.                                                         
    my $point = new mapscript::pointObj();                                   
    $point = $line->get($point_num);                                         
    print "Got Point #$point_num";                                           
    $px = $point->{x};                                                       
    $py = $point->{y};                                                       
    print " X=$px Y=$py\n";                                                  
  }                                                                          
}                                                                            
print "\n";                                                                  
                                                                             
```                                                                          
----                                                                         
back to PerlMapScrip
