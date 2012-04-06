{{{                                                                                                                                                                                               
#!perl                                                                                                                                                                                            
#!/usr/bin/perl -w                                                                                                                                                                                
#                                                                                                                                                                                                 
# Given a shapefile name this routine will dump out the attribute information.                                                                                                                    
#                                                                                                                                                                                                 
# Required modules are mapscript (installed as part of make install)                                                                                                                              
#    & Getopt (normally included with Perl).                                                                                                                                                      
#   Note: It does not require XBase. In 3.5 there is a DBFInfo object                                                                                                                             
#         available which makes READING basic dbf information much easier IMHO.                                                                                                                   
#   Please download boundary.tar.gz also, and:                                                                                                                                                    
#     tar -xf boundary.tar.gz --ungzip                                                                                                                                                            
#                                                                                                                                                                                                 
# Suggested run line = ./shpinfo.pl -file=boundary                                                                                                                                                
use XBase;                                                                                                                                                                                        
use mapscript;                                                                                                                                                                                    
use Getopt::Long;                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Declare a hash for easy access to shape types & file types.                                                                                                                                     
%shptypes = ( '1' => 'point',                                                                                                                                                                     
           '3' => 'arc',                                                                                                                                                                          
           '5' => 'polygon',                                                                                                                                                                      
           '8' => 'multipoint'                                                                                                                                                                    
         );                                                                                                                                                                                       
%typetypes = ( '0' => 'String',                                                                                                                                                                   
            '1' => 'Integer',                                                                                                                                                                     
            '2' => 'Double',                                                                                                                                                                      
            '3' => 'Invalid'                                                                                                                                                                      
          );                                                                                                                                                                                      
#                                                                                                                                                                                                 
# Grab the filename from the input.                                                                                                                                                               
&GetOptions("file=s", \$file);                                                                                                                                                                    
#                                                                                                                                                                                                 
# Check the input filename.                                                                                                                                                                       
if(!$file) {                                                                                                                                                                                      
  print "Syntax: shpinfo.pl -file=[filename]\n";                                                                                                                                                  
  exit 0;                                                                                                                                                                                         
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Open the shapefile.                                                                                                                                                                             
$shapefile = new mapscript::shapefileObj($file, -1) or die "Unable to open shapefile $file.";                                                                                                     
#                                                                                                                                                                                                 
# Print the shapefile name, type, number of features, & bounds.                                                                                                                                   
print "Shapefile $file:\n\n";                                                                                                                                                                     
print "\ttype: ". $shptypes{$shapefile->{type}} ."\n";                                                                                                                                            
print "\tnumber of features: ". $shapefile->{numshapes} ."\n";                                                                                                                                    
printf "\tbounds: (%f,%f) (%f,%f)\n", $shapefile->{bounds}->{minx}, $shapefile->{bounds}->{miny}, $shapefile->{bounds}->{maxx}, $shapefile->{bounds}->{maxy};                                     
#                                                                                                                                                                                                 
# Create the XBase object.                                                                                                                                                                        
$table = new XBase $file.'.dbf' or die XBase->errstr;                                                                                                                                             
#                                                                                                                                                                                                 
# Print the dbf file name & number of records..                                                                                                                                                   
print "\nDBFInfo table $file.dbf:\n\n";                                                                                                                                                           
print "\tnumber of records: ". ($table->last_record+1) ."\n";                                                                                                                                     
#                                                                                                                                                                                                 
# Print the header for the field dump.                                                                                                                                                            
print "\tnumber of fields: ". ($table->last_field+1) ."\n\n";                                                                                                                                     
print "\tName             Type    Length Decimals\n";                                                                                                                                             
print "\t---------------- ------- ------ --------\n";                                                                                                                                             
#                                                                                                                                                                                                 
# Loop through each field.                                                                                                                                                                        
for ($table->field_names) {                                                                                                                                                                       
  #                                                                                                                                                                                               
  # Print the field name, type, width, & # of decimal places.                                                                                                                                     
  printf "\t%-16s %-7s %6d %8d\n", $_, $table->field_type($_), $table->field_length($_); $table->field_decimal($_)                                                                                
}                                                                                                                                                                                                 
#                                                                                                                                                                                                 
# Close the dbf file.                                                                                                                                                                             
undef $table;                                                                                                                                                                                     
}}}                                                                                                                                                                                               
----                                                                                                                                                                                              
back to PerlMapScrip
