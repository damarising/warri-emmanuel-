                                                                                                                                                                                                  

```                                                                                                                                                                                               
#!perl                                                                                                                                                                                            
#!/usr/bin/perl -w                                                                                                                                                                                
#                                                                                                                                                                                                 
# Copyright (C) 2002, Lowell Filak.                                                                                                                                                               
# You may distribute this file under the terms of the Artistic                                                                                                                                    
# License.                                                                                                                                                                                        
#                                                                                                                                                                                                 
# Given a shapefile name, item name, & item value this routine will dump out                                                                                                                      
#   the matching records.                                                                                                                                                                         
#                                                                                                                                                                                                 
# Required modules are mapscript (installed as part of make install),                                                                                                                             
#   Getopt (normally included with Perl), & XBase.                                                                                                                                                
#   Please download boundary.tar.gz also, and:                                                                                                                                                    
#     tar -xf boundary.tar.gz --ungzip                                                                                                                                                            
#                                                                                                                                                                                                 
# Suggested run line = ./find.pl -file=boundary -item=loc_name -value='Medina City'                                                                                                               
use mapscript;                                                                                                                                                                                    
use Getopt::Long;                                                                                                                                                                                 
use XBase;                                                                                                                                                                                        
#                                                                                                                                                                                                 
# Retrieve the input values.                                                                                                                                                                      
GetOptions('file=s' => \$file, 'item=s' => \$item, 'value=s' => \$value);                                                                                                                         
#                                                                                                                                                                                                 
# Check the input values.                                                                                                                                                                         
if ( (!$file) || (!$item) || (!$value) ) {                                                                                                                                                        
  print "Syntax: find.pl -file=[filename] -item=[item_name] -value=[item_value]\n";                                                                                                               
  exit 0;                                                                                                                                                                                         
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Open the existing shapefile.                                                                                                                                                                    
$shapefile = new shapefileObj($file, -1) or die "Unable to open shapefile $file.";                                                                                                                
#                                                                                                                                                                                                 
# Print out the entered values.                                                                                                                                                                   
print "Shapefile: $file\nItem Name: $item\nItem Value: $value\n\n";                                                                                                                               
#                                                                                                                                                                                                 
# Open the db handle.                                                                                                                                                                             
my $dbh = new XBase "$file" or die XBase->errstr;                                                                                                                                                 
#                                                                                                                                                                                                 
# Grab and process the information.                                                                                                                                                               
my @row;                                                                                                                                                                                          
#                                                                                                                                                                                                 
# Set the number of results to 0.                                                                                                                                                                 
my $results = 0;                                                                                                                                                                                  
#                                                                                                                                                                                                 
# Open a new shape object to hold the found shapes.                                                                                                                                               
my $shape = new shapeObj(-1);                                                                                                                                                                     
#                                                                                                                                                                                                 
# Use xbase to query by attribute.                                                                                                                                                                
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
  # Does the value from the field match the query.                                                                                                                                                
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
  print "The Row Found: $record\nThe Value Found: $fndvalue - go figure :-)\n";                                                                                                                   
  #                                                                                                                                                                                               
  # Increment the results counter.                                                                                                                                                                
  $results = $results + 1;                                                                                                                                                                        
  #                                                                                                                                                                                               
  # Grab shape #$record and stick it into the shape holder.                                                                                                                                       
  $shapefile->get($record, $shape);                                                                                                                                                               
  #                                                                                                                                                                                               
  # Get the bounds of the shape.                                                                                                                                                                  
  my $minx = $shape->{bounds}->{minx};                                                                                                                                                            
  my $miny = $shape->{bounds}->{miny};                                                                                                                                                            
  my $maxx = $shape->{bounds}->{maxx};                                                                                                                                                            
  my $maxy = $shape->{bounds}->{maxy};                                                                                                                                                            
  #                                                                                                                                                                                               
  # Print the bounds of the shape.                                                                                                                                                                
  printf "bounds (%f,%f) (%f,%f)\n", $minx, $miny, $maxx, $maxy;                                                                                                                                  
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Print the number of records found.                                                                                                                                                              
print "Number of Results: $results\n";                                                                                                                                                            
```                                                                                                                                                                                               
                                                                                                                                                                                                  
----                                                                                                                                                                                              
[wiki:PerlMapScript back to PerlMapScript
