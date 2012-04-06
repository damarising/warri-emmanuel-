#!perl                                                                                                          
#!/usr/bin/perl                                                                                                 
                                                                                                                
# Script: Thin.pl                                                                                               
#                                                                                                               
# Purpose: An adaption of the ArcView Avenue example script genfeat.ave. It's                                   
#          basically the Douglas-Peucker generalization algorithm.                                              
#                                                                                                               
# Initial Coding: 04/10/2000                                                                                    
# Last Modified: 04/17/2000                                                                                     
# Version: 1.1                                                                                                  
#                                                                                                               
# Author: Stephen Lime (steve.lime@dnr.state.mn.us)                                                             
                                                                                                                
use POSIX;                                                                                                      
use XBase;                                                                                                      
use mapscript;                                                                                                  
use Getopt::Long;                                                                                               
                                                                                                                
&GetOptions("input=s", \$infile,                                                                                
            "output=s", \$outfile,                                                                              
            "tolerance=n", \$tolerance                                                                          
           );                                                                                                   
                                                                                                                
if(!$infile or !$outfile or !$tolerance) {                                                                      
  print "Syntax: thin.pl -input=[filename] -output=[filename] -tolerance=[maximum distance between vertices]\n";
  exit 0;                                                                                                       
}                                                                                                               
                                                                                                                
die "Tolerance must be greater than zero." unless $tolerance > 0;                                               
                                                                                                                
# initialize counters for reporting                                                                             
$incount = 0;                                                                                                   
$outcount = 0;                                                                                                  
                                                                                                                
# open the input shapefile                                                                                      
$inSHP = new shapefileObj($infile, -1) or die "Unable to open shapefile $infile.";                              
die "Cannot thin point/multipoint shapefiles." unless ($inSHP->{type} == 5 or $inSHP->{type} == 3);             
                                                                                                                
# create the output shapefile                                                                                   
system("rm $outfile.shp $outfile.shx $outfile.dbf $outfile.qix");                                               
$outSHP = new shapefileObj($outfile, $inSHP->{type}) or die "Unable to create sh                                
apefile '$outfile'. ", $mapscript::ms_error->{message};                                                         
system("cp $infile.dbf $outfile.dbf");                                                                          
                                                                                                                
$inshape = new shapeObj(-1); # something to hold shapes                                                         
                                                                                                                
for($i=0; $i<$inSHP->{numshapes}; $i++) {                                                                       
                                                                                                                
  $inSHP->get($i, $inshape);                                                                                    
  $outshape = new shapeObj(-1);                                                                                 
                                                                                                                
  for($j=0; $j<$inshape->{numlines}; $j++) {                                                                    
                                                                                                                
    $inpart = $inshape->get($j);                                                                                
    $outpart = new lineObj();                                                                                   
                                                                                                                
    @stack = ();                                                                                                
                                                                                                                
    $incount += $inpart->{numpoints};                                                                           
                                                                                                                
    $anchor = $inpart->get(0); # save first point                                                               
    $outpart->add($anchor);                                                                                     
    $aIndex = 0;                                                                                                
                                                                                                                
    $fIndex = $inpart->{numpoints} - 1;                                                                         
    push @stack, $fIndex;                                                                                       
                                                                                                                
    # Douglas - Peucker algorithm                                                                               
    while(@stack) {                                                                                             
                                                                                                                
      $fIndex = $stack[$#stack];                                                                                
      $fPoint = $inpart->get($fIndex);                                                                          
                                                                                                                
      $max = $tolerance; # comparison values                                                                    
      $maxIndex = 0;                                                                                            
                                                                                                                
      # process middle points                                                                                   
      for (($aIndex+1) .. ($fIndex-1)) {                                                                        
                                                                                                                
        $point = $inpart->get($_);                                                                              
        $dist = $point->distanceToLine($anchor, $fPoint);                                                       
                                                                                                                
        if($dist >= $max) {                                                                                     
          $max = $dist;                                                                                         
          $maxIndex = $_;                                                                                       
        }                                                                                                       
      }                                                                                                         
      if($maxIndex > 0) {                                                                                       
        push @stack, $maxIndex;                                                                                 
      } else {                                                                                                  
        $outpart->add($fPoint);                                                                                 
        $anchor = $inpart->get(pop @stack);                                                                     
        $aIndex = $fIndex;                                                                                      
      }                                                                                                         
    }                                                                                                           
                                                                                                                
    # check for collapsed polygons, use original data in that case                                              
    if(($outpart->{numpoints} < 4) and ($inSHP->{type} == 5)) {                                                 
      $outpart = $inpart;                                                                                       
    }                                                                                                           
                                                                                                                
    $outcount += $outpart->{numpoints};                                                                         
    $outshape->add($outpart);                                                                                   
                                                                                                                
  } # for each part                                                                                             
                                                                                                                
  $outSHP->add($outshape);                                                                                      
  undef($outshape); # free memory associated with shape                                                         
                                                                                                                
} # for each shape                                                                                              
                                                                                                                
$outSHP = ''; # write the file                                                                                  
print "The old file ($incount vertices) has been generalized to $outcount vertices.\n";                         
}}}                                                                                                             
----                                                                                                            
back to PerlMapScrip
