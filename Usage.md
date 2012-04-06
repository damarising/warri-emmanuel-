Here is a simple perl example that loads a mapfile,                                                      
creates an image and saves it:                                                                           

```                                                                                                      
#!perl                                                                                                   
#!/usr/bin/perl -w                                                                                       
use mapscript;                                                                                           
#                                                                                                        
# Include XBase for access to XBase/DBF files.                                                           
use XBase;                                                                                               
#                                                                                                        
# Include DBI so XBase files can be queried in an SQL manner.                                            
use DBI;                                                                                                 
#                                                                                                        
# Start the map.                                                                                         
my $map = new mapscript::mapObj('boundary.map') or die('Unable to open mapfile.');                       
#                                                                                                        
# Render the map.                                                                                        
my $img = $map->draw() or die('Unable to draw map');                                                     
#                                                                                                        
# Save the rendered image.                                                                               
my $void = $img->saveImage('example.png', $mapscript::MS_PNG, $map->{transparent}, $map->{interlace}, 0);
```                                                                                                      
----                                                                                                     
back to PerlMapScrip
