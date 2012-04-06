A perl mapscript 3.6.4 sub-routine which converts a reference map mouse click into new map extents for main map. Really just a code snippet. Very little error checking.
                                                                                                                                                                        
Author: Eric Bridger eric@gomoos.org, eric@maine.com                                                                                                                    
----                                                                                                                                                                    
{{{                                                                                                                                                                     
#!perl                                                                                                                                                                  
# This is based entirely on the code in mapserver3.6.4 maptemplate.c::setExtent() case FROMREFPT                                                                        
# calculate new map extents from mouse click in reference map.                                                                                                          
# assumes CGI Form with:                                                                                                                                                
# <input type="image" name="ref_map" src="mapserver_reference_map">                                                                                                     
# <input type="hidden" name="imgext" value="MINX MINY MAXX MAXY"                                                                                                        
                                                                                                                                                                        
use mapscript;                                                                                                                                                          
use CGI ":cgi";                                                                                                                                                         
my $q = new CGI;                                                                                                                                                        
                                                                                                                                                                        
$map = new mapscript::mapObj("mapfile.map");                                                                                                                            
                                                                                                                                                                        
# This routine sets $map->{extent}{minx}, etc. and returns the new map extent as a string, which can be                                                                 
# set in the html form as your current hidden imgext variable.                                                                                                          
                                                                                                                                                                        
sub ref_to_map_ext {                                                                                                                                                    
    my ($q, $map) = @_;                                                                                                                                                 
                                                                                                                                                                        
    # as default just return current map extent                                                                                                                         
    my $mapext = join(' ', $map->{extent}->{minx},$map->{extent}->{miny},$map->{extent}->{maxx},$map->{extent}->{maxy});                                                
    if(!$q->param('imgext')){                                                                                                                                           
        return $mapext;                                                                                                                                                 
    }                                                                                                                                                                   
    if(!$q->param('ref_map.x')){                                                                                                                                        
        return $mapext;                                                                                                                                                 
    }                                                                                                                                                                   
                                                                                                                                                                        
    # get the current map extent from form imgext variable.                                                                                                             
    my @imgext = split(' ', $q->param('imgext'));                                                                                                                       
                                                                                                                                                                        
    # get mouse click x, y from ref_map image.                                                                                                                          
    my $refx = $q->param('ref_map.x');                                                                                                                                  
    my $refy = $q->param('ref_map.y');                                                                                                                                  
                                                                                                                                                                        
 # reference map vars from  the map file                                                                                                                                
    # These never change unless the map file changes.                                                                                                                   
    my $ref_width = $map->{reference}{width};                                                                                                                           
    my $ref_height = $map->{reference}{height};                                                                                                                         
    my $ref_minx = $map->{reference}{extent}{minx};                                                                                                                     
    my $ref_maxx = $map->{reference}{extent}{maxx};                                                                                                                     
    my $ref_miny = $map->{reference}{extent}{miny};                                                                                                                     
    my $ref_maxy = $map->{reference}{extent}{maxy};                                                                                                                     
                                                                                                                                                                        
    my $map_width = $map->{width};                                                                                                                                      
    my $map_height = $map->{height};                                                                                                                                    
                                                                                                                                                                        
    my $cellx = ($ref_maxx - $ref_minx) / $ref_width;                                                                                                                   
    my $celly = ($ref_maxy - $ref_miny)/ $ref_height;                                                                                                                   
    my $map_x = $ref_minx + $cellx*$refx;                                                                                                                               
    my $map_y = $ref_maxy - $celly*$refy;                                                                                                                               
                                                                                                                                                                        
    # set $map to new extent                                                                                                                                            
    $map->{extent}->{minx} = $map_x - .5*($imgext[2] - $imgext[0]); # calculate new extent                                                                              
    $map->{extent}->{miny} = $map_y - .5*($imgext[3] - $imgext[1]);                                                                                                     
    $map->{extent}->{maxx} = $map_x + .5*($imgext[2] - $imgext[0]);                                                                                                     
    $map->{extent}->{maxy} = $map_y + .5*($imgext[3] - $imgext[1]);                                                                                                     
                                                                                                                                                                        
    # reset $mapext to new value                                                                                                                                        
    $mapext = join(' ', $map->{extent}->{minx},$map->{extent}->{miny},$map->{extent}->{maxx},$map->{extent}->{maxy});                                                   
                                                                                                                                                                        
    return($mapext);                                                                                                                                                    
}                                                                                                                                                                       
}}}                                                                                                                                                                     
= Partial map file =                                                                                                                                                    
{{{                                                                                                                                                                     
############################################################                                                                                                            
# A patial map file.                                                                                                                                                    
#######################                                                                                                                                                 
MAP                                                                                                                                                                     
  STATUS ON                                                                                                                                                             
  EXTENT -71.50 39.5 -63.0 46.0 # MINX MINY MAXX MAXY                                                                                                                   
  SIZE 504 385                                                                                                                                                          
  UNITS DD                                                                                                                                                              
  IMAGETYPE PNG                                                                                                                                                         
                                                                                                                                                                        
# reference map                                                                                                                                                         
REFERENCE                                                                                                                                                               
  STATUS ON                                                                                                                                                             
  IMAGE "../../data/mapserver/raster/gom/ref_gom30b.png"                                                                                                                
  SIZE 150 115                                                                                                                                                          
  EXTENT -71.5 39.5 -63.0 46.0 # MINX MINY MAXX MAXY                                                                                                                    
  COLOR -1 -1 -1                                                                                                                                                        
  OUTLINECOLOR 255 0 0                                                                                                                                                  
END                                                                                                                                                                     
}}}                                                                                                                                                                     
----                                                                                                                                                                    
back to PerlMapScript                                                                                                                                                   

