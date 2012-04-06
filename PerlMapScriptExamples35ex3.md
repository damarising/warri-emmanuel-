                                                                                                                                                                                                      

```                                                                                                                                                                                                   
#!perl                                                                                                                                                                                                
#!/usr/bin/perl -w                                                                                                                                                                                    
#                                                                                                                                                                                                     
# Given a shapefile name this routine will dump out the shapefile type,                                                                                                                               
#   members, & applicable coordinate information.                                                                                                                                                     
#                                                                                                                                                                                                     
# Required modules are mapscript (installed as part of make install)                                                                                                                                  
#    & Getopt (normally included with Perl).                                                                                                                                                          
#   Please download boundary.tar.gz also, and:                                                                                                                                                        
#     tar -xf boundary.tar.gz --ungzip                                                                                                                                                                
#                                                                                                                                                                                                     
# Suggested run method = ./dump.pl -file=boundary > /tmp/boundary.dump                                                                                                                                
use mapscript;                                                                                                                                                                                        
use Getopt::Long;                                                                                                                                                                                     
#                                                                                                                                                                                                     
# Declare a hash for easy access to the shape types.                                                                                                                                                  
%types = ( '1' => 'point',                                                                                                                                                                            
           '3' => 'arc',                                                                                                                                                                              
           '5' => 'polygon',                                                                                                                                                                          
           '8' => 'multipoint'                                                                                                                                                                        
         );                                                                                                                                                                                           
#                                                                                                                                                                                                     
# Declare an initial value for the shapefile extents.                                                                                                                                                 
my $shapeminx = 0,                                                                                                                                                                                    
my $shapeminy = 0;                                                                                                                                                                                    
my $shapemaxx = 1;                                                                                                                                                                                    
my $shapemaxy = 1;                                                                                                                                                                                    
#                                                                                                                                                                                                     
# Check input for a specified file name.                                                                                                                                                              
&GetOptions("file=s", \$file);                                                                                                                                                                        
if(!$file) {                                                                                                                                                                                          
  print "Syntax: dump.pl -file=[filename]\n";                                                                                                                                                         
  exit 0;                                                                                                                                                                                             
}                                                                                                                                                                                                     
#                                                                                                                                                                                                     
# Open the specified shapefile.                                                                                                                                                                       
$shapefile = new shapefileObj($file, -1) or die "Unable to open shapefile $file";                                                                                                                     
#                                                                                                                                                                                                     
# Print shapefile type & number of shapes.                                                                                                                                                            
print "Shapefile opened (type=". $types{$shapefile->{type}} .") with ".                                                                                                                               
$shapefile->{numshapes} ." shape(s)\n";                                                                                                                                                               
#                                                                                                                                                                                                     
# Open a shape object for pulling the shapefile shapes into.                                                                                                                                          
$shape = new shapeObj(-1);                                                                                                                                                                            
#                                                                                                                                                                                                     
# Loop through each shape in the shapefile.                                                                                                                                                           
for($i=0; $i<$shapefile->{numshapes}; $i++) {                                                                                                                                                         
    #                                                                                                                                                                                                 
    # Pull the nth shape into the shape object for analysis.                                                                                                                                          
    $shapefile->get($i, $shape);                                                                                                                                                                      
    #                                                                                                                                                                                                 
    # Print the shapes number of lines.                                                                                                                                                               
    print "Shape $i has ". $shape->{numlines} ." part(s) - ";                                                                                                                                         
    #                                                                                                                                                                                                 
    # Get the bounds.                                                                                                                                                                                 
    my $minx = $shape->{bounds}->{minx};                                                                                                                                                              
    my $miny = $shape->{bounds}->{miny};                                                                                                                                                              
    my $maxx = $shape->{bounds}->{maxx};                                                                                                                                                              
    my $maxy = $shape->{bounds}->{maxy};                                                                                                                                                              
    #                                                                                                                                                                                                 
    # Print the bounds for the shape.                                                                                                                                                                 
    printf "bounds (%f,%f) (%f,%f)\n", $minx, $miny, $maxx, $maxy;                                                                                                                                    
    #                                                                                                                                                                                                 
    # Is this the first shape.                                                                                                                                                                        
    if ( $i == 0 ) {                                                                                                                                                                                  
      #                                                                                                                                                                                               
      # Set the initial bounds to an actual shape.                                                                                                                                                    
      $shapeminx = $minx;                                                                                                                                                                             
      $shapeminy = $miny;                                                                                                                                                                             
      $shapemaxx = $maxx;                                                                                                                                                                             
      $shapemaxy = $maxy;                                                                                                                                                                             
    }                                                                                                                                                                                                 
     else {                                                                                                                                                                                           
      #                                                                                                                                                                                               
      # Create compounded shapefile extent.                                                                                                                                                           
      #   Note: This is just an example, there is a better way to do this.                                                                                                                            
      #         See shpinfo.pl .                                                                                                                                                                      
      if ($minx < $shapeminx) {                                                                                                                                                                       
        $shapeminx = $minx;                                                                                                                                                                           
      }                                                                                                                                                                                               
      if ($miny < $shapeminy) {                                                                                                                                                                       
        $shapeminy = $miny;                                                                                                                                                                           
      }                                                                                                                                                                                               
      if ($maxx > $shapemaxx) {                                                                                                                                                                       
        $shapemaxx = $maxx;                                                                                                                                                                           
      }                                                                                                                                                                                               
      if ($maxy > $shapemaxy) {                                                                                                                                                                       
        $shapemaxy = $maxy;                                                                                                                                                                           
      }                                                                                                                                                                                               
     }                                                                                                                                                                                                
    #                                                                                                                                                                                                 
    # Loop through each line in the shape.                                                                                                                                                            
    for($j=0; $j<$shape->{numlines}; $j++) {                                                                                                                                                          
        #                                                                                                                                                                                             
        # Create the line object.                                                                                                                                                                     
        $part = $shape->get($j);                                                                                                                                                                      
        #                                                                                                                                                                                             
        # Print the number of points in the line.                                                                                                                                                     
        print "Part $j has ". $part->{numpoints} ." point(s)\n";                                                                                                                                      
        #                                                                                                                                                                                             
        # Loop through each point.                                                                                                                                                                    
        for($k=0; $k<$part->{numpoints}; $k++) {                                                                                                                                                      
            #                                                                                                                                                                                         
            # Create the point object.                                                                                                                                                                
            $point = $part->get($k);                                                                                                                                                                  
            #                                                                                                                                                                                         
            # Print the x & y coordinate for the point.                                                                                                                                               
            print "$k: ". $point->{x} .", ". $point->{y} ."\n";                                                                                                                                       
        }                                                                                                                                                                                             
    }                                                                                                                                                                                                 
}                                                                                                                                                                                                     
#                                                                                                                                                                                                     
# Print the shapefiles bounding rectangle.                                                                                                                                                            
print "$file\'s Bounding Rectangle is $shapeminx, $shapeminy, $shapemaxx, $shapemaxy\n";                                                                                                              
```                                                                                                                                                                                                   
                                                                                                                                                                                                      
----                                                                                                                                                                                                  
back to [wiki:PerlMapScript]
