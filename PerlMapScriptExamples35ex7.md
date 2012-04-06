{{{                                                                                                                                                                                               
#!perl                                                                                                                                                                                            
#!/usr/bin/perl -w                                                                                                                                                                                
#                                                                                                                                                                                                 
# Copyright (C) 2002, Lowell Filak.                                                                                                                                                               
# You may distribute this file under the terms of the Artistic                                                                                                                                    
# License.                                                                                                                                                                                        
#                                                                                                                                                                                                 
# Given a shapefile name, a coordinate, & an item name                                                                                                                                            
#   this routine will select a shape,                                                                                                                                                             
#   select all the shapes with the same item value, & create a shapefile of                                                                                                                       
#   the selected shape(s).                                                                                                                                                                        
#                                                                                                                                                                                                 
# Required modules are mapscript (installed as part of make install),                                                                                                                             
#   Getopt (normally included with Perl) & XBase.                                                                                                                                                 
#   Please download boundary.tar.gz also, and:                                                                                                                                                    
#     tar -xf boundary.tar.gz --ungzip                                                                                                                                                            
#   Note: The suggested run assumes a pick point on a 600x600 image.                                                                                                                              
#                                                                                                                                                                                                 
# Suggested run method = ./qry_point.pl -file=boundary -coorx=372 -coory=115 -item=loc_name                                                                                                       
use mapscript;                                                                                                                                                                                    
use Getopt::Long;                                                                                                                                                                                 
use XBase;                                                                                                                                                                                        
#                                                                                                                                                                                                 
# Retrieve the input values.                                                                                                                                                                      
GetOptions('file=s' => \$file, 'coorx=s' => \$coorx, 'coory=s' => \$coory, 'item=s' => \$item);                                                                                                   
#                                                                                                                                                                                                 
# Check the input values.                                                                                                                                                                         
if ( (!$file) || (!$coorx) || (!$coory) || (!$item) ) {                                                                                                                                           
  print "Syntax: find.pl -file=[filename] -coorx=[x_coordinate] -coory=[y_coordinate] -item=[item_name]\n";                                                                                       
  exit 0;                                                                                                                                                                                         
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Upcase the item name.                                                                                                                                                                           
$item = uc $item;                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Create mapfile name.                                                                                                                                                                            
my $mapfile = $file . '.map';                                                                                                                                                                     
#                                                                                                                                                                                                 
# Open map using default map file.                                                                                                                                                                
my $map = new mapObj("$mapfile") or die('Unable to Open Default MapFile!');                                                                                                                       
#                                                                                                                                                                                                 
# Subtract one pixel.                                                                                                                                                                             
# Why is this done?: I don't directly recall but I think it has to do with                                                                                                                        
#   mapextents starting at a positive integer while image starts at 0,0.                                                                                                                          
$imgx = $map->{width} - 1;                                                                                                                                                                        
$imgy = $map->{height} - 1;                                                                                                                                                                       
#                                                                                                                                                                                                 
# Find the extents of the map.                                                                                                                                                                    
$minx = $map->{extent}->{minx};                                                                                                                                                                   
$miny = $map->{extent}->{miny};                                                                                                                                                                   
$maxx = $map->{extent}->{maxx};                                                                                                                                                                   
$maxy = $map->{extent}->{maxy};                                                                                                                                                                   
#                                                                                                                                                                                                 
# Caculate a delta x & delta y.                                                                                                                                                                   
$dx = $maxx - $minx;                                                                                                                                                                              
$dy = $maxy - $miny;                                                                                                                                                                              
#                                                                                                                                                                                                 
# Divide delta x & y by pixel extents to find factor x & y.                                                                                                                                       
$fctrx = $dx / $imgx;                                                                                                                                                                             
$fctry = $dy / $imgy;                                                                                                                                                                             
#                                                                                                                                                                                                 
# Adjust to real world coordinates.                                                                                                                                                               
$coorx = $coorx * $fctrx;                                                                                                                                                                         
$coory = $coory * $fctry;                                                                                                                                                                         
$coorx = $coorx + $minx;                                                                                                                                                                          
$coory = $maxy - $coory;                                                                                                                                                                          
#                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Create point object for pick query.                                                                                                                                                             
$pnt = new pointObj();                                                                                                                                                                            
$pnt->{x} = $coorx;                                                                                                                                                                               
$pnt->{y} = $coory;                                                                                                                                                                               
#                                                                                                                                                                                                 
# Print the point coordinates.                                                                                                                                                                    
print "Selecting Using Point Coordinates: x=$coorx y=$coory\n";                                                                                                                                   
#                                                                                                                                                                                                 
# Get layer for boundary query.                                                                                                                                                                   
#   Note: Most of this is already set in the mapfile and is here for sample                                                                                                                       
#         only.                                                                                                                                                                                   
my $lyr = $map->getLayerByName("$file") or die('Unable to Open Boundary Layer!');                                                                                                                 
$lyr->{status} = $mapscript::MS_ON;                                                                                                                                                               
$lyr->{type} = $mapscript::MS_LAYER_POLYGON;                                                                                                                                                      
$lyr->{data} = "$file";                                                                                                                                                                           
#                                                                                                                                                                                                 
# Query the layer using the created point.                                                                                                                                                        
$lyr->queryByPoint($map,$pnt,$mapscript::MS_SINGLE,0);                                                                                                                                            
#                                                                                                                                                                                                 
# Create a resultcache object to see how many results.                                                                                                                                            
my $rsltcache = $lyr->{resultcache};                                                                                                                                                              
#                                                                                                                                                                                                 
# How many matches did we find.                                                                                                                                                                   
print "Found $rsltcache->{numresults} Result.\n";                                                                                                                                                 
#                                                                                                                                                                                                 
# Grab the first result (there should only be one).                                                                                                                                               
my $rslt = $lyr->getResult(0);                                                                                                                                                                    
#                                                                                                                                                                                                 
# What is the shape number.                                                                                                                                                                       
my $record = $rslt->{shapeindex};                                                                                                                                                                 
#                                                                                                                                                                                                 
# Print the shape number.                                                                                                                                                                         
print "The Query Found Shape #$record.\n";                                                                                                                                                        
#                                                                                                                                                                                                 
# Query the dbf for the item matching records.                                                                                                                                                    
#   Note: The routine is written to utilize dbf files as they originally are.                                                                                                                     
#         Normally you would want to at least add a record number field to the                                                                                                                    
#         dbf file so you could use the DBI & DBD::XBase modules to query                                                                                                                         
#         the db. You could also load the dbf data into an dbms and use                                                                                                                           
#         the DBI & DBD::x modules to query once the record number field                                                                                                                          
#         exists.                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Open the db handle.                                                                                                                                                                             
my $dbh = new XBase "$file" or die XBase->errstr;                                                                                                                                                 
#                                                                                                                                                                                                 
# What is the number of the key field.                                                                                                                                                            
my @names = $dbh->field_names;                                                                                                                                                                    
#                                                                                                                                                                                                 
# How many fields are there.                                                                                                                                                                      
my $fldcnt = $dbh->last_field;                                                                                                                                                                    
#                                                                                                                                                                                                 
# Set the field number to initially 0.                                                                                                                                                            
my $fieldnum = 0;                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Loop through the fields and find the one we want.                                                                                                                                               
for ($field=0; $field<=$fldcnt; $field++){                                                                                                                                                        
  #                                                                                                                                                                                               
  # Is this the field we were looking for.                                                                                                                                                        
  if ( $names[$field] eq $item ) {                                                                                                                                                                
    #                                                                                                                                                                                             
    # If so then exit loop.                                                                                                                                                                       
    $fieldnum = $field;                                                                                                                                                                           
    #                                                                                                                                                                                             
    # Print the field number.                                                                                                                                                                     
    print "The Key Item is Field #$fieldnum.\n";                                                                                                                                                  
    last;                                                                                                                                                                                         
  }                                                                                                                                                                                               
   else {                                                                                                                                                                                         
    #                                                                                                                                                                                             
    # Fall through.                                                                                                                                                                               
   }                                                                                                                                                                                              
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Grab the key record & the key item value.                                                                                                                                                       
my @row = $dbh->get_record_nf($record, $fieldnum) or die $dbh->errstr;                                                                                                                            
#                                                                                                                                                                                                 
# What is the value for the key item.                                                                                                                                                             
my $value = $row[1];                                                                                                                                                                              
#                                                                                                                                                                                                 
# Print the key item value for the key record.                                                                                                                                                    
print "The Value of $item for Shape #$record = $value.\n";                                                                                                                                        
#                                                                                                                                                                                                 
# Start the number of results at 0.                                                                                                                                                               
my $results = 0;                                                                                                                                                                                  
#                                                                                                                                                                                                 
# Open the selection set shapefile.                                                                                                                                                               
#   Note: There is a way to obtain a selection set without saving to a                                                                                                                            
#         shapefile, however due to the type of data I am accustomed to,                                                                                                                          
#         by writing to a shapefile, a type of shapefile cache can be setup.                                                                                                                      
#         By naming all shapefiles in a particular directory in a way                                                                                                                             
#         that allows them to be reopened for any repetitious queries                                                                                                                             
#         the actual work of the query can be bypassed.                                                                                                                                           
my $shapesel = new shapefileObj('selected',$mapscript::MS_SHAPEFILE_POLYGON);                                                                                                                     
#                                                                                                                                                                                                 
# Open the existing shapefile for grabbing the found shapes out of.                                                                                                                               
my $shapefile = new shapefileObj("$file",-1);                                                                                                                                                     
#                                                                                                                                                                                                 
# Loop through each record (there are experimental modules for using indexes                                                                                                                      
#   available according to xbase man page).                                                                                                                                                       
for ($record=0; $record<$dbh->last_record; $record++){                                                                                                                                            
  #                                                                                                                                                                                               
  # Grab the record.                                                                                                                                                                              
  my @row = $dbh->get_record($record, "$item") or die $dbh->errstr;                                                                                                                               
  #                                                                                                                                                                                               
  # Is the record marked for deletion.                                                                                                                                                            
  my $deleted = $row[0];                                                                                                                                                                          
  if ( $deleted == 1 ) {                                                                                                                                                                          
    #                                                                                                                                                                                             
    # If so then skip it.                                                                                                                                                                         
    next;                                                                                                                                                                                         
  }                                                                                                                                                                                               
   else {                                                                                                                                                                                         
    #                                                                                                                                                                                             
    # Fall through.                                                                                                                                                                               
   }                                                                                                                                                                                              
  #                                                                                                                                                                                               
  # Set the value for the search field.                                                                                                                                                           
  my $fndvalue = $row[1];                                                                                                                                                                         
  #                                                                                                                                                                                               
  # Does the value from the field match the value for the key record.                                                                                                                             
  if ( "$fndvalue" ne "$value" ) {                                                                                                                                                                
    #                                                                                                                                                                                             
    # If not skip it.                                                                                                                                                                             
    next;                                                                                                                                                                                         
  }                                                                                                                                                                                               
   else {                                                                                                                                                                                         
    #                                                                                                                                                                                             
    # Fall through.                                                                                                                                                                               
   }                                                                                                                                                                                              
  #                                                                                                                                                                                               
  # Print the found record information.                                                                                                                                                           
  print "Record #$record Matches with a Value of $fndvalue - good thing :-)\n";                                                                                                                   
  #                                                                                                                                                                                               
  # Increment the results counter.                                                                                                                                                                
  $results = $results + 1;                                                                                                                                                                        
  #                                                                                                                                                                                               
  # Create the shape object for holding the found shapes.                                                                                                                                         
  my $shape = new shapeObj(-1);                                                                                                                                                                   
  #                                                                                                                                                                                               
  # Grab shape #$record and stick it into the shape holder.                                                                                                                                       
  #                                                                                                                                                                                               
  $shapefile->get($record - 1, $shape);                                                                                                                                                           
  #                                                                                                                                                                                               
  # Add that shape to the selection set shapefile.                                                                                                                                                
  $shapesel->add($shape);                                                                                                                                                                         
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Close the new shapefile.                                                                                                                                                                        
undef $shapesel;                                                                                                                                                                                  
#                                                                                                                                                                                                 
# Create dbf to go with it.                                                                                                                                                                       
my $newdbh = $dbh->create("name" => "selected.dbf");                                                                                                                                              
#                                                                                                                                                                                                 
# Reopen the selected set shapefile.                                                                                                                                                              
$shapesel = new shapefileObj("selected", -1);                                                                                                                                                     
#                                                                                                                                                                                                 
# Get the extent of selected set.                                                                                                                                                                 
$newrect = $shapesel->{bounds};                                                                                                                                                                   
$newminx = $newrect->{minx};                                                                                                                                                                      
$newmaxx = $newrect->{maxx};                                                                                                                                                                      
$newminy = $newrect->{miny};                                                                                                                                                                      
$newmaxy = $newrect->{maxy};                                                                                                                                                                      
$numseld = $shapesel->{numshapes};                                                                                                                                                                
undef $shapesel;                                                                                                                                                                                  
#                                                                                                                                                                                                 
# Print the extents.                                                                                                                                                                              
print "The Extents of the Selected Set: minx=$newminx miny=$newminy maxx=$newmaxx maxy=$newmaxy.\n";                                                                                              
#                                                                                                                                                                                                 
# Print the number of selected records.                                                                                                                                                           
print "The Number of Selected Shapes = $numseld.\n";                                                                                                                                              
}}}                                                                                                                                                                                               
----                                                                                                                                                                                              
back to PerlMapScrip
