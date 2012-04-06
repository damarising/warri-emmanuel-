#!perl                                                                                         
#!/usr/bin/perl                                                                                
                                                                                               
## This script finds all the section (s) and quarter-section (qs) combinations                 
# within a given search radius around a given qs origin. This script is for                    
# display output only, hence, it asks for user input, and displays a grid of                   
# found qs. The actual script would be converted into a function that would return             
# a suitable structure for further computation. The maximum search radius allowed is           
# 2.5 miles. Higher numbers become non-sensical in that even at 2.5 mi search radius,          
# an area of more than 19.5 sq mi is searched and 441 qs are returned.                         
#                                                                                              
# Puneet Kishor                                                                                
# pkishor@geoanalytics.com                                                                     
# August 2002                                                                                  
# Use under the same license as Mapserver                                                      
                                                                                               
## Define the PLSS structure.                                                                  
# Each row is an array of qs in a township.                                                    
# Each element of the row array is a hash with values for that s and qs.                       
# There are 12 row arrays with 12 qs in each array, hence, a 144 element structure             
@row0 = (                                                                                      
            { s => 6, qs => 'nw' }, { s => 6, qs => 'ne' },                                    
            { s => 5, qs => 'nw' }, { s => 5, qs => 'ne' },                                    
            { s => 4, qs => 'nw' }, { s => 4, qs => 'ne' },                                    
            { s => 3, qs => 'nw' }, { s => 3, qs => 'ne' },                                    
            { s => 2, qs => 'nw' }, { s => 2, qs => 'ne' },                                    
            { s => 1, qs => 'nw' }, { s => 1, qs => 'ne' }                                     
         );                                                                                    
@row1 = (                                                                                      
            { s => 6, qs => 'sw' }, { s => 6, qs => 'se' },                                    
            { s => 5, qs => 'sw' }, { s => 5, qs => 'se' },                                    
            { s => 4, qs => 'sw' }, { s => 4, qs => 'se' },                                    
            { s => 3, qs => 'sw' }, { s => 3, qs => 'se' },                                    
            { s => 2, qs => 'sw' }, { s => 2, qs => 'se' },                                    
            { s => 1, qs => 'sw' }, { s => 1, qs => 'se' }                                     
         );                                                                                    
@row2 = (                                                                                      
            { s => 7, qs => 'nw' }, { s => 7, qs => 'ne' },                                    
            { s => 8, qs => 'nw' }, { s => 8, qs => 'ne' },                                    
            { s => 9, qs => 'nw' }, { s => 9, qs => 'ne' },                                    
            { s => 10, qs => 'nw' }, { s => 10, qs => 'ne' },                                  
            { s => 11, qs => 'nw' }, { s => 11, qs => 'ne' },                                  
            { s => 12, qs => 'nw' }, { s => 12, qs => 'ne' }                                   
        );                                                                                     
@row3 = (                                                                                      
            { s => 7, qs => 'sw' }, { s => 7, qs => 'se' },                                    
            { s => 8, qs => 'sw' }, { s => 8, qs => 'se' },                                    
            { s => 9, qs => 'sw' }, { s => 9, qs => 'se' },                                    
            { s => 10, qs => 'sw' }, { s => 10, qs => 'se' },                                  
            { s => 11, qs => 'sw' }, { s => 11, qs => 'se' },                                  
            { s => 12, qs => 'sw' }, { s => 12, qs => 'se' }                                   
                );                                                                             
@row4 = (                                                                                      
            { s => 18, qs => 'nw' }, { s => 18, qs => 'ne' },                                  
            { s => 17, qs => 'nw' }, { s => 17, qs => 'ne' },                                  
            { s => 16, qs => 'nw' }, { s => 16, qs => 'ne' },                                  
            { s => 15, qs => 'nw' }, { s => 15, qs => 'ne' },                                  
            { s => 14, qs => 'nw' }, { s => 14, qs => 'ne' },                                  
            { s => 13, qs => 'nw' }, { s => 13, qs => 'ne' }                                   
        );                                                                                     
@row5 = (                                                                                      
            { s => 18, qs => 'sw' }, { s => 18, qs => 'se' },                                  
            { s => 17, qs => 'sw' }, { s => 17, qs => 'se' },                                  
            { s => 16, qs => 'sw' }, { s => 16, qs => 'se' },                                  
            { s => 15, qs => 'sw' }, { s => 15, qs => 'se' },                                  
            { s => 14, qs => 'sw' }, { s => 14, qs => 'se' },                                  
            { s => 13, qs => 'sw' }, { s => 13, qs => 'se' }                                   
        );                                                                                     
@row6 = (                                                                                      
            { s => 19, qs => 'nw' }, { s => 19, qs => 'ne' },                                  
            { s => 20, qs => 'nw' }, { s => 20, qs => 'ne' },                                  
            { s => 21, qs => 'nw' }, { s => 21, qs => 'ne' },                                  
            { s => 22, qs => 'nw' }, { s => 22, qs => 'ne' },                                  
            { s => 23, qs => 'nw' }, { s => 23, qs => 'ne' },                                  
            { s => 24, qs => 'nw' }, { s => 24, qs => 'ne' }                                   
        );                                                                                     
@row7 = (                                                                                      
            { s => 19, qs => 'sw' }, { s => 19, qs => 'se' },                                  
            { s => 20, qs => 'sw' }, { s => 20, qs => 'se' },                                  
            { s => 21, qs => 'sw' }, { s => 21, qs => 'se' },                                  
            { s => 22, qs => 'sw' }, { s => 22, qs => 'se' },                                  
            { s => 23, qs => 'sw' }, { s => 23, qs => 'se' },                                  
            { s => 24, qs => 'sw' }, { s => 24, qs => 'se' }                                   
        );                                                                                     
@row8 = (                                                                                      
            { s => 30, qs => 'nw' }, { s => 30, qs => 'ne' },                                  
            { s => 29, qs => 'nw' }, { s => 29, qs => 'ne' },                                  
            { s => 28, qs => 'nw' }, { s => 28, qs => 'ne' },                                  
            { s => 27, qs => 'nw' }, { s => 27, qs => 'ne' },                                  
            { s => 26, qs => 'nw' }, { s => 26, qs => 'ne' },                                  
            { s => 25, qs => 'nw' }, { s => 25, qs => 'ne' }                                   
        );                                                                                     
@row9 = (                                                                                      
            { s => 30, qs => 'sw' }, { s => 30, qs => 'se' },                                  
            { s => 29, qs => 'sw' }, { s => 29, qs => 'se' },                                  
            { s => 28, qs => 'sw' }, { s => 28, qs => 'se' },                                  
            { s => 27, qs => 'sw' }, { s => 27, qs => 'se' },                                  
            { s => 26, qs => 'sw' }, { s => 26, qs => 'se' },                                  
            { s => 25, qs => 'sw' }, { s => 25, qs => 'se' }                                   
         );                                                                                    
@row10 = (                                                                                     
            { s => 31, qs => 'nw' }, { s => 31, qs => 'ne' },                                  
            { s => 32, qs => 'nw' }, { s => 32, qs => 'ne' },                                  
            { s => 33, qs => 'nw' }, { s => 33, qs => 'ne' },                                  
            { s => 34, qs => 'nw' }, { s => 34, qs => 'ne' },                                  
            { s => 35, qs => 'nw' }, { s => 35, qs => 'ne' },                                  
            { s => 36, qs => 'nw' }, { s => 36, qs => 'ne' }                                   
         );                                                                                    
@row11 = (                                                                                     
            { s => 31, qs => 'sw' }, { s => 31, qs => 'se' },                                  
            { s => 32, qs => 'sw' }, { s => 32, qs => 'se' },                                  
            { s => 33, qs => 'sw' }, { s => 33, qs => 'se' },                                  
            { s => 34, qs => 'sw' }, { s => 34, qs => 'se' },                                  
            { s => 35, qs => 'sw' }, { s => 35, qs => 'se' },                                  
            { s => 36, qs => 'sw' }, { s => 36, qs => 'se' }                                   
          );                                                                                   
                                                                                               
# End PLSS structure                                                                           
                                                                                               
## Ask user input for the origin.                                                              
# Input values are township (t), range (r), s, qs, and search radius (sr).                     
# The sr is calculated in increments of 0.25 miles since each qs is 0.25 m sq.                 
# Maximum sr allowed is 2.5 m. Right now there is some array creation                          
# error for sr greater than 2.5. Also, way too many qs are returned for such                   
# high values.                                                                                 
print "Enter a township (10): ";                                                               
$t = <STDIN>; chop($t);                                                                        
                                                                                               
print "Enter a range (10): ";                                                                  
$r = <STDIN>; chop($r);                                                                        
                                                                                               
print "Enter a section between 1 and 36 (1): ";                                                
$s = <STDIN>; chop($s);                                                                        
                                                                                               
print "Enter a quarter-section like ne, nw, se, sw (ne): ";                                    
$qs = <STDIN>; chop($qs);                                                                      
                                                                                               
print "Enter a search radius less than 2.5 miles (0.5): ";                                     
$sr = <STDIN>; chop($sr);                                                                      
# End user input                                                                               
                                                                                               
## set some defaults                                                                           
$t = 10 if ($t == "");                                                                         
$r = 10 if ($r == "");                                                                         
$s = 1 if ($s == "");                                                                          
$qs = "ne" if ($qs == "");                                                                     
$sr = 0.5 if ($sr == "");                                                                      
                                                                                               
# Given a sr, num_of_skins is the number of "concentric" squares of qs around                  
# our origin. This values has a higher bound of 2.5 m.                                         
$num_of_skins = ($sr > 2.5 ? 2.5 / 0.25 : int($sr / 0.25));                                    
# End defaults                                                                                 
                                                                                               
#print "s: $s, qs: $qs, r: $r, n: $num_of_skins \n";                                           
                                                                                               
## Start calculations                                                                          
# First loop through the PLSS structure row by row.                                            
for $i (0..11) {                                                                               
    $thisrow = "row" . $i;                                                                     
                                                                                               
    # For each row, loop through each hash element by element                                  
    for $j (0..11) {                                                                           
                                                                                               
        # Check if our s,qs matches the hash                                                   
        if (($s == $$thisrow[$j]{'s'}) && ($qs eq $$thisrow[$j]{'qs'})) {                      
                                                                                               
            # Calculate the number of qs                                                       
            $num_of_qs = ($num_of_skins * 2 + 1) * ($num_of_skins * 2 + 1);                    
                                                                                               
            print "\nThe following $num_of_qs qs were found within $sr miles of $t$r$s$qs\n\n";
                                                                                               
            # Loop through each row in the "concentric" square of qs around our origin.        
            # Negative rows are above the origin, positive elements are below.                 
            for ($k = -$num_of_skins; $k <= $num_of_skins; $k++) {                             
                                                                                               
                # Find the row number and copy it to a temporary current row                   
                $row = $i + $k;                                                                
                if ($row < 0) {                                                                
                    $row += 12;                                                                
                    $tw = $t + 1;                                                              
                } elsif ($row > 11) {                                                          
                    $row -= 12;                                                                
                    $tw = $t - 1;                                                              
                } else {                                                                       
                    $tw = $t;                                                                  
                }                                                                              
                $currrow = "row" . $row;                                                       
                @currrow = @$currrow;                                                          
                                                                                               
                # Loop through each hash element in the current row. Once again, Negative      
                # elements are to the left of origin, positive elements are to the right.      
                for ($l = -$num_of_skins; $l <= $num_of_skins; $l++) {                         
                                                                                               
                    # Calculate each hash element's position correctly                         
                    $cell = $j + $l;                                                           
                    if ($cell < 0) {                                                           
                        $cell += 12;                                                           
                        $rg = $r - 1;                                                          
                    } elsif ($cell > 11) {                                                     
                        $cell -= 12;                                                           
                        $rg = $r + 1;                                                          
                    } else {                                                                   
                        $rg = $r;                                                              
                    }                                                                          
                    $sec = $currrow[$cell]{'s'};                                               
                                                                                               
                    ## Prefix a 0 so the output looks pretty (this script is for               
                    # display only. This step won't be needed in actual computation).          
                    $tw = "0" . $tw if ($tw < 10);                                             
                    $rg = "0" . $rg if ($rg < 10);                                             
                    $sec = "0" . $sec if ($sec < 10);                                          
                    # End prefix                                                               
                                                                                               
                    print " $tw$rg$sec$currrow[$cell]{'qs'} ";                                 
                }                                                                              
                print "\n";                                                                    
            }                                                                                  
        }                                                                                      
    }                                                                                          
}                                                                                              
```                                                                                            
----                                                                                           
back to PerlMapScrip
