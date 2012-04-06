#!perl                                                                                                            
 #!/usr/bin/perl                                                                                                  
# This has been tested with mapserver 4.2 beta3. I found that Perl mapscript4.0.2 was not quite ready for prime   
# time.  Missing methods: class->getStyle(), colorObj support, etc. It illustrates some of the changes needed to  
# adapt a mapscript3.6 application to mapscript4.2. Thanks to Sean Giles for all his work on mapscript.           
########################                                                                                          
# This is an example of creating a layer of point objects (actually buoys in the Gulf of Maine, U.S.) It should be
# easily adaptable to any set of points.  It creates the points as features in the layer which means that they    
# can still be retrieved using $layer->queryByPoint() even though no shape file or database connection exists     
# for them.                                                                                                       
# In this revised edition:                                                                                        
# a) the points are drawn using $point->draw(). This allows us to do a getClass() and getStyle() for each point   
# and set style values  such as $class->{style}{size} for each point.                                             
# b) We call addFeature() so that the layer is queryable, which is the main point.                                
# c) We use a numeric key value as the $shape->{index} which maps to our "database". This will be returned by     
#    queryByPoint().                                                                                              
# d) No need to call $layer->draw() since we already called $point->draw(). Thus the features serve as            
#    an invisible layer just for querying.                                                                        
# Author: Eric Bridger eric@gomoos.org eric@maine.com                                                             
# Date:                                                                                                           
# This has been tested with mapserver 4.2 beta3. It does not work with mapserver4.0.2                             
########################                                                                                          
                                                                                                                  
use strict;                                                                                                       
use mapscript42;                                                                                                  
use CGI ":cgi";                                                                                                   
                                                                                                                  
$ENV{MS_ERRORFILE} = '/tmp/mapserver4.log';                                                                       
                                                                                                                  
my $q = new CGI;                                                                                                  
my $msg = '';                                                                                                     
                                                                                                                  
# A hash of points. First field is the key value used in addFeature() and returned by queryByPoint().             
# This could come from any external database, etc.                                                                
my %points = (                                                                                                    
        10202 =>        {'longitude' => -67.0173,                                                                 
                                'latitude'  => 44.8911,                                                           
                                'size'  => 5,                                                                     
                                'label'  => 'one',                                                                
                                },                                                                                
        20103 =>        {'longitude' =>  -66.0146,                                                                
                                'latitude' => 45.2045,                                                            
                                'size'  => 10,                                                                    
                                'label'  => 'two',                                                                
                                },                                                                                
        30105 =>        {'longitude' => -68.3578,                                                                 
                                'latitude' => 43.7148,                                                            
                                'size'  => 12,                                                                    
                                'label'  => 'three',                                                              
                                },                                                                                
        40102 =>        {'longitude' => -66.5528,                                                                 
                                'latitude' => 43.6243,                                                            
                                'size'  => 18,                                                                    
                                'label'  => 'four',                                                               
                                },                                                                                
        50105 =>        {'longitude' => -68.9983,                                                                 
                                'latitude' => 44.0555,                                                            
                                'size'  => 20,                                                                    
                                'label'  => 'five',                                                               
                                },                                                                                
        60102 =>        {'longitude' => -67.8800,                                                                 
                                'latitude' => 43.4900,                                                            
                                'size'  => 10,                                                                    
                                'label'  => 'six',                                                                
                                },                                                                                
        70104 =>        {'longitude' => -70.5665,                                                                 
                                'latitude' => 42.5185,                                                            
                                'size'  => 10,                                                                    
                                'label'  => 'seven',                                                              
                                },                                                                                
        80103 =>        {'longitude' => -70.4278,                                                                 
                                'latitude' => 43.1807,                                                            
                                'size'  => 10,                                                                    
                                'label'  => 'eight',                                                              
                                },                                                                                
        90104 =>        {'longitude' => -68.1087,                                                                 
                                'latitude' => 44.1058,                                                            
                                'size'  => 8,                                                                     
                                'label'  => 'nine',                                                               
                                },                                                                                
        100202 =>       {'longitude' => -70.0578,                                                                 
                                'latitude' => 43.5673,                                                            
                                'size'  => 15,                                                                    
                                'label'  => 'ten',                                                                
                                },                                                                                
);                                                                                                                
                                                                                                                  
my $image_name = sprintf("tmp/%0.10d",rand(1000000000)) . ".png";                                                 
# see points42.map                                                                                                
my $map = new mapscript42::mapObj("points42.map");                                                                
                                                                                                                  
                                                                                                                  
if(!$map){                                                                                                        
        warn "New mapObj() error: $mapscript42::ms_error->{message}\n";                                           
}                                                                                                                 
                                                                                                                  
# Get symbol indexes from map's symbolset                                                                         
my $circle_idx = $map->{symbolset}->index("circle");                                                              
my $plus_idx = $map->{symbolset}->index("plus");                                                                  
                                                                                                                  
# Colors have changed in 4.2. No longer just an index, $clr_index = $map->addColor() is gone.                     
# You can also use $styleOjb->{color}->setRGB() and $class->{label}->{color}->setRGB();                           
my $blue = new mapscript42::colorObj(0,0,255);                                                                    
my $red = new mapscript42::colorObj(255,0,0);                                                                     
my $black = new mapscript42::colorObj(0,0,0);                                                                     
                                                                                                                  
# Create a point object representing the mouse click on the map.                                                  
my ($x, $y) = get_click($q, $map);                                                                                
                                                                                                                  
my $click_pt = undef;                                                                                             
if($x != 0 && $y != 0){                                                                                           
        $click_pt = new mapscript42::pointObj();                                                                  
        $click_pt->{x} = $x;                                                                                      
        $click_pt->{y} = $y;                                                                                      
}                                                                                                                 
                                                                                                                  
# create an image for drawing.                                                                                    
my $img = $map->draw();                                                                                           
                                                                                                                  
if(!$img){                                                                                                        
        warn "prepareImage() error: $mapscript42::ms_error->{message}\n";                                         
}                                                                                                                 
                                                                                                                  
my $layerObj = undef;                                                                                             
                                                                                                                  
# Add points as Features to the point layer.                                                                      
$layerObj = $map->getLayerByName('points');                                                                       
                                                                                                                  
                                                                                                                  
# Queries will return index into this array.                                                                      
foreach my $point_id (keys %points){                                                                              
        my $point = new mapscript42::pointObj();                                                                  
        $point->{x} = $points{$point_id}{longitude};                                                              
        $point->{y} = $points{$point_id}{latitude};                                                               
        # Features require shape objects, which require lines, so create a single point line.                     
    my $line = new mapscript42::lineObj();                                                                        
        $line->add($point);                                                                                       
        my $shp = new mapscript42::shapeObj($mapscript42::MS_SHAPE_POINT);                                        
        $shp->add($line);                                                                                         
        #$shp->setBounds();                                                                                       
        # Don't set any text, $point->draw() will draw the text.                                                  
        #$shp->{text} = $point_id;                                                                                
        # set the shape index to our database key value.                                                          
        # the $shp->{index} can be any NUMERIC value. If our database key values were alphanumeric                
        # we would need to use a lookup array and set $shp-{index} to 0,1,2,...                                   
        # queryByPoint() results will return this value, but it must be numeric.                                  
        $shp->{index} = $point_id;                                                                                
        $layerObj->addFeature($shp);                                                                              
        # we only have one class in this layer.                                                                   
        my $class = $layerObj->getClass(0);                                                                       
                                                                                                                  
        # TWO APPROACHES illustrated here.                                                                        
        # NEW SYTLE OBJ                                                                                           
        #my $style = new mapscript42::styleObj();                                                                 
        # OR                                                                                                      
        # EXISTING STYLE OBJ: This picks up defaults from map file e.g. black outline color                       
        my $style = $class->getStyle(0);                                                                          
        # Add new style.                                                                                          
        #my $style = new mapscript42::styleObj($class);                                                           
                                                                                                                  
        # point size based on our "database"                                                                      
        $style->{size} = $points{$point_id}{size};                                                                
        if($point_id == 40102){                                                                                   
                $style->{symbol} = $plus_idx;                                                                     
        }else{                                                                                                    
                $style->{symbol} = $circle_idx;                                                                   
        }                                                                                                         
        #$style->{symbol} = $plus_idx;                                                                            
                                                                                                                  
        # COLORS                                                                                                  
        # just to illustrate assigning a reference to a colorObj.                                                 
        my $color = $red;                                                                                         
        if($point_id == 20103){                                                                                   
                $color = $black;                                                                                  
        }                                                                                                         
        $style->{color} = $color;                                                                                 
                                                                                                                  
        # since this is a STYLE from the map file we don't need this.                                             
        # it's in the map file.                                                                                   
        $style->{outlinecolor} = $black;                                                                          
                                                                                                                  
        $class->{label}->{color} = $blue;                                                                         
                                                                                                                  
        $point->draw($map, $layerObj, $img, undef, $points{$point_id}{label});                                    
                                                                                                                  
        # NEW STYLE must REMOVE if you inserted it.                                                               
        #$class->removeStyle(0);                                                                                  
}                                                                                                                 
                                                                                                                  
# Query based on the mouse click point.                                                                           
if($click_pt){                                                                                                    
        $msg .= "<p>\n";                                                                                          
        # this is un-needed.                                                                                      
        $layerObj = $map->getLayerByName('points');                                                               
                                                                                                                  
        if($layerObj->queryByPoint($map,$click_pt,$mapscript42::MS_SINGLE,0)){                                    
                $msg .= "No Points found<br>\n";                                                                  
        }else{                                                                                                    
                # In 4.2 $layerObj-{resultcache} is no longer used.                                               
                # use $num_results = $layerObj->getNumResults() then                                              
                # foreach my $i (0 .. $num_results-1){                                                            
                #    my $rslt = $layerObj->getResult($i);                                                         
                #}                                                                                                
                # we only expect one result.                                                                      
                my $rslt = $layerObj->getResult(0);                                                               
                # this is the numeric value we used for the shape passed to addFeature() above.                   
                my $point_id = $rslt->{shapeindex};                                                               
                $msg .= "Click found point: $point_id.<br>\n";                                                    
                $msg .= "name is:  $points{$point_id}{label}.<br>\n";                                             
                $msg .= "size is:  $points{$point_id}{size}.<br>\n";                                              
                $msg .= "lat:  $points{$point_id}{latitude} long:  $points{$point_id}{longitude}.<br>\n";         
                                                                                                                  
        }                                                                                                         
        $msg .= "</p>\n";                                                                                         
                                                                                                                  
}                                                                                                                 
                                                                                                                  
# display the click point                                                                                         
if($click_pt){                                                                                                    
        $layerObj = $map->getLayerByName('click');                                                                
        $click_pt->draw($map, $layerObj, $img, undef, "Click");                                                   
}                                                                                                                 
                                                                                                                  
$map->drawLabelCache($img);                                                                                       
                                                                                                                  
$img->save($image_name);                                                                                          
                                                                                                                  
# NOTA BENA Deconstructors (~) have been created for all mapscript objects. Don't call $imgObj->free().           
# It will be removed, but now causes core dump.                                                                   
#$img->free();                                                                                                    
                                                                                                                  
# Output the HTML form and map                                                                                    
                                                                                                                  
print $q->header();                                                                                               
                                                                                                                  
print $q->start_html(-title=>'MapServer4.2 - Dynamic Points', -bgcolor=>"#ffffff");                               
                                                                                                                  
print "<form name=\"pointmap\" action=\"points42.cgi\" method=\"GET\">\n";                                        
print "<table border=\"1\" cellpadding=\"5\" cellspacing=\"2\">\n";                                               
print "<tr>\n";                                                                                                   
print "<td>\n";                                                                                                   
print "<input border=\"2\" type=\"image\" name=\"img\" src=\"$image_name\">\n";                                   
print "</td>\n";                                                                                                  
print "</tr>\n";                                                                                                  
print "</table>\n";                                                                                               
print "</form>\n";                                                                                                
print "$msg<br>\n";                                                                                               
print "<p><br><br><br></p>\n";                                                                                    
                                                                                                                  
                                                                                                                  
print $q->end_html();                                                                                             
                                                                                                                  
# translate mouse click x,y into map longitude, latitude based on map extent. This is based on set_extent() in    
# mapquakes.pl                                                                                                    
                                                                                                                  
sub get_click {                                                                                                   
        my ($q, $map) = @_;                                                                                       
        my ($x, $y, $cx, $cy) = (0,0,0,0);                                                                        
        my $minx = $map->{extent}->{minx};                                                                        
        my $miny = $map->{extent}->{miny};                                                                        
        my $maxx = $map->{extent}->{maxx};                                                                        
        my $maxy = $map->{extent}->{maxy};                                                                        
                                                                                                                  
        if($q->param('img.x')) { # Make sure we got a click                                                       
                $x = $q->param('img.x');                                                                          
                $y = $q->param('img.y');                                                                          
                                                                                                                  
                $cx = ($maxx-$minx)/($map->{width}-1); # calculate cellsize in x and y                            
                $cy = ($maxy-$miny)/($map->{height}-1);                                                           
                                                                                                                  
                $x = $minx + $cx*$x; # change x,y from image to map coordinates                                   
                $y = $maxy - $cy*$y;                                                                              
        }                                                                                                         
                                                                                                                  
        return ($x, $y);                                                                                          
}                                                                                                                 
```                                                                                                               
                                                                                                                  
= points42.map =                                                                                                  

```                                                                                                               
MAP                                                                                                               
  NAME "points42"                                                                                                 
  STATUS ON                                                                                                       
  EXTENT -71.5 39.5 -63.0 46.0                                                                                    
  SIZE 504 385                                                                                                    
  IMAGETYPE PNG                                                                                                   
  UNITS DD                                                                                                        
  PROJECTION                                                                                                      
     "proj=latlong"                                                                                               
  END                                                                                                             
  OUTPUTFORMAT                                                                                                    
    NAME PNG                                                                                                      
    DRIVER "GD/PNG"                                                                                               
    MIMETYPE "image/png"                                                                                          
    # 24bit                                                                                                       
    IMAGEMODE RGB                                                                                                 
    # 8 bit psuedo color                                                                                          
    #IMAGEMODE PC256                                                                                              
    #EXTENSION "png"                                                                                              
  END                                                                                                             
                                                                                                                  
SYMBOL                                                                                                            
  TYPE ELLIPSE                                                                                                    
  NAME "circle"                                                                                                   
  POINTS 1 1 END                                                                                                  
  FILLED TRUE                                                                                                     
END                                                                                                               
                                                                                                                  
SYMBOL                                                                                                            
  TYPE VECTOR                                                                                                     
  NAME "plus"                                                                                                     
  POINTS .5 0 .5 1 -99 -99 0 .5 1 .5 END                                                                          
END                                                                                                               
                                                                                                                  
LAYER                                                                                                             
  NAME "points"                                                                                                   
  TYPE POINT                                                                                                      
  STATUS ON                                                                                                       
  TOLERANCE 10                                                                                                    
  # Need fake template for querys to work                                                                         
  TEMPLATE "bogus.html"                                                                                           
  CLASS                                                                                                           
    NAME "buoy"                                                                                                   
    STYLE                                                                                                         
      SYMBOL "circle"                                                                                             
      SIZE 0                                                                                                      
      COLOR 0 255 0                                                                                               
      OUTLINECOLOR 0 0 0                                                                                          
    END                                                                                                           
    LABEL                                                                                                         
      COLOR 255 0 0                                                                                               
      TYPE BITMAP                                                                                                 
      SIZE MEDIUM                                                                                                 
      POSITION AUTO                                                                                               
      PARTIALS FALSE                                                                                              
      BUFFER 2                                                                                                    
    END # end of label                                                                                            
  END                                                                                                             
  PROJECTION                                                                                                      
    "proj=latlong"                                                                                                
  END                                                                                                             
END                                                                                                               
                                                                                                                  
LAYER                                                                                                             
  NAME "click"                                                                                                    
  TYPE POINT                                                                                                      
  STATUS ON                                                                                                       
  CLASS                                                                                                           
    NAME "click"                                                                                                  
    STYLE                                                                                                         
      SYMBOL "plus"                                                                                               
      SIZE  6                                                                                                     
      COLOR 0 0 0                                                                                                 
    END                                                                                                           
    LABEL                                                                                                         
      TYPE BITMAP                                                                                                 
      SIZE TINY                                                                                                   
      COLOR 0 0 0                                                                                                 
      POSITION AUTO                                                                                               
      PARTIALS FALSE                                                                                              
      BUFFER 1                                                                                                    
    END                                                                                                           
  END                                                                                                             
END                                                                                                               
                                                                                                                  
END                                                                                                               
```                                                                                                               
----                                                                                                              
back to PerlMapScrip
