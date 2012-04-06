These instructions were sent to mapserver-users by Lindsay C. Blanton:  
                                                                        
To get php_mapscript working properly on a Redhat Fedora box, I used the
following steps:                                                        
                                                                        
  1.  Download the php-devel rpm from Redhat and install                
  2.  Compile mapserver with the --with-php=/usr/include/php option     
  3.  Copy mapserv to /cgi-bin                                          
  4.  Copy the php_mapscript.so file into /usr/lib/php4/                
  5.  Create a php wrapper script (php.sh) for the php cgi and save it  
    into /cgi/bin:                                                      
{{{                                                                     
    #!/bin/bash                                                         
    export SCRIPT_FILENAME=$PATH_TRANSLATED                             
    /usr/bin/php                                                        
}}}                                                                     
  6.  Add the following action handlers for phtml (and/or .php) files in
httpd.conf                                                              
{{{                                                                     
    Action phtml-script /cgi-bin/php.sh                                 
    AddHandler phtml-script .phtml                                      
}}}                                                                     
Now apache calls the php cgi for mapscript php files, and the php apache
module is called for everything else.                                   
                                                                        
Hope this helps someone out there using preinstalled Linux              
distributions.                                                          
                                                                        
Go back to: [wiki:PHPMapScript
