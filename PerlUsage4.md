Available by re-swigging(sp?) with NEXT_GENERATION_API swig -perl5 -shadow -DNEXT_GENERATION_API mapscript.i    
                                                                                                                
== Optional New Naming Convention ==                                                                            
Available by re-swigging(sp?) with NEXT_GENERATION_NAMES swig -perl5 -shadow -DNEXT_GENERATION_NAMES mapscript.i
                                                                                                                
== Example ==                                                                                                   
Here is a simple perl example that loads a mapfile, creates an image and saves it:                              
{{{                                                                                                             
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
my $void = $img->save('example.png');                                                                           
}}}                                                                                                             
----                                                                                                            
back to PerlMapScript                                                                                           

