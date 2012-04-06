{{{                                                                                                                                                                             
#!perl                                                                                                                                                                          
#!/usr/bin/perl                                                                                                                                                                 
#                                                                                                                                                                               
# Copyright (C) 2002, Lowell Filak.                                                                                                                                             
# You may distribute this file under the terms of the Artistic                                                                                                                  
# License.                                                                                                                                                                      
use strict;                                                                                                                                                                     
#                                                                                                                                                                               
# Given the name of an existing polygon shapefile this routine will add                                                                                                         
#   4 fields to the dbf file for the least bounding rectangle for each                                                                                                          
#   polygon and populate those fields.                                                                                                                                          
#                                                                                                                                                                               
# Required modules are mapscript (installed as part of make install                                                                                                             
#   http://mapserver.gis.umn.edu),                                                                                                                                              
#   Getopt (normally included with Perl),                                                                                                                                       
#   & XBase (cpan).                                                                                                                                                             
#   Please download boundary.tar.gz also, and:                                                                                                                                  
#     tar -xf boundary.tar.gz --ungzip                                                                                                                                          
#                                                                                                                                                                               
# Suggested run line = ./bound_add.pl -pfile=boundary                                                                                                                           
#                                                                                                                                                                               
# Include the mapscript module.                                                                                                                                                 
use mapscript;                                                                                                                                                                  
#                                                                                                                                                                               
# Include the xbase and dbi modules for searching and updating values.                                                                                                          
use XBase;                                                                                                                                                                      
#                                                                                                                                                                               
# Include the getopt module to read input.                                                                                                                                      
use Getopt::Long;                                                                                                                                                               
#                                                                                                                                                                               
# Grab the file name from the input.                                                                                                                                            
my $pfile='';                                                                                                                                                                   
&GetOptions('pfile=s' => \$pfile);                                                                                                                                              
if ( !$pfile ) {                                                                                                                                                                
  print "Syntax: bound_add.pl -pfile=[in_polygon_shapefile_name]";                                                                                                              
  exit 0;                                                                                                                                                                       
}                                                                                                                                                                               
#                                                                                                                                                                               
# Create a unique name for a new mapfile for querying the polygons.                                                                                                             
#                                                                                                                                                                               
# Grab the date.                                                                                                                                                                
my ($sec,$min,$hr,$mdy,$mnth,$yr,$wdy,$ydy,$isdst) = localtime;                                                                                                                 
#                                                                                                                                                                               
# Create the name.                                                                                                                                                              
my $sfile = "T$hr$min$sec";                                                                                                                                                     
#                                                                                                                                                                               
# Open the mapfile for writing.                                                                                                                                                 
open(MAPFILE, ">$sfile.map");                                                                                                                                                   
#                                                                                                                                                                               
# Open the existing polygon shapefile.                                                                                                                                          
                                                                                                                                                                                
my $inshpf = new shapefileObj($pfile, -1) or die "Unable to open shapefile $pfile.";                                                                                            
#                                                                                                                                                                               
# What are the extents.                                                                                                                                                         
my $inshpminx = $inshpf->{bounds}->{minx};                                                                                                                                      
my $inshpminy = $inshpf->{bounds}->{miny};                                                                                                                                      
my $inshpmaxx = $inshpf->{bounds}->{maxx};                                                                                                                                      
my $inshpmaxy = $inshpf->{bounds}->{maxy};                                                                                                                                      
#                                                                                                                                                                               
# Open the existing dbf file for appending fields.                                                                                                                              
# Thanks to: Chris Stuber.                                                                                                                                                      
my $dbh = new XBase "$pfile.dbf" or die XBase->errstr;                                                                                                                          
my @fn = $dbh->field_names();                                                                                                                                                   
my @ft = $dbh->field_types();                                                                                                                                                   
my @fl = $dbh->field_lengths();                                                                                                                                                 
my @fd = $dbh->field_decimals();                                                                                                                                                
#                                                                                                                                                                               
# Push additional values into each                                                                                                                                              
# of the arrays above. (ie new Columns)                                                                                                                                         
push @fn, "recno","minx","miny","maxx","maxy";                                                                                                                                  
push @ft, "N","N","N","N","N";                                                                                                                                                  
push @fl, "9","20","20","20","20";                                                                                                                                              
push @fd, "0","8","8","8","8";                                                                                                                                                  
#                                                                                                                                                                               
my $newtable = XBase->create("name" => "$sfile.dbf", "field_names" => [@fn], "field_types" => [@ft], "field_lengths" => [@fl], "field_decimals" => [@fd] ) or die XBase->errstr;
for my $x (0 .. $dbh->last_record) {                                                                                                                                            
  my @rec = $dbh->get_record($x);                                                                                                                                               
  my $deleted = shift @rec;                                                                                                                                                     
  next if ($deleted > 0);                                                                                                                                                       
  #                                                                                                                                                                             
  # push new data onto @rec or leave empty                                                                                                                                      
  print "Copying Record $x\n";                                                                                                                                                  
  push @rec,($x);                                                                                                                                                               
  $newtable->set_record($x,@rec) or die XBase->errstr;                                                                                                                          
}                                                                                                                                                                               
#                                                                                                                                                                               
# Close the dbf file, replace it, and reopen it for setting values.                                                                                                             
undef $newtable;                                                                                                                                                                
undef $dbh;                                                                                                                                                                     
unlink "$pfile.dbf";                                                                                                                                                            
rename "$sfile.dbf","$pfile.dbf";                                                                                                                                               
$dbh = new XBase "$pfile.dbf" or die XBase->errstr;                                                                                                                             
#                                                                                                                                                                               
# Create the contents of the mapfile.                                                                                                                                           
print MAPFILE <<EOF;                                                                                                                                                            
#                                                                                                                                                                               
NAME $sfile                                                                                                                                                                     
STATUS ON                                                                                                                                                                       
SIZE 600 600                                                                                                                                                                    
SYMBOLSET "$sfile.sym"                                                                                                                                                          
EXTENT $inshpminx $inshpminy $inshpmaxx $inshpmaxy                                                                                                                              
UNITS FEET                                                                                                                                                                      
SHAPEPATH ""                                                                                                                                                                    
IMAGECOLOR 255 255 255                                                                                                                                                          
LAYER                                                                                                                                                                           
  NAME bnd_qry                                                                                                                                                                  
  TYPE POLYGON                                                                                                                                                                  
  STATUS ON                                                                                                                                                                     
  DATA "$pfile"                                                                                                                                                                 
  TEMPLATE 'bogus.html'                                                                                                                                                         
  CLASS                                                                                                                                                                         
    COLOR 255 0 0                                                                                                                                                               
    NAME "bnd_qry"                                                                                                                                                              
  END                                                                                                                                                                           
END                                                                                                                                                                             
END                                                                                                                                                                             
EOF                                                                                                                                                                             
#                                                                                                                                                                               
# Close the mapfile.                                                                                                                                                            
close MAPFILE;                                                                                                                                                                  
#                                                                                                                                                                               
# Open the symbol file for writing.                                                                                                                                             
open(SYMFILE, ">$sfile.sym");                                                                                                                                                   
#                                                                                                                                                                               
# Create the contents of the symbol file.                                                                                                                                       
print SYMFILE <<EOF;                                                                                                                                                            
SYMBOLSET                                                                                                                                                                       
END                                                                                                                                                                             
EOF                                                                                                                                                                             
#                                                                                                                                                                               
# Close the symbol file.                                                                                                                                                        
close SYMFILE;                                                                                                                                                                  
#                                                                                                                                                                               
# How many polygons are there.                                                                                                                                                  
my $innumshp = $inshpf->{numshapes};                                                                                                                                            
#                                                                                                                                                                               
# Create the mapobject.                                                                                                                                                         
my $map = new mapObj("$sfile.map") or die("Unable to Open Default MapFile $sfile.map!");                                                                                        
#                                                                                                                                                                               
# Create the layer object for the queries.                                                                                                                                      
my $lyr = $map->getLayerByName("bnd_qry") or die('Unable to Open Polygon Layer!');                                                                                              
#                                                                                                                                                                               
# Set the default query result to blank.                                                                                                                                        
my @row = ();                                                                                                                                                                   
#                                                                                                                                                                               
# Create the shape holder.                                                                                                                                                      
my $inpol = new shapeObj(-1);                                                                                                                                                   
#                                                                                                                                                                               
# Loop through each polygon and record.                                                                                                                                         
for (my $innum=0; $innum<$innumshp; $innum++ ) {                                                                                                                                
  print "Recording Polygon #$innum...\n";                                                                                                                                       
  #                                                                                                                                                                             
  # Grab the polygon by number.                                                                                                                                                 
  my $junk = $inshpf->get($innum, $inpol);                                                                                                                                      
  #                                                                                                                                                                             
  # Grab the bounds of the polygon.                                                                                                                                             
  my $minx = $inpol->{bounds}->{minx};                                                                                                                                          
  my $maxx = $inpol->{bounds}->{maxx};                                                                                                                                          
  my $miny = $inpol->{bounds}->{miny};                                                                                                                                          
  my $maxy = $inpol->{bounds}->{maxy};                                                                                                                                          
  #                                                                                                                                                                             
  # Grab the dbf record.                                                                                                                                                        
  my @recd = $dbh->get_record($innum);                                                                                                                                          
  my $deleted = shift @recd;                                                                                                                                                    
  my $grbg = pop(@recd);                                                                                                                                                        
  my $grbg = pop(@recd);                                                                                                                                                        
  my $grbg = pop(@recd);                                                                                                                                                        
  my $grbg = pop(@recd);                                                                                                                                                        
  #                                                                                                                                                                             
  # Push the values into place.                                                                                                                                                 
  push @recd,($minx,$miny,$maxx,$maxy);                                                                                                                                         
  #                                                                                                                                                                             
  # Record the new record.                                                                                                                                                      
  $dbh->set_record($innum,@recd) or die XBase->errstr;                                                                                                                          
}                                                                                                                                                                               
#                                                                                                                                                                               
# Get rid of the temporary mapfile & symbol file.                                                                                                                               
unlink "$sfile.map";                                                                                                                                                            
unlink "$sfile.sym";                                                                                                                                                            
}}}                                                                                                                                                                             
----                                                                                                                                                                            
back to PerlMapScrip
