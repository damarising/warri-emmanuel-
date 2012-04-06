#!perl                                                                                                                                                    
#!/usr/bin/perl                                                                                                                                           
                                                                                                                                                          
use pdflib_pl;                                                                                                                                            
use CGI qw(:standard :html);                                                                                                                              
use mapscript36;                                                                                                                                          
                                                                                                                                                          
$page_resolution = 72; # dpi                                                                                                                              
$page_width = 11; # inches                                                                                                                                
$page_height = 8.5;                                                                                                                                       
$page_width_pixels = $page_width*$page_resolution;                                                                                                        
$page_height_pixels =$page_height*$page_resolution;                                                                                                       
                                                                                                                                                          
$map_resolution = 144; #dpi                                                                                                                               
$map_width = 10; # inches                                                                                                                                 
$map_height = 7.5;                                                                                                                                        
$map_width_pixels = $map_width*$map_resolution;                                                                                                           
$map_height_pixels =$map_height*$map_resolution;                                                                                                          
$map_scaling = $page_resolution/$map_resolution;                                                                                                          
                                                                                                                                                          
sub error() {                                                                                                                                             
  my($err) = @_;                                                                                                                                          
                                                                                                                                                          
  die $err;                                                                                                                                               
}                                                                                                                                                         
                                                                                                                                                          
$cgi = new CGI();                                                                                                                                         
                                                                                                                                                          
print header(-type=>'application/pdf');                                                                                                                   
                                                                                                                                                          
$pdf = PDF_new();                                                                                                                                         
                                                                                                                                                          
&error("Unable to open output PDF file.") if (PDF_open_file($pdf, "-") == -1);                                                                            
                                                                                                                                                          
# PDF Metadata                                                                                                                                            
PDF_set_info($pdf, 'Creator', 'lvprint.pl');                                                                                                              
PDF_set_info($pdf, 'Author', 'Minnesota Department of Natural Resources: LandView');                                                                      
                                                                                                                                                          
#                                                                                                                                                         
# Build the map                                                                                                                                           
#                                                                                                                                                         
&error("No mapfile specified.") unless $cgi->param('map');                                                                                                
$mapfile =  $cgi->param('map');                                                                                                                           
$mapfile = $ENV{$cgi->param('map')} if $ENV{$cgi->param('map')};                                                                                          
$map = new mapscript36::mapObj($mapfile) or &error("Unable to open ". $cgi->param('map') .".");                                                           
                                                                                                                                                          
# Step 0: override any necessary settings                                                                                                                 
$map->{width} = $map_width_pixels;                                                                                                                        
$map->{height} = $map_height_pixels;                                                                                                                      
$map->{scalebar}->{status} = $mapscript36::MS_OFF;                                                                                                        
                                                                                                                                                          
# Step 1: set a new extent                                                                                                                                
($map->{extent}->{minx}, $map->{extent}->{miny}, $map->{extent}->{maxx}, $map->{extent}->{maxy}) = split(/ /, $cgi->param(mapext)) if $cgi->param(mapext);
                                                                                                                                                          
# Step 2: turn the write layers on                                                                                                                        
for(my $i=0; $i<$map->{numlayers}; $i++) {                                                                                                                
  $layer = $map->getLayer($i);                                                                                                                            
                                                                                                                                                          
  if($layer->{status} == $mapscript36::MS_DEFAULT) {                                                                                                      
    next;                                                                                                                                                 
  }                                                                                                                                                       
                                                                                                                                                          
  $layer->{status} = $mapscript36::MS_OFF;                                                                                                                
                                                                                                                                                          
  foreach (split(/ /, $cgi->param(layers))) {                                                                                                             
    if(($layer->{name} eq $_) or ($layer->{group} eq $_)) {                                                                                               
      $layer->{status} = $mapscript36::MS_ON;                                                                                                             
    }                                                                                                                                                     
  }                                                                                                                                                       
}                                                                                                                                                         
                                                                                                                                                          
# Step 3: draw the map                                                                                                                                    
$img = $map->draw();                                                                                                                                      
                                                                                                                                                          
# Step 4: save the image to a temporary file                                                                                                              
$imgfile = $map->{web}->{imagepath} . $$ . time() . ".gif";                                                                                               
$img->saveImage($imgfile, $mapscript36::MS_PNG, $mapscript36::MS_FALSE, $mapscript36::MS_FALSE, -1);                                                      
                                                                                                                                                          
# clean up                                                                                                                                                
$img->free();                                                                                                                                             
undef $map;                                                                                                                                               
#                                                                                                                                                         
# End map building                                                                                                                                        
#                                                                                                                                                         
                                                                                                                                                          
#                                                                                                                                                         
# Build the PDF file                                                                                                                                      
#                                                                                                                                                         
$top_margin = $page_resolution*.5; # 1/2"                                                                                                                 
$bottom_margin = $left_margin = $right_margin = $page_resolution/2.0; # 1/2"                                                                              
                                                                                                                                                          
PDF_begin_page($pdf, $page_width_pixels, $page_height_pixels);                                                                                            
                                                                                                                                                          
# PDF_set_parameter($pdf, "imagewarning", "true");                                                                                                        
                                                                                                                                                          
# Open and place the map                                                                                                                                  
&error("Could not open image file ($imgfile).") if (($png = PDF_open_image_file($pdf, 'png', $imgfile, '', 0)) == -1);                                    
$x = $left_margin+1;                                                                                                                                      
$y = $page_height_pixels - $map_height_pixels*$map_scaling - $top_margin - 1;                                                                             
PDF_place_image($pdf, $png, $x, $y, $map_scaling);                                                                                                        
PDF_close_image($pdf, $png);                                                                                                                              
                                                                                                                                                          
# Neatline                                                                                                                                                
                                                                                                                                                          
PDF_setrgbcolor_stroke($pdf, .267, .4, .2); # 446633 => 68 102 51                                                                                         
$x = $left_margin + 1;                                                                                                                                    
$y = $page_height_pixels - $map_height_pixels*$map_scaling - $top_margin - 1;                                                                             
$w = $map_width_pixels*$map_scaling;                                                                                                                      
$h = $map_height_pixels*$map_scaling;                                                                                                                     
PDF_rect($pdf, $x, $y, $w, $h);                                                                                                                           
PDF_stroke($pdf);                                                                                                                                         
                                                                                                                                                          
PDF_end_page($pdf);                                                                                                                                       
                                                                                                                                                          
PDF_close($pdf);                                                                                                                                          
PDF_delete($pdf);                                                                                                                                         
                                                                                                                                                          
unlink $imgfile;                                                                                                                                          
}}}                                                                                                                                                       
----                                                                                                                                                      
back to PerlMapScrip
